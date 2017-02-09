---
layout: post
title: Resolver(リゾルバ)という名前を持つクラスの役割とは
---

フレームワークなどでHogeResolverという名前のクラスを見かけることがあります。

Resolve(リソルブ)＝解決するという意味ですが、解決するクラスといっても良くわかりません。解決って何だろう。
ソフトウェアっていうのは全般的に何かを解決するものともいえます。

このResolverですが、自分ではあまりつけない名前だったので、どういう役割のクラスのときにこの名前をつけるのが適切なのか調べてみました。

## リゾルバといえばDNSリゾルバ

元々Resolver(リゾルバ)という言葉から連想されるのはDNSリゾルバです。

DNSリゾルバは、名前解決を行うソフトウェアプログラム。

DNSリゾルバがやっていることは2つ。

- ドメイン名からIPアドレスの情報を検索する
- IPアドレスからドメイン名の情報を検索する


## springフレームワークにおけるResolver

### LocaleResolver

リクエストパラメータを受け取って、Localeオブジェクトを返す。

http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/servlet/LocaleResolver.html


### ThemeResolver

リクエストパラメータを受け取って、theme nameを返す

http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/servlet/ThemeResolver.html

### ViewResolver

ビューの論理名からビューのインスタンスを探す

http://d.hatena.ne.jp/ryoasai/20101121/1290317629

上記3つのやっていることを見ることでなんとなくResolverのイメージができてきました。

「名前」や「環境情報」といったインプットを元に、それに応じたアウトプットを取得するといった感じでしょうか。

if文（つまりロジック）を抜き出して、インターフェースを統一するStrategyパターンと類似している印象も受けます。

### クラス名の参考情報によると

また↓によると

http://qiita.com/KeithYokoma/items/ee21fec6a3ebb5d1e9a8

「ユーザの環境に応じた処理のルーティングをするレイヤ」とのこと

ルーティングとはMVCでいえばリクエストURLから実行するコントローラのメソッドを呼び出すこと。

DNSの名前解決に近いですね。

解決という言葉だけだと、「問題」に対する「解決」をするイメージがありリゾルバのイメージが抽象的になりすぎますが、
「名前解決」という言葉における「解決」をイメージすると、リゾルバのイメージを限定できそうです。




