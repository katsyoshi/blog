+++

title = "ItamaeつかってOSのインストールをやってみた"
date = 2016-10-09
comments = true
categories = "gentoo itamae gentoo-install-battle"
+++

9月はRubyKiagiにいってたりしました。
そのときに[mikutterのコミッター](http://mikutter.blogspot.jp/2016/09/mikutter-3312-343.html)とかになったようです。

シルバーウィークにThinkPadのOS再インストールをしたのでその記録を

## なにやったの？

[@mtsmfm](https://twitter.com/mtsmfm) と以前話していたとき、``ansibleつかってOSインストールしてるんだけど、完全自動化できないんですよね~'' みたいな事を聞いたので[Itamae](https://github.com/itamae-kitchen/itamae) をつかってやってみました。
結論から言うと完全自動化は無理だけど、ある程度は自動化できた。

### 環境
* machine: ThinkPad X250
* OS: Gentoo Linux
* Provisioning tool: Itamae
* repo: https://github.com/katsyoshi/itamae-recipes

## インストール
インストールディスクを起動するところはipmiとか搭載していない(しらべていない)し[Ironic](https://wiki.openstack.org/wiki/Ironic)はつかいたくないので手動で起動し、
sshdの起動とrootのパスワードを設定します。起動したら以下の手順でパーティション作成からカーネルのインストールまでします。

```console
git clone https://github.com/katsyoshi/itamae-recipes.git
cd itamae-recipes
bundle install
wget http://ftp.iij.ad.jp/pub/linux/gentoo/releases/amd64/autobuilds/current-stage3-amd64/stage3-amd64-20161006.tar.bz2
cp stage3-amd64-20161006.tar.bz2 cookbook/install/stage3.tar.bz2
itamae ssh -h nu-machine cookbook/install/gentoo.rb -u root -j cookbook/install/gentoo.json
```

でOSのインストールまでできるのですが、gentooのインストールメディアだと、diffがインストールされていないのでitamaeからfileのコピーができません。
都度コピーしてください(というよりgentooインストーラーにdiffを入れたほうが早そう)

## パッケージのインストール
ここまで終ったら、パッケージのインストールします。これも以下コマンドを実行することで終ります。

```console
itamae ssh -h nu-machine cookbook/gentoo/emerge.rb -j cookbook/gentoo/package.json
```

## owari

ItamaeつかってOSのインストールを実行してみました。diffが失いところがとくにつらいですね。
おわり
