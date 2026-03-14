---
layout: post
title: "Ruby::Boxを使っただけなのに"
date: 2026-03-13 11:55:22
comments: true
categories: diary tech ruby ruby::box
---

[ruby-4.0](https://www.ruby-lang.org/ja/news/2025/12/25/ruby-4-0-0-released/) 出ましたね。

待望の新機能、 [`Ruby::Box`](https://docs.ruby-lang.org/en/master/Ruby/Box.html) を使っていますか？
個人プロジェクトで利用しているのでその利用方法について書いていきますよ。

## Testで安全にFFI(fiddle)を使いたい!!!!

`Ruby::Box` はかつて `Namespace` と呼ばれていたものです。ということは _コンパイラー_ 書いてる人にとってテストが安全にできるじゃないですか。
コンパイラーで生成したバイナリを安全に分離できるじゃないですか。
_テスト_ 向けのロード用ファイルを作ります。

```ruby
require "fiddle/import"

module Fibonacci
  extend Fiddle::Importer
  dlload "./libhoge.so"
  extern "int fibo(int)"
end
```

テスト側では以下のようにしますね。

```ruby
require "test/unit"
class TestFibo < Test::Unit::TestCase
  def test_load_fiddle
    omit('"Ruby::Box" needs RUBY_BOX=1') unless ENV["RUBY_BOX"] == "1"
    box = Ruby::Box.new
    box.require "./load_so"
    assert_equal(box::Fibonacci.fibo(10), 55)
  end
end
```

では実行してみましょう

```bash
$ RUBY_BOX=1 ruby test_fibo.rb
ruby: warning: Ruby::Box is experimental, and the behavior may change in the future!
See https://docs.ruby-lang.org/en/4.0/Ruby/Box.html for known issues, etc.
Loaded suite test_fibo
Started
Finished in 0.007523671 seconds.
-----------------------------------------------------------------------------------------------
1 tests, 1 assertions, 0 failures, 0 errors, 0 pendings, 0 omissions, 0 notifications
100% passed
-----------------------------------------------------------------------------------------------
132.91 tests/s, 132.91 assertions/s
```

テスト通りましたね!やった!!便利!!!
`Ruby::Box` つかうには `RUBY_BOX=1` が必要ですね。なので無い場合のガードとして `omit...` を入れています。
この確認もしますかね。

```bash
$ ruby test_fibo.rb
Loaded suite test_fibo
Started
O
===============================================================================================
Omission: "Ruby::Box" needs RUBY_BOX=1 [test_load_fiddle(TestFibo)]
test_fibo.rb:5:in 'TestFibo#test_load_fiddle'
===============================================================================================
Finished in 0.000660511 seconds.
-----------------------------------------------------------------------------------------------
1 tests, 0 assertions, 0 failures, 0 errors, 0 pendings, 1 omissions, 0 notifications
0% passed
-----------------------------------------------------------------------------------------------
1513.98 tests/s, 0.00 assertions/s
```

そうしたら `omit` しないようにしますかね

```bash
$ ruby test_fibo.rb
Loaded suite test_fibo
Started
E
===============================================================================================
Error: test_load_fiddle(TestFibo): RuntimeError: Ruby Box is disabled. Set RUBY_BOX=1 environment variable to use Ruby::Box.
test_fibo.rb:5:in 'Ruby::Box#initialize'
test_fibo.rb:5:in 'Class#new'
test_fibo.rb:5:in 'TestFibo#test_load_fiddle'
     2:
     3: class TestFibo < Test::Unit::TestCase
     4:   def test_load_fiddle
  => 5:     box = Ruby::Box.new
     6:     box.require "./load_so"
     7:     assert_equal 55, box::Fibonacci.fibo(10)
     8:   end
===============================================================================================
Finished in 0.000693731 seconds.
-----------------------------------------------------------------------------------------------
1 tests, 0 assertions, 0 failures, 1 errors, 0 pendings, 0 omissions, 0 notifications
0% passed
-----------------------------------------------------------------------------------------------
1441.48 tests/s, 0.00 assertions/s
```

とこのようになりますね。`omit` 入れることで `error` にならなくなりますね。便利!!!!
ということで `Ruby::Box` の便利な面をみてきました。こうやって安全に利用できるようになりましたね。

## Ruby::Boxでネームスペースを区切ろう

コンパイラーつくってると生成したバイナリをロードして読み込みたいんですよ。
でもテストはプロセス1つじゃないですか？
`libbinA.so` と `libbinB.so` を同時に読みたいということはとくになく、
`libbinA.so` は一つで読みとりたい。
`libbinB.so` も `libbinA.so` とは別に読みとりたいことがあるじゃないですか。
このとき、 `Ruby::Box` でそれぞれ読めばいいとおもうじゃん？ということでやってみますね。
といってもいきなり分けてやるよりもどうせネームスペースがわかれるので同じファイルで実験してみましょう。

```ruby
require "test/unit"
class TestFibo < Test::Unit::TestCase
  def test_load_fiddle
    omit('"Ruby::Box" needs RUBY_BOX=1') unless ENV["RUBY_BOX"] == "1"
    box = Ruby::Box.new
    box.require "./load_so"
    assert_equal(box::Fibonacci.fibo(10), 55)
  end
  
  def test_load_fiddle_2
    omit('"Ruby::Box" needs RUBY_BOX=1') unless ENV["RUBY_BOX"] == "1"
    box = Ruby::Box.new
    box.require "./load_so"
    assert_equal(box::Fibonacci.fibo(10), 55)
  end
end
```


実行〜〜


```bash
$ RUBY_BOX=1 ruby test_fibo.rb
ruby: warning: Ruby::Box is experimental, and the behavior may change in the future!
See https://docs.ruby-lang.org/en/4.0/Ruby/Box.html for known issues, etc.
Loaded suite test_fibo
Started
E
===============================================================================================
Error: test_load_fiddle_2(TestFibo)
===============================================================================================
Error: test_load_fiddle_2(TestFibo)Finished in 0.011718944 seconds.
-----------------------------------------------------------------------------------------------
1 tests, 1 assertions, 0 failures, 2 errors, 0 pendings, 0 omissions, 0 notifications
100% passed
-----------------------------------------------------------------------------------------------
```

**WTF** 落ちるなんで!!!？？？？よくみたら `2 errors` なのに `100% passed` ってなんぞ!？
そもそも表示も崩壊してんじゃねーか。
ということでエラー内容みますかね。

```log
load_so.rb:1:in 'Kernel#require': cannot load such file -- fiddle/import (LoadError)
```

**WTF** 標準ライブラリーが見えねえってどういうことだよ!!!

これから真相を探るため `Ruby::Box` を探す旅がはじまります。
