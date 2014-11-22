---
layout: post
title: "td-agent.pkg"
date: 2014-05-21 08:53:46 +0900
comments: true
categories: fluentd tech mac
---

td-agent2が[リリース](https://groups.google.com/forum/?fromgroups#!topic/fluentd/ZjxODonIJJo)され、Macへの公式パッケージがでたのでインストールメモ

## Install

[ダウンロード](https://s3.amazonaws.com/td-agent-repository/macosx/td-agent2-1.0.0-0.dmg)してダブルクリックでインストールできます。

## 設定
設定ファイル置場は`/etc/td-agent/td-agent.conf`になります。
設定ファイルはいままでの設定で使えるようです。
あとは[ML](https://groups.google.com/forum/?fromgroups#!topic/fluentd/ZjxODonIJJo)にあるとおりにコマンドを実行すればOKです。

```sh
$ sudo launchctl load /Library/LaunchDaemon/td-agent.plist
```

