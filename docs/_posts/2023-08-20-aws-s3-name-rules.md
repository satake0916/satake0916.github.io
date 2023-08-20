---
layout: post
title: S3のバケット名に大文字が使えないワケ
subtitle: 結論：DNSが大文字と小文字を区別しないから
date:   2023-08-20 22:09:24 +09:00
categories: aws
tags: AWS, S3
---

お仕事でAWSを触ることがあります。

IaCにはAWS SAMを使用することが多いのですが、S3のバケット名を大文字にしていたせいでデプロイが失敗して沼りました（2回目）。
2度と同じ過ちを繰り返さないためにも、なぜAWS S3のバケット名に大文字が許可されていないのか調べてみました。

語尾誤謬などあればコンタクトからご指摘いただけますと幸いです。

## ポイント1. S3のバケット名はURLに含まれる

まず前提としてS3はパブリックに公開されていれば次のような仮装ホスト形式のURLでアクセスすることができます（[引用元][s3_access]）。

`https://bucket-name.s3.region-code.amazonaws.com/key-name`

例えばバケット名が*lower-bucket*、リージョンが*us-west-2*（オレゴン）、オブジェクト名が*puppy.png*のS3オブジェクトにアクセスするURLは次のようになります。

`https://lower-bucket.s3.us-west-2.amazonaws.com/puppy.png`

大事なことは、S3のバケット名はURLにそのまま含まれるということです。

## ポイント2. DNSは大文字と小文字を区別しない

なんとなくご存じだと思いますが、世の中のドメインは全て小文字になっています(google.comとか、amazon.co.jpとか)。

これはなぜかというと、[RFC4343][rfc_4343]でDNSが小文字と大文字を区別しないように決められているからです。
RFCに則って、DNSが全部大文字小文字区別せずに管理しており便宜上全て小文字として表示しているということですね。

実際`curl https://SATAKE0916.GITHUB.IO/`とかでアクセスしてみても、普通に名前解決してくれます。

## S3のバケット名に大文字が使えないワケ

もしS3のバケット名に大文字が使えた場合、アクセスするURLにも大文字が入るべきということになります。

例えば上の例でバケット名を`UPPER-BUCKET`とした場合、URLは次のようになります。

`https://UPPER-BUCKET.s3.us-west-2.amazonaws.com/puppy.png`

しかしDNSは大文字と小文字を区別せずに名前解決をしてしまうので、AWSは上のようなアクセスがあった際に`upper-bucket`というバケットを探してしまい、「そんなバケットはないよ」となってしまいます。

ここの混乱を防ぐためにS3バケット名はDNS準拠というルールがあるようです。

## まとめ

- AWS S3は仮装ホスト形式でS3オブジェクトへのアクセスをサポートしている。そのURLにオブジェクト名が含まれる。
- DNSは大文字と小文字を区別しないため、バケット名に大文字が含まれていたらそのバケットに正しくアクセスできない。
- 無用な混乱を防ぐためにバケット名はDNS準拠というルールがある。

## 参考

- [バケットへのアクセス方法, AWS S3 ユーザーガイド][s3_access]
- [RFC4343][rfc_4343]
  - DNSが大文字小文字を区別しないことについての詳細
- [バージニア北部(us-east-1)リージョンのS3バケット命名規則が2018年3月から変わります, quiver(DevelopersIO)][class]
  - 2018年3月1日にS3バケットの命名規則が今のものに変わった際のドキュメント紹介記事です。

[s3_access]: https://docs.aws.amazon.com/ja_jp/AmazonS3/latest/userguide/access-bucket-intro.html
[rfc_4343]: https://www.ietf.org/rfc/rfc4343.txt
[class]: https://dev.classmethod.jp/articles/us-east-s3-bucket-naming-rule-change-in-2018-march/