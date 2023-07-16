---
layout: post
title:  個人的に新しいと感じるプログラミング言語を3行でまとめてみた(2023.07)
subtitle: Carbon, HackからKotlin, Rustまで
date:   2023-07-16 12:09:04 +9:00
categories: programing
---

最近あまり新しい言語を学ぶということをしていないなも思ったので、聞いたことがあるプログラミング言語のうち個人的に最近寄りだと感じる言語を短くまとめてみました。(2023年7月時点)

カバーできていない言語も無数にありますがご了承ください。

## 一覧

### Carbon

[GitHub][carbon]
<!-- 2022年 -->

C++の後継としてGoogleが開発している言語で、まだコンパイラもなく実験的な段階です。

C++は1980年代に登場した古代言語でありながら、パフォーマンスに重きを置くソフトウェアでは採用せざるを得ないのが現状です。

そこでツール郡やメモリセーフなサブセットをCarbonに取り入れ、C++からCarbonに段階的に移行することを目標に開発が進められています。

### Hack

[公式サイト][hack]
<!-- 2014年 -->

PHPの後継としてFacebookによって開発されたサーバサイド開発向け言語。

静的動的両方の型付けを使用した上で、PHPのような開発スピードを保てることがポイントです。

HHVMという処理系によってJITコンパイルして実行するため、PHPよりも動作が早いことも特徴です。

#### 参考リンク

- [PHP7 と HHVM/Hack 言語って何が違うの？, infiniteloop, YamaYuski][hack_study]

### Swift

[公式サイト][swift]
<!-- 2014年 -->

Object-Cの後継としてApple製品でアプリを開発するためのAppleが開発した言語。

Object-Cは1980年代に登場した古代言語で、それをモダン化するためにAppleが腰を上げてSwiftを開発した（というイメージ）。

しかしAppleのためのAppleが使う言語という感じで、一般開発者の中にはObject-Cを使い続けている人も少なくない印象です。

### Elixir

[公式サイト][elixir]
<!-- 2012年 -->

Erlangの後継として開発された関数型言語。

文法がErlangと比べてわかりやすく、Phoenixフレームワークを使用することでwebアプリを開発もできます。

正直なところ関数型言語の何が嬉しいのかもよくわかっていないので、詳細は他記事を参照ください。

#### 参考リンク

- [[翻訳] Elixir - 次に来る大物Web言語, Qiita, HirofumiTamori][elixir_detail]

### Julia

[公式サイト][julia]
<!-- 2012年 -->

FORTRANやPythonの後継として開発された数値解析を得意とする言語。

Pythonが得意としている機械学習やシミュレーションに特化することで、Pythonの弱点である速度を大きく改善しています。

ライブラリの豊富さなどはPythonに遠く及ばないので、そこを解消しPythonに取って代わることになるのか注目しています。

### 参考リンク

- [バイバイ Python。 ハロー Julia！, Qiita, baby degu][julia_hello]

### TypeScript

[公式サイト][typescript]
<!-- 2012年 -->

Microsoftによって開発された静的型付け機能が搭載されたJavaScriptです。

完全にJavaScriptの上位互換になっているので、TypeScriptのコードはJavaScriptで利用できます。

今時であれば特別な理由がない限りTypeScriptを使用するべきでしょう。

### Dart

[公式サイト][dart]
<!-- 2011年、Flutterは2017年 -->

JavaScriptの後継としてGoogleによって開発されたクライアント開発向け言語。

マルチプラットフォーム対応や、強い静的型付け言語でCライクな構文を強みとしています。

最近だとFlutterフレームワークのイメージが強いです（Ruby on Railsみたいな印象）。

### Kotlin

[公式サイト][kotlin]
<!-- 2011年 -->

Javaの後継としてJetBrainsによって開発された言語。

Javaの最大の欠点として記述量の多さが挙げられますが、その点を解消した上でNullセーフなどJavaで渇望されていた機能も採用されています。

今からやるなら間違いなくKotlin、と思えるくらいJavaの正統進化になっています。

### Rust

[公式サイト][rust]
<!-- 2010年 -->

C++の後継として開発された言語。

C++と同程度の速度を誇りながら、メモリ安全性を保証できたりエラーメッセージが丁寧だったりするのが大きな特徴です。

習得が難しいとされている一方で、プログラマからの評価は高い印象があります。

## まとめ

軽くまとめてみて半分以上チュートリアルすらやっていないことに気づき愕然としました。中にはいつ使う言語なのかすらよく分かっていないものもありました...。反省です。

自分はHackやRustがちょっと気になるので触ってみたいですね。

似た悩みを抱える人の助けになれば幸いです。

### 余談

そもそものきっかけは[情熱プログラマー][jonetsu]でした。

第1章に自分の時間を技術に投資できていますか？どのような技術に投資するか真剣に考えられていますか？(なんとなく流行りや仕事に流されていませんか？)ということが書かれていました。

いやまさに。
まさに今の自分のことだと。

仕事でAWSを使っているからその付近の知識を偶発的に調べ、最近だとTypeScriptやPythonをたまたま流れで書くことになったから勉強しているだけだと自覚しました。

これではいけないと、身をつまされた思いでこの記事を書き上げたのでした。

[dart]: https://dart.dev/
[elixir]: https://elixir-lang.jp/
[elixir_detail]: https://qiita.com/HirofumiTamori/items/0dfdbada30c7d8f183fd
[hack]: https://hacklang.org/
[hack_study]: https://www.infiniteloop.co.jp/tech-blog/2018/07/difference-between-php7-and-hhvm-hack/
[carbon]: https://github.com/carbon-language/carbon-lang
[julia]: https://julialang.org/
[swift]: https://www.swift.org/
[typescript]: https://www.typescriptlang.org/
[kotlin]: https://kotlinlang.org/
[rust]: https://www.rust-lang.org/
[julia_hello]: https://qiita.com/baby-degu/items/7d4e295964ebb6e92de4
[jonetsu]: https://www.amazon.co.jp/%E6%83%85%E7%86%B1%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9E%E3%83%BC-%E3%82%BD%E3%83%95%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A2%E9%96%8B%E7%99%BA%E8%80%85%E3%81%AE%E5%B9%B8%E3%81%9B%E3%81%AA%E7%94%9F%E3%81%8D%E6%96%B9-Chad-Fowler/dp/4274067939
