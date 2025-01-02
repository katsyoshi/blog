+++
path = "/blog/2017/05/29/installing-apache-arrow"
layout = "post"
title = "installing apache arrow"
date = 2017-05-29T21:45:29+09:00
comments = true
categories = "tech diary"
+++

[最近](https://slide.rabbit-shocker.org/authors/kou/nagoya-rubykaigi-03/)[すとうさん](https://github.com/kou)が[一押し](https://slide.rabbit-shocker.org/authors/kou/data-science-rb/)している[apache arrow](https://arrow.apache.org/)をインストールしてみた

## 環境
```
$ uname -a
Linux rin 4.9.10-gentoo #6 SMP Tue Mar 28 01:29:26 JST 2017 x86_64 Intel(R) Xeon(R) CPU E5-2620 v4 @ 2.10GHz GenuineIntel GNU/Linux
$ gcc -v
組み込み spec を使用しています。
COLLECT_GCC=/usr/x86_64-pc-linux-gnu/gcc-bin/5.4.0/gcc
COLLECT_LTO_WRAPPER=/usr/libexec/gcc/x86_64-pc-linux-gnu/5.4.0/lto-wrapper
ターゲット: x86_64-pc-linux-gnu
configure 設定: /var/tmp/portage/sys-devel/gcc-5.4.0-r3/work/gcc-5.4.0/configure --host=x86_64-pc-linux-gnu --build=x86_64-pc-linux-gnu --prefix=/usr --bindir=/usr/x86_64-pc-linux-gnu/gcc-bin/5.4.0 --includedir=/usr/lib/gcc/x86_64-pc-linux-gnu/5.4.0/include --datadir=/usr/share/gcc-data/x86_64-pc-linux-gnu/5.4.0 --mandir=/usr/share/gcc-data/x86_64-pc-linux-gnu/5.4.0/man --infodir=/usr/share/gcc-data/x86_64-pc-linux-gnu/5.4.0/info --with-gxx-include-dir=/usr/lib/gcc/x86_64-pc-linux-gnu/5.4.0/include/g++-v5 --with-python-dir=/share/gcc-data/x86_64-pc-linux-gnu/5.4.0/python --enable-languages=c,c++,fortran --enable-obsolete --enable-secureplt --disable-werror --with-system-zlib --enable-nls --without-included-gettext --enable-checking=release --with-bugurl=https://bugs.gentoo.org/ --with-pkgversion='Gentoo 5.4.0-r3 p1.3, pie-0.6.5' --enable-libstdcxx-time --enable-shared --enable-threads=posix --enable-__cxa_atexit --enable-clocale=gnu --enable-multilib --with-multilib-list=m32,m64 --disable-altivec --disable-fixed-point --enable-targets=all --disable-libgcj --enable-libgomp --disable-libmudflap --disable-libssp --disable-libcilkrts --disable-libmpx --enable-vtable-verify --enable-libvtv --enable-lto --without-isl --enable-libsanitizer
スレッドモデル: posix
gcc バージョン 5.4.0 (Gentoo 5.4.0-r3 p1.3, pie-0.6.5)
```

## いんすとーる

今回まだGentooにパッケージがないのでgitからインストールします。ほしいのはrubygems.orgに公開されている[red-arrow](https://rubygems.org/gems/red-arrow)をコンパイルするためにglibとこれを利用するために必要な依存パッケージとしてcppをインストールします。インストールはかんたんで `cmake` を実行すればインストールデキルはずです。

```console
$ git clone git@github.com:apache/arrow.git
$ cd arrow/cpp
$ mkdir release
$ cd release
$ cmake .. -DCMAKE_BUILD_TYPE=Release
```

でリリース用パッケージがビルドされるはずですが[エラー](https://gist.github.com/katsyoshi/4486792ad43feae4d690d589dac1a157)が出ます。
これはJIRAで[検索した結果](https://issues.apache.org/jira/browse/ARROW-667)より `gcc` のバージョン情報がとれずにエラーをだしているやりとりが発見されたのでわかりました。
ログを見ると実際に取得できていないことも確認しました。で `cpp/cmake_modules/CompilerInfo.cmake` をみるとどうやら日本語のバージョン情報は考慮されていないような作りになっていました。
なので `LANG=C` をつけて `cmake` 再びつけて実行します。

``` console
$ LANG=C cmake .. -DCMAKE_BUILD_TYPE=Release
$ make
$ make install
```

これでC++のインストールは終了です。続いてglibをインストールします。

```console
$ cd ../../c_glib
$ ./autgen.sh
$ ./configure
$ make
$ make install
```

でインストールできます。こちら久々に野良ビルドしたため `PKG_CONFIG_PATH` や `LD_LIBRARY_PATH` の設定をわすれてただけなのですんなりいけました。
で最後に `gem install red-arrow` を実行して目的を達成しました!!!!11

## おわり
ほんとうのもくてきは `fluentd` のぷらぐいんをかくことですがつかれたのできょうはここまで
