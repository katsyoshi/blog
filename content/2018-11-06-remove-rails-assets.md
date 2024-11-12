+++
path = "/blog/2018/11/06/remove-rails-assets"
layout = "post"
title = "remove rails assets"
date = 2018-11-06
comments = true
categories = "tech rails"
+++

ひさびさに `Ruby on Rails` の話で、自作の rails application で `rails-assets` というところからいくつか gem を利用してたので
それを `Yarn` で同様のパッケージをインストールするように変更した。

## なにをやったのか？

単純に一旦 `rails-assets` からインストールしている gem を `Gemfile` から削除します。

そのあと、 `yarn add` で `package.json` を生成し `node_modules` にインストールします。

```console
yarn add bootstrap@3 font-awesome jquery
yarn install
rails s
```

インストールが終了し、railsを起動したら必要な情報([font-awesome](https://blog.zerokol.com/2018/06/ruby-on-rails-yarn-font-awesome-in.html), [bootstrap](https://qiita.com/fuqda/items/c5756b8f56dcc238c110)) を `app/assets/{javascripts,stylesheets}` に記載。

```javascript
// app/assets/javascripts/application.js
//= require rails-ujs
//= require turbolinks
//= require jquery/dist/jquery.js
//= require bootstrap-sass/assets/javascripts/bootstrap
//= require_tree .
```

```css
# app/assets/stylesheets/application.scss
@import 'bootstrap-sass/assets/stylesheets/bootstrap';
@import "font-awesome/scss/variables";
$fa-font-path: "font-awesome/fonts/";
@import "font-awesome/scss/mixins";
@font-face {
  font-family: 'FontAwesome';
  src: font-url('#{$fa-font-path}/fontawesome-webfont.eot?v=#{$fa-version}');
  src: font-url('#{$fa-font-path}/fontawesome-webfont.eot?#iefix&v=#{$fa-version}') format('embedded-opentype'),
    font-url('#{$fa-font-path}/fontawesome-webfont.woff2?v=#{$fa-version}') format('woff2'),
    font-url('#{$fa-font-path}/fontawesome-webfont.woff?v=#{$fa-version}') format('woff'),
    font-url('#{$fa-font-path}/fontawesome-webfont.ttf?v=#{$fa-version}') format('truetype'),
    font-url('#{$fa-font-path}/fontawesome-webfont.svg?v=#{$fa-version}#fontawesomeregular') format('svg');
  font-weight: normal;
  font-style: normal;
}
@import "font-awesome/scss/core";
@import "font-awesome/scss/larger";
@import "font-awesome/scss/fixed-width";
@import "font-awesome/scss/list";
@import "font-awesome/scss/bordered-pulled";
@import "font-awesome/scss/animated";
@import "font-awesome/scss/rotated-flipped";
@import "font-awesome/scss/stacked";
@import "font-awesome/scss/icons";
@import "font-awesome/scss/screen-reader";
$icon-font-path: "bootstrap-sass/assets/fonts/bootstrap";
```

ってかいたら使えるようになっています!

## おわり

ということで *IMASARA* ですが `rails-assets` からの脱却ついでに簡単に `yarn` を利用した assets の導入をやってみました!
