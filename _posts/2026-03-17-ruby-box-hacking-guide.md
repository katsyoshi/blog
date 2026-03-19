---
layout: post
title: "Ruby::Box Hacking Guide"
date: 2026-03-17 13:09:14 +0900
comments: true
categories: diary tech
---

`Ruby::Box` と戦っているので `Ruby::Box` を [**Codex**](https://chatgpt.com/codex) に **Ruby::Box Hacking Guide** としてまとめてもらいました。

ここから本文

## 1. Ruby::Box とは何か

`Ruby::Box` は、同じ Ruby VM の中で、box ごとのロード状態や class / module に紐づく内部状態の一部を切り分けるための仕組みです。`Ruby::Box.new` で新しい box を作ると、`$LOAD_PATH`、`$LOADED_FEATURES`、global variable table、class / module に紐づく一部の内部状態が box ごとに分かれます。

Ruby::Box の目的は、同じ VM の中で box ごとの状態分離を実現することです。そのためには、どの状態を共有し、どの状態を box ごとに保持し、どこで解放するのかという ownership を正しく管理する必要があります。

## 2. まず読むファイル

Ruby::Box を追うときは、次の順で読むと流れを掴みやすいです。

- `box.c`
- `class.c`
- `internal/class.h`
- `test/ruby/test_box.rb`

まず `box.c` で Ruby 側 API と box の lifecycle を見て、次に `class.c` と `internal/class.h` で class / module 周りの内部状態の扱いを追います。最後に `test/ruby/test_box.rb` を読むと、外部からどのような振る舞いが期待されているかと、既存のテストの流儀をまとめて確認できます。

## 3. 各ファイルの責務

`Ruby::Box` を追うときは、まず「どのファイルが何を担当しているか」を押さえた方が読みやすくなります。

`box.c` は Ruby 側 API と box 自体の lifecycle を担当します。`Ruby::Box.new`、`Ruby::Box.current`、`Ruby::Box#require`、`Ruby::Box#eval` といった入口に加えて、box-local な load path、loaded features、global variable table、local extension cleanup もここで扱います。

`class.c` は class / module に紐づく Ruby::Box の内部実装の中心です。`rb_class_duplicate_classext`、`class_duplicate_iclass_classext`、`rb_class_classext_free` など、box ごとの class/module 状態を複製・管理・解放する処理があります。

`internal/class.h` は `rb_classext_t` の定義と、その読み書きを行う macro / helper を持ちます。box ごとの classext lookup がどのように切り替わるかを見るにはここが基準になります。

`test/ruby/test_box.rb` は Ruby::Box に期待されている外部挙動を確認する場所です。API の使い方だけでなく、どのような regressions が既に認識されているかを把握するうえでも重要です。

## 4. `rb_classext_t` と box ごとの状態

Ruby::Box を理解するうえで重要なのは、class object 自体を box ごとに丸ごと複製しているわけではない、という点です。実際に box ごとの差分を支えているのは `rb_classext_t` です。

概念的には、次のような構造をイメージすると分かりやすいです。

```text
                +----------------------+
                | class / module obj   |
                |       (shared)       |
                +----------------------+
                           |
          +----------------+----------------+
          |                                 |
          v                                 v
  +----------------------+        +----------------------+
  | prime classext       |        | box classext         |
  +----------------------+        +----------------------+
  | m_tbl                |        | m_tbl                |
  | const_tbl            |        | const_tbl            |
  | cvc_tbl              |        | cvc_tbl              |
  | subclasses / caches  |        | subclasses / caches  |
  +----------------------+        +----------------------+
```

Ruby::Box の class/module 周りの挙動は、「どの class object を見るか」ではなく「どの box 用 classext を使うか」で切り替わります。したがって、method table、constant table、class variable cache table、subclass 関連の情報、一部の cache 類が、shared のままよいのか、box ごとに持つべきかを常に意識する必要があります。

`include` が絡むと `ICLASS` 用の classext も登場しますが、それは基本構造の上に乗る特殊ケースです。まずは「shared な class/module object に対して、box ごとに classext がぶら下がる」という見方を固めるのが先です。

## 5. Box の基本操作はどう実装されているか

Ruby 側から見える操作の大半は `box.c` にあります。`Ruby::Box.new` は root box の状態を土台にして、新しい box-local 状態を初期化します。`Ruby::Box.current`、`Ruby::Box.root`、`Ruby::Box.main` は、現在どの box 文脈で実行しているかを表す入口です。

`Ruby::Box#require`、`Ruby::Box#load`、`Ruby::Box#require_relative` は、box ごとの load path と loaded features を使って feature を解決します。現状の実装では、新しい box は root 側の `$LOAD_PATH` や `$LOADED_FEATURES` を初期値として複製し、その後は独立して変化します。そのため、box 作成後に main 側で行った path の変更や gem activation は box 側に自動では反映されず、`require` の解決結果が一致しないことがあります。

`Ruby::Box#eval` は、指定した box の文脈でコードを評価する入口です。再現コードの切り分けや、box ごとの定数・メソッド解決を確認するときに重要です。

また、Ruby::Box は Ruby-level API だけで完結していません。class/module 周りの状態は `class.c` と `internal/class.h` が支えており、必要な classext は読むタイミングや書くタイミングで作られます。新しい box を作った瞬間に全てが eager に複製されるわけではありません。

## 6. Box 拡張時の設計上の論点

以下は、現状の Ruby::Box 実装から読み取れる範囲で、box ごとの状態分離を考えるうえで整理が必要になる点をまとめたものである。

新しい機能や内部状態を Box 対応させるときは、まず次の 4 点を整理する必要があります。

1. その状態は shared でよいのか
2. box-local にするならどこに持つのか
3. いつ複製するのか
4. どこで解放するのか

box-local な load path や loaded features のように `rb_box_t` にぶら下がっているものもあれば、class/module に紐づくので `rb_classext_t` に置かれているものもあります。新しい state を追加するときは、まずその所有単位が box なのか class/module なのかを見定める必要があります。

次に必要なのは duplication の方針です。eager に複製するのか、最初の write や最初の lookup まで遅延させるのかで実装が変わります。Ruby::Box は copy-on-write に寄せた構造が多いので、複製の入口と invalidation の入口を分けて考えた方が追いやすくなります。

最後に、mark、write barrier、free の対応も確認対象になります。Ruby object への参照が増えるなら GC の mark/update が必要になりますし、table entry を heap 上に積むなら cleanup 側で誰が free するのかを決める必要があります。Ruby::Box の拡張では、機能追加そのものより ownership と lifecycle の整合性が問題になりやすいです。

## 7. テストの書き方

Ruby::Box のテスト入口は `test/ruby/test_box.rb` です。ここには `enabled?`、`current`、`root`、`main`、`require`、`load`、nested box、ICLASS 周りの回帰テストがまとまっています。

通常の機能追加なら、まず既存のテストと同じ粒度で `setup_box` を使ったテストを書けば十分です。一方で、exit 時 crash や process-global な副作用が絡むなら subprocess 系 assertion を使います。

- `assert_separately`
- `assert_ruby_status`
- `assert_in_out_err`

exit status だけ見たいなら `assert_ruby_status` が軽く、標準出力や標準エラーも検証したいなら `assert_in_out_err` を使います。Box 内部の regression test では、可能な限り pure Ruby の再現に寄せ、stdlib native extension 依存は避ける方が無難です。

## 8. Box 拡張時の実装確認項目

実装を入れた後は、少なくとも次を確認した方がよいです。

- root / main / user box で見え方が変わるか
- `require` / `load` / `eval` をまたいで状態が期待通りに見えるか
- class/module に紐づく状態なら classext duplication と cleanup が対応しているか
- GC mark/update と write barrier が不足していないか
- process 終了時や box teardown 時にだけ壊れないか

特に class/module に紐づく state は、「作る場所」と「free する場所」をセットで確認した方がよいです。複製側だけを見ていると shallow copy の見落としをしやすく、cleanup 側だけを見ていると shared state を壊しやすくなります。

## 9. Appendix: 壊れやすい箇所

Ruby::Box の不具合は、表面的には `NameError`、`LoadError`、stale cache、終了時 crash のように見えても、内部では ownership mismatch に収束することが多いです。特に class/module 系では、次の 3 点を一本の線で追うと整理しやすくなります。

1. box 作成時に何が複製されるか
2. 何が shared のまま残るか
3. 終了時に何が free されるか

特に class/module 系では、次の関数から読むと追いやすいです。

- `rb_class_duplicate_classext`
- `class_duplicate_iclass_classext`
- `rb_class_classext_free`

class variable 系なら `variable.c` の `rb_cvar_set` が `rb_cvar_class_tbl_entry` を作り、`vm_insnhelper.c` がそれを cache として使います。allocation と free が本当に 1 対 1 になっているかを見ると、exit 時 crash の切り分けがしやすくなります。

## 10. Appendix: build / run の落とし穴

Ruby::Box の調査では、VM の問題と build/run 環境の問題が混ざりやすいです。

- source tree Ruby に別 install の native extension を混ぜると ABI mismatch で壊れる
- 新しい box は root 側の `$LOAD_PATH` や `$LOADED_FEATURES` を初期値として複製し、その後は独立して変化する
- `extconf.rb` を変な場所で叩くと top-level `Makefile` を壊すことがある

再現コードを削るときは、まず unpatched build だけで確実に落ちるケースを作り、その後で patched build に切り替えて差分を見る方がよいです。最初から両方を並行で追うと、再現条件が曖昧なまま比較してしまいやすくなります。
