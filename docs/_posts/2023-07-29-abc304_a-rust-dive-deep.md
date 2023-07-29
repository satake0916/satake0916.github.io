---
layout: post
title:  ABC304 A問題をRustで詳しめに解く
subtitle: イテレーションを活用する
date:   2023-07-29 15:55:14 +09:00
categories: rust
latex: true
---

[ABC304 A - First Player][problem]

### 問題文

人 $1$ 、人 $2$ 、$\ldots$ 、人 $N$ と番号付けられた $N$ 人が、この順番で時計回りに円卓に座っています。
特に、時計回りで人 $N$ の次の位置には人 $1$ が座っています。
$i = 1, 2, \ldots, N$ について、人 $i$ の名前は $S_i$ 、年齢は $A_i$ です。
ここで、異なる $2$ 人が同じ名前や同じ年齢であることはありません。
年齢が最も小さい人を起点として、座っている位置の時計回りの順番で、$N$ 人全員の名前を出力してください。

### 制約

略

### 入力

$N$  
$S_1$ $A_1$  
$\vdots$  
$S_N$ $A_N$

## 解法

### 標準入力

AtCoderなら`input!`が使えます。

```rust
input! {
  n: usize,
  p: [(String, u32); n],
}
```

宣言とともに配列で受け取れて綺麗です。

### ロジック1

とりあえず年齢が最も低い人のインデックスを取得したいです。
まずは多言語でもやるようにforループをiを0からnまで回してみます。

```rust
let mut min_age = p[0].1;
let mut min_age_index = 0;
for i in 0..n {
  if p[i].1 < min_age {
    min_age = p[i].1;
    min_age_index = i;
  }
}
```

`p[i]`とアクセスしているのがなんだか野暮な感じがしますね。
`emumerate`を使えばindexと要素を同時に得ることもできます。

```rust
let mut min_age = p[0].1;
let mut min_age_index = 0;
for (i, value) in p.iter().enumerate(){
  if value.1 < min_age {
    min_age = value.1;
    min_age_index = i;
  }
}
```

しかしrustではそもそもforループよりもイテレーションを使うことが推奨されています（[参考][loop_iter]）。
イテレーションで`min_age_index`を取得してみます。

```rust
let min_age_index = (0..n).min_by_key(|i| p[*i].1).unwrap();
```

[min_by_key][min_by_key]によって引数に渡した関数の値が最も小さな要素をOptionalで得ることができます。
また引数の関数の型は`FnMut(&T) -> K`となっており借用かつ暗黙的な参照外しはしてくれないかたち（[参考][ref]）なので、参照外しをしてあげます。

### ロジック2

あとは`min_age_index`から順番に名前を表示していけばOKです！

```rust
for i in 0..n {
  println!("{}", p[(i + min_age_index) % n].0)
}
```

しかし、ここでもforループではなくイテレーションを使うことができます。

```rust
println!("{}", (0..n).map(|i| p[(i + min_age_index) % n].0.clone()).join("\n"));
```

戒めとして`clone()`しないとStringはコピーできないのでムーブしようとし、Vecotorの一部分だけムーブすることは許されていないのでコンパイルエラーとなってしまいます。
明示的に`clone()`を呼び出すか、`to_string()`を用いてstrに変換してやる必要があります。

### ソースコード

```rust
use proconio::*;
use itertools::*;

fn main() {
    input!{
        n: usize,
        p: [(String, usize); n]
    }

    let min_age_index = (0..n).min_by_key(|i| p[*i].1).unwrap();
    println!("{}", (0..n).map(|i| p[(i + min_age_index)%n].0.clone()).join("\n"));
}
```

AC!

## 参考

- [sansenさんの提出コード][sansen]
  - とても美しくて惚れ惚れしたのでこの記事を書くに至りました。
- [パフォーマンス比較: ループVSイテレータ, The Rust Programming Language 日本語版][loop_iter]
- [std::cmp::min_by_key, The Rust Standard Library][min_by_key]
- [String vs &str in Rust, thoughtram][string_str]
- [Rustをはじめよう その8(参照・仕組み編), DCS][ref]

[problem]: https://atcoder.jp/contests/abc304/tasks/abc304_a
[loop_iter]: https://doc.rust-jp.rs/book-ja/ch13-04-performance.html
[min_by_key]: https://doc.rust-lang.org/stable/std/cmp/fn.min_by_key.html
[string_str]: https://blog.thoughtram.io/string-vs-str-in-rust/
[sansen]: https://atcoder.jp/contests/abc304/submissions/41933648
[ref]: https://blog.dcs.co.jp/rust/20210527-rust-8.html#section_3