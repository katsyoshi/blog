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

```
$ rake setup_github_pages
set your github repogitory
$ rake generate
$ rake deploy
```

そのあと、DNSでCNAMEに `katsyoshi.github.io` を設定し、リポジトリ<del>のトップ</del>に `blog.katsyoshi.org` と書いた `source/CNAME` ファイルを追加しおわりです。

## おわり

おわり

## 追記
実は記事を追加したあと `rake deploy` ができないという問題がありましたが、そのときは、

```
$ rake generate
$ cd _deploy
$ git pull origin/master
$ cd ../
$ rake deploy
```

で更新されます。


### 参考サイト

1. http://morizyun.github.io/blog/octopress-gitpage-minimum-install-guide/
1. http://blog.textfile.org/20141014/github/
1. http://stackoverflow.com/questions/17609453/rake-gen-deploy-rejected-in-octopress
1. http://learnaholic.me/2012/10/10/deploying-octopress-to-github-pages-and-setting-custom-subdomain-cname/
