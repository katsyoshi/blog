+++

title = "Gentoo Install Battle Part II"
date = 2014-08-21
comments = true
categories = "tech linux gentoo mikutter"
+++

X240の準備は終ったので、[インストール](http://wiki.gentoo.org/wiki/Quick_install_guide)します。基本的にはこの流れです。

## USBブート

[作成したUSBメモリ](/blog/2014/08/20/gentoo-install-battle-part-i/)をX240に差して起動します。
起動したら、まずパーティションの設定を行います。

```
$ gpart /dev/sda
```

で不要なパーティションを削除し、必要なパーティションを作成します。

## 無線LAN

ネットワークを接続しますが、今回は無線LANを利用してインストールを行います。

まず、どこか適当な位置に[リンク先](http://www.xmisao.com/2014/01/16/how-to-connect-wpa2-wireless-lan-using-wpa-supplicant.html)のファイルを作成してください。

次に以下のコマンドを実行します。

```
$ wpa_supplicant -Dnl80211 -ieth0 -c/path/to/supplicant.config
```

実行したら、接続されているのでATL + F2とかでコンソールの表示を変更してください。

## おわり

有線のLANケーブルがなかったからはまった。
続きは明日以降に…
