---
layout: post
title: "Lrama の Ruby Parser を Ruby で組みたてたい v 0.0.1"
subtitle: "Lrama の Ruby Parser に Action が必要な理由"
date: 2026-10-25 00:00:00 +0900
comments: true
categories: diary tech parser lrama
---

[![katsyoshi/arpaka - GitHub](https://gh-card.dev/repos/katsyoshi/arpaka.svg)](https://github.com/katsyoshi/arpaka)

[前回](/blog/2026/10/18/arpaka-extension-for-lrama/)は _パーサージェネレーター_ を使って
_parser_ を作るための準備し、構築する流れをみていきました。
今回は実際に _parser_ を作っていきます。というか _parser_ を吐き出すようにします。
**Lrama** の基本的な動作については、[かねこさん](https://github.com/yui-knk) の
[Ruby Parser開発日誌 (5)](https://yui-knk.hatenablog.com/entry/2023/03/13/101951) などに詳しいので、
ここではArpakaで問題になるActionの部分に絞ります。

## Rubyででてこないわけ

これも [Ruby Parser開発日誌 (6)](https://yui-knk.hatenablog.com/entry/2023/04/04/190413) などに詳しく書かれています。
結論からいうと出力される **C ファイル** に _parser_ と _lexer_ が密結合しており分離する
ことが困難で分割できないと。
分離した上で **Ruby** に置き換える意味は薄い、というのは私見です。

## Parse できる

まずは Lrama に簡単な文法を渡して、生成した _Parser_ で token 列を解析します。
電卓の文法なら、`NUMBER` と演算子の token 列を渡して、Action の結果として計算値を
返せます。

```ruby
parser_class = Lrama::Ruby.compile(File.read("examples/calculator.y"),
  filename: "examples/calculator.y", class_name: "Calculator")
parser = parser_class.new
parser.parse([[:NUMBER, 2], ["+", nil], [:NUMBER, 3], ["*", nil], [:NUMBER, 4]])
# => 14
```

この時点で、Lrama の解析表と Ruby の Parser は動いています。Parser に渡している
のはすでに token になった値なので、ここではまだ Ruby のソースを読んでいません。

## parse.y だけで parse できる

最初に Ruby の Parser を生成する入口は、`--ruby-source` でした。Ruby のソース
ツリーをそのまま渡し、その中の `parse.y` と token 定義を読み込んで Parser を
生成します。まずは必要なものを見落とさないように、Ruby のソースツリーごと
持ってくる形にしました。

そこから、Parser の生成に必要なものを少しずつ切り分けます。文法だけなら
`parse.y` を入力にすればよく、token の定義や Action、Lexer は別の部品として
扱えます。`--ruby-source` は最初に動かすための入口で、最終的な構成をそのまま
固定するためのものではありません。

次に、Ruby の文法定義である `parse.y` を Lrama に渡して Parser を生成します。
Ruby のソースツリー全体を先に組み込まなくても、`parse.y` を入力にして解析表を
作れるようにします。

ただし、`parse.y` だけで用意できるのは文法と Parser です。入力を token 列へ変換
する Lexer、規則を適用したときに値を作る Action、そこから結果を組み立てる処理は
別に必要です。そこで次に、Ruby の `parse.y` から何を持ってくるのかを見ていきます。

## Actionとはなにか

文法の規則に付いている `{ ... }` が **Action** です。_Parser_ は規則に合う
_token_ を見つけると、その規則の Action を実行します。文法は「どの並びを
受け付けるか」を決め、Action は「受け付けたものから何を作るか」を決めます。

たとえば電卓の文法なら、次のような規則を書きます。

```yacc
expression: NUMBER { $$ = $1 }
          | expression '+' expression { $$ = $1 + $3 }
```

`$1` や `$3` は規則の右辺で得た値、`$$` はこの規則から返す値です。規則だけ
でも入力を受け付けるかは決められますが、解析結果を組み立てるには Action が必要です。

Ruby の `parse.y` では、Action は結果を作るだけではありません。`while` の
規則で `COND_PUSH(1)` を呼ぶように、Lexer の状態を更新する Action もあります。
その状態によって、Lexer が後の `do` をどの token として返すかが変わります。
この Parser と Lexer の状態のやり取りについては、[Ruby Parser開発日誌 (6)]
(https://yui-knk.hatenablog.com/entry/2023/04/04/190413) に詳しく書かれています。

つまり、Lrama が文法から解析表を作れたとしても、Ruby の `parse.y` に書かれた
C の Action をそのまま Ruby の _Parser_ で動かすことはできません。Action の
意味処理も Ruby の処理として用意する必要があります。

## Mottekuru Bubunn

Lrama の汎用バックエンドが作ってくれるのは、文法から作った解析表と、それを
動かす _Parser_ です。[最初の実装](https://github.com/katsyoshi/arpaka/commit/13c091c)
では、`Lrama::Ruby.compile` に文法を渡し、
呼び出し側が `[token, value]` の列を渡していました。これは電卓のような小さな
文法を動かすには十分ですが、Ruby のソースを読むには Lexer と値を組み立てる処理が
必要です。

そこで Ruby の `parse.y` と Lexer の実装、
さらに [Ruby ソースから standalone frontend を生成する処理](https://github.com/katsyoshi/arpaka/commit/d91c60b)
を持ってきます。`parse.y` から
文法を読み込み、Ruby 側で必要になる Action は対応する Ruby の処理へ置き換えます。
さらに Lexer と Builder を組み合わせて、Parser だけでなく Ruby のソースを
token 列へ変換するところまで一つのフロントエンドにします。

ここで持ってくるのは Ruby の Parser を C のまま実行するためのファイルでは
ありません。Ruby の文法と token の対応を入力として使い、Lrama で解析表を作り、
Action と実行時の部品は Ruby で生成します。最終的には Parser、Lexer、Builder
を含んだ Ruby ファイルを出力します。

## Parse Ruby

まずは生成したフロントエンドを Ruby から読み込みます。

```ruby
require "arpaka"

frontend = Arpaka.compile(
  ruby_source: "/path/to/ruby",
  class_name: "RubyParser"
)

tree = frontend.parse("a = 1\na + 2", filename: "example.rb")
p tree
```

この `parse` は、最初の `Lrama::Ruby` のように token 列を直接渡すものでは
ありません。生成した Lexer が Ruby のソースを token 列へ変換し、Parser が
文法を適用し、Action と Builder が解析結果を組み立てます。ここまで動けば、
「Ruby の文法を Lrama に読ませる」だけでなく、「Ruby のソースを Ruby の
プログラムから parse する」入口になります。

生成したソースを保存したい場合は `Arpaka.generate` を使います。

```ruby
code = Arpaka.generate(
  ruby_source: "/path/to/ruby",
  class_name: "RubyParser"
)
File.write("ruby_parser.rb", code)
```

この段階のフロントエンドはまだ Ruby 全体を扱えるわけではありません。まずは
Action を Ruby へ移植し、Parser と Lexer を組み合わせて、Ruby の入力を
Ruby の処理へ渡す流れを通すところまでを目標にしています。

## まとめ
