---
layout: post
title: "RubyKaigi 2025 at Matz-yama"
date: 2025-04-25 23:00:00 +0900
comments: true
categories: diary tech rubykaigi
---

今年も [RubyKaigi 2025](https://rubykaigi.org/2025) に行ってきました。

今年みたセッション一覧を日毎に。

## Day 1

1. [Ruby Taught Me About Encoding Under the Hood]()
1. [Bringing Linux pidfd to Ruby](https://mensfeld.github.io/bringing_linux_pidfd_to_ruby/)
1. [The Evolution of the CRuby Build System](https://speakerdeck.com/kateinoigakukun/the-evolution-of-the-cruby-build-system)
1. [Deoptimization: How YJIT Speeds Up Ruby by Slowing Down](https://speakerdeck.com/k0kubun/rubykaigi-2025)
1. [Ruby's Line Breaks](https://speakerdeck.com/yui_knk/rubys-line-breaks)
1. [State of Namespace](https://speakerdeck.com/tagomoris/state-of-namespace)
1. [TRICK 2025: Episode I](https://github.com/tric/trick2025)

初日のキーノートは、 [@ima1zumi](https://github.com/ima1zumi) さんで文字コードの話してくれました。
つぎは [@mensfeld](https://github.com/mensfeld) の `pidfd` の話を聞きました。
これは server 動かしているとはまる問題、 `pid` の使い回しによるバグの対処方法として `pidfd` を `Ruby` に対応したという話でした。
オチは、そもそも **overkill** で不要だったねという。
午前の終りは [@kateinoigakukun](https://github.com/kateinoigakukun) さんのビルドシステムの話でした。 _Ruby_ のビルドシステムのリプレースを考えているようでした。
午後一発目はお馴染 **Shopify** の _YJIT_ チームの [@k0kubun](https://github.com/k0kubun) さんの話。 _YJIT_ を速くするために、如何に _deoptimization_ するかという話で、速くするための方法を話していました。
**パーサーギャング** 一味のドン、[@yi-knk](https://github.com/yui-knk) さんの話で今年楽しみにしてた発表。
_Namespace_ の話は [@tagomoris](https://github.com/tagomoris) さんがやってくれました。
今年のうちに入りそうな雰囲気を感じた。
いったん休憩して **trick** へ。

## Day 2

1. [Performance Bugs and Low-level Ruby Observability APIs](https://docs.google.com/presentation/d/1lDnxFkc4URsi0LP4w1M5IXv5A02AG_HUy3gW_SpEWOA/edit#slide=id.g34edb417dc3_0_1471)
1. [Dissecting and Reconstructing Ruby Syntactic Structures](https://speakerdeck.com/ydah/dissecting-and-reconstructing-ruby-syntactic-structures)
1. [ZJIT: Building a Next Generation Ruby JIT]()
1. Bazel for Ruby
1. [The Implementations of Advanced LR Parser Algorithm](https://speakerdeck.com/junk0612/the-implementations-of-advanced-lr-parser-algorithm)
1. Lightning Talks

[_dd-trace-rb_](https://github.com/DataDog/dd-trace-rb) の作者 [@ivoanjo](https://github.com/ivoanjo) の _observability_ の話。
もう同じ目的をもった [@osyoyu]() が[_スマートバンクブログ_](https://blog.smartbank.co.jp/entry/2025/04/25/110000)で解説しているのでそちらを参照。
新米 [**Ruby Commiter**](https://github.com/orgs/ruby/people) の [@ydah](https://github.com/ydah) です。
[_lrama_](https://github.com/ruby/lrama) で _Ruby_ の `parse.y` をどう読み込むのか、どう構造化するのかの話でした。
**Shopify** _YJIT_ チームの親分、 [maximecb](https://github.com/maximecb) の発表。
新しい _JIT_ 、今年一番の衝撃 _ZJIT_ の提案。詳細は [Proposal to upstream ZJIT](https://bugs.ruby-lang.org/issues/21221) を見てくれ。
**パーサーギャング** 一味のトリ [@junk0612](https://github.com/junk0612) の発表。
_lrama_ に _IELR_ を適用してみる話。まあ遅かったりするようです。
最後に

## Day 3

1. Ruby Committers and the World
1. [Improving my own Ruby](https://speakerdeck.com/sisshiki1969/improve-my-own-ruby)
1. [The Ruby One-Binary Tool, Enhanced with Kompo](https://speakerdeck.com/ahogappa/the-ruby-one-binary-tool-enhanced-with-kompo)
1. [Toward Ractor local GC](https://atdot.net/~ko1/activities/2025_rubykaigi2025.pdf)
1. [Modular Garbage Collectors in Ruby]()
1. [Matz Keynote]()

[@sisshiki1969](https://github.com/sisshiki1969) さんで、_rust_ の _ruby_ 処理系を作ってる人で、速くした話でした。
[@ahogappa](https://github.com/ahogappa) さんが作ってる _kompo_ というツールの話で、この _kompo_ はバイナリー1つにまとめてくれる便利ツールです。
今回は _Rails_ アプリが動くようになった話をしていました。
[@peterzhu2118](https://github.com/peterzhu2118) さんが _GC_ (_Garbage Collection_) をモジュール化して入れ替えれるような話をしてました。
さいごに [@matz](https://github.com/matz) のキーノートで _Ruby_ のこれからの話、_AI_ の話などなど。

## おわり

おそくなったけど、一旦聞いたセッションをまとめてみました。来年は函館で東京から電車一本で行けるので楽そうですね。
おわり。
