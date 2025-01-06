+++
path = "/blog/2014/06/10/pitfall-in-boot2docker"
layout = "post"
title = "docker on OS Xで嵌ったおはなし"
date = 2014-06-10T19:52:53+09:00
comments = true
categories = "tech docker"
+++

MacOS XでDockerを動かしてたら嵌ったのでメモ

## Docker on OS X
OS XでもDockerが[動かせるよう](http://docs.docker.com/installation/mac/)になりました。
インストールは[ここ](http://dev.classmethod.jp/tool/docker/getting-started-docker-on-osx/)とかを参考にしてください。

## で嵌ったところ
Docker上で動いているアプリケーションとどうしても通信ができない。どうやらDockerとアプリケーションは起動はしてるようなんだが、
どうしても通信できないということがわかりました。

```
$ docker run -p 9199:9199 hogehoge
$ netstat -a | grep 9199
```

でも見えない。なんでかなあと思ってたら[こんな](https://github.com/dotcloud/docker/issues/4007)ことや[こんな](http://k-shogo.github.io/article/2014/02/13/boot2docker-portforward/)ことがわかりました。

## 解決策
VirtualBoxからboot2docker-vmのポートフォワードを設定すればいいです。

```
$ netstat -a | grep 9199
tcp4       0      0  localhost.9199         *.*                    LISTEN
```

ちゃんちゃん。
