+++
path = "/blog/2016/12/10/itamae-loves-gentoo"
layout = "post"
title = "板前さんでGentooを管理"
date = 2016-12-10
comments = true
categories = "itamae gentoo advent_calendar tech"
+++

[Gentoo Advent Calendar 2016](http://www.adventar.org/calendars/1493) の10日目の記事です

## Itamaeでgentoo管理するには？
そらはー([@sora_h](https://twitter.com/sora_h))が作ってる [itamae-plugin-resource-portage](https://github.com/sorah/itamae-plugin-resource-portage) 使え

## 自作のportage管理ソフトの紹介

[ここ](https://github.com/katsyoshi/itamae-recipes/tree/master/cookbook/gentoo)にあります。

### 使いかた
簡単な使いかたとしては以下のようになります

```
itamae local gentoo.rb -j package.json
```

これだけです。

### パッケージの指定方法

```yaml
packages:
 - name: 'category/package'
   portage:
     use: 'package'
     accept_keywords: 'package'
     license: 'pakcage'
```

`name` で `category/package` を指定することでインストールすることができます。
`portage` では `use` などのフラグを指定して管理します。
`use` ディレクトリを作成してそこにファイルを置く必要があります。
自動生成したいけど、依存パッケージの指定方法とかまだ考えていないので取り敢えず直接指定です。

## おわり

この方法ではitamaeのパッケージ管理方法とちがうのでいろいろとあれだなあ
とおもってたら itamae-plugin-resource-portage みつけてしまったので
itamaeで管理したいとおもう人は itamae-plugin-resource-portage を使いましょう
