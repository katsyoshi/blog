---
layout: post
title: "Visual Studio Codeを使ってみた"
date: 2015-04-30 13:09:55 +0900
comments: true
categories: tech
---

[Visual Studio Code](https://code.visualstudio.com/)というものがでたので普段利用している
Linuxで使ってみた。

## インストール
は簡単にできて、ウェブサイトからZIPファイルをダウンロードして展開し、展開したディレクトリで
以下のコマンドで実行できます

```
$ ./Code
```

## 感想
VSC使ってみたけど、日本語表示がされない。

-> ![](/images/screenshot/vsc_japanese.webp) <-

_追記_

フォントを変更すると表示できるようになりました。

`File -> Preferences -> User Settings` から、 `settings.json` を開き、
以下のように追加する

```json
{
  "editor.fontFamily": "Ricty"
}
```

-> ![](/images/screenshot/vsc_settings.webp) <-

_追記おわり_

デバッガ動かすためにMONOが必要。

![](/images/screenshot/vsc_debugger_needed_mono.webp)

補完がよさそう。Rubyでの補完の出かたです。

-> ![](/images/screenshot/vsc_completion.webp) <-

などあります。

そのうちなにかこれで書くかなぁ。
