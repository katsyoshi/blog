---
layout: post
title: "Dockerではまってます"
date: 2014-10-01 21:11:04 +0900
comments: true
categories: tech linux gentoo docker
---

タイトルのとおりです。
Gentoo/LinuxでDockerが起動しないのでとりあえずメモ。

## インストール
dockerの[Gentooインストールページ](https://docs.docker.com/installation/gentoolinux/)を参考に[overlay](https://github.com/tianon/docker-overlay)を導入します。

```
$ sudo layman -a docker
$ sudo eix-sync
$ sudo emerge -av app-emulators/docker
```

ここでカーネルオプションの警告が出るのでひとつひとつ潰しておきます。

## 起動
インストールおわったら、起動させますが、以下のようなログが出てるので起動できません。

```
2014/10/01 18:39:17 docker daemon: 1.1.2 d84a070; execdriver: native; graphdriver:
[6c541422] +job serveapi(unix:///var/run/docker.sock)
[6c541422] +job initserver()
[6c541422.initserver()] Creating server
[6c541422] +job init_networkdriver()
[6c541422.init_networkdriver()] creating new bridge for docker0
package not installed
[6c541422] -job init_networkdriver() = ERR (1)
package not installed
[6c541422] -job initserver() = ERR (1)
2014/10/01 18:39:17 package not installed
```

このときは `bridge-utils` をインストールすればいいのかなと思ってたら、インストールしても解決できないです。


## おわり
どなたか助けてくだされ〜


## 追記

再構築したカーネルをインストールしてなかったというオチでした。

ちゃんちゃん
