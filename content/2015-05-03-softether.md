+++
path = "/blog/2015/05/03/softether"
layout = "post"
title = "softether"
date = 2015-05-03
comments = true
categories = "tech vpn network"
+++

Ubuntuに[SoftEther](https://www.softether.jp/)を導入してみた。

## マシン構成

```
OS: Ubuntu Linux 14.4.0.2 LTS
Machine: SakuraVPS 1GBプラン
```

## インストール

[ここ](http://qiita.com/Makotonton/items/18683a9f1e846433c035)を参考にインストール、設定すればいけます。

### 嵌ったところ

`nginx` で `ssl` 動かしてた。

設定ができなかったが `sudo service nginx stop` として、 `nginx` を止めたあと、 `sudo vpncmd` とすれば設定できます。

## おわり

自作のRailsアプリCIを自宅で動かそうと思ってVPN導入してみたが、一人で開発してるしCIに問題がない限り必要ないというのを構築したあと気が付いた。


### 参考

1. http://qiita.com/Makotonton/items/18683a9f1e846433c035
1. https://www.softether.jp/

