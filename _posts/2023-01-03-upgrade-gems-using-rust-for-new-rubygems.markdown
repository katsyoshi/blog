---
layout: post
title: "Update rubygems using Rust for Ruby 3.2"
date: 2023-01-03 23:59:59 +0900
comments: true
categories: diary tech
---

[以前このブログ](/blog/2022/08/12/how-to-use-rust-in-ruby-gems/)で [**Rust**](https://www.rust-lang.org/) を利用して [**rubygems**](https://rubygems.org) を作成した
[**rust_uuid**](https://github.com/katsyoshi/rust_uuid) が [**Ruby 3.2** がリリース](https://www.ruby-lang.org/ja/news/2022/12/25/ruby-3-2-0-released/) によりコンパイルできなくなったのでその修正顛末。

## 環境

|system|failed version|succeed version|
|---|---|---|
|ruby|3.2.0|3.2.0|
|gem|3.4.1|3.4.2|
|bundler|2.3.19|2.4.2|
|rb-sys|0.9.29|0.9.53|
|magnus|0.3.2|0.4.4|

## what's happened?

**Ruby 3.2** が出てたのでアップデートして試してみるかーとおもいコマンドを実行!!!!

```bash
$ bundle exec rake build
.
.
...
error[E0425]: cannot find value `RUBY_ABI_VERSION` in the crate root
  --> /path/to/cargo/dir/registry/src/github.com-1ecc6299db9ec823/rb-sys-0.9.29/src/ruby_abi_version.rs:14:73
   |
14 | pub const __RB_SYS_RUBY_ABI_VERSION: std::os::raw::c_ulonglong = crate::RUBY_ABI_VERSION as _;
   |                                                                         ^^^^^^^^^^^^^^^^ not found in the crate root

For more information about this error, try `rustc --explain E0425`.
error: could not compile `rb-sys` due to previous error
warning: build failed, waiting for other jobs to finish...
gmake: *** [Makefile:564: target/release/librust_uuid.so] エラー 101
rake aborted!
Command failed with status (2): [/usr/bin/gmake...]
/path/to/rbenv/versions/3.2.0/bin/bundle:25:in `load'
/path/to/rbenv/versions/3.2.0/bin/bundle:25:in `<main>'
Tasks: TOP => build => compile => compile:x86_64-linux => compile:rust_uuid:x86_64-linux => copy:rust_uuid:x86_64-linux:3.2.0 => tmp/x86_64-linux/rust_uuid/3.2.0/rust_uuid.so
(See full trace by running task with --trace)
bundle exec rake build  63.55s user 10.61s system 541% cpu 13.705 total
```

おーなるほどなるほど、 **cargo** の [**rb-sys**](https://github.com/oxidize-rb/rb-sys) が **Ruby 3.2** に対応していないバージョンつかってるんだなと理解。**cargo** を更新っするぞい。

## うpだて **cargo** ぱっけーじ

ということで `cargo build` が通るようにパッケージを更新するぞい。
**cargo** も **bundler** 同様、 `cargo update` でいい感じにアップデートしてくれます。

```bash
$ cd ext/rust_uuid
$ cargo update
$ cargo build
   Compiling magnus v0.3.2
error[E0432]: unresolved import `crate::ruby_sys::ruby_rstring_consts`
  --> /path/to/cargo/dir/registry/src/github.com-1ecc6299db9ec823/magnus-0.3.2/src/r_string.rs:23:47
   |
23 | use crate::ruby_sys::{rb_str_to_interned_str, ruby_rstring_consts::RSTRING_EMBED_LEN_SHIFT};
   |                                               ^^^^^^^^^^^^^^^^^^^ could not find `ruby_rstring_consts` in `ruby_sys`

error[E0599]: no variant or associated item named `RSTRING_EMBED_LEN_MASK` found for enum `ruby_rstring_flags` in the current scope
   --> /path/to/cargo/dir/registry/src/github.com-1ecc6299db9ec823/magnus-0.3.2/src/r_string.rs:368:38
    |
368 |             f &= ruby_rstring_flags::RSTRING_EMBED_LEN_MASK as VALUE;
    |                                      ^^^^^^^^^^^^^^^^^^^^^^ variant or associated item not found in `ruby_rstring_flags`

error[E0599]: no variant or associated item named `RSTRING_EMBED_LEN_MASK` found for enum `ruby_rstring_flags` in the current scope
   --> /path/to/cargo/dir/registry/src/github.com-1ecc6299db9ec823/magnus-0.3.2/src/r_string.rs:968:42
    |
968 |                 f &= ruby_rstring_flags::RSTRING_EMBED_LEN_MASK as VALUE;
    |                                          ^^^^^^^^^^^^^^^^^^^^^^ variant or associated item not found in `ruby_rstring_flags`

Some errors have detailed explanations: E0432, E0599.
For more information about an error, try `rustc --explain E0432`.
error: could not compile `magnus` due to 3 previous errors
```

`cargo update` しましたが、やっぱり駄目でしたね。今度は [**magnus**](https://github.com/matsadler/magnus) が駄目そう。
`Cargo.toml` を見てみると、 `magnus = { version = "0.3", features = ["rb-sys-interop"] }` と指定してあり、 version 0.3 系が駄目そうということが類推されます。ということで公式ページを見ると新しいバージョンが出ているのでこちらにします。

```diff
--- a/ext/rust_uuid/Cargo.toml
+++ b/ext/rust_uuid/Cargo.toml
@@ -10,7 +10,7 @@ crate-type = ["cdylib"]
 [dependencies]
 rb-sys = "0.9"
 rb-allocator = "0.9"
-magnus = { version = "0.3", features = ["rb-sys-interop"] }
+magnus = { version = "0.4", features = ["rb-sys-interop"] }

 [dependencies.uuid]
 version = "1.1.2"
```

再度ビルド!

```bash
$ cargo update
$ cargo build
   Compiling rust_uuid v0.1.0 (/home/katsyoshi/Program/Ruby/rust_uuid/ext/rust_uuid)
    Finished dev [unoptimized + debuginfo] target(s) in 0.18s
```

おおっ通りました!やったね!

## build

ということ `cargo build` 通ったので `gem install` しましよう。

```bash
$ cd /path/to/rust_uuid
$ bundle exec rake build
cd tmp/x86_64-linux/rust_uuid/3.2.0
/usr/bin/gmake
generating target/release/librust_uuid.so (release)
cargo rustc --target-dir target --manifest-path ../../../../ext/rust_uuid/Cargo.toml --lib --release -- -C linker=gcc -L native=/path/to/rbenv/versions/3.2.0/lib -L native=/path/to/rbenv/versions/3.2.0/lib -L native=/home/katsyoshi/.local/lib:-L/home/katsyoshi/.local/lib: -C link-arg=-lm -l pthread
   Compiling libc v0.2.139
   Compiling proc-macro2 v1.0.49
   Compiling quote v1.0.23
   Compiling unicode-ident v1.0.6
   Compiling clang-sys v1.4.0
   Compiling regex-syntax v0.6.28
   Compiling syn v1.0.107
   Compiling rb-sys-env v0.1.1
   Compiling libloading v0.7.4
   Compiling nom v7.1.2
   Compiling aho-corasick v0.7.20
   Compiling magnus v0.4.4
   Compiling bindgen v0.60.1
   Compiling getrandom v0.2.8
   Compiling uuid v1.2.2
   Compiling cexpr v0.6.0
   Compiling regex v1.7.0
   Compiling magnus-macros v0.3.0
   Compiling rb-sys-build v0.9.53
   Compiling rb-sys v0.9.53
   Compiling rb-allocator v0.9.6
   Compiling rust_uuid v0.1.0 (/path/to/rust_uuid/ext/rust_uuid)
    Finished release [optimized] target(s) in 12.03s
cd -
mkdir -p tmp/x86_64-linux/stage/lib/rust_uuid
/usr/bin/gmake install target_prefix=
generating target/release/librust_uuid.so (release)
cargo rustc --target-dir target --manifest-path ../../../../ext/rust_uuid/Cargo.toml --lib --release -- -C linker=gcc -L native=/path/to/rbenv/versions/3.2.0/lib -L native=/path/to/rbenv/versions/3.2.0/lib -L native=/home/katsyoshi/.local/lib:-L/home/katsyoshi/.local/lib: -C link-arg=-lm -l pthread
    Finished release [optimized] target(s) in 0.02s
installing rust_uuid.so to /path/to/rust_uuid/lib/rust_uuid
/usr/bin/install -c -m 0755 rust_uuid.so /path/to/rust_uuid/lib/rust_uuid
cp tmp/x86_64-linux/rust_uuid/3.2.0/rust_uuid.so tmp/x86_64-linux/stage/lib/rust_uuid/rust_uuid.so
rake aborted!
Running `gem build -V /path/to/rust_uuid/rust_uuid.gemspec` failed with the following output:

WARNING:  description and summary are identical
WARNING:  open-ended dependency on benchmark-ips (>= 0, development) is not recommended
  use a bounded requirement, such as '~> x.y'
WARNING:  open-ended dependency on rake (>= 13.0.0, development) is not recommended
  if rake is semantically versioned, use:
    add_development_dependency 'rake', '~> 13.0', '>= 13.0.0'
WARNING:  open-ended dependency on rake-compiler (>= 0, development) is not recommended
  use a bounded requirement, such as '~> x.y'
WARNING:  open-ended dependency on rb_sys (>= 0, development) is not recommended
  use a bounded requirement, such as '~> x.y'
WARNING:  open-ended dependency on rspec (>= 0, development) is not recommended
  use a bounded requirement, such as '~> x.y'
WARNING:  See https://guides.rubygems.org/specification-reference/ for help
ERROR:  While executing gem ... (Gem::InvalidSpecificationException)
    You have specified rust based extension, but Cargo.lock is not part of the gem files. Please run `cargo generate-lockfile` or any other command to generate Cargo.lock and ensure it is added to your gem files section in gemspec.

/path/to/rbenv/versions/3.2.0/bin/bundle:25:in `load'
/path/to/rbenv/versions/3.2.0/bin/bundle:25:in `<main>'
Tasks: TOP => build
(See full trace by running task with --trace)
bundle exec rake build  32.87s user 4.33s system 291% cpu 12.741 total
```

なるほど？ `Cargo.lock` ファイルもあるし問題なさそうだな。よくわからんので **rubygems** のリポジトリでエラーメッセージを探してみます。

すると[以下のようなコード](https://github.com/rubygems/rubygems/blob/05df95280e5f25299f4130a25dc44100226c2235/lib/rubygems/specification_policy.rb#L467-L474)が


```ruby
  def validate_rust_extensions(builder) # :nodoc:
    rust_extension = @specification.extensions.any? {|s| builder.builder_for(s).is_a? Gem::Ext::CargoBuilder }
    missing_cargo_lock = !@specification.files.include?("Cargo.lock")

    error <<-ERROR if rust_extension && missing_cargo_lock
You have specified rust based extension, but Cargo.lock is not part of the gem files. Please run `cargo generate-lockfile` or any other command to generate Cargo.lock and ensure it is added to your gem files section in gemspec.
    ERROR
  end
```

なろほどなろほど、プロジェクトの root 直下に置いておく必要があるのね。でもそのファイルどういうものなの？

## gem, bundler update

ってなわけで、今度は正式に **rubygems** で正式にサポートされ、 **bundler** でも **gem** を作成するときにも `bundle gem --ext=rust gem_name` で[スケルトンが作成](https://github.com/rubygems/rubygems/pull/6149)されるようになりました。ということでこれを利用して `Cargo.toml` を作ってみましょう

```bash
$ bundle gem --mit --ext=rust gem_name
ERROR: "bundle gem" was called with arguments ["rust", "gem_name"]
Usage: "bundle gem NAME [OPTIONS]"
```

ok, ok, これは **bundler** のバージョンが古いな

```bash
$ bundle version
Bundler version 2.3.19 (2022-07-27 commit 4f496f93e6)
```

さっきの **PR** は入ったのは **Ruby 3.2** リリース直前なのでまだ入ってないよなとおもったけど、実際は `Gemfile.lock` の `BUNDLED WITH` が `2.3.19` を指定してるだけだったのです。
なので対象の 2 行を削除して `bundle update` を実行し、続いて **rust** サポートした **gem** を生成して、 `Cargo.toml` を見てみましょう。

```bash
$ bundle gem --mit --ext=rust gem_name
Creating gem 'gem_name'...
MIT License enabled in config
Changelog enabled in config
Initializing git repo in /path/to/rust_uuid/gem_name
      create  gem_name/Gemfile
      create  gem_name/lib/gem_name.rb
      create  gem_name/lib/gem_name/version.rb
      create  gem_name/sig/gem_name.rbs
      create  gem_name/gem_name.gemspec
      create  gem_name/Rakefile
      create  gem_name/README.md
      create  gem_name/bin/console
      create  gem_name/bin/setup
      create  gem_name/.gitignore
      create  gem_name/.github/workflows/main.yml
      create  gem_name/LICENSE.txt
      create  gem_name/CHANGELOG.md
      create  gem_name/Cargo.toml
      create  gem_name/ext/gem_name/Cargo.toml
      create  gem_name/ext/gem_name/extconf.rb
      create  gem_name/ext/gem_name/src/lib.rs
Gem 'gem_name' was successfully created. For more information on making a RubyGem visit https://bundler.io/guides/creating_gem.html
$ cat Cargo.toml
# This Cargo.toml is here to let externals tools (IDEs, etc.) know that this is
# a Rust project. Your extensions depedencies should be added to the Cargo.toml
# in the ext/ directory.

[workspace]
members = ["./ext/gem_name"]
resolver = "2"
```

というようなファイルが得られるので真似して作成しちゃいましょう。

```toml
[workspace]
members = ["./ext/rust_uuid"]
resolver = "2"
```

そうしたらもう一度ビルドしてしまいます!

```bash
$ cargo generate-lockfile
$ git add Cargo.toml Carog.lock
$ bundle exec rake build
/usr/bin/gmake install target_prefix=
generating target/release/librust_uuid.so (release)
cargo rustc --target-dir target --manifest-path ../../../../ext/rust_uuid/Cargo.toml --lib --release -- -C linker=gcc -L native=/path/to/rbenv/versions/3.2.0/lib -L native=/path/to/rbenv/versions/3.2.0/lib -L native=/home/katsyoshi/.local/lib:-L/home/katsyoshi/.local/lib: -C link-arg=-lm -l pthread
   Compiling rust_uuid v0.1.0 (/path/to/rust_uuid/ext/rust_uuid)
    Finished release [optimized] target(s) in 0.26s
installing rust_uuid.so to /path/to/rust_uuid/lib/rust_uuid
/usr/bin/install -c -m 0755 rust_uuid.so /path/to/rust_uuid/lib/rust_uuid
cp tmp/x86_64-linux/rust_uuid/3.2.0/rust_uuid.so tmp/x86_64-linux/stage/lib/rust_uuid/rust_uuid.so
rust_uuid 0.1.0 built to pkg/rust_uuid-0.1.0.gem.
$ bundle exec rake install
/usr/bin/gmake install target_prefix=
generating target/release/librust_uuid.so (release)
cargo rustc --target-dir target --manifest-path ../../../../ext/rust_uuid/Cargo.toml --lib --release -- -C linker=gcc -L native=/path/to/rbenv/versions/3.2.0/lib -L native=/path/to/rbenv/versions/3.2.0/lib -L native=/home/katsyoshi/.local/lib:-L/home/katsyoshi/.local/lib: -C link-arg=-lm -l pthread
    Finished release [optimized] target(s) in 0.03s
installing rust_uuid.so to /path/to/rust_uuid/lib/rust_uuid
/usr/bin/install -c -m 0755 rust_uuid.so /path/to/rust_uuid/lib/rust_uuid
cp tmp/x86_64-linux/rust_uuid/3.2.0/rust_uuid.so tmp/x86_64-linux/stage/lib/rust_uuid/rust_uuid.so
$ bundle exec ruby -rrust_uuid -e 'puts RustUUID.v4'
2eb053de-8ae7-4669-853b-95f06c872300
```

ようやっと通ったああぁあ!!!!

## conclusion

実際は11月の末あたりに **head** でコンパイルできないなあと気がついていたのですが、そのうち直るやろと思っててなにもしなかったのです。いざ **Ruby 3.2** がリリースされたときに試して動かなかったのでやっと対応してみました。

ということで正式に **Rust** がサポートされるようになったのでまた YARUKI がでてきますね。

それはそうと今度は、[**jekyll**](https://jekyllrb.com) が **Ruby 3.2** で動かなくなった。
