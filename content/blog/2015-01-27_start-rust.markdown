+++
layout: post
title: "Start Rust by Example"
date: 2015-01-27 07:10:29 +0900
comments: true
categories: diary tech linux
+++

年ぁけたので、新しいプログラミング言語を覚えようと思い[Rust Lang](http://www.rust-lang.org)を始めました。

チュートリアルとして[Rust bye Example](http://rustbyexample.com)を[やっていって](https://github.com/katsyoshi/rustbyexamle)ます。

## 気が付いた点

Rustという言語は変更が多いらしいのでここでは、Chap.1-14(2015/01/25時点)までで気がついた点で書きます。 まず、今回環境を下に示します。

```
$ rustc --version
rustc 1.0.0-nightly (29bd9a06e 2015-01-20 23:03:09 +0000)
```

### i -> is

まず、[Chap. 2のコード](https://github.com/katsyoshi/rustbyexample/blob/master/2.FormattedPrint/FormattedPrint.rs#L9)で`integer`のsuffixが`is`に変更されています。

```
FormattedPrint.rs:9:69: 9:71 warning: the `i` suffix on integers is deprecated; use `is` or one of the fixed-sized suffixes
FormattedPrint.rs:9     println!("{} of {:b} people know binary, the other half don't", 1i, 2i);
                                                                                        ^~
```

### range(a, b) –> (a..b)

つぎに、[Chap. 10のコード](https://github.com/katsyoshi/rustbyexample/blob/master/10.ForAndRange/ForAndRange.rs#L2)で`range(a, b)`がunstableになっているので`(a..b)`に変更しました。

```
ForAndRange.rs:2:14: 2:19 warning: use of unstable item: will be replaced by range notation, #[warn(unstable)] on by default
ForAndRange.rs:2     for n in range(1u32, 101) {
                              ^~~~~
```


## おわり

いま確認してたらそもそもRust by Example自体が変更されてた…

ちゃんちゃん
