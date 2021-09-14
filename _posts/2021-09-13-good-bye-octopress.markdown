---
layout: post
title: "Octopress 脱出"
date: 2021-09-13 00:00:00 +0900
comments: true
categories: diary
---

長年利用してた [Octopress](https://github.com/octopress/octopress) がずいぶん前にサポート外になってたので [Jekyll](https://github.com/jekyll/jekyll) へ変更することにした。

## 問題点

サポート外になったことによる問題点があり、その問題によって変更することになった。
問題点としては、 *[Pygments.rb](https://rubygems.org/gems/pygments.rb)* と *[Compass](https://rubygems.org/gems/compass)* と利用している *[Ruby](https://ruby-lang.org/)* のバージョンが 2.3.7 というのが主で
