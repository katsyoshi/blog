---
layout: post
title: "Railsで空のテストファイルを削除"
date: 2014-11-21 16:07:38 +0900
comments: true
categories: git tech rails rspec tips memo
---

Ruby on RailsのプロジェクトでGit管理されているTestで空のテストファイルを
削除するには以下のコマンドを利用することで削除できます。

```
$ git grep -e it --or -e specify -L -- spec/{model,controller}s | xgrep git rm -f --
```

### 参考

- [man git grep](https://www.kernel.org/pub/software/scm/git/docs/git-grep.html)
- [Git grepを便利に使う-eオプションについて](http://qiita.com/tbaba/items/a67c8d79c6c4d0dc9b73)
- [複数人での Git 開発に便利な 3 つのコマンド](http://qiita.com/rosylilly/items/9648ad2c8aa53465372b)
