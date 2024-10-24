+++
layout: post
title: "happy new year and new language"
date: 2018-01-16 00:32:16 +0900
comments: true
categories: diary tech
+++

いまさらですが、あけましておめでとうございます。

## happy new language

年開けから新しくプログラミング言語(Rust)始めました。(ってわけでもない)

雑に [rust book 2nd edition](https://doc.rust-lang.org/book/second-edition/) を一通り読み終えたので、
[言語処理100本ノック](http://www.cl.ecei.tohoku.ac.jp/nlp100) をやりはじめました。
とりあえず第1章が終ったので、メモとしてのこします。
基本的に `Rust` (に限らず)でやりたいことがとくにないので
[ちょうどよさそうな勉強](http://yamasy1549.hateblo.jp/entry/2017/12/28/222631) として自然言語処理を選択しています。

## 躓いたところ

躓いていないところがないです。
まあやっていくうちに以下二つは常に書いておくと楽になるかなと。

```rust
// 1文字毎
fn chars(string: &str) -> Vec<char> {
    string.chars().collect::<Vec<char>>()
}

// 1単語毎
fn words(sentence: &str) -> Vec<&str> {
    sentence.split(' ').collect::<Vec<&str>>()
}
```

### [00. 文字列の逆順](http://www.cl.ecei.tohoku.ac.jp/nlp100/#sec00)
これは簡単で(でもなかった)、1文字ずつ分解して[反対化](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.rev)したあと `String` にします。

```rust
fn reverse_string(string: &str) {
    println!("{}", string.chars().rev().collect::<String>());
}
```

### [01. 「パタトクカシーー」](http://www.cl.ecei.tohoku.ac.jp/nlp100/#sec01)
これも簡単で、1文字ずつ分解して抜き出します。(絶対違う)

```rust
fn main() {
    let c = "パタトクカシーー".chars().collect::<Vec<char>>();
    println!("{}{}{}{}", c[0], c[2], c[4], c[6]);
}
```

## おわり
とりあえず2問解いてみたけど、自然言語処理 && `Rust` 難しい。
