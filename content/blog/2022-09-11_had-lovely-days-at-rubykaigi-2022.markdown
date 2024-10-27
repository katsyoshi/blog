+++

title = "RubyKaigi 最高！"
date = 2022-09-11
comments = true
categories = "tech diary ruby rubykaigi"
+++

[RubyKaigi 2022](https://rubykaigi.org/2022) に行ってきました! ので感想を

## DAY 0

今回も [やんちゃハウス](https://yancya.house/) に楽をして泊まることにしてたのでなにも気にせず宿へ。
着いて他メンバーが来るまで宿で待機してたが、最寄り駅の周りが思った以上になにもなく、途方に暮れてました。

#### 夕飯

もうひとり宿に着いたので入れ変わりで夕飯へ出かけたが、そもそも最寄り駅前にはなにもないので伊勢駅まで出ることにした。

この判断が間違えてたようで、伊勢駅に出てもとくに変らなかった。しかし幸い伊勢駅前に担々麺屋があったのでそこで食料摂取した。

## DAY 1

この日の聞いたセトリと感想は以下のとおり

#### [Ruby meets WebAssembly](https://rubykaigi.org/2022/presentations/kateinoigakukun.html): [presentation]()

この日のキーノートは [`Ruby`](https://www.ruby-lang.org/) の [`wasm`](https://webassembly.org/) 対応の話で [`wasi`](https://wasi.dev/) を通じてブラウザで `Ruby` が動かすまでのたのしいことをしゃべってたようです。
おなかいたくてトイレ行ってたら面白いところ聞き逃がしてしまった。

#### ランチ
適当に幕の内弁当を最初に選択。

#### [Making \*MaNy\* threads on Ruby](https://rubykaigi.org/2022/presentations/ko1.html): [presentation]()

笹田さんの発表で如何に `Ruby` で CPU をフルに使えるようにするかの話。 `Ruby` での `Thread` のはなしから [`Ractor`](https://github.com/ruby/ruby/blob/master/doc/ractor.md) を利用してフルフルに CPU 利用するための書きかたとかの話していました。

#### [Building a Lightweight IR and Backend for YJIT](https://rubykaigi.org/2022/presentations/maximecb.html): [presentation]()

`Ruby 3.1` で [`YJIT`](https://github.com/Shopify/yjit) が入って `Ruby 3.2` でサポートされる CPU アーキテクチャが増えたはなし。RubyVM(？) に `YJIT` 用 `Ruby IR` を作成して `JIT` を実行するという話。
聞いてて疑問になった点としては、 `Ruby IR` では直接機械語に翻訳しているようだったが、このバックエンドに [`LLVM`](https://llvm.org) を利用していないのはなんでなんだろう。聞き逃がしているという話はあるので、詳しいひと教えて。

#### [Tools for Providing rich user experience in debugger](https://rubykaigi.org/2022/presentations/ono-max.html): [presentation]()

[`debug.gem`](https://github.com/ruby/debug) を [`VS Code`](https://code.visualstudio.com/) だけじゃなく、 [`Chrome`](https://www.google.com/intl/ja_jp/chrome/) のリモートデバッグ機能を利用したデバッグの話でした。
この機能自体はすごく便利だと思うのですが、そもそも両方とも宗教上の理由で利用できない体なので聞くだけでした。

#### [TRICK 2022 (Returns)](https://rubykaigi.org/2022/presentations/tric.html): [repos]()

いつも通り ~~変な~~ エキセントリックなプログラムしか出てこなかったし、いつもの面子が賞もらってたのですごいなーという感想。

#### 夕飯

この日は津駅があまりなさそうだったので、駅近くで飲むのではなく、宿寄りの場所松坂駅で飲むことにした。
ここで、津の夜情報を聞き込んだり、朝ごはん情報自体を仕入れてました。

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">昨日の夕飯 <a href="https://twitter.com/hashtag/rubykaigi?src=hash&amp;ref_src=twsrc%5Etfw">#rubykaigi</a> <a href="https://t.co/G8lmetYX9J">pic.twitter.com/G8lmetYX9J</a></p>&mdash; katsyoshi (@katsyoshi) <a href="https://twitter.com/katsyoshi/status/1568012256566480896?ref_src=twsrc%5Etfw">September 8, 2022</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## DAY 2
2日目は、ネットワークが不安定になったりしてこちらの集中力が切れたりしたのであまり覚えていない。

#### 朝ごはん

前日の夕飯に教えてもらった市場の人向け食堂が松坂駅前にあり、その食堂で食事をした。

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">朝メッシ <a href="https://twitter.com/hashtag/rubykaigi?src=hash&amp;ref_src=twsrc%5Etfw">#rubykaigi</a> <a href="https://t.co/PvbaUrRUIf">pic.twitter.com/PvbaUrRUIf</a></p>&mdash; katsyoshi (@katsyoshi) <a href="https://twitter.com/katsyoshi/status/1568019085430259712?ref_src=twsrc%5Etfw">September 8, 2022</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

#### [Matz Keynote](https://rubykaigi.org/2022/presentations/yukihiro_matz.html): [presentation]()

#### [Do Pure Ruby Dream of Encrypted Binary Protocol?](https://rubykaigi.org/2022/presentations/yu_suke1994.html): [presentation]()

ノーコメント。

#### ランチ

この日も一番に弁当を取得。弁当はみんな大好き松坂牛ローストビーフ丼にした。感想としては、もう肉はいいかな。

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">昼飯　<a href="https://twitter.com/hashtag/rubykaigi?src=hash&amp;ref_src=twsrc%5Etfw">#rubykaigi</a> <a href="https://t.co/vpOf6Uwf0K">pic.twitter.com/vpOf6Uwf0K</a></p>&mdash; katsyoshi (@katsyoshi) <a href="https://twitter.com/katsyoshi/status/1568070622210826240?ref_src=twsrc%5Etfw">September 9, 2022</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

#### 自キKaigi

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">きょうも <a href="https://twitter.com/hashtag/%E8%87%AA%E3%82%ADKaigi?src=hash&amp;ref_src=twsrc%5Etfw">#自キKaigi</a> やります。ランチ食べたらスポンサーエリアの手前あたりにお集まりください <a href="https://twitter.com/hashtag/rubykaigi?src=hash&amp;ref_src=twsrc%5Etfw">#rubykaigi</a></p>&mdash; 🇺🇦hasumikin🇺🇦 (@hasumikin) <a href="https://twitter.com/hasumikin/status/1568023525407211521?ref_src=twsrc%5Etfw">September 8, 2022</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

誘われたので参加しました。iPad で活動しようと思い [corne](https://github.com/foostan/crkbd) 持っていってたので参加してた。

#### [Ruby x BPF in Action: How important observability is](https://rubykaigi.org/2022/presentations/udzura.html): [presentation]()
#### [Hunting Production Memory Leaks with Heap Sampling](https://rubykaigi.org/2022/presentations/KnuX.html): [presentation]()
このあたりネットワークが不安定になり、セッションが中断されたりしたためあまり覚えていない。

#### 立ち話

久し振りの対面での参加だったのでセッションあまり聞かずに受け付け前で立ち話している人たちがいたので、その立ち話に参加した。
たまたま、最近読んでる本の翻訳者がいたのでこれからの話などを聞いたりしていた。

#### [Caching With MessagePack](https://rubykaigi.org/2022/presentations/shioyama.html): [presentation]()
#### [Ruby Committers vs The World](https://rubykaigi.org/2022/presentations/rubylangorg.html)

#### 夕飯

夕飯は、駅近くのお店でかるく3人で。

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">いまどこ</p>&mdash; tagomoris (@tagomoris) <a href="https://twitter.com/tagomoris/status/1568221035702685696?ref_src=twsrc%5Etfw">September 9, 2022</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

こんなリプライがきてたようなので一緒にいたひとにまかせた。ギリギリまで粘ってたので帰宿後すぐ就寝。

## DAY 3

3日目はそもそも前2日の疲れが出たりしたのかほとんど覚えていない。あと [ruby puzzle](https://ruby-puzzles-2022.cookpad.tech/) をやっていてセッションをあまり聞けていない。

#### 朝ごはん

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">今日も一日 <a href="https://twitter.com/hashtag/rubykaigi?src=hash&amp;ref_src=twsrc%5Etfw">#rubykaigi</a> <a href="https://t.co/PKnj8sHxwE">pic.twitter.com/PKnj8sHxwE</a></p>&mdash; katsyoshi (@katsyoshi) <a href="https://twitter.com/katsyoshi/status/1568387387088322560?ref_src=twsrc%5Etfw">September 9, 2022</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

#### [Megaruby - Running mruby/c programs on Sega Mega Drive](https://rubykaigi.org/2022/presentations/yujiyokoo.html): [presentation]()

これに間に合うように出たはずだったが、間に合わず聞いていた。メガドライブすげえ。

#### [Automatically Find Memory Leaks in Native Gems](https://rubykaigi.org/2022/presentations/peterzhu2118.html): [presentation]()
#### [Fast data processing with Ruby and Apache Arrow](https://rubykaigi.org/2022/presentations/ktou.html): [presentation]()
#### [Fixing Assignment Evaluation Order](https://rubykaigi.org/2022/presentations/jeremyevans0.html): [presentation]()
#### [Stories from developing YJIT](https://rubykaigi.org/2022/presentations/alanwusx.html): [presentation]()

## まとめ

3年ぶりの現地開催だったので参加してきました。対面での参加はやはりよいもので新しい出会いや、疑問を本人、解っている人に直接聞くことができてすばらしい体験でした。


### 三重について
三重県の酒のイメージは、行く前は「作」「 而今」「伊勢角谷屋麦酒」というイメージでした。
とくに日本酒は美味しいイメージがあり、知らない日本酒もあったりするんだろうなあと思っていました。
行ってみると日本酒はイメージと変わらないのですが、だいたい似た味の傾向で飽きるというかなんというか
美味しいんだけど、好きじゃないという感想になってしまった。
ビールについては瓶ビールしか飲まなかったので感想はなしで。

三重は大きな街が比較的分散しているようで、津も松坂も伊勢も鳥羽も駅前が小さく感じました。
とくに0日目の食事が大変で、調達が難しかった。
