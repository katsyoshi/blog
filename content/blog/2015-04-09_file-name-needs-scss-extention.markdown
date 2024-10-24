+++
layout: post
title: "ファイル名間違えてテスト通らなかった話"
date: 2015-04-09 15:38:49 +0900
comments: true
categories: rails twitter-bootstrap tech diary
+++

[rails-assets.org](https://rails-assets.org) から `bootstrap-sass-official` をインストールし、動かしたら、 test環境でテストが落ちてたのでメモ。

## オチ

CSSの[ファイル名](https://twitter.com/izumin5210/status/586054782994874368)が `application.css.scss` じゃなかった。

## 設定

まず、`Gemfile` に以下のように追記をします。

```ruby
source 'https://rails-assets.org' do
  gem 'rails-asstes-bootstrap-sass-official'
end
```

あとはいつも通りにインストールします。
次に `app/assets/javascripts/application.js` や `app/assets/stylesheets/application.css` に必要な読み込みを[設定](http://qiita.com/izumin5210/items/73f93347a9fe458dbbf5)します。

## 問題
以上の設定が終ったあと、 `cucumber` でテストを実行すると、

```
File to import not found or unreadable: bootstrap-sass-official/bootstrap-sprockets.
```

などといわれますのでオチを参考にファイル名変更して終了です。

## おわり

おわり

### 参考

1. http://qiita.com/izumin5210/items/73f93347a9fe458dbbf5
2. https://twitter.com/izumin5210/status/586054782994874368
