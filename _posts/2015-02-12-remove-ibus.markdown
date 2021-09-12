---
layout: post
title: "とりあえずibusやめてみた"
date: 2015-02-12 07:10:29 +0900
comments: true
categories: tech gentoo linux ime
---

[ibus-1.5](https://www.google.co.jp/webhp?sourceid=chrome-instant&ion=1&espv=2&ie=UTF-8#newwindow=1&q=ibus1.5)からあまり評判がよろしくない(特に不満はない)ので、あたらしく[fcitx](https://code.google.com/p/fcitx/)を導入してみた。

## 前提条件

ここでは[Gentoo/Linux](http://www.gentoo.org)での導入方法について述べています。
日本語変換エンジンとしてMozcについて述べています。

## 導入

とりあえずfcitxをインストール、設定します。

```
$ emerge -av fcitx

$ $EDITOR .xinitrc

export XMODIFIERS=@im="fcitx"
export GTK_IM_MODULE="fcitx"
export QT_IM_MODULE="fcitx"
fcitx
```

これでインストールはこれでおわりです。

## fcitxでmozc

これは、[overlay](http://gpo.zugaina.org/app-i18n/mozc)が提供されいているのでそれで導入します。

```
$ $EDITOR /etc/portage/make.conf
USE="fcitx ${USE}"

$ sudo emerge -av mozc
```

で、インストールすれば使えるようになります。

## おわり

[こういうの](http://anond.hatelabo.jp/20150210030728)があったので、真面目にかこうかと思ったんですがどめんどうなので導入メモになりました。

あと引越ししたので贈っていただいてもいいんですよ？
http://www.amazon.co.jp/registry/wishlist/V0YZPO1OYFH5/
