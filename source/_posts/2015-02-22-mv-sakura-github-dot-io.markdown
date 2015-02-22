---
layout: post
title: "blogの引越し"
date: 2015-02-22 18:29:09 +0900
comments: true
categories: tech diary github
---

自分でblogを管理するのがちょっとだけ面倒になってきたので、
このblogを[github.io](katsyoshi.github.io)に移行した。

## 手順

github.com に `katsyoshi.github.io` というリポジトリを作成し、以下のコマンドを実行

```sh
$ rake setup_github_pages
set your github repogitory
$ rake generate
$ rake deploy
```

そのあと、DNSでCNAMEに `katsyoshi.github.io` を設定し、リポジトリのトップに `blog.katsyoshi.org` と書いた `CNAME` ファイルを追加しおわりです。

## おわり

おわり

### 参考サイト

1. http://morizyun.github.io/blog/octopress-gitpage-minimum-install-guide/
1. http://blog.textfile.org/20141014/github/

