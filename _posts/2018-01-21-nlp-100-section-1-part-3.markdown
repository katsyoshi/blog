---
layout: post
title: "nlp 100 section 1 part 3"
date: 2018-01-21 10:23:02 +0900
comments: true
categories: rust tech diary
---

前回、[言語処理100本ノック](http://www.cl.ecei.tohoku.ac.jp/nlp100) の04までやったので05からやります。

### [05. ngram](http://www.cl.ecei.tohoku.ac.jp/nlp100/#sec05)
こいつはbi-gramを単語、文字二つを実装するひつようがあります

```rust
fn bigram(words: Vec<String>) -> Vec<String> {
    let mut bi: Vec<String> = Vec::new();
    let mut i = 0;

    loop {
        let w = i + 2;
        if w > words.len() { break; }
        bi.push(words[i..w].join(""));
        i += 1;
    }
    bi
}

fn main() {
    let words = "I am an NLPer".split(' ').map(|m| m.to_string()).collect::<Vec<String>>();
    println!("\n===word bi-gram");
    for word in bigram(words) {
        println!("{}", word);
    }

    let words = "I am an NLPer".chars().map(|m| m.to_string()).collect::<Vec<String>>();
    for word in bigram(words) {
        println!("\"{}\"", word);
    }

}
```

### [06. 集合](http://www.cl.ecei.tohoku.ac.jp/nlp100/#sec06)

これは単純に [`HashSet`](https://doc.rust-lang.org/beta/std/collections/struct.HashSet.html) を利用して、解決します。`HashSet` の差集合は [`difference`](https://doc.rust-lang.org/beta/std/collections/struct.HashSet.html#method.difference) を利用し、和集合は [`union`](https://doc.rust-lang.org/beta/std/collections/struct.HashSet.html#method.union) を、積集合は [`intersection`](https://doc.rust-lang.org/beta/std/collections/struct.HashSet.html#method.intersection) をそれぞれ利用します。また、特定の要素が含有していることを判定するには [`contains`](https://doc.rust-lang.org/beta/std/collections/struct.HashSet.html#method.contains) を利用して判定します。

```rust
use std::collections::HashSet;

fn bigram(words: Vec<String>) -> HashSet<String> {
    let mut bi: HashSet<String> = HashSet::new();
    let mut i = 0;

    loop {
        let w = i + 2;
        if w > words.len() { break; }
        bi.insert(words[i..w].join(""));
        i += 1;
    }
    bi
}

fn chars(s: String) -> Vec<String> {
    s.chars().map(|m| m.to_string()).collect::<Vec<String>>()
}

fn main() {
    let s1 = bigram(chars("paraparaparadise".to_string()));
    let s2 = bigram(chars("paragraph".to_string()));

    println!("===UNION===");
    for x in s1.union(&s2) {
        println!("{}", x);
    }

    println!("\n===DIFF===");
    println!("===s1 - s2===");
    for x in s1.difference(&s2) {
        println!("{}", x);
    }
    println!("===s2 - s1===");
    for x in s2.difference(&s1) {
        println!("{}", x);
    }

    println!("\n===intersection===");
    for x in s1.intersection(&s2) {
        println!("{}", x);
    }

    println!("\n===INCLUDE===");
    let se = "se";
    println!("s1: {}", s1.contains(se));
    println!("s2: {}", s2.contains(se));
}
```

### [07. テンプレートによる文生成](http://www.cl.ecei.tohoku.ac.jp/nlp100/#sec07)

これは [`format!`]() を使えば終りです。(問題意図ほんとこれなんか？)

```rust
fn string_template(x: i8, y: &str, z: f32) -> String {
    format!("{}時の{}は{}", x, y, z)
}

fn main() {
    let string = string_template(12, "気温", 22.5);
    println!("{}", string);
}
```

### [08. 暗号文](http://www.cl.ecei.tohoku.ac.jp/nlp100/#sec08)
ASCII以外の判定と、小文字のASCIIが判れば簡単です。

```rust
use std::ascii::AsciiExt;

fn cipher(src: &str) -> String {
    let chars = src.chars().collect::<Vec<char>>();
    let mut result: String = String::new();
    for c in chars {
        let s = if c.is_ascii() {
            let var: u8 = c as u8;
            match var {
                97 ... 122 => (219 - (var)) as char,
                _ => c,
            }
        } else {
            c
        };
        result.push(s);
    }
    result
}

fn main() {
    println!("{}", cipher("Today is fine."));
    println!("{}", cipher(&cipher("Today is fine.")));
}
```

### [09. Typoglycemia](http://www.cl.ecei.tohoku.ac.jp/nlp100/#sec09)

こちらは、 `Vec` に `shuffle` 的なものがないので、[`rand`](https://doc.rust-lang.org/rand) を呼び出して [`shuffle`](https://doc.rust-lang.org/rand/rand/trait.Rng.html#method.shuffle) を使います。

```rust
extern crate rand;
use rand::Rng;

fn words(src: &str) -> Vec<String> {
    let mut result: Vec<String> = Vec::new();

    for s in src.split(' ').collect::<Vec<&str>>() {
        let mut chars = s.chars().collect::<Vec<char>>();
        if chars.len() < 5 {
            result.push(s.to_string());
        } else {
            let last_index = chars.len() - 1;
            let first_char = chars[0];
            let last_char = chars[last_index];

            let rand_chars = &mut chars[1..last_index];
            shuffle(rand_chars);
            let mut rand_string = String::new();
            for c in rand_chars { rand_string.push(*c) }
            result.push(format!("{}{}{}", first_char, rand_string, last_char));
        }
    }
    result
}

fn shuffle(chars: &mut [char]) {
    rand::thread_rng().shuffle(chars);
}

fn main() {
    let paragraph = "I couldn't believe that I could actually understand what I was reading : the phenomenal power of the human mind .";

    for w in words(paragraph) {
        println!("{}", w);
    }
}
```

## おわり

ということで `Rust` で言語処理100本ノック1章をやってみました。
最近 `Ruby` しか書いていなかったので、新鮮で楽しいですね `Rust` 。
