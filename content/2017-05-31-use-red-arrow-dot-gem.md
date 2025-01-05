+++
path = "/blog/2017/05/31/use-red-arrow-dot-gem"
layout = "post"
title = "use red-arrow.gem"
date = 2017-05-31T23:27:57+09:00
comments = true
categories = "tech arrow diary"
+++

[こないだインストール](https://blog.katsyoshi.org/blog/2017/05/29/installing-apache-arrow/) した [Apache Arrow](https://arrow.apache.org/) がとりあえず [Ruby](https://github.com/red-data-tools/red-arrow) でうごくようになったのでメモ

## メモ
gemのインストールは前回のインストールを行なえば問題ないです。ですが、arrowを利用しようとすると失敗します。

```ruby
require "arrow"
/home/katsu/.rbenv/versions/2.4.1/lib/ruby/gems/2.4.0/gems/gobject-introspection-3.1.4/lib/gobject-introspection/loader.rb:37:in `require':GObjectIntrospection::RepositoryError::TypelibNotFound: Typelib file for namespace 'Arrow' (any version) not found
```

これは `GObjectIntrospection` の[ロードに失敗](https://github.com/red-data-tools/red-arrow/blob/master/lib/arrow.rb#L25)しているようです。
なので[ここ](http://www.clear-code.com/blog/2013/12/16.html)や[ここ](http://qiita.com/groonga/items/71b145b37d77bd160bf2)を参考に環境変数 `GI_TYPELIB_PATH` を設定すると読み込まれるようになり実行できます。

```bash
$ export GI_TYPELIB_PATH=/path/to/girepository-1.0
$ irb -rarrow
```

とやるとエラーがなくなります。

最後に[サンプル](https://github.com/red-data-tools/red-arrow/tree/master/example)を実行して確認しました!

```bash
% ruby write-file.rb
% ruby read-file.rb
========================================
record-batch[0]:
  uint8: [1, 2, 4, 8]
  uint16: [1, 2, 4, 8]
  uint32: [1, 2, 4, 8]
  uint64: [1, 2, 4, 8]
  int8: [1, -2, 4, -8]
  int16: [1, -2, 4, -8]
  int32: [1, -2, 4, -8]
  int64: [1, -2, 4, -8]
  float: [1.100000023841858, -2.200000047683716, 4.400000095367432, -8.800000190734863]
  double: [1.1, -2.2, 4.4, -8.8]
========================================
record-batch[1]:
  uint8: [2, 4, 8]
  uint16: [2, 4, 8]
  uint32: [2, 4, 8]
  uint64: [2, 4, 8]
  int8: [-2, 4, -8]
  int16: [-2, 4, -8]
  int32: [-2, 4, -8]
  int64: [-2, 4, -8]
  float: [-2.200000047683716, 4.400000095367432, -8.800000190734863]
  double: [-2.2, 4.4, -8.8]
```

## おわり

最初ろーどえらーでこまってた
