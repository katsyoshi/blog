+++
title = "Hello, ZOLA"
date = 2024-11-14
path = "/blog/2024/11/06/hello-zola"
+++

HELLO, [ZOLA](https://getzola.org) World!!!!!

## ELECTIONS

**[Jekyll](http://jekyllrb.com/)** でサイト作るの飽きてきたので、
新しい **SSG(Static Site Generator)** に変更することにした。

選定必須基準として、以下の条件を。

1. [`markdown`](https://tex2e.github.io/rfc-translater/html/rfc7763.html)[^markdown] で書けること
1. **Jekyll** と同じ **URL** が生成されること

いまどき直接 **HTML** で書く人はあまりいないし、いままでの記事があるので `markdown` で書けることは当然ですね。
もうひとつの必須条件として、 **Jekyll** で設定した **URL** で記事が書けることとなります。
これは現在出している記事の **URL** を変更しないために必須です。

移行に苦労しないようにするために以下の条件をとることにした。

1. **Jekyll** と同じ様に **markdown** の先頭に **front formatter** でいろいろと設定できること
1. **Jekyll** と同様に **テーマ** が選択できること
1. バイナリーで配ってること

**front fromatter** については移行を楽にするため多少の書き替えはしかたがないとしています。
デザイン変更を楽にするため **テーマ** が選択できる **SSG** だといいなあと思い。
バイナリーで配ってると楽なんですよ[^binary]。

以上の条件を踏まえいくつか検討してみましたが、 **zola** にしました。

[![getzola/zola - GitHub](https://gh-card.dev/repos/getzola/zola.svg)](https://github.com/getzola/zola)

## Jekyll to Zola


[^markdown]: [RFC](https://www.ietf.org/process/rfcs/)(Request For Comment) になっててびびってる
[^binary]: **Jekyll** は依存解決とかあって最新の
