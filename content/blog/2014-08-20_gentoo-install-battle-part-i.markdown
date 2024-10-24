+++
layout: post
title: "Gentoo Install Battle Part I"
date: 2014-08-20 23:48:45 +0900
comments: true
categories: tech linux gentoo mikutter
+++

ここにThinkPad X240があります。これの中身を Gentoo/Linux にします。

## 準備

準備として、Gentoo/LinuxのLiveCDを作成、Bootの設定を行います。


### イメージ作成
まずLiveCDイメージをダウンロードしUSBフラッシュメモリにコピーします。

```
$ wget ftp://ftp.iij.ad.jp/pub/linux/gentoo/releases/amd64/autobuilds/current-iso/install-amd64-minimal-20140814.iso
$ dd of=/path/to/disk if=install-amd64-minimal-20140814.iso bs=4m
```

でイメージの作成終了です。
次に、X240の設定を行います。

### X240の起動準備
Windowsを起動し、Shift押しながら再起動します。
再起動したら、以下の順番でBIOSを起動させます。

```
トラブルシューティング -> 詳細オプション -> UEFIファームウェアの設定 -> 再起動
```

でBIOSがたちあがるので、Secure BootとUEFI BootとBoot順の設定を変更します。

Seucre Bootでは`Securety -> Secure Boot -> Secure Boot [Enable]`を`[Disable]`に変更

UEFI Bootでは`Startup -> UEFI/Legacy Boot [UEFI only]`を`[Both]`に変更

Boot順の設定では、`Startup -> Boot`でBootさせたい順番に変更

BIOSを変更したらRestartします。

## まとめ

とりあえず今日はここで終りです。インストールは明日以降にします。
