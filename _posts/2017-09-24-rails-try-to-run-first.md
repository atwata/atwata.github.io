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

- インストール完了後、バージョン確認

```
rails -v
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

- 起動するポートを変更したい場合

Address already in useエラーが発生して、起動できない場合がある。
その場合は指定するポート番号を変更する。

```
rails server -b 0.0.0.0 -p 3001
```

