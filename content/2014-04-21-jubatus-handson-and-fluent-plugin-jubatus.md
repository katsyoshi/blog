+++
path = "/blog/2014/04/21/jubatus-handson-and-fluent-plugin-jubatus"
layout = "post"
title = "jubatus handsonにいってきたのでfluent-plugin-jubatusについてちょっとだけ"
date = 2014-04-21T23:45:43+09:00
comments = true
categories = "tech jubatus fluentd fluent-plugin"
+++

## [Jubatus Handson](http://connpass.com/event/5641/)行ってきました

行ってきました。

## fluent-plugin-jubatus 0.0.2をリリースしました

先日[大江戸Ruby会議04](http://regional.rubykaigi.org/oedo04)に行ってみたらやる気が湧いてきたので半年ほど寝かせておいた[fluent-plugin-jubatus](http://rubygems.org/gems/fluent-plugin-jubatus)をアップデートしました。

大きな変更点としては、異常検知（jubaanomaly）をサポートするようになってます。

### プラグインの今後

今回も入れなかったのですが、このプラグインでは未だ判定のみのサポートです。
理由としては

- 計算量
- メモリ

のふたつがあげられます。計算量に関しては以前手元のMacで異常検出を動作させていたときマシンの動作がもっさりになるほどの計算量があったためです。メモリに関しては今のところ不安には感じてはいないですが、データ量(数)が巨大になったときどこまで必要なのかってのがよくわからない。という理由で積極的に導入していないです。

これらを解消する方法としては忘却([unlearning](https://github.com/jubatus/jubatus/tree/feature/unlearning))が考えられるのですが、じゃぁどの必要のないデータを忘却させるのって問題があります。

とはいえ、そろそろ導入したいので[pull req](https://github.com/katsyoshi/fluent-plugin-jubatus)まってます。



