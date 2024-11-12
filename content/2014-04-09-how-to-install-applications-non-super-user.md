+++
path = "2014/04/09/how-to-install-applications-non-super-user
title = "スーパーユーザ権限もたずに好きなソフトをインストール"
date = 2014-04-09
comments = true
categories = "tech linux"
+++

Linux使っててこのソフトが入ってないしsudoも使えないってときありませんか？
そんなときに好きなソフト(tmux, emacs)をインストールする方法を書いときます。
ここでは、tmuxとemacsについてインストール方法を書いときます。

## 方法
方法として以下のふたつがあると思う

1. 頑張っていれる
1. パッケージマネージャ

## 頑張って入れる
この場合はちょっと管理がめんどうかもしれないが書いときます。

前提条件としてgccとwgetがインストールされていることでX11関係のライブラリがインストールされていないこととします。

### emacs
以下のコマンドでインストールできます。

```
 $ wget http://ftp.gnu.org/pub/gnu/emacs/emacs-24.3.tar.xz
 $ tar xvf emacs-24.3.tar.xz
 $ cd emacs-24.3
 $ ./configure --prefix=${HOME}/.local/ --without-x --without-dbus --without-gnutls --without-makeinfo
 $ make -j4 bootstrap
 $ make install
 $ export PATH=${HOME}/.local/bin:${PATH}
 $ emacs
```

### tmux
以下のコマンドで必要ライブラリのncursesをインストールします。

```
 $ wget http://ftp.gnu.org/pub/gnu/ncurses/ncurses-5.9.tar.gz
 $ tar xvf ncurses-5.9.tar.gz
 $ cd ncurses-5.9
 $ ./configure --prefix=${HOME}/.local
 $ make -j4
 $ make install
```

次にlibeventをインストールします。

```
 $ wget https://github.com/downloads/libevent/libevent/libevent-2.0.21-stable.tar.gz
 $ tar xvf libevent-2.0.21-stable.tar.gz
 $ cd libevent-2.0.21-stable
 $ ./configure --prefix=${HOME}/.local
 $ make -j4
 $ make install
```

最後にtmuxをインストールします。

```
 $ wget http://downloads.sourceforge.net/project/tmux/tmux/tmux-1.9/tmux-1.9a.tar.gz
 $ tar xvf tmux-1.9a.tar.gz
 $ cd tmux-1.9a
 $ CFLAGS="-I${HOME}/.local/include -I${HOME}/.local/include/ncurses" LDFLAGS=-L${HOME}/.local/lib ./configure --prefix=${HOME}/.local
 $ make -j4
 $ make install
 $ LD_LIBRARY_PATH=${HOME}/.local/lib tmux
```

すべてインストール終ったら設定をします
```
 $ echo 'export PATH=${HOME}/.local/bin:${PATH}' >> ${HOME}/.bashrc
 $ echo 'export LD_LIBRARY_PATH=${HOME}/.local/lib:${LD_LIBRARY_PATH}' >> ${HOME}/.bashrc
```


## パッケージマネージャ
Gentoo/Prefix使え
