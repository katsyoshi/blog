---
layout: post
title: "Lrama の Ruby Parser を Ruby で組みたてたい v 0.0.0"
date: 2026-10-18 00:00:00 +0900
comments: true
categories: diary tech keyboard parser lrama
---

皆さんごぞんじだと思いますが [**Lrama**](https://github.com/ruby/lrama) っていう [**Ruby**](https://github.com/ruby/ruby)
製の _パーサージェネレーター_ があります。

_パーサージェネレーター_ で有名なのは、[**Yacc(yet another compiler compiler)**](https://www.tuhs.org/cgi-bin/utree.pl?file=V6/usr/source/yacc)
や [**Bison**](https://cgit.git.savannah.gnu.org/cgit/bison.git) があります。
**Lrama** はこの **Yacc/Bison 形式** で書かれたパーサー定義ファイルから _Parser(構文解析)_ を作ります。

**Lrama** は **Yacc/Bison形式** の構文定義ファイルから _parser_ を生成できます。
ただし、標準の出力先は **C言語** で、 **Ruby** に向けのバックエンドはありません。


## ないならしょうがない

出力されていないならしょうがないつくるしかない。
のでつくりましょう。
とその前にどのような _parser_ にしたいのかというものを書いていきましょう。

1. **Lrama** で **Ruby** の文法定義ファイル(**parse.y**) を読み込む
1. 読み込んだ定義から _Ruby 向け Parser_ を作る
1. _Lexer(字句解析）_ と生成した _Parser_ を組合せて **Ruby** を読み込む入口となる **Ruby ファイル** を作る
1. 作成した入口のプログラムを **Ruby** から利用できるようにする

というのが簡単な流れです。

## Design Workflow

ということでやることが決まったので詳細を考えていきましょう。

![architecture](/images/arpaka/architecture.webp)

上の図が簡単な流れですね。

**Lrama** に **Yacc/Bison 形式の定義ファイル** を渡し、
文法（_Grammar_）と **Action** から **Ruby Parser** を生成します。
生成された _Parser_ に _Lexer_ と _AST Builder_ を組み合わせ、
**Ruby** だけで動作する一つのフロントエンドファイルとして出力します。

ここで **Action** は、文法規則に対応して実行される意味処理です。
構文木のノードを組み立てる処理などを担当します。

_Lexer_ は Ruby のソースを読み取り、
_Parser_ が扱う _token_ の列に変換します。
_Parser_ はその _token_ 列を文法定義に従って解析します。

_Parser_ の処理の簡易的な流れは以下のようになります。

```
Ruby source -> Lexer -> token 列 -> Parser -> AST
```

生成されたファイルを **Ruby** の _Parser_ として利用できるようにし、
**Ruby** から読み込んで **Ruby** プログラムを構文解析できるようにします。


## おわり

設計が決まり実装していく手順が見えてきました。
次回は実際に実装していきましょう。

[![katsyoshi/arpaka - GitHub](https://gh-card.dev/repos/katsyoshi/arpaka.svg)](https://github.com/katsyoshi/arpaka)
