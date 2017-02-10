---
layout: post
title: jekyllでGithub.io(Github Pages)作成したサイトに独自ドメインを設定する方法
categories: blog
tags: github
---

Github Pagesで管理しているサイトで独自ドメインを使用したい。

デフォルトだとgithub.ioがドメインとなる。
私のアカウントだと下記のようになる。

https://atwata.github.io

これを下記の独自ドメインでアクセスできるようにしたい。

https://blog.atwata.com

やるべきことは3つ。


## 1.独自ドメインの取得

これはGithub pagesに限ったことではないので詳細は割愛。

私はお名前.comで取得しました。


## 2.DNSサーバにAレコードを設定

下記２つのIPアドレスをAレコードに設定します。

- 192.30.252.153
- 192.30.252.154

IPアドレスの情報については下記のサイトに記載があります。変更されることもあるようなので、最新情報は下記の公式ページを確認してください。

<https://help.github.com/articles/tips-for-configuring-an-a-record-with-your-dns-provider/>

以下はお名前.comにおける設定画面のキャプチャです。

ドメインNavi > DNS関連機能の設定 > DNSレコード設定を利用する

![20170211 domain1]({{site.baseurl}}/images/20170211 domain1.png)

Aレコードが追加されている状態

![20170211 domain2]({{site.baseurl}}/images/20170211_domain2.png)

## 3.CNAMEファイルをリポジトリにpush

設定したドメインをCNAMEという名前のファイルに記載し、それをリポジトリ直下においてcommit&pushします。

```
$ echo blog.atwata.com > CNAME
$ cat CNAME
blog.atwata.com
```

独自ドメインの設定は以上になりmす。簡単ですね。
