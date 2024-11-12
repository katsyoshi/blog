+++
path = "/blog/2017/02/12/octopress-themes"
layout = "post"
title = "Blogのテーマかえた"
date = 2017-02-12
comments = true
categories = "diary"
+++

表題のとおり、Blog(octopress) のテーマをdefaultの classics から [slash](https://github.com/tommy351/Octopress-Theme-Slash) に変更した。
変更の方法は以下のようになります[^theme]。

```
$ cd ~/octopress
$ git clone git://github.com/tommy351/Octopress-Theme-Slash.git .themes/slash
$ bundle exec rake install\['slash'\]
$ bundle exec rake generate
$ bundle exec rake preview
$ bundle exec rake deploy
```

ついでにfaviconもかえよう。

[^theme]: Octopressのテーマを変更してみよう!, facestarbaby, http://qiita.com/fakestarbaby/items/ab532088453105e1bea4, 2017/02/12 閲覧
