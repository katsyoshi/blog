---
layout: post
title: "endgame keyboard..."
date: 2019-02-11 22:48:24 +0900
comments: true
categories: diary tech keyboard fortitude60
---

新しいキーボードを作りました!

## [Fortitude60](https://github.com/Pekaso/fortitude60)

去年の9月頃に **Group Buy** の募集があったので応募して購入。

## くみたて

[ビルドガイド](https://github.com/Pekaso/fortitude60/blob/master/Documents/buildguide_jp_v1.0.md) に沿って組み立てていけば問題ないです。

今回は、 LED をキー毎に光らせる方向ですすめてみました。

1. 基板にダイオードつけます。

![](/images/photo/fortitude60-diode.jpg)

2. 基板にLED 用に抵抗(470Ω)をスイッチ毎につけます。制御用の抵抗(1kΩ)と FET を各ボード毎につけます。

![](/images/photo/fortitude60-fet-resister.jpg)

3. スイッチをプレートにはめます。

![](/images/photo/fortitude60-switch-in-plate.jpg)

4. 基板とスイッチをはめたプレートを合体させます

![](/images/photo/fortitude60-docking-switch.jpg)

5. LED をスイッチに刺します。

![](/images/photo/fortitude60-led-in-switch.jpg)

6. 半田付します。

![](/images/photo/fortitude60-soldering-switch.jpg)

7. MCU を基板に接続

![](/images/photo/fortitude60-mcu-on-keyboard.jpg)

8. 通電確認!

![](/images/photo/fortitude60-lighting-keyboard.jpg)

9. keycap まうんと!

![](/images/photo/fortitude60-completing.jpg)

## おわり

という漢字になりました。
さすがに透過キーキャップだと部屋が明るくなりすぎるので不透明キーキャップに変更していまはつかってますので、大分おちつています。
