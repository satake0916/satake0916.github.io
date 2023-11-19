---
layout: post
title: 【Rust】HomebrewでRust製CLIを公開した
subtitle: Github releaseとbrew tapを添えて
date: 2023-11-19 17:02:01 +09:00
categories: Rust
tags: Rust, CLI, Homebrew
---

[crates.ioでRust製CLIを公開した]({% post_url 2023-11-19-thue %})の続きです。

Homebrewで公開までやったので手順を忘備録として残しておきます。

## Homebrewで公開

とはいったものの、今回も手順をブログに公開してくださっている方がいたのでまねただけです。感謝しきりです。

[RustのcliをHomebrewで公開する][happy]

自分のリポジトリは以下です。

[Github release][ghrelease]

[Homebrew Formula][ghtap]

## 詰まった点

まず参考サイトの手順通りにやったのですが、HomebrewでMacOSにインストールしても`cannot execute binary file`と出て実行できませんでした。

というのも自分は今までUbuntu上で開発をしていたので、単純に`cargo build --release`とするとUbuntu用のバイナリが生成されてしまっていました。

そこでmacOS用に`cargo build --release --target x86_64-apple-darwin`と実行したのですが、以下のようなエラーが出てビルドできませんでした。

```console
error: linking with `cc` failed: exit status: 1
  |
  = note: LC_ALL="C" PATH="/home/..."
  (中略)
  = note: cc: error: x86_64: No such file or directory
          cc: error: unrecognized command line option '-arch'
```

少し調べてみると、`-arch`オプションはXcode専用のものなんだそうです（[参考][stack]）

> The -arch option is part of the Apple extensions to gcc. You need to use the gcc supplied by Apple's Developer Tools, Xcode.

じゃあUbuntuにXcode入れればいいじゃん！と思いさらに調べてみると、実は規約的にNGでした。

> THIS SOFTWARE IS AUTHORIZED ONLY FOR EXECUTION ON AN APPLE-BRANDED PRODUCT RUNNING MACOS.
> ANY OTHER DOWNLOAD OR USE OF THIS SOFTWARE IS NOT AUTHORIZED AND IS IN BREACH OF THIS AGREEMENT.
>（[Xcode and Apple SDKs Agreement][xcode]より引用）

最終的にUbuntu上でmacOS用のバイナリを生成するのは諦め、おとなしくMacBook Airでビルドしました。

## 感想

Homebrewというと大御所感がありますが、Github releaseとbrew tapを利用すればこんなあっさり公開できるんだと驚きました。

また普段何気なく使っていたbrew tapがどういうものなのかを知る機会にもなってよかったです。

thueコマンドはあまり機能を追加するつもりはないですが、ちょっとずつメンテできたらいいなぁと思います。

## 参考

文中で上げたもの以外を記載しておきます。

* [How to Publish your Rust project on Homebrew, Federico Terzi][original]
  * 最初に紹介した記事の参考に上げられていた記事。Formulaの説明なんかもあってこちらもとても参考になりました。
* [linuxでxcodeを使ってiosアプリを開発したいです。茨の道ですか？, teratail][teratail]
  * Xcodeについて調べていて初めて規約に触れていた内容。実際一次ソースを読んでもNGでした。
* [cargo build, The Cargo Book][build]
  * macOS用のビルド方法を調べたときに参考にしました。

[happy]: https://blog.ymgyt.io/entry/release-rust-with-homebrew/
[ghrelease]: https://github.com/satake0916/thue/releases/tag/v0.1.1_mac
[ghtap]: https://github.com/satake0916/homebrew-thue
[stack]: https://stackoverflow.com/questions/4391192/why-do-i-get-cc1plus-error-unrecognized-command-line-option-arch
[xcode]: https://www.apple.com/legal/sla/docs/xcode.pdf
[teratail]: https://teratail.com/questions/149625
[build]: [https://doc.rust-lang.org/cargo/commands/cargo-build.html]
[original]: [https://federicoterzi.com/blog/how-to-publish-your-rust-project-on-homebrew/]