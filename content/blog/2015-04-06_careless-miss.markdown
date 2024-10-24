+++
layout: post
title: "不注意なこと"
date: 2015-04-06 22:25:28 +0900
comments: true
categories: tech ubuntu sudo
+++

Ubuntuで `lxc` を利用して、サーバを立ててたときに嵌ったこと

## はじまり

Proxyを利用してインターネットに接続している。
このネットワークとして `ping` はLAN内なら届くが、外(google.com)とかには出ない。
設定がうまくいってるかどうかの確認する方法として、 `apt-get update` を利用して確認している。

最初は、 `lxc` の設定がうまくいっていないと思いなんども見直してたりした。
それでもWANの方に出ることができなかった。

サーバのネットワーク設定がよくないと思ってなんども見直したけど、問題なさそう。
最後に `sudo -i` で root にログインしたあと、 `apt-get update` したら、
ダウンロードはじまり、次へ進むことができた。

## 原因

原因としては `sudo command` で実行しても `Defaults reset_env` で環境変数がリセットされてたようだった。
ので、`Defaults keep_env+="PATH"` とか追加して解決した。

## おわり
簡単なミスで3時間ほど時間を費したのでなんとかしたい。

### 参考URL
1. http://www.maepachi.com/blog/entry?id=128
1. http://office.tsukuba-bunko.org/ppoi/entry/systemwide-rbenv
