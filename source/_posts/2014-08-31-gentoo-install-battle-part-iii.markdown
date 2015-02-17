---
layout: post
title: "Gentoo Install Battle Part III"
date: 2014-08-31 10:47:14 +0900
comments: true
categories: tech linux mikutter gentoo
---

これ[まで](/blog/2014/08/20/gentoo-install-battle-part-i/)の[流れ](/blog/2014/08/21/gentoo-install-battle-part-ii/)で準備が整ったので、Installを始めます。

## インストール

これは[Gentoo Wiki](http://wiki.gentoo.org/)を見ながらで大丈夫です。

### 嵌ったところ

音が鳴らないという問題が発生しました。[理由](http://pocke.hatenablog.com/entry/2014/02/02/100636)は簡単でしたのですぐに設定しました。

つぎにネットワークの設定をnetmountからNetworkManagerに変更しました。
設定は[ここ](http://wiki.gentoo.org/wiki/NetworkManager)で行なえばよいです。

最後にibus-mozcをインストールして終りです。

## mikutter

GentooInstallBattleはmikutterをインストールしてtweetすることなのでmikutterをインストールします。
ここではrbenvを利用してmikutterのインストールします。
まずはrbenvとruby-buildをインストールします。

```sh
$ git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
$ git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
```

つぎにrbenvの設定を行います。以下の設定を`~/.bashrc`あたりに追記してください

```sh
export PATH=~/.rbenv/bin:$PATH
eval "$(rbenv init -)"
```

Rubyのインストールは`rbenv install 2.1.2`でインストールします。
最後にmikutterをダウンロードし、環境を構築します。

```sh
$ git clone https://github.com/mikutter/mikutter.git ~/mikutter
$ cd ~/mikutter
$ bundle install
$ bundle exec ruby mikutter.rb
```

でインストール終了!!
これで快適なmikutterライフがておくれますね。

![mikutter](/images/photo/mikutter.png)

