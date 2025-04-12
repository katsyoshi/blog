---
layout: post
title: "irb の補完の色がいじれるようになったのでかっこよくしてみた"
date: 2023-11-08 23:59:59 +0900
comments: true
categories: diary
---

[`Reline`](https://github.com/ruby/reline) が `0.4.0` になり、タイトルのとおり [`irb`](https://github.com/ruby/irb) で[補完画面の色を好きなように変更](https://github.com/ruby/reline/blob/master/doc/reline/face.md)できるようになりました。

変更できるようになったので [**dracula**](https://draculatheme.com/) 風のテーマを [**gem**](https://rubygems.org) としてリリースしました!!!!
リポジトリは以下です!!!

[![katsyoshi/irb-theme-dracula - GitHub](https://gh-card.dev/repos/katsyoshi/irb-theme-dracula.svg)](https://github.com/katsyoshi/irb-theme-dracula)

### インストール

ということで使いかたを。まずは `gem i irb-theme-dracula` で **インストール** します。
**ダーク** と **ライト** を作りました。 **ダーク** は明るい **コンソール** 向けに、 **ライト** は暗い **コンソール** 向けに作っています。
インストール後は `irbrc` ファイルに `require "irb/theme/dracula/dark"` か `require "irb/theme/dracula/light"` を追加。

`irb` を実行し、補完をしてみましょう。まず、`irbrc` ファイルになにもかかないデフォルトの場合は以下

![](/images/screenshot/irb-default.webp)

次に **ダーク** の `require "irb/theme/dracula/dark"` を書いた場合は以下

![](/images/screenshot/dracula-dark.webp)

最後に **ライト** `require "irb/theme/dracula/light"` の場合は以下

![](/images/screenshot/dracula-light.webp)

### おわり

**irb** も便利になってカスタマイズができるようになりました。色とか考えたくないひとは **gem** にして一発で決まるようにすると便利ですよ。たぶん。
