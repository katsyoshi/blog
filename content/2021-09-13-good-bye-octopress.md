+++
path = "/blog/2021/09/13/good-bye-octopress"
layout = "post"
title = "Octopress 脱出"
date = 2021-09-13T00:00:00+09:00
comments = true
categories = "diary"
+++

長年利用してた [Octopress](https://github.com/octopress/octopress) がずいぶん前にサポート外になってたので [Jekyll](https://github.com/jekyll/jekyll) へ変更することにした。

## 問題点

サポート外になったことによる問題点があり、その問題によって変更することになった。
問題点としては、 *[Pygments.rb](https://rubygems.org/gems/pygments.rb)* と *[Compass](https://rubygems.org/gems/compass)* と利用している *[Ruby](https://ruby-lang.org/)* のバージョンが 2.3.7 というのが主であった。

## 変更
やったことは以下のとおり。

1. gem の整理
  - 最初に不要な *gem* を削除。特に消したいのは *Pygments.rb* と *Compass* それ以外にも不要なものがあるので消す。
1. ディレクトリの整理
  - posts は元々の設定が `source/_posts` に入れていたが、 `_posts` に変更
  - 画像も同様に `source/images` だったものを `images` に変更
1. テンプレートの整理
  - テンプレート *[minima](https://rubygems.org/gems/minima)* を利用するにあたり `_includes` などの中身を整理
1. プラグインの整理
  - いまのところ *[jekyll-paginate-v2](https://rubygems.org/gems/jekyll-paginate-v2)* のみ変更で他は入れていない。
    - そのうち theme も含めて検討したい
1. [Rake コマンド](https://rubygems.org/gems/rake) の整理
  - とりあえず octopress で利用してた *Rakefile* から必要最低限の変更のみ実行。

## まとめ

日曜日の夜中 **[RubyKaigi の日記](/blog/2021/09/12/takout-dot-rubykaigi-dot-org-2021/)** をポストしてからこのリポジトリを *jekyll* にしようと **雑に** 弄ってたら、 `rake generate` が `Unknown language: xml` という謎の *Pygments* エラーが出てしまった。
このエラーを修正しようとしがんばってみたが、失敗して修正できなかったので *jekyll* に変更したものでデプロイした。

そんなこんなあって *Octopress* はやめて *jekyll* に変更しました。
