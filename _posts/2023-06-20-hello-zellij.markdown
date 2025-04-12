---
layout: post
title: "Zellij はじめました"
date: 2023-06-19 23:59:59 +0900
comments: true
categories: diary
---

はい。タイトルの通り、 [**Zellij**](https://zellij.dev/) をはじめてみました。
切っ掛けは以下のツイートを見つけたので。

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">tmux みたいなやつは terminal multiplexer とか呼ばれてて，たぶん沼さんが求めてるのは zellij<a href="https://t.co/KlyMMeatzH">https://t.co/KlyMMeatzH</a></p>&mdash; sksat (@sksat_tty) <a href="https://twitter.com/sksat_tty/status/1670656419174191105?ref_src=twsrc%5Etfw">June 19, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

というっっっっっっっっっっっっっっっっっっっっっっっっっっっっっっっっっっっっっっっっっっっことでね、はじめます。

## Zellijとは？

[**Rust**](https://rust-lang.org) 製の [**tmux**](https://github.com/tmux/tmux) クローン。
はやってますね **Rust** 。

## Install

つかってるのが **Gentoo Linux** なので `portage` からもはいります。

```bash
$ sudo emerge zellij
```

他の **OS** やソースコードからのインストール方法は公式から見て入れてください。

## Customize

**tmux** のクローンとあり、カスタマイズもかなりできるようです。
とりあえず、起動してみましょう。

```bash
$ zellij
```

![](/images/screenshot/zellij-default.webp)

デフォルトでもナビがあり、簡単に利用できて便利です。が、キーバインドが気に食わないので変更しましょう。
といってもカスタムするファイルを書いていませんでしたね。初期設定を標準出力に `zellij setup --dump-config` で出せるので _リダイレクト_ なり、 _コピペ_ なりでファイルを作りましょう。保存先はどこでもいいのですが、標準で読み込んでくれる `$HOME/.config/zellij/config.kdl` がいいでしょう。ファイル形式は [**KDL**](https://kdl.dev/) となっていますがなんんもわからん。

```kdl
keybinds clear-defaults=true {
  shared {
    bind "Ctrl g" { SwitchToMode "Normal"; }
  }
  normal {
    bind "Ctrl t" { SwitchToMode "Tmux"; }
  }
  tmux {
    bind "c" { NewTab; SwitchToMode "Normal"; }
    bind "o" { FocusNextPane; SwitchToMode "Normal"; }
  }
}
```

`keybinds` でキーバインドの設定がデキルのですが、 `clear-defaults=true` を指定してあげることで _デフォルト_ のキーバインドを消すことができます。[モードがたくさんあり](https://zellij.dev/documentation/keybindings-modes.html)、そのモードに対してそれぞれキーバインドを設定することができます。上記の例では、**Normal** モードで `Ctrl t` を押すと、**Tmux** モードに入ります。ここで、`c` を入力すると新しい **tab** で **Shell** が動きます。今どのモードに居るのかわからなくなっても必ず **Normal** モードへ `Ctrl g` で戻れるように `shared` に設定してあります。
こんな感じで設定は簡単にできます。

## おわりに

かんたんに **zellij** の現在(2023/6/20 バージョン `0.36.0` で確認。最新バージョンは `0.37.0`。)の設定をみてきました。カスタムできるのはキーバインドだけじゃなく、カラースキーム、レイアウトなどなど様々に変更できるので御参考までにーーーー。
ねむい。
