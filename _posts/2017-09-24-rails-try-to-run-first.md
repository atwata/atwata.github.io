---
layout: post
title: railsをとりあえず動かしてみる
categories: app
tags: ruby rails
---

## まずはrailsをインストール

- rubyとgemがインストールされていることが前提

```
gem install rails
```

## ひな形の作成

- sampleという名前で作ってみる

``` 
rails new sample
```

## 試しに起動してみる

- localhost以外からの接続も受け付ける場合は `-b 0.0.0.0` をつける


```
cd sample
rails server -b 0.0.0.0
```

下記にアクセスしてようこそ画面がでてくればまずはOK

```
http://動かしているIP:3000/
```

