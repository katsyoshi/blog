+++
path = "/blog/2017/04/08/use-dnsmasq-for-domestic-network"
layout = "post"
title = "raspiでdnsを運用してみはじめた"
date = 2017-04-09
comments = true
categories = "tech dnsmasq diary"
+++

RasPi2 が3台あってつかっていないので DNS として動かすようにしてみた。
RasPi とはいえ中身は Ubuntu Linux なのでのインストールは `apt install dnsmasq` するだけでおわります。

## はまったところ

いつもどおりはまりました。今回は以下の二点

- /etc/dnsmasq.d/ に hosts ファイルを置くとエラー
- /etc/hosts ファイルのパーミッションが `600` になっていたためエラー


### /etc/dnsmasq.d/ に hosts ファイルを置くとエラー
ログを見ても以下のログしか出ておらず理由の調査に時間がかかった。hosts ファイルは設定ファイルではないため当然といえば当然なのですが…

```
Jan  1 00:00:08 localhost dnsmasq[673]: bad option at line 1 of /etc/dnsmasq.d/hosts
```

今、`/etc/defaults/dnsmasq` を調べてみるとそうなってました。はい。

```
CONFIG_DIR=/etc/dnsmasq.d,.dpkg-dist,.dpkg-old,.dpkg-new
```

### /etc/hosts ファイルのパーミッションが `600` になっていたためエラー
こちらは上記問題を解決したあと、反映されないのでログを見たときにわかりました。

```
Apr  8 08:57:20 localhost dnsmasq[4733]: failed to load names from /etc/hosts: Permission denied
```

これの原因は `itamae` でファイルを送信するときに `mode '644'` を指定する必要があったのだが、指定せずに
送信してしまったため発生。

## おわり
おわり

### 関連リポジトリ
https://github.com/katsyoshi/itamae-recipes
