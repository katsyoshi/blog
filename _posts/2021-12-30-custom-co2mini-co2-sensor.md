---
layout: post
title: "custom CO2-mini で CO2 を見えるようにしよう"
date: 2021-12-30 16:59:59 +0900
comments: true
categories: diary tech
---

コロナになって結構前に [**custom CO2-mini**](https://www.kk-custom.co.jp/emp/CO2-mini.html)
に [話題になった](https://www.itmedia.co.jp/pcuser/articles/2012/18/news069.html)
ので買って放置してあったの[^buy-co2mon] を活用しようと思いたった。
とりあえず値は取得はできているので [**mackerel**](https://meckerel.io) との連携をしてグラフに表示できるようにします。
あと  **mercker-plugin** を [**Rust**](https://www.rust-lang.org) で書いてみたいとおもったので、やってみることにしました。

以下のリポジトリにコードはあります。

[![katsyoshi/mackerel-plugin-co2mon - GitHub](https://gh-card.dev/repos/katsyoshi/mackerel-plugin-co2mon.svg)](https://github.com/katsyoshi/mackerel-plugin-co2mon)

## mackerel plugin として作る
**mackerel** に投稿する前にこの **custom CO2-mini** が **Rust** で読めるのかを調査してみましたら、[**co2mon**](https://crates.io/crates/co2mon) がピンズドな感じでありました。
確認としてセンサーの読み込みは **co2mon** の [README の通り](https://github.com/lnicola/co2mon#permissions) にやることで読みとることができます。

センサーの値が読み込めるようになったら、今度は **mackerel** へ投げれるようにします。
と言ってもやることは [公式にあるよう](https://mackerel.io/ja/docs/entry/advanced/custom-metrics#post-metric) に以下のフォーマットで標準出力に出すだけのようです。

```
{metric name}\t{metric value}\t{unix epoch time}
```

ということなので適当に **metric name** を `CO2MINI.co2/temp.living` [^custom-co2mini] にして出力しています。
**mackerel-plugin** として動かすために、 **mackerel-agent.conf** に以下のような設定を追加し、再起動することでグラフが追加できます。

```toml
[plugin.metrics.CO2MINI]
command = ["/path/to/build/bin/mackerel-plugin-co2mon"]
```

グラフは以下のように表示されました!やったね!
![](/images/screenshot/co2mini-metrics.png)

## おわり

ずっとやろうやろうと思ってた **Rust** で **mackerel** のプラグイン作成、
面倒で先延しにしてたのですが、チョットやってみたらすぐにできたのでよかったです。
今後としては **CO<sub>2</sub>** の値に応じて窓開けたりできるようにしたいなあと思っています[^window]。

---

[^buy-co2mon]: Amazon で確認したら買ったの 2020/03 だった……
[^custom-co2mini]: mackerel のグラフ表示部分のタイトルが `custom.CO2MINI.co2.living` となり、メーカー名も入っていいじゃんとなった。
[^window]: 窓開閉する道具もないのでそこから仕入れる必要がありいつになるか不明です。
