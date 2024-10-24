+++
layout: post
title: "good-bye elasticsearch and jenkins"
date: 2014-08-10 23:22:55 +0900
comments: true
categories: tech ubuntu fluentd elasticsearch jenkins
+++

この環境では慢性的にメモリが足りなかったのでメモリを大量に使ってる[Elasticsearch](http://www.elasticsearch.org/)を削除しました。
Jenkinsについては利用していなかったので停止してます。ついでに12.04から14.04にupgradeしました。

## 削除とUpgrade
削除する前に[Mackerel](https://mackerel.io)を導入したので、fluentd + Elasticsearch + Kibanaな構成にする必要がなくなりました。
Elasticsearchの削除とJenkinsの停止は以下のコマンドで行ないます。

```
$ sudo apt-get purge elasticsearch
$ sudo update-rc.d jenkins disable
```

つぎに14.04へupgradeを行ないます。

```
$ sudo do-release-upgrade -d
```

これで14.04になってます。

## 環境
- Machine: さくらVPS
- CPU: Intel Xeon E312xx (SandyBridge) * 2
- MEM: 1GB
- HDD: 100GB
- OS: Ubuntu Linux 12.04

## インストール済ソフトウェア（一部）
- Elasticsearch
- fluentd
- Jenkins
- Jubatus
- Norikra
- Nginx
