---
layout: post
title: "2021.takout.rubykaigi.org"
date: 2021-09-12 00:00:00 +0900
comments: true
categories: diary rubykaigi
---

今年も **COVID-19** の影響で [RubyKaigi](https://rubykaigi.org) のオンラインイベント化された [RubyKaigi Takeout 2021](https://rubykaigi.org/2021-takeout) に行ってきた。

去年も **COVID-19** の影響で takeout をやっていたがすっかり忘れてたので参加していなかった。ので 2 年ぶりの参加である。

見たセッションは開催日毎に以下のようになった。

## day: 1

1. Yusuke Endo, "TypeProf for IDE: Enrich Dev-Experience without Annotations," https://rubykaigi.org/2021-takeout/presentations/mametter.html
1. Takeshi Kokubun, "Why Ruby's JIT was slow," https://rubykaigi.org/2021-takeout/presentations/k0kubun.html
1. Jeremy Evance, "Optimizing Partial Backtraces in Ruby 3," https://rubykaigi.org/2021-takeout/presentations/jeremyevans0.html
1. Nick Schwaderer, "Ruby Archaeology," https://rubykaigi.org/2021-takeout/presentations/schwad4hd14.html
1. Masaki Shioi, "Toycol: Define your own application protocol," https://rubykaigi.org/2021-takeout/presentations/coe401_.html
1. Masatoshi Seki, and Tatsuya Sonokawa, "dRuby in the real-world embedded systems.," https://rubykaigi.org/2021-takeout/presentations/m_seki.html
1. Uchio Kondo, "Story of Rucy - How to \"compile\" a BPF binary from Ruby," https://rubykaigi.org/2021-takeout/presentations/udzura.html
1. Ufuk Kayserilioglu, " Demystifying DSLs for better analysis and understanding," https://rubykaigi.org/2021-takeout/presentations/paracycle.html

## day: 2

1. Chris Seaton, "The Future Shape of Ruby Objects," https://rubykaigi.org/2021-takeout/presentations/chrisgseaton.html
1. Hitoshi HASUMI, "PRK Firmware: Keyboard is Essentially Ruby," https://rubykaigi.org/2021-takeout/presentations/hasumikin.html
1. Maxime Chevalier-Boisvert, "YJIT - Building a new JIT Compiler inside CRuby," https://rubykaigi.org/2021-takeout/presentations/maximecb.html
1. Shugo Maeda, "include/prepend in refinements should be prohibited," https://rubykaigi.org/2021-takeout/presentations/shugomaeda.html
1. Satoshi "moris" Tagomori, "Ractor's speed is not light-speed," https://rubykaigi.org/2021-takeout/presentations/tagomoris.html
1. CRuby Committers, "Ruby Commiters vs. the World," https://rubykaigi.org/2021-takeout/presentations/rubylangorg.html

## day: 3

1. osyo, "Use Macro all the time ~ マクロを使いまくろ ~," https://rubykaigi.org/2021-takeout/presentations/pink_bangbi.html
1. Mauro Eldritch, "Crafting exploits, tools and havoc with Ruby," https://rubykaigi.org/2021-takeout/presentations/MauroEldritch.html
1. Mike Dalessio, "Building Native Extensions. This Could Take A While...," https://rubykaigi.org/2021-takeout/presentations/flavorjones.html
1. Richard Schneeman, "Beware the Dead End!!," https://rubykaigi.org/2021-takeout/presentations/schneems.html
1. Yusuke Nakamura, "Ruby, Ractor, QUIC," https://rubykaigi.org/2021-takeout/presentations/yu_suke1994.html
1. Mat Schaffer, "10 years of Ruby-powered citizen science," https://rubykaigi.org/2021-takeout/presentations/matschaffer.html
1. Yukihiro "Matz" Matsumoto, "Matz Keynote," https://rubykaigi.org/2021-takeout/presentations/yukihiro_matz.html

## セッションへの感想

印象に残ったセッションとしては、 2 日目の PRK, 3 日目 のマクロ, 3 日目の deadend, あたりが強烈に残っています。

PRK に関しては、 *[Promiro](https://www.switch-science.com/catalog/3914/)* 互換 *[RP2040](https://www.sparkfun.com/rp2040#boards)* で Ruby を使って firmware(keymap) を書けるところがとてもイイ!

マクロに関しては *[AST](https://docs.ruby-lang.org/ja/latest/class/RubyVM=3a=3aAbstractSyntaxTree.html)* を利用したマクロでこれなら自分でも使えそうだしだなあ。(最近さわってる Rust にも macro あるけど、書くのは一見むずかしそう)。とりあえず後で触ってみるかという気分にさせてくれる発表内容でした。以下の2つのgemから利用できるようです。

- kenma: https://rubygems.org/gems/kenma
- rensei: https://rubygems.org/gems/rensei

最後に *[dead_end](https://rubygems.org/gems/dead_end)* は `ruby -w` でも同じようなことができそうだけど、 *[syntax error](https://docs.ruby-lang.org/ja/latest/class/SyntaxError.html)* を早めに分かるための Gem でした。印象としてはメッチャ便利!と思ってたら[本体に入れる提案](https://bugs.ruby-lang.org/issues/18159)が走ってるようです。

## 感想戦の感想
2 日目、 3 日目の最終セッション後に zoom などで笹田さんを中心に感想戦が行なわれていました。
2 日目は[最近話題になっていた議論](https://bugs.ruby-lang.org/issues/12075)を進めていたようです(この日はチョット疲れたので17:00頃に上ったけど、19:00頃までやっていたようです)。
3 日目は Matz の振り返りを中心に感想戦が行なわれて印象に残った発表を聞いたりしてました。他にも[昔から要望してた機能](https://bugs.ruby-lang.org/issues/14579)の取り込みが行なわれてました。

## Takeout 全体の感想

全体通しての感想としては、配信は開始直後にちょっとしたトラブルがあった以外は非常に快適でした。
あとこの配信サービス自体も[自前で開発](https://github.com/ruby-no-kai/takeout-app)されたようで大変感謝しています。

折角なので他の人と感想話したいなーとおもったのでなんどか twitter spaces を開いてみたけど(当然)誰も参加してくれなかったのがちょっとさみしかったけど、mitaka-rb のみなさんが [spatialchat](https://spatial.chat/s/spatial-mitaka)  を立ててくれてたので雑談を毎回2時間ほどしてました。

## matome

久しぶりの RubyKaigi 参加で大変たのしく、興奮した3日間でした!!!

