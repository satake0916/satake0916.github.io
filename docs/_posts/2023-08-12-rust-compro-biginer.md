---
layout: post
title: 水色C++コーダーがRustで50問解いて思ったこと
subtitle: 結論：Rustはおすすめ
date:   2023-08-12 19:40:17 +09:00
categories: procon
tags: 競プロ, Rust. C++
---

2週間ほど前からRustでAtCoderの問題を解いています。
[ABS][abs]とABC300~313のABC埋めが終わったので、初学者なりに感じたことをまとめてみます。

筆者のバックグラウンドとしては数年前にC++を使って水色に到達し、その後ずっと放置していましたが最近復帰し言語を変えてみたという経緯です。

Rustを使おうか迷っている人の参考になれば幸いです。

## 幸せなところ

一応個人的に重要だと思う順に並んでいます。

### エラーメッセージが丁寧

`Segmentation Fault.`

C++erならこのメッセージと格闘したことがない人はいないでしょう...競プロなら大体範囲外アクセスです。

Rustならどこで範囲外アクセスが起きたのか教えてくれます。もっと言えばイテレーションを使えば良いことも多いので、そもそも添字アクセスする機会すら少なくなります。最高です。

### 強い人がいる

自分は新しい言語で競プロをするときは、

1. まず自分でACする
2. その言語で強い人のコードを見て真似する

という方法をとります。
その言語特有の書き方や便利な標準ライブラリなど学べることがとても多いからです。

Rustだと自分は特に[sansenさん][sansen]を参考にしています。
コードがスマートですし、赤コーダーかつABC312では1位を取るなど実力も凄まじいです。参考にできる人がいるというのは大事ですよね。

### ツール群が便利

[Rust Book][rust-book]にもある通りRustは開発ツールが整備されています。

#### Cargo

*Cargo*はRustの依存関係管理ツール兼ビルドツールで、ライブラリやツールを導入したいと思ったら大抵はCargoで解決できます。

#### Rustfmt

*Rustfmt*という標準フォーマッタが用意されており、簡単に導入できます。自分は実装が苦手なのもあり愛用しています。

（参考：[実装力で戦える！　～競プロにおける実装テクニック14選～, @e869120][zissou]）

#### rust-analyzer

*rust-analyzer*によってメソッドのサジェストや型推論、コンパイルエラーもほぼ事前に教えてくれるなどとにかくコーディングが楽になります。

（参考：[rust-analyzerを使ってみたら思いのほか素敵だったので紹介してみる, @simonritchie][rust-analyzer]）

## 辛いところ

### 文字列の扱いが複雑

そもそも`String`と`&str`は違うとか、結合は`String`と`&str`じゃないとできないとか、`String`はコピーできないからクローンしろとか...文字列に関しては今も苦しんでいます。

幸いAtCoderでは*proconio*が使えて、入力で[proconio::marker::Chars][chars]として受け取ればC++のStringチックに扱えるので最近はもっぱらそうしています。

### 所有権が難しい

Rustではメモリ管理のためにmalloc/freeやGCを採用せず[所有権][ownership]という仕組みを採用しています。

これがびっくりするくらい難しい。moveできないとかborrowできないとかこれはsecond borrowだからダメだとか...

こればっかりはもうちょっと仲良くならないといけないな、と感じます。

### C++じゃない

当然ですが、RustはC++ではありません。

競プロの解説には大抵C++はありますがRustはありません。Qiitaなんかの記事の参考実装もC++であることがほとんどです。世界最強touristのコードを参考にしようにもC++です。

Rustに限らずC++以外で競プロをやると立ちはだかる壁だとは思いますが、今までC++でやっていただけに少しストレスです。

## まとめ

総評としては満足で、今後もRustを使っていこうと思っています。

幸せになれる点も多いですし、個人的には記法も好みでかなり気持ちよくコーディングできています。辛い点も最後の点以外は勉強すれば解消していけると前向きに捉えています。

Rust、おすすめです。

## 参考

- [実装力で戦える！　～競プロにおける実装テクニック14選～, @e869120][zissou]
- [rust-analyzerを使ってみたら思いのほか素敵だったので紹介してみる, @simonritchie][rust-analyzer]
- [Rustの実行速度が速い理由, @asparagasu][rust-speed]
- [付録D - 便利な開発ツール, The Rust Programming Language 日本語版][rust-tools]

[abs]: https://atcoder.jp/contests/abs
[rust-analyzer]: https://qiita.com/simonritchie/items/a8bdcbc65a0ae1861e6b
[sansen]: https://atcoder.jp/users/sansen
[zissou]: https://qiita.com/e869120/items/920a6e63435bf6efe539
[rust-book]: https://doc.rust-jp.rs/book-ja/ch00-00-introduction.html
[chars]: https://docs.rs/proconio/latest/proconio/marker/enum.Chars.html
[ownership]: https://doc.rust-jp.rs/book-ja/ch04-00-understanding-ownership.html
[rust-speed]: https://qiita.com/asparagasu/items/d6078de68fc10a2a86bc
[rust-tools]: https://doc.rust-jp.rs/book-ja/appendix-04-useful-development-tools.html