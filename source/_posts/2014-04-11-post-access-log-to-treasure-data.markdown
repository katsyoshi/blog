---
layout: post
title: "アクセスログをTreasureData.comへ"
date: 2014-04-11 00:50:27 +0900
comments: true
categories: linux tech log fluentd
---

いいかげん[treasure data](http://www.treasuredata.com)を使おうかな。
ということでこの鯖のアクセスログをfluentdを使って保存しようとおもいます。

今回は[td-agent](https://github.com/treasure-data/td-agent)を利用せずにgemからインストールしたfluentdを利用します。

## 環境
環境としてUbuntu 12.04を利用しています。

## 準備
事前準備としてユーザ、グループの作成、rubyのインストールを行ないます。

### ユーザ、グループの作成
以下のコマンドでユーザ、グループの作成を行います。
```
$ sudo adduser fluentd -s /bin/false
```
指示に従って入力するとユーザ、グループが作成されています。
このユーザは[ログインできません](http://qiita.com/shunichi/items/c7744878f5c02eaab18d#2-5)。

### Rubyのインストール

以下のコマンドを入力しRubyのインストールを行ないます。

```
$ sudo aptitude build-dep ruby1.9.3
$ sudo aptitude install git
$ sudo git clone git://github.com/sstephenson/rbenv.git /usr/local/rbenv
$ sudo git clone git://github.com/sstephenson/ruby-build.git /usr/local/rbenv/plugins/ruby-build
$ export RBENV_ROOT=/usr/local/rbenv
$ export PATH=${RBENV_ROOT}/bin:${PATH}
$ eval "$(rbenv init -)"
```

ここまで入力したら`sudo visudo -f /etc/sudoers.d/00_base`と入力し、以下を[入力してください](http://office.tsukuba-bunko.org/ppoi/entry/systemwide-rbenv)。
```
Defaults !secure_path
Defaults env_keep += "PATH RBENV_ROOT"
```
入力したらRuby 2.1.1をインストールします。
```
$ sudo rbenv install 2.1.1
$ sudo rbenv rehash
```
でインストール完了です。次にfluentdをインストールします。

### fluentdとプラグインのインストール
gemでfluentdとtdプラグインのfluent-plugin-tdをインストールします。

```
$ sudo gem install fluentd fluent-plugin-td
```

つぎに設定ファイル`fluentd.conf`を作成します。

{% include_code lang:xml fluentd.conf %}
作成後起動確認を行なってください`fluentd -c fluentd.conf`。
起動確認を行ったら`fluentd.conf`を`/etc/fluentd/fluentd.conf`に移動します。
これで終了です。

### init.dスクリプトの作成
まず`/etc/init.d/skelton`を`/etc/init.d/fluentd`にコピーします。
コピーしたら以下の様にします。

{% include_code fluentd_diff_skelton %}

このままでは起動しないので`/etc/default/fluentd`を作成します。

{% include_code /etc/defaults/fluentd lang:sh fluentd.default %}

としたら、以下のコマンドを入力し、fluentdデーモンを起動します。

```
$ sudo update-rc.d fluentd defaults
$ sudo mkdir -p /var/log/fluentd
$ sudo chown fluentd:fluentd /var/log/fluentd
$ sudo service fluentd start
```

で起動してるはずでう。

## おわり

いくつかアクセスしてみてなげられてるのを確認できたらねます。
最後に[さけまつり](http://katsyoshi.doorkeeper.jp/events/10420)やるかもしれないのできてみてくだしあ。


