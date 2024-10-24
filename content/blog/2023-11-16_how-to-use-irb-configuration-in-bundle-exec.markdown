+++
layout: post
title: "いいかんじに Bundler で管理されていない Gem を使いたい"
date: 2023-11-16 23:59:59 +0900
comments: true
categories: diary
+++

前回作成した `irb-theme-dracula` を **bundler** で **gem** が管理されているプロジェクトで利用したい。
利用したいが、そのままでは利用できないです。
これは **gem** が **bundler** で管理されているので、 `Gemfile` に書いていない **gem** は利用できないです。

## 対処案

**bundler** で管理されていない **gem** をどうしても利用したい場合は以下のような方法が考えられます。

1. :100: `Gemfile` に追加: 正攻法。ただ複数人で開発しているなどの場合色付けだけの機能で追加するのかというと…
1. :+1: 古きよき方法: [`setup.rb`](https://i.loveruby.net/ja/projects/setup/doc/usage.html) を利用して、対象 **gem** の必要ファイルをインストールする。
1. :poop: 実行する **Ruby** の `$LOAD_PATH` に **gem** のパスを追加: 強引にパスを追加して `require` することで解決。

ということでそれぞれ方法をみてみましょう。

### Gemfile に追加

正攻法ですね。便利で全員が使い、必要なら追加しましょう。
どうしても利用したい場合でプロジェクトの `Gemfile` に書きたくない場合はプロジェクトを管理している**バージョン管理システム** にコミットしないなどオペレーションを行いましょう。
管理方法が大変なのでこの方法はないなと。

### 古きよき方法

**rubygems** が生まれる前の方法をとりましょう。ここでは **`setup.rb`** で `site_ruby` に必要なファイルをインストールしてくれます。
便利なやつです。

#### 使い方

```console
$ gem install setup # gem でイントールします。
# インストールしたい gem のリポジトリをコピーなどして手元にもってきましょう。
$ cd /path/to/install/gem
$ setup.rb install # gem のインストールが行なわれます。
$ cd /path/to/your/project
$ bundle exec ruby your/scrip.rb
```

これでできるのですが問題点があり、この方法では利用したいプロジェクト以外でも利用できてしまうので特定のプログラムだけで `site_ruby` を読み込むとかしていいかんじに使い分けるには少し工夫が必要です。
全部の **Ruby** プログラムで利用したい訳ではないのでこの方法については断念。

### 実行する Ruby の $LOAD_PATH に利用する gem のパスを追加

この方法は単純で、プログラム側で必要なファイルを読み込む時のみ利用する。
利用するファイル(`require "irb/theme/dracula/light"` を書いているファイル)で利用したい **gem** へのパスを `$LOAD_PATH` へ追加します。
`$LOAD_PATH` への追加方法としては以下の方法があります。

1. 環境変数 (`$RUBYLIB`) に指定: 環境変数を利用するごとに指定することができる。
1. 実行時に指定: 実行時に `-I/path/to/gem` を `ruby` の引数に利用可能。利用するごとに指定する必要がある。
1. 実行ファイルで指定: 設定ファイルを読み込んで実行するような場合では楽。

ここでは **実行ファイルで指定** する方法を見ていきましょう。

実行対象のプログラムは `irb` です。 `irb` は `~/.irbrc` を読み込んで起動するため、 `~/.irbrc` に以下のような設定を書きます。

```ruby
# Reline 0.4.0 以上に対応した irb のバージョンチェック
if Gem::Version.new(IRB::VERSION) >= Gem::Version.new("1.9.0")
  # gem のインストールされるパスを取得。rbenv を利用している場合は以下
  # preview 判では version に previewX や 0 がついたりするので * を付与し、検索
  ld_path = File.join(ENV["RBENV_ROOT"], "versions", RUBY_VERSION + "*", "lib", "ruby", "gems", RUBY_VERSION.sub(/\d+$/, "0*"), "gems")
  # 読み込む gem 対象のパスを取得
  gem_path = File.join(Dir.glob(File.join(ld_path, "irb-theme") + "-*").last, "lib")
  $LOAD_PATH.unshift(gem_path) # LOAD_PATH に追加
  $LOAD_PATH.uniq! # LOAD_PATH に追加した重複してた場合削除
  require "irb/theme/dracula/light"
end
```

この設定を書いたら **Ruby** を `3.3.0-preview3` 以上にして `bundle exec irb` と実行してみましょう。

![](/images/screenshot/force-load-bundler-external-gem.png)

かった!

## おわり

**gem** は入ってるけど、プロジェクトで利用できないなあとおもい使えるようにしたいということでやってみました。
今回の `irb` は **Ruby** `2.2` 以下だとデフォルトでインストールされているのでそもそも
`Reline` の新しいバージョン(`0.4.0`)をサポートしていないのこの方法を利用してみました。
