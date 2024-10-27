+++

title = "すくりーんきゃすと"
date = 2015-05-15
comments = true
categories = "movie screencast diary"
+++

LinuxでScreenCast用の環境を整えてみた。

## 必要パッケージ

* ffmpeg
* gtk-recordMyDesktop

## 使いかた

`gtk-recordMyDesktop` でデスクトップをキャプチャする。
その後は、 `ffmpeg` を使って好きな動画形式に変換します。
ここでは、gif画像に変更しています。

```
$ ffmpeg -i ~/screencast.ovg img/screencast.gif
```

-> ![](/images/screenshot/gtk-recordMyDesktop.gif =512x) <-

### 参考サイト
1. http://d.hatena.ne.jp/over80/20080802/1217693705
