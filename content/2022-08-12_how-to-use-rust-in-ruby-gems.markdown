+++

title = "Ruby gem で Rust をつかって爆速にしたい!!!!!!11"
date = 2022-08-12
comments = true
categories = "rust"
+++

[Ruby Gems](https://rubygems.org) で [Rust](https://www.rust-lang.org) が [Native として利用可能になった](https://github.com/rubygems/rubygems/pull/5175) のでとりあえず [`UUIDv4`](https://www.rfc-editor.org/rfc/rfc4122.html) を生成してみた。

### リポジトリ
[![katsyoshi/rust_uuid - GitHub](https://gh-card.dev/repos/katsyoshi/rust_uuid.svg)](https://github.com/katsyoshi/rust_uuid)

## 準備

Ruby 側の `gem` に Rust を利用する準備として [`rb_sys`](https://github.com/oxidize-rb/rb-sys) と [`rake-compiler`](https://github.com/rake-compiler/rake-compiler) を利用します。この二つの `gem` は native compile するためにインストールしておきます。
Rust 側から Ruby へ関数を公開するために [`rb-sys`](https://github.com/oxidize-rb/rb-sys) と [`magnus`](https://github.com/matsadler/magnus) を利用します。

## gem install

とりあえず `cargo` で Rust のパッケージを作って Rust を書いてみます。

```bash
> bundle gem rust_uuid --mit --ext rust_uuid # --ext を指定してnative build する gem を作成
> cd rust_uuid # 作成した gem のディレクトリへ移動
> cd ext/rust_uuid # ビルドするディレクトリへ移動
> cargo init . --lib # cargo を初期化
> rm -f *.c *.h # C のファイルが生成されるので削除
> cargo add rb-sys rb-allocator
> cargo add magnus --features rb-sys-interop
> cargo add uuid --features v4 # uuid v4 を指定
```

`ext/rust_uuid/extconf.rb` を以下のように編集します。

```diff
@@ -1,5 +1,6 @@
 # frozen_string_literal: true
 
 require "mkmf"
+require "rb_sys/mkmf"
 
-create_makefile("rust_uuid/rust_uuid")
+create_rust_makefile("rust_uuid/rust_uuid")
```

次に `ext/rust_uuid/src/lib.rs` を以下の様に変更します。

```rust
use magnus::{define_module, function, prelude::*, Error};
use rb_allocator::ruby_global_allocator;
use uuid::Uuid;

ruby_global_allocator!();

// UUIDv4 を文字列として公開
fn v4() -> String {
    Uuid::new_v4().to_string()
}

#[magnus::init]
fn init() -> Result<(), Error> {
    let module = define_module("RustUUID")?;
    // RustUUID.v4 と利用するようにシングルトンメソッドを定義
    module.define_singleton_method("v4", function!(v4, 0))?;
    Ok(())
}
```

これまでできたら一旦 Rust をコンパイルしましょう。

```bash
> cd ext/rust_uuid
> cargo build
> git add .
> rake build
.... # install cargo dependencies and packages
cp: '/home/katsyoshi/Program/Ruby/rust_uuid/tmp/x86_64-linux/rust_uuid/3.1.2/target/release/librust_uuid.so' を stat できません: そのようなファイルやディレクトリはありません
gmake: *** [Makefile:551: foo_bar.so] エラー 1
rake aborted!
Command failed with status (2): [/usr/bin/gmake...]

Tasks: TOP => build => compile => compile:x86_64-linux => compile:foo_bar:x86_64-linux => copy:rust_uuid:x86_64-linux:3.1.2 => tmp/x86_64-linux/rust_uuid/3.1.2/rust_uuid.so
```

とエラーになります。
これは `ext/rust_uuid/Cargo.toml` の設定が足りていません。そこで以下を追加してみてください。

```toml
[lib]
crate-type = ["cdylib"]
```

追加したら `gem` をビルド&&インストール&&試してみましょう!

```bash
> rake install
....
> ruby -rrust_uuid -e 'puts RustUUID.v4'
2be6f4d2-200b-4d08-9a1a-11fa523b316b
```

## べんちまーく

以下、 `SecureRandom.uuid` との比較用のベンチマークコードを示します。

```ruby
require "benchmark/ips"
require "securerandom"
require "rust_uuid"

Benchmark.ips do |x|
  x.report("standard") { SecureRandom.uuid }
  x.report("rust lib") { RustUUID.v4 }
  x.compare!
end
```

### 結果発表〜

Rust を利用することでだいたい 5 倍ほど速くなっています。

```console
> ruby bentimark.rb
Warming up --------------------------------------
            standard    36.437k i/100ms
            rust lib   177.585k i/100ms
Calculating -------------------------------------
            standard    365.407k (± 1.4%) i/s -      1.858M in   5.086491s
            rust lib      1.793M (± 1.8%) i/s -      9.057M in   5.053179s

Comparison:
            rust lib:  1792925.9 i/s
            standard:   365407.3 i/s - 4.91x  (± 0.00) slower

ruby bentimark.rb  9.88s user 4.30s system 99% cpu 14.175 total
```

TOO HAYAI

## まとめ
簡単に Rust を利用して速くしてみました。
思った以上に速くなっていたので重い処理をする場合に `C` や `C++` 以外でも簡単に利用できるようになって
選択肢が増えたのはよいことでした。

実はこの `uuid` crate の features に `fast-rng` を追加すると 10 倍速くなるんですが、 ruby 側の終了時に `SEGV` してしまうので載せていないです。 `SEGV` しないように原因を調査などはまた今度。
