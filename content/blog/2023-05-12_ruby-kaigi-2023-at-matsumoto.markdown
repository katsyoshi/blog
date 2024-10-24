+++
layout: post
title: "RubyKaigi2023 @MATZ本"
date: 2023-05-15 23:59:59 +0900
comments: true
categories: diary
+++

行ってきたのでまとめます。LT やったのですが、 LT の話はまた今度。

## Day -1

前日は恒例の [Asakusa.rb](https://asakusarb.esa.io) の [predrinkup](https://asakusarb.doorkeeper.jp/events/154786) に行ってました。ここでは、 [nobu](https://twitter.com/n0kada) とかとはなししたりしてすごしました。
drinkup は次の日のため終了後すぐさま帰宅し、就寝した。

## Day 0
この日は、 [RubyistsOnRails](https://cookpad.connpass.com/event/277569/) というすばらしいイベント列車があったのでノータイム [^sonnnakotohanai] 8時の会に参加申し込み。6時に起床し、7時に出発。
会ではRubyistと話しながら、山見ながら、松本に到着。
松本に到着後飯として [みよ田](https://tabelog.com/nagano/A2002/A200201/20000092/) という蕎麦屋に直行。
蕎麦を食べた直後、松本城へ。松本城では [まつもとさん](https://twitter.com/yukihiro_matz/) 集団を見つけたので、みんなで Matz本城 背景に記念撮影。まつもとさん一行は開発者会議があったらしく記念撮影後すぐにわかれました。
われわれは、この記念撮影後松本城観光し、もう一軒蕎麦屋へ昼食へ。二軒目の蕎麦屋は [蕎麦倶楽部佐々木](https://tabelog.com/nagano/A2002/A200201/20002376/) へ。

そんなことしてたら宿がチェックイン可能になったので、荷物置くのと下着類を購入しひとやすみ(10min程)。

Day 0 最大のイベント、 [KeebKaigi](https://keebkaigi.org/2023/) へ!ここで豪華ノベリティを大量に頂きました!!!1とくによいのが城キーキャップで松本城を3DプリントしたISO エンターキーを頂きました。しかしながらこのキー差すキーボードがないので知人に譲る予定です。

KeebKaigi 自体もたいへん面白い Kaigi で楽しめました。とくに、 t-code の紹介のLT、頭文字Vなどよかったです。
KeebKaigi 後は懇親会には参加せず、友人とかるく夕飯をして宿へ。

## Day 1

ここまででも大変楽しいものでしたが、[RubyKaigi本番](https://rubykaigi.org/2023/) はこの日から。

1日目の朝食は会場行く途中でもともと聞いていた店の [栞日](https://sioribi.jp/) さんへ。
会場開いて登録したあとノベリティの列をみてげんなりしてたので一旦スポンサーブースをうろうろしてた。
この日聞いたセッションは以下です。

### [Matz Keynote](https://rubykaigi.org/2023/presentations/yukihiro_matz.html#day1)

毎年恒例 Matz のキーノート。

### 昼飯
ランチはたまたま居合わせた6人で [おきな堂](https://tabelog.com/nagano/A2002/A200201/20000098/) へ。名物のボルガライスとリンゴジュースを。シードルがあったので一緒にいったひとへオススメしてたりした。

### [The future vision of Ruby Parser](https://rubykaigi.org/2023/presentations/spikeolaf.html#day1)

コミッターの [spikelaf](https://twitter.com/spikeolaf) さんによるパーサーの開発話。魔窟じゃないよーなんかいってまいましたが、難しい問題を丁寧にひとつひとつ倒していった話をしていました。

### [Make Regexp#match much faster](https://rubykaigi.org/2023/presentations/makenowjust.html#day1)

去年 [ReDoS](https://www.ruby-lang.org/ja/news/2023/03/30/redos-in-time-cve-2023-28756/) の対応をしたコミッターの発表。正規表現の可能性を話してくれています。

### [High-performance real-time 3D graphics with Vulkan](https://rubykaigi.org/2023/presentations/fredolinhares.html#day1)

[Valkan](https://www.vulkan.org/) を Ruby で利用できるようにした話。このあたりは LT への緊張感でほとんど覚えていないです。

### [Power up your REPL life with types](https://rubykaigi.org/2023/presentations/tompng.html#day1)

去年の [TRICK Winner](https://github.com/tric/trick2022/blob/master/01-tompng/entry.rb) で今回の話は `irb` を利用しているときにどういう感じで補完が出るのか、出すためにどのような型推論を行なっているのかの話だったとおもう。

### LT

しゃべった

### オフィシャルパーティー

つかれたのでそこそこで帰った

## Day 2

朝飯は [珈琲茶房かめのや](https://tabelog.com/nagano/A2002/A200201/20020720/) へ。ここもワンオペで配膳されるまで少し時間がかかった。というか、朝食べくるひといないのかどこもワンオペだったので Day 3 も早めに出るようにしようとなった。

### [On Ruby and ꝩduЯ, or How Scary are Trojan Source Attacks](https://rubykaigi.org/2023/presentations/duerst.html#day2)

昔他の言語で話題になった Ruby のコード的には正しいが、見た目ではわからない
不正な文字が入ったときの対策のはなしでした。

### [Build a mini Ruby debugger in under 300 lines](https://rubykaigi.org/2023/presentations/_st0012.html#day2)

300 行といったなあれは嘘だ。という話。Ruby の Debugger を200 行くらいでつくったはなしでした。

### 昼飯

[種村](https://tabelog.com/nagano/A2002/A200201/20015852/) で蕎麦と岩魚と日本酒。

### [Revisiting TypeProf - IDE support as a primary feature](https://rubykaigi.org/2023/presentations/mametter.html#day2)

Ruby に梱包されている [`TypeProf`](https://github.com/ruby/typeprof/blob/master/doc/doc.ja.md) が遅いので速くして、また書いている途中でもエラーがでたときどうするかとかの説明をしていました。まだ利用できていないので試したくなるものだった。

### [Ruby Implementation of QUIC: Progress and Challenges](https://rubykaigi.org/2023/presentations/yu_suke1994.html#day2)

[Unasuke](https://twitter.com/yu_suke1994) 大先生のライフワーク、 Ruby の [QUIC](https://datatracker.ietf.org/doc/html/rfc9000) 対応のはなし。Python での実装はどうとかのはなしで今回は実装がすすんでいるようでよかった。とはいえ道程はまだまだかかりそう。

### afternoon break

お昼休憩後は Jeremy Evans や Maxime の話を聞いていたが疲れてなにも覚えていない。

### [Leaner Drinkup at RubyKaigi 2023](https://leanertechnologies.connpass.com/event/280191/)

この日は [Leaner](https://leaner.jp/) さん主催の drinkup へ行きました。ここでは主に日本を飲んですごしました。でここで、spikelaf が kaigi でも話してた [lrama](https://github.com/ruby/lrama) が Ruby 本体のとりこまれ、 [Ruby 3.3.0-preview1 がリリースされました](https://www.ruby-lang.org/ja/news/2023/05/12/ruby-3-3-0-preview1-released/)!なんかさわいでるなと思たらリリース作業シテイタトハ…
drinkup 終了後は [onk](https://twitter.com/onk) とふたりで近所のワインバーへ行き、軽くのんで宿へ帰りました。

宿帰ったら [ESM](https://esm.co.jp/) さんのノベリティの [Rubyメソッドかるた](https://blog.agile.esm.co.jp/entry/ruby-method-karuta) でみんなでわいわいしながら遊んでました。

## Day 3

この日の朝食は [山山食堂](https://tabelog.com/nagano/A2002/A200201/20023046) というところでしました。

### Committers and the Worldのあと

今回は Matz本 ということで会場にいる「まつもとさん」一同会して写真を撮ることを目的としてあつめてました。写真は以下になります

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">松本さん大集合に、当社の松本さんも混ぜていただきありがとうございましたっ🏯<a href="https://twitter.com/hashtag/RubyKaigi2023?src=hash&amp;ref_src=twsrc%5Etfw">#RubyKaigi2023</a> <a href="https://t.co/neYYer5e2A">pic.twitter.com/neYYer5e2A</a></p>&mdash; Yasunori Suzuki (@yasuzukisan) <a href="https://twitter.com/yasuzukisan/status/1657253330056519680?ref_src=twsrc%5Etfw">May 13, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 

### [Ruby + ADBC - A single API between Ruby and DBs](https://rubykaigi.org/2023/presentations/ktou.html#day3)

[須藤さん](https://github.com/kou) の発表で、 [`ADBC`](https://arrow.apache.org/docs/format/ADBC.html) で DB へ接続し、さらのその接続からのよみかきの高速化のはなしでした。あいかわらずすごいなと思いながら聞いていました。

### [Ruby JIT Hacking Guide](https://rubykaigi.org/2023/presentations/k0kubun.html#day3)

[国分さん](https://github.com/k0kubun) の新作 [RubyJIT](https://github.com/ruby/ruby/pull/7448)。PRみた感じだとやってるネタにてるなあとおもってたけど、アプローチがすこし違い、またこちらは直接ELFを書いているようなのでまだちがうようだ。とはいえ今やっていることは似ているのであとで参考にしよう。

### After Party and Music Mixin

[STORESさん](https://stores.jp/) の [アフターパティー](https://hey.connpass.com/event/277763/) と [pixivさん](https://www.pixiv.net) の [ruby music mixin](https://conference.pixiv.co.jp/2023/rubymusicmixin) にいってきました。両方ともサイコーでした!!!!

## Day 4

一人で野沢温泉村いって温泉はいって、ジンのんで、ビールのみいこうと一旦山おりたら温泉にしか飲むとこなかったのでもどったりして帰りました。

## おわり

今回のRubyKaigi久しぶりのFull開催で大変楽しめたのですが、LTとは言えはじめての発表があり、心から楽しめたかというとあれですが、すごく楽しかったし、刺激になりました。来年の那覇は本編に出せるようになんとか精進します!!!

+++

[^sonnnakotohanai]: 海外の人が登録して行ったほうがいいんじゃないかと一瞬思った
