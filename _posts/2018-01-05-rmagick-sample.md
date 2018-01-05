---
layout: post
title: rubyのrmagickで画像を結合・リサイズ・合成する
categories: app
tags: ruby rmagick imagemagick
---

## インストール

まずはLinux上にImageMagickをインストールする(CentOS)

言語に依存しない画像処理ソフトウェアであるImageMagick（イメージマジック）をインストールする。

```
sudo yum install ImageMagick ImageMagick-devel
```

rubyで扱えるようにするためのgemを入れる

```
gem install rmagick
```

## 使い方

### 画像をファイルから読み込んで２倍の大きさにしてファイルに書き出すサンプル

```
require 'rmagick'

image = Magick::ImageList.new("from.jpg")
image = image.scale(2.0)
image.write("to.jpg")
```

###  ２つの画像を結合するサンプル

```
require 'rmagick'

image = Magick::ImageList.new("from1.jpg", "from2.jpg")
image = image.append(false)	#falseだと左右につなげる。trueだと上下につなげる
image.write("to.jpg")
```

### 画像をリサイズする

```
require 'rmagick'

imageList = Magick::ImageList.new("from.jpg")
imageList = imageList.resize(200, 200)
imageList.write("to.jpg")
```

### 二つの画像を合成する

透過pngなど透明部分がある画像ならそこは抜いてくっつける。


```
imageListFrom = Magick::ImageList.new("from.jpg")
imageListFrame = Magick::ImageList.new("frame.png")
imageList = imageListFrom.composite(imageListFrame, 0, 0, Magick::OverCompositeOp)
imageList.write("to.png")
```


