---
layout: post
title:  ガベージコレクションのアルゴリズムを3行でまとめてみる
subtitle: Reference Counting, Mark-Sweep, CopyingからIncremental, Generationalまで
date:   2023-08-07 12:01:53 +09:00
categories: programing
latex: false
---

ふと気になったのでガベージコレクション（GC）の代表的なアルゴリズムを要点だけ3行でまとめてみます。

内容は基本的に[一般教養としてのGarbage Collection][gc_pdf]を参考にしています。ありがとうございます。

GCとは何かについては省略します。参考文献を参照ください。

## 基礎的なアルゴリズム

### Reference Counting

どこかから参照されているオブジェクトは使われている、という考え方のもとオブジェクトごとに参照されている数を保持しておきゼロになったら解放するという方式です。

利点としては次に紹介するTracing GCのように一気に解放ということをしないので、停止時間が短くなることです。

欠点としてはオブジェクトを参照するたびに余計な処理が増えるので実行時間が遅くなること、循環参照した場合にメモリリークが発生してしまうことです。

### Tracing GC

プログラムが直接触れるレジスタやスタックなどから参照されている（参照からさらに参照されているものも含む）オブジェクトのみ必要で、それ以外は使われていないという考え方です。
よって定期的にヒープ全体を見渡して、使われていないオブジェクトを解放するという方式になります。

利点は循環参照でも解放できること、欠点は停止時間が大きくなってしまうことです。

以下の2つがTracing GCを実装するメジャーなアルゴリズムです。

#### Mark-Sweep GC

各オブジェクトに探索済みかどうかを示すマークビットを割り当て、0であれば未探索、1であれば探索済みとします。

最初は全て0なので、まずマークフェイズでレジスタなどから参照されているオブジェクトを再帰的に1にしていきます。
次にスイープフェイズでマークビットが0のオブジェクトを解放します。

#### Copying GC

ヒープを二分割しておいて、片方のエリアがいっぱいになったらまだ使っているオブジェクトをもう片方のエリアにコピーするという方式です。

実はReference CountingとMark-Sweepはどちらも空いているメモリがまばらになってしまうため連続した領域を確保しづらい（＝フラグメンテーション）という問題がありました。
Copyingではコピーするときにもう片方のエリアにオブジェクトを連続して並べることで、フラグメンテーションが起きないという利点があります。

## アルゴリズムを改良する

前節が大まかなアルゴリズムの紹介になりますが、これほどナイーブな実装をそのまま採用していることは今時ないと思います。

代表的な改良方法もいくつか併せて紹介します。

### Lazy Sweep

Mark-Sweepでは一気にマークフェイズとスイープフェイズを実行するために停止時間が大きくなってしまうという問題がありました。

Lazy Sweepだとヒープがいっぱいになったタイミングではマークフェイズだけを実行することにします。
そして実際に`malloc`要求が発生したタイミングでスイープフェイズを少しずつ実行して、空き領域をユーザープログラムに渡していきます。

### Incremental GC

こちらもTracing GCでの停止時間を短くするための方法です。

Incremental GCでは探索処理をまとめてやるのではなく、ユーザープログラムと交互にちょっとずつ実行することで停止時間を短くします。
しかしGCの最中にユーザープログラムによってオブジェクトが書き換えられてしまうため、write barrierというしくみが別途必要になります（詳細は[参考元][gc_pdf]をご覧ください）。

### Generational GC

こちらも停止時間を短くするための方法ですが、探索する範囲を狭めるというアプローチです。

Generational GCでは新しめのヒープと古めのヒープに分けてオブジェクトを管理し、新しく作成されたオブジェクトはまず新しめのヒープに確保されます。
新しめのヒープで何度かGCされても解放されなかったオブジェクトはこの次のGCでも生き残るはずだと考え、古めのヒープに移されます。
古めのヒープには新規オブジェクトは作成されず、いっぱいにならない限りGCされないので、結果としてGCするべき範囲が少なくなります。

## おわりに

自分の勉強の整理も兼ねて、短くまとめてみました。

正直全くGCの予備知識なしにこの記事を読んでもいまいちピンとこないと思いますので、参考にもっとわかりやすい資料を挙げています。
興味がわいた方はぜひそちらもご覧ください。

## 参考

- [一般教養としてのGarbage Collection, 遠藤敏夫][gc_pdf]
  - GC全般について一通り知っておきたいなという時に最適です。
- [レアでアレなGCの話, るびま][ruby]
  - RubyのMark-Sweep GCについての記事です。前半のMark&Sweep, Copying, ReferenceCountについての例え話が秀逸でおすすめです。
- [3 Garbage Collector Implementation, HotSpot Virtual Machine Garbage Collection Tuning Guide][java]
  - Java VMのチューニングガイドです。Java VMで使用されているGenerational GCについて詳しく説明されています。
- [メモリー管理, JavaScript MDN][javascript]
  - JavaScriptでのメモリ管理についての記事です。Mark-Sweepや重要な概念である弱い参照についての説明があります。
- [PythonのGarbageCollection, Neil Schemenauer (翻訳：中村 成洋)][python]
  - PythonのReference Countについての記事です。なぜReference Countなのか、どうやって循環参照を見つけるかなど書かれていておもしろいです。

## もっと詳しく！

調べる中でもっと詳細を網羅的に説明している資料が紹介されていたので、自分も読めてはいませんがネクストステップとして挙げておきます。

- [GCアルゴリズム詳細解説][wiki]
- [ガベージコレクションのアルゴリズムと実装, 中村成洋 相川光][book]

[gc_pdf]: http://matsu-www.is.titech.ac.jp/~endo/gc/gc.pdf
[ruby]: https://magazine.rubyist.net/articles/0025/0025-RareAreGCTalk.html
[java]: https://docs.oracle.com/en/java/javase/20/gctuning/garbage-collector-implementation.html#GUID-23844E39-7499-400C-A579-032B68E53073
[javascript]: https://developer.mozilla.org/ja/docs/Web/JavaScript/Memory_Management
[python]: http://www.narihiro.info/translate/garbage_collection_for_python_jp.html
[wiki]: https://seesaawiki.jp/w/author_nari/d/GC
[book]: https://www.amazon.co.jp/%E3%82%AC%E3%83%99%E3%83%BC%E3%82%B8%E3%82%B3%E3%83%AC%E3%82%AF%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%AE%E3%82%A2%E3%83%AB%E3%82%B4%E3%83%AA%E3%82%BA%E3%83%A0%E3%81%A8%E5%AE%9F%E8%A3%85-%E4%B8%AD%E6%9D%91-%E6%88%90%E6%B4%8B/dp/4798025623/ref=rvi_sccl_1/358-1733826-2905257?pd_rd_w=rldo5&content-id=amzn1.sym.a4dc92d7-7100-437e-b3e3-2349e8298523&pf_rd_p=a4dc92d7-7100-437e-b3e3-2349e8298523&pf_rd_r=2QR09HSQFWHWRXD38TZB&pd_rd_wg=1kzOc&pd_rd_r=421d1d0a-3f2d-45c3-a0be-f01d3f8b263c&pd_rd_i=4798025623&psc=1
