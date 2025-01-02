+++
path = "/blog/2014/09/12/jubatus-in-gentoo"
layout = "post"
title = "Gentoo LinuxむけのJubatusパッケージ書いたよ"
date = 2014-09-12T12:29:20+09:00
comments = true
categories = "linux portage jubatus gentoo tech"
+++

[PC](/blog/2014/08/20/gentoo-install-battle-part-i/)を[買い替えた](/blog/2014/08/21/gentoo-install-battle-part-ii/)ので[環境構築](/blog/2014/08/31/gentoo-install-battle-part-iii/)してます。
[Jubatusのoverlay](https://github.com/kazuki/overlay/tree/master/sci-calculators/jubatus)があったのですが、古かったので新しく[Jubatusのoverlay](https://github.com/katsyoshi/overlay)作成しました。

## インストール

これは、以下のコマンドでインストールができます。

```
# curl https://raw.githubusercontent.com/katsyoshi/overlay/master/profiles/layman.xml > /etc/layman/overlays/katsyoshi.xml
# layman -f -a katsyoshi
# eix-sync
# emerge -av jubatus
```

でいけるとおもいます。

## 未実装
- jubatus_coreのオプションがはたらいてない(ハズ)
- jubatus_msgpack-rpcの依存パッケージにdev-libs/msgpackをいれていない
- jubatusのオプションを確認していない

などなどあるので、PRまってます。

## まとめ
[よくみたら](https://github.com/kazuki/overlay/)必要なライブラリのパッケージもあったのでこれのバージョンとjubatus_core追加するだけでよかったかも。

ちゃんちゃん

