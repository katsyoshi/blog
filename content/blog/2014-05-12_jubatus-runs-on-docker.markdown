+++
layout: post
title: JubatusをDocker上で動かしてみた
date: 2014-05-12 23:50:33 +0900
comments: true
categories: jubatus docker
+++

最近話題の[Docker](https://www.docker.io/)使ってJubatusを動かしてみたのでメモ的なものを

## 環境
環境はVagrantでUbuntu14.04を動かしその上でDockerでJubatusVMを動作

- Vagrant: 1.5.4
- docker: 0.11.1

## Dockerfile

[リポジトリ](https://github.com/katsyoshi/docker-jubatus)を作成しておきましたのでcloneして実行します。

```
$ git clone https://github.com/katsyoshi/docker-jubatus
$ cd docker-jubatus
$ docker build .
$ docker run -p 9199:9199 最後に出てきたハッシュ値
```

で動きはじめます。
