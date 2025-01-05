+++
path = "/blog/2015/01/31/emacs-fonts"
layout = "post"
title = "Rictyやめるってよ"
date = 2015-01-31T07:10:29+09:00
comments = true
categories = "diary tech linux"
+++
普段、emacs上でフォントを `Ricty` にして使ってたけど、微妙にずれるのを解消できなかったはなし

## 症状と設定

Rictyのときの症状と設定をのせておきます。

-> ![Ricty](/images/screenshot/Ricty.png) <-

```lisp
(set-face-attribute 'default nil :family "Ricty" :height 135)
(set-fontset-font (frame-parameter nil 'font) 'japanese-jisx0208 '("Ricty" . "iso10646-1"))
(setq face-font-rescale-alist '(("Ricty" . 1.2)))
```

で解消できそうにないので DejaVu を用いて表示するように変更しました。

-> ![DejaVu](/images/screenshot/DejaVu.png) <-

```lisp
(set-face-attribute 'default nil :family "DejaVu" :height 135)
```

こんな感じになりました。

## おわり
よくわからないので諦めました。ので参考に

最後に引っ越ししました。

http://www.amazon.co.jp/registry/wishlist/V0YZPO1OYFH5/

## 追記

<blockquote class="twitter-tweet" lang="en"><p><a href="https://twitter.com/katsyoshi">@katsyoshi</a> (setq face-font-rescale-alist &#39;((&quot;Ricty&quot; . 1.0))) で常用していてずれないのですが、これだとどうなりますか?</p>&mdash; つまみ (@polamjag) <a href="https://twitter.com/polamjag/status/561875516118949889">February 1, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

とアドバイスがありましたので、やってみました!!!!!!

-> ![Ricty-1.0](/images/screenshot/Ricty1.0.png) <-

ほんとにちょっとずれて気になる…
