+++

title = "hello language server"
date = 2018-12-01
comments = true
categories = "lsp emacs tech diary"
+++

ここ1、2週間、 emacs で `lsp-emacs` を使いはじめたのでそのメモ

[Language Server](https://langserver.org/) と [Language Server Protocol](https://microsoft.github.io/language-server-protocol/specification) ってなによ？って人はリンクをみてください。

## 導入

導入は簡単で以下の3つをpackage-installでインストールしてしまえば OK です。

```
lsp-mode
lsp-ui
lsp-ruby
```

インストール後は `.emacs.d/init.el` あたりに、

```
(require 'lsp-mode)
(require 'lsp-ui)
(require 'lsp-ruby)
(add-hook 'ruby-mode-hook #'lsp-ruby-enable)
```

を追記すると emacs 側の設定はできました。

`lsp-ruby`(emacs) を起動する前に `gem install solargraph` で `solargraph` をインストールしておきましょう。

## 利用開始!

利用開始するときは利用したいファイルのトップディレクトリ(プロジェクトディレクトリ)に、 `Gemfile` が必要です。

でこんなかんじに表示してくれます。

-> ![](/images/screenshot/lsp-ruby.png) <-

## matome

`lsp-ruby` を導入したけど、`solargraph` だとちょっとあれな表示が出て残念な気分に。
また `rubocop` も基本的に要求されるようで…
