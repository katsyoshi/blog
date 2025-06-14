---
layout: post
title: "Gentoo のバイナリ配布公式レポを発見したので対応してみたら大失敗した"
date: 2025-05-31 23:59:59 +0900
comments: true
categories: diary gentoo tech
---

[__Gentoo Linux__] でバイナリが公式配っていたのを[発見](https://pong.ursm.jp/posts/4)したので、
[退役したデスクトップ](https://gist.github.com/katsyoshi/3d28b9609b7d395826ed93dd8288f0a9)をサーバー用途として利用してみます。

## 設定

設定自体は発見したブログを参考に設定することで問題はないです。

### インストール

ではインストールをしてみましょう。
ブログを参考にリポジトリなどを設定後、以下のコマンドでインストールします。

```
emerge -uDN world
```

### そして失敗へ……

インストールが終わり、動いているのを確認しましょう。
動いていますね。OK, OK では不要なパッケージを片づけましょうかね。

```
emerge --depclean
```

一部バイナリになって便利になりましたねー。便利ー。

[__Zellij__](https://zellij.dev) のセッションが残っている間は問題が特にわからなかったです。
次の日あたり、クライアントから [__Samba__](https://www.samba.org/) がなぜか繋がらなくなっています。
原因を探るため、 __ssh__ でログインしてみましょう。
これもログイン失敗します。しかたないのでユーザーを替えてログインしてみます。こいつはログインできます。
ログインしたアカウントから `doas` してみますが、 `doas` できる権限を与えていないので当然できません。
`sudo` は `sudo` 自体を削除しているのでできないですね。
また `su` も権限がないので別ユーザーへ変更できないです。
ということで何かまずいことが起きているのでログインできるようにサーバーの復旧を行ないます。

いつも通り __OS__ インストールと同様にインストールディスクを起動して[必要なディレクトリをマウント](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base/ja#%E5%BF%85%E8%A6%81%E3%81%AA%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0%E3%82%92%E3%83%9E%E3%82%A6%E3%83%B3%E3%83%88%E3%81%99%E3%82%8B)します。
そうするとなぜかいつも利用しているはずのコマンド `eix` 、 `git` や `zsh` が無いのです!!!

ということでインストールします。

```bash
emerge --sync # リポジトリを git で読むようにしているので失敗
emerge git # デフォルトリンカを mold にしているので失敗
# mold 使わないように修正
emerge git
emerge eix
eix-sync # ようやく成功
emerge zsh
```

というようなコマンドを実施してなんとか復旧。

### おわり
バイナリの配布リポジトリを設定できるのは薄々知ってはいたのですが、
動いているのを変更するの面倒だったので今回替えてみました。
カーネルもバイナリでインストールできるようになったので便利になりました(たぶん)。
あとうかつに `emerge --depclean` はするもんじゃない。
