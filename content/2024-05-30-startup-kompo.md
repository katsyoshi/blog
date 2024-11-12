+++
path = "/blog/2024/05/30/startup-kompo"
layout = "post"
title = "Ruby を KOMPO してみた"
date = 2024-05-29
comments = true
categories = "diary tech"
+++

**RubyKaigi2024** の発表、[**It's about time to pack Ruby and Ruby scripts in one binary**](https://speakerdeck.com/ahogappa0613/its-about-time-to-pack-ruby-and-ruby-scripts-in-one-binary)
で話されていた [**kompo**](https://rubygems.org/gems/kompo) を試してみた。

## じゅんびというかとらしゅーというかうごかすまでのきろく

とりあえず動かしてみましょう!!

```
$ gem install kompo
$ mkdir hello; cd hello;
$ echo puts \"hello, world\" > hello.rb
$ kompo
which: no brew in (/home/katsyoshi/.rbenv/versions/3.3.1/bin:/home/katsyoshi/.rbenv/libexec:/home/katsyoshi/.rbenv/plugins/ruby-build/bin:/home/katsyoshi/.local/share/zinit/plugins/rust/bin:/home/katsyoshi/.local/share/zinit/plugins/sk/bin:/home/katsyoshi/.local/share/zinit/plugins/hs/bin:/home/katsyoshi/.local/share/zinit/plugins/fd/bin:/home/katsyoshi/.rbenv/shims:/home/katsyoshi/.rbenv/bin:/home/katsyoshi/.pyenv/plugins/pyenv-virtualenv/shims:/home/katsyoshi/.pyenv/shims:/home/katsyoshi/.pyenv/bin:/home/katsyoshi/.local/share/zinit/polaris/bin:bin:/home/katsyoshi/.go/bin:/home/katsyoshi/bin:/home/katsyoshi/.bin:/usr/lib/llvm/17/bin:/usr/lib/llvm/18/bin:/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin:/bin:/sbin)
which: no brew in (/home/katsyoshi/.rbenv/versions/3.3.1/bin:/home/katsyoshi/.rbenv/libexec:/home/katsyoshi/.rbenv/plugins/ruby-build/bin:/home/katsyoshi/.local/share/zinit/plugins/rust/bin:/home/katsyoshi/.local/share/zinit/plugins/sk/bin:/home/katsyoshi/.local/share/zinit/plugins/hs/bin:/home/katsyoshi/.local/share/zinit/plugins/fd/bin:/home/katsyoshi/.rbenv/shims:/home/katsyoshi/.rbenv/bin:/home/katsyoshi/.pyenv/plugins/pyenv-virtualenv/shims:/home/katsyoshi/.pyenv/shims:/home/katsyoshi/.pyenv/bin:/home/katsyoshi/.local/share/zinit/polaris/bin:bin:/home/katsyoshi/.go/bin:/home/katsyoshi/bin:/home/katsyoshi/.bin:/usr/lib/llvm/17/bin:/usr/lib/llvm/18/bin:/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin:/bin:/sbin)
/home/katsyoshi/.rbenv/versions/3.3.1/lib/ruby/gems/3.3.0/gems/kompo-0.2.0/lib/kompo.rb:110:in `valid?': kompo-cli not found. Please install 'kompo-cli'. (RuntimeError)
        from /home/katsyoshi/.rbenv/versions/3.3.1/lib/ruby/gems/3.3.0/gems/kompo-0.2.0/lib/kompo.rb:120:in `block in cd_work_dir'
        from /home/katsyoshi/.rbenv/versions/3.3.1/lib/ruby/3.3.0/tmpdir.rb:99:in `mktmpdir'
        from /home/katsyoshi/.rbenv/versions/3.3.1/lib/ruby/gems/3.3.0/gems/kompo-0.2.0/lib/kompo.rb:118:in `cd_work_dir'
        from /home/katsyoshi/.rbenv/versions/3.3.1/lib/ruby/gems/3.3.0/gems/kompo-0.2.0/exe/kompo:8:in `<top (required)>'
        from /home/katsyoshi/.rbenv/versions/3.3.1/bin/kompo:25:in `load'
        from /home/katsyoshi/.rbenv/versions/3.3.1/bin/kompo:25:in `<main>'

```

「なるほどなるほど、 **`brew`** が必要なのか。そっかー。」
とおもったのだが、そもそも今 **Linux** だぞ？となり、[リポジトリ](https://github.com/ahogappa0613/kompo)を見ることに。
リポジトリを見ると **kompo-vfs** をインストールしろと書いてあるな。
[Building](https://github.com/ahogappa0613/kompo?tab=readme-ov-file#building) に書いてあるように、ダンロードして手元でビルドして、環境変数を設定してみる。

```
$ git clone git@github.com:ahogappa0613/kompo-vfs
$ cd kompo-vfs && cargo build --release
$ export KOMPO_CLI=$(realpath target/release/kompo-cli)
$ export LIB_KOMPO_DIR=$(realpath target/release)
$ kompo
which: no brew in (/home/katsyoshi/.rbenv/versions/3.3.1/bin:/home/katsyoshi/.rbenv/libexec:/home/katsyoshi/.rbenv/plugins/ruby-build/bin:/home/katsyoshi/.local/share/zinit/plugins/rust/bin:/home/katsyoshi/.local/share/zinit/plugins/sk/bin:/home/katsyoshi/.local/share/zinit/plugins/hs/bin:/home/katsyoshi/.local/share/zinit/plugins/fd/bin:/home/katsyoshi/.rbenv/shims:/home/katsyoshi/.rbenv/bin:/home/katsyoshi/.pyenv/plugins/pyenv-virtualenv/shims:/home/katsyoshi/.pyenv/shims:/home/katsyoshi/.pyenv/bin:/home/katsyoshi/.local/share/zinit/polaris/bin:bin:/home/katsyoshi/.go/bin:/home/katsyoshi/bin:/home/katsyoshi/.bin:/usr/lib/llvm/17/bin:/usr/lib/llvm/18/bin:/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin:/bin:/sbin)
which: no brew in (/home/katsyoshi/.rbenv/versions/3.3.1/bin:/home/katsyoshi/.rbenv/libexec:/home/katsyoshi/.rbenv/plugins/ruby-build/bin:/home/katsyoshi/.local/share/zinit/plugins/rust/bin:/home/katsyoshi/.local/share/zinit/plugins/sk/bin:/home/katsyoshi/.local/share/zinit/plugins/hs/bin:/home/katsyoshi/.local/share/zinit/plugins/fd/bin:/home/katsyoshi/.rbenv/shims:/home/katsyoshi/.rbenv/bin:/home/katsyoshi/.pyenv/plugins/pyenv-virtualenv/shims:/home/katsyoshi/.pyenv/shims:/home/katsyoshi/.pyenv/bin:/home/katsyoshi/.local/share/zinit/polaris/bin:bin:/home/katsyoshi/.go/bin:/home/katsyoshi/bin:/home/katsyoshi/.bin:/usr/lib/llvm/17/bin:/usr/lib/llvm/18/bin:/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin:/bin:/sbin)
/home/katsyoshi/.rbenv/versions/3.3.1/lib/ruby/gems/3.3.0/gems/kompo-0.2.0/lib/kompo.rb:112:in `valid?': Entrypoint not found: '/home/katsyoshi/hello/main.rb'. Please specify the entry file path with '-e' or '--entrypoint' option. (RuntimeError)
        from /home/katsyoshi/.rbenv/versions/3.3.1/lib/ruby/gems/3.3.0/gems/kompo-0.2.0/lib/kompo.rb:120:in `block in cd_work_dir'
        from /home/katsyoshi/.rbenv/versions/3.3.1/lib/ruby/3.3.0/tmpdir.rb:99:in `mktmpdir'
        from /home/katsyoshi/.rbenv/versions/3.3.1/lib/ruby/gems/3.3.0/gems/kompo-0.2.0/lib/kompo.rb:118:in `cd_work_dir'
        from /home/katsyoshi/.rbenv/versions/3.3.1/lib/ruby/gems/3.3.0/gems/kompo-0.2.0/exe/kompo:8:in `<top (required)>'
        from /home/katsyoshi/.rbenv/versions/3.3.1/bin/kompo:25:in `load'
        from /home/katsyoshi/.rbenv/versions/3.3.1/bin/kompo:25:in `<main>'
```

まだ駄目みたですね。エラーメッセージを見ると `-e` でエントリーポイントを指定しろとあるな。
ということで `kompo -e hello.rb` と入力してみますね。

```
$ kompo -e hello.rb
...snip...
gcc -O3 -Wall -I/tmp/d20240530-1369030-t2mico/dest_dir/include/ruby-3.3.0/x86_64-linux -I/tmp/d20240530-1369030-t2mico/dest_dir/include/ruby-3.3.0 -L/tmp/d20240530-1369030-t2mico/dest_dir/lib -L/home/katsyoshi/Program/Ruby/kompo/kompo-vfs/target/release  main.c -Wl,--start-group  fs.o /tmp/d20240530-1369030-t2mico/ruby/ext/extinit.o /tmp/d20240530-1369030-t2mico/ruby/enc/encinit.o /tmp/d20240530-1369030-t2mico/ruby/ext/bigdecimal/bigdecimal.a /tmp/d20240530-1369030-t2mico/ruby/ext/cgi/escape/escape.a /tmp/d20240530-1369030-t2mico/ruby/ext/continuation/continuation.a /tmp/d20240530-1369030-t2mico/ruby/ext/coverage/coverage.a /tmp/d20240530-1369030-t2mico/ruby/ext/date/date_core.a /tmp/d20240530-1369030-t2mico/ruby/ext/digest/bubblebabble/bubblebabble.a /tmp/d20240530-1369030-t2mico/ruby/ext/digest/digest.a /tmp/d20240530-1369030-t2mico/ruby/ext/digest/md5/md5.a /tmp/d20240530-1369030-t2mico/ruby/ext/digest/rmd160/rmd160.a /tmp/d20240530-1369030-t2mico/ruby/ext/digest/sha1/sha1.a /tmp/d20240530-1369030-t2mico/ruby/ext/digest/sha2/sha2.a /tmp/d20240530-1369030-t2mico/ruby/ext/erb/escape/escape.a /tmp/d20240530-1369030-t2mico/ruby/ext/etc/etc.a /tmp/d20240530-1369030-t2mico/ruby/ext/fcntl/fcntl.a /tmp/d20240530-1369030-t2mico/ruby/ext/fiddle/fiddle.a /tmp/d20240530-1369030-t2mico/ruby/ext/io/console/console.a /tmp/d20240530-1369030-t2mico/ruby/ext/io/nonblock/nonblock.a /tmp/d20240530-1369030-t2mico/ruby/ext/io/wait/wait.a /tmp/d20240530-1369030-t2mico/ruby/ext/json/generator/generator.a /tmp/d20240530-1369030-t2mico/ruby/ext/json/parser/parser.a /tmp/d20240530-1369030-t2mico/ruby/ext/monitor/monitor.a /tmp/d20240530-1369030-t2mico/ruby/ext/nkf/nkf.a /tmp/d20240530-1369030-t2mico/ruby/ext/objspace/objspace.a /tmp/d20240530-1369030-t2mico/ruby/ext/openssl/openssl.a /tmp/d20240530-1369030-t2mico/ruby/ext/pathname/pathname.a /tmp/d20240530-1369030-t2mico/ruby/ext/psych/psych.a /tmp/d20240530-1369030-t2mico/ruby/ext/pty/pty.a /tmp/d20240530-1369030-t2mico/ruby/ext/rbconfig/sizeof/sizeof.a /tmp/d20240530-1369030-t2mico/ruby/ext/ripper/ripper.a /tmp/d20240530-1369030-t2mico/ruby/ext/socket/socket.a /tmp/d20240530-1369030-t2mico/ruby/ext/stringio/stringio.a /tmp/d20240530-1369030-t2mico/ruby/ext/strscan/strscan.a /tmp/d20240530-1369030-t2mico/ruby/ext/syslog/syslog.a /tmp/d20240530-1369030-t2mico/ruby/ext/zlib/zlib.a /tmp/d20240530-1369030-t2mico/ruby/enc/libenc.a /tmp/d20240530-1369030-t2mico/ruby/enc/libtrans.a -lkompo -lruby-static -Wl,-Bstatic -lz -lrt -lgmp -lcrypt -lffi -lssl -lcrypto -lyaml -Wl,-Bdynamic -ldl -lm -lpthread -Wl,--end-group -o hello
/usr/lib/gcc/x86_64-pc-linux-gnu/13/../../../../x86_64-pc-linux-gnu/bin/ld: -lgmp が見つかりません: そのようなファイルやディレクトリはありません
/usr/lib/gcc/x86_64-pc-linux-gnu/13/../../../../x86_64-pc-linux-gnu/bin/ld: -lffi が見つかりません: そのようなファイルやディレクトリはありません
/usr/lib/gcc/x86_64-pc-linux-gnu/13/../../../../x86_64-pc-linux-gnu/bin/ld: -lssl が見つかりません: そのようなファイルやディレクトリはありません
/usr/lib/gcc/x86_64-pc-linux-gnu/13/../../../../x86_64-pc-linux-gnu/bin/ld: -lcrypto が見つかりません: そのようなファイルやディレクトリはありません
/usr/lib/gcc/x86_64-pc-linux-gnu/13/../../../../x86_64-pc-linux-gnu/bin/ld: -lyaml が見つかりません: そのようなファイルやディレクトリはありません
collect2: エラー: ld はステータス 1 で終了しました
...snip...
```

いったとおもったら、今度はライブラリ(**libgmp**、**libffi**、**openssl**、**libcrypt**、**libyaml**)のリンクができないようですね。
必要なライブラリはインストールされているのか見てみましょう。

```
$ eix -ICU 'dev-libs' -s 'gmp|libcrybp|libffi|libyaml|openssl'
[I] dev-libs/gmp
     Available versions:  6.3.0-r1(0/10.4)^d {+asm +cpudetection +cxx doc pic static-libs ABI_MIPS="n32 n64 o32" ABI_S390="32 64" ABI_X86="32 64 x32"}
     Installed versions:  6.3.0-r1(0/10.4)(02時44分45秒 2024年05月30日)(asm cpudetection cxx -doc -pic -static-libs ABI_MIPS="-n32 -n64 -o32" ABI_S390="-32 -64" ABI_X86="64 -32 -x32")
     Homepage:            https://gmplib.org/
     Description:         Library for arbitrary-precision arithmetic on different type of numbers

[I] dev-libs/libffi
     Available versions:  3.4.4-r4(0/8)^t ~3.4.6(0/8)^t {debug exec-static-trampoline pax-kernel static-libs test ABI_MIPS="n32 n64 o32" ABI_S390="32 64" ABI_X86="32 64 x32"}
     Installed versions:  3.4.4-r4(0/8)^t(02時43分18秒 2024年05月30日)(-debug -exec-static-trampoline -pax-kernel -static-libs -test ABI_MIPS="-n32 -n64 -o32" ABI_S390="-32 -64" ABI_X86="64 -32 -x32")
     Homepage:            https://sourceware.org/libffi/
     Description:         Portable, high level programming interface to various calling conventions

[I] dev-libs/libyaml
     Available versions:  0.2.2^t 0.2.5^t {doc static-libs test}
     Installed versions:  0.2.5^t(02時44分57秒 2024年05月30日)(-doc -static-libs -test)
     Homepage:            https://github.com/yaml/libyaml
     Description:         YAML 1.1 parser and emitter written in C

[I] dev-libs/openssl
     Available versions:  [M]1.0.2u-r1^td [M]1.1.1w(0/1.1)^t 3.0.11(0/3)^t 3.0.12(0/3)^t 3.0.13(0/3)^t ~3.0.13-r1(0/3)^t 3.0.13-r2(0/3)^t ~3.1.5-r1(0/3)^t ~3.1.5-r2(0/3)^t ~3.2.1-r1(0/3)^t ~3.2.1-r2(0/3)^t **3.3.0(0/3)^t {+asm bindist fips gmp kerberos ktls rfc3779 sctp sslv2 (+)sslv3 static-libs test tls-compression (+)tls-heartbeat vanilla verify-sig weak-ssl-ciphers ABI_MIPS="n32 n64 o32" ABI_S390="32 64" ABI_X86="32 64 x32" CPU_FLAGS_X86="sse2"}
     Installed versions:  3.0.13-r2(0/3)^t(02時43分56秒 2024年05月30日)(asm -fips -ktls -rfc3779 -sctp -static-libs -test -tls-compression -vanilla -verify-sig -weak-ssl-ciphers ABI_MIPS="-n32 -n64 -o32" ABI_S390="-32 -64" ABI_X86="64 -32 -x32" CPU_FLAGS_X86="sse2")
     Homepage:            https://www.openssl.org/
     Description:         Robust, full-featured Open Source Toolkit for the Transport Layer Security (TLS)
```

入ってますねこれ…もういちどよくコンパイルしてエラーとなった行をよくみてみましょう。
どのファイルも `.a` でおわってますね。
ということはこれらのファイルに静的コンパイルされたファイルがなさそうですね。
**Gentoo Linux** のパッケージマネージャー **portage** では
`static-libs` というオプションをつけてあげることにより静的コンパイルされます。

```
$ doas emerge -uDN world
$ kompo -e hello.rb
...snip...
$ ./hello
`RubyGems' were not loaded.
`error_highlight' was not loaded.
`did_you_mean' was not loaded.
`syntax_suggest' was not loaded.
hello, world
```

yatta!!ugoita!!!

## まとめ

**kompo** を動かしてみました。
コンパイルに時間がかかりますが、これで **Ruby** のプログラムを簡単に配ることができそうですね。
