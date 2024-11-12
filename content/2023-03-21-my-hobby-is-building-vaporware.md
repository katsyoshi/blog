+++
path = "/blog/2023/03/21/my-hobby-is-building-vaporware"
layout = "post"
title = "趣味は vaporware 造りですv0.0.0"
date = 2023-03-21
comments = true
categories = "diary compiler"
+++

プログラマー三大勉強はしたけど、実装はしたことないものといえば
CPU、OS、コンパイラーなのです[^kojinsa]が、先日 [ruby30th 誕生日会のキーノート](https://www.publickey1.jp/blog/23/ruby30static_compiler_for_ruby.html)で
matz が "Static Compiler for Ruby" という今はまだない `vaporware` として
挙げていたのでこの **static compiler** を作ろうとなりました。

[![katsyoshi/vaporware - GitHub](https://gh-card.dev/repos/katsyoshi/vaporware.svg)](https://github.com/katsyoshi/vaporware)

## Goal

**Ruby** のコードを **static compile** できるようにする。

コンパイル先のターゲットは **x86** とします。 **ARM** や **RISC-V** などは今回の実装ではターゲットにしないです。

とはいえすべての **Ruby** の機能を実装すると時間がかかりすぎるので個人で無理のない範囲
で作ろうとします。無理のない実装範囲は以下なのかなと

1. 四則演算
1. 変数
1. メソッド
1. 制御構文
1. プリミティブ型

この5つの機能を実装する予定です。

### 実現する機能以外のことについて

5つの機能を実現すること以上のことはやらない予定です。
やらないこととしては _最適化_ 、 _GC_ 、 外部ファイルで定義したメソッドやクラスの読み込みは実装しない予定です。
実装しない個人的意見を以下に書いていきます。

最適化は **Ruby** のコードを単純に _機械語_ におとしただけでは現在の **RubyVM**
より速くならないと考えているからです(要確認)。
**LLVM IR** などへの変換ではなく、 _機械語_ なのは **LLVM** をインストールする必要があるなどして
面倒なのが大きいです[^gentoo]。あとバージョン毎に **LLVM IR** が異なるのも現状では対応しにくい点となっています。

_GC_ (Garbage Collection: ガベージコレクション) についてはそもそもクラスをサポートできない、
変数などのメモリを確保しておく時間が長いプログラムを対象としないので今回はスコープ外としています。

外部ファイル読み込みについてですが、外部ファイルの読み込みして _コンパイル_ するだけなら
そこまで問題にはならないと考えていますが、外部ファイルで定義された _メソッド_ や _クラス_ を
事前に _コンパイル_ して最後に _リンク_ するのは型が不定になるのでサポートするのは難しいと考えています。

## 実装方針
Goal までの実装は [低レイヤを知りたい人のためのCコンパイラ作成入門](https://www.sigbus.info/compilerbook) を参考にすすめていきます。
最初に _コンパイラー_ を実装するものとして _C 言語_ がたぶん勉強してきてうかぶと思います[^java]。
_C 言語_ だと _機械語_ や _VM_ のバイトコードへ落とすことのできる資料が多いので選択しています。
とは言っても **Ruby** からの脳内変換はある程度必要なので慣れているというのもあります。

実装としては _AST_(Abstract Syntax Tree: 構文抽象木) から愚直に **x86 アセンブラ** をファイルへ書き出し、
そこから _C コンパイラー_ (`gcc` or `clang`) をつかって機械語へ _コンパイル_ します。
フルフルの **Ruby** を実装するわけじゃないので依存する **gem** の依存も極力減らしたいです。

### 実装環境

- CPU: Ryzen Thread Ripper 1950x
- gcc: 12.2.1
- clang: 16.0.0
- OS: Gentoo Linux
- Linux Kernel: 6.2 系

## パーサー

まず、_AST_ を得るために _パーサー_ が必要なのですが、 **Ruby** の構文は複雑なのでここは頑張らないようにします。ここをどうやって解決するのかというと `RubyVM::AbstractSyntaxTree` や `Ripper` をつかうのか、`parser.gem` をつかうのかを決めるひつようがあります。今回というかしばらくは `parser.gem` を利用して _AST_ を得ることにします。
ゆくゆくは `RubyVM::AbstractSyntaxTree` への置き替えはするよていです。

ここはそのまま `parser.gem` のチュートリアルどおりにすれば _AST_ が得られます。

```ruby
require "parser/current"

puts Parser::CurrentRuby.parse("(1 + 2) * 3 / (5 - 4)")
```

## きょうはここまで

とりあえず手を動かしはじめましたが、ななななんと、似たような機能が実は **Ruby 3.3** 向けに
_JIT_ として入ったようです[^rjit]。
ということでねこの話はね、勉強の話しかないんですが一旦 Goal まで作ってみましょうね。

----

[^kojinsa]: 個人差があります。
[^gentoo]: Gentoo Linux 使っているので毎回コンパイルしているので本当にめんどう。バイナリあるよとかそういう正論は受け付けていないです。
[^java]: 時代によって変わるかも。**Java** だったり、**Lisp** だったりする人がいるかも。
[^rjit]: https://github.com/ruby/ruby/pull/7448
