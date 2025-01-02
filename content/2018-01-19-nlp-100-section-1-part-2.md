+++
path = "/blog/2018/01/19/nlp-100-section-1-part-2"
layout = "post"
title = "NLP100本ノック section 1 part 2"
date = 2018-01-19T20:05:49+09:00
comments = true
categories = "rust tech diary"
+++

前回、[言語処理100本ノック](http://www.cl.ecei.tohoku.ac.jp/nlp100) の01までやったので02からやっていきます。
## `extern crate nlp100;`
ってやれるように [`Cargo`](https://github.com/katsyoshi/zatsu/tree/master/rust/nlp100) を作成
```
cargo new nlp100
```

### [02. 「パトカー」+「タクシー」=「パタトクカシーー」](http://www.cl.ecei.tohoku.ac.jp/nlp100/#sec02)
これはムズカシイので素直に [`zip`](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.zip) を利用する

```rust
fn concat(t: (Vec<char>, Vec<char>)) {
    let (f, s) = t;
    let mut r = String::new();

    for (x, y) in f.iter().zip(s.iter()) {
        r.push_str(&format!("{}{}", x, y));
    }
    println!("{}", r);
}

fn main() {
    let p: Vec<char> = String::from("パトカー").chars().collect();
    let t: Vec<char> = String::from("タクシー").chars().collect();
    let f = (p, t);
    concat(f);
}
```

### [03. 円周率](http://www.cl.ecei.tohoku.ac.jp/nlp100/#sec03)
この問題は思い切り勘違いしてたので、「これのどこが円周率なの？」って思ってました。こいつは [`Regex`](https://doc.rust-lang.org/regex/regex/index.html) を用いて単語毎に分解、単語毎に文字数数えて解決してます。

```rust
fn char_count_list(w: &str) -> Vec<usize> {
    Regex::new(r"\W+").unwrap().split(w).map(|m| m.len()).collect()
}

fn main() {
    let pi = char_count_list("Now I need a drink, alcoholic of course, after the heavy lectures involving quantum mechanics.");
    println!("{:?}", pi);
}
```

### [04. 元素記号](http://www.cl.ecei.tohoku.ac.jp/nlp100/#sec04)
これは、英語版「水兵リーベー僕の船」ですので条件に合うときだけ1文字に変更します。

```rust
use std::collections::HashMap;

fn main() {
    let atomic_words: Vec<&str> = "Hi He Lied Because Boron Could Not Oxidize Fluorine. New Nations Might Also Sign Peace Security Clause. Arthur King Can.".split(' ').collect();
    let mut atomic_table = HashMap::new();
    for (i, a) in atomic_words.iter().enumerate() {
        let chars = a.chars().map(|v| v.to_string()).collect::<Vec<String>>();
        let r = match i {
            0 | 4...8 | 14...15 | 18 => chars[0].to_string(),
            _ => chars[0..2].join(""),
        };
        atomic_table.insert(r, i + 1);
    }

    for (k, v) in &atomic_table {
        println!("{}: {}", k, v);
    }
}
```

## おわり

やっぱり難しいのを実感
