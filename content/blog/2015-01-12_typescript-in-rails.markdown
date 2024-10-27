+++

title = "Ruby on RailsでTypeScriptを使ってみる"
date = 2015-01-12
comments = true
categories = "tech rails ruby js typescript"
+++

ふとRuby on RailsでTypeScriptを使いたくなったのでうごくようにしてみた。


## インストール

これは簡単で `Gemfile` に `typescript-rails` を追加すればいけます。ですが、[jQueryがTypeScript v1.4以上しかサポートしていないようなので](http://stackoverflow.com/questions/28117786/why-am-i-not-able-to-compile-a-file-that-references-jquery-d-ts)それに対応したものも追加します。

```ruby
gem 'typescript-src', github: 'katsyoshi/typescript-src-ruby'
gem 'typescript-rails'
```

インストールはこれだけでOKです。

### jQueryをつかう

TypeScriptでjQueryを使う場合、TypeScriptの型定義ファイルをインストールする必要があります。
で、それを自力で作るのは大変なので、 `tsd` を利用してインストールします。

#### tsd をインストール

`tsd` は `npm` でインストールできるので、それでインストールします。

```
$ npm install tsd -g
```

でインストールできたので、この `tsd` を利用してjQueryの型定義ファイルをインストールします。

```
$ tsd init
$ $EDITOR tsd.json
  path: "app/assets/javascripts/typings",
  bundle: "app/assets/javascripts/typings/tsd.d.ts"
$ tsd query jquery -osa install
```

でjQueryを利用する準備は整いました。

## TypeScriptを書いてみよう

コンソールに `hello, world!` を出すだけの例を示します。

```javascript
$(() => {
  console.log("hello, world!");
});
```

でいけますよ。
が `$('#hello').text('hoge');` いかねぇ。
