---
layout: post
title: "wireguardをはじめました"
date: 2018-08-17 00:38:12 +0900
comments: true
categories: tech vpn
---

title 通り、 [`wiregaurd`](https://www.wireguard.com/) で家と [さくらのVPS](https://vps.sakura.ad.jp/) にあるサーバーを繋いでみました

## install

導入は簡単で `gentoo` は公式にあるので

```
sudo emerge wireguard
```

だけで、 `ubuntu` の場合も[インストールガイド](https://www.wireguard.com/install/#packages)があるため簡単にインストールできます


```
sudo apt install software-properties-common
sudo add-apt-repository ppa:wireguard/wireguard
sudo apt-get update
sudo apt-get install wireguard
```

## 引込

あとは[非常にかんたん](https://speakerdeck.com/fadis/zuo-tuteli-jie-suruwireguard)で[ここ](https://wiki.archlinux.jp/index.php/WireGuard)を適宜読み替えることで
接続できます。

## おわり

あとは、再起動時に自動で接続するように変更する必要がありそうですがつながったし、おそいのでこれでおわり
