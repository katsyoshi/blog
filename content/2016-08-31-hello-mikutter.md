+++
path = "/blog/2016/08/31/hello-mikutter"
layout = "post"
title = "hello mikutter"
date = 2016-08-31T22:55:03+09:00
comments = true
categories = "mikutter ruby gems"
+++

表題のとおりmikutterをgemでインストールできるようにしてみました。

## どうやって？
[rubygems](https://rubygems.org) からはまだインストールはできません。
ので、以下の様にしてgem packageを作成します。

```bash
$ git clone github.com/katsyoshi/mikutter.git
$ cd mikutter
$ git checkout reokure-ru
$ bundle install
$ bundle exec rake build
$ gem install pkg/mikutter-3.5.0.pre.dev.gem
$ mikutter
```

これでmikutterコマンドで起動できるようになっています
これすらめんどうな人は[ここ](https://katsyoshi.org/mikutter-3.5.0.pre.dev.gem)にあります。
ダウンロードして `gem install mikutter-3.5.0.pre.dev.gem` でインストールできます。
プラグインで起動できないとかあるなら必要なgemをインストールしてください。
