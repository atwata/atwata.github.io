---
layout: post
title: Github.io(Github Pages)で管理しているJekyllのサイトでsitemapを生成する方法
categories: blog
tags: github sitemap
---

## Github Pagesでsitemap.xmlを生成する方法

このサイトがそうですが、Github Pagesで管理しているJekyllで作成したサイト
のsitemap.xmlを作成する方法について説明します。

はてなブログなどのサービスは標準で生成されますし、wordpressだとJetpackやAll in one SEOといったプラグインでsitemap.xmlを生成できます。

Github Pagesを使用している場合も簡単です。

公式サイトに記載がありますが、config.ymlに下記を追加するだけです。


```
gems:
  - jekyll-sitemap
```
公式のヘルプ

<https://help.github.com/articles/sitemaps-for-github-pages/>

## 生成されているか確認する

[自分のサイトのURL]/sitemap.xml
にアクセスしてみる。

<http://blog.atwata.com/sitemap.xml>

こんな感じの画面が表示されればOKです。

![20170211 domain1]({{site.baseurl}}/images/20170211_sitemap.png)
