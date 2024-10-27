+++

title = "Elixirのインストール方法"
date = 2017-04-01
comments = true
categories = "tech elixir diary"
+++

[Elixir Conf Japan](http://www.elixirconf.jp/) に行きましたが、よく考えてみたら
まだこの PC に [Elixir](http://elixir-lang.org/) をインストールしていないことに気がついたので
隣の[英語のうまいおじさん](https://twitter.com/zzak_jp)にインストール方法を[教えてもらいながら](https://gist.github.com/katsyoshi/7ac2579bbe903ff65685570fd3873379)インストールしました[^zakk]


## 準備

ここでは [Gentoo Linux](https://gentoo.org/) を前提としております。
まず、Elixir を動かすために [Erlang](https://www.erlang.org) をインストールします。


```console
sudo eix-sync
sudo emerge erlang
```

準備はこれだけです。

## インストール

次にインストールなのですが、Elixir は Earlang VM 上で動くプログラミング言語のため、コンパイルされたバイナリを置くだけでインストールがおわります。

```console
wget https://github.com/elixir-lang/elixir/releases/download/v1.4.2/Precompiled.zip
unzip Precompiled.zip -d elixir
mv elixir /path/to/elixir
echo 'export PATH=/path/to/elixir/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```


## おわり

さいごに `iex` を起動して確認すればおわり


### 参考

[^zakk]: elixir getting started, https://gist.github.com/zzak/a765d6a63860d75c4444e35f57daed13, 2017/04/01 閲覧
