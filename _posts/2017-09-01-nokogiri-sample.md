---
layout: post
title: rubyでnokogiriを利用してみる
categories: app
tags: ruby nokogiri
---

## nokogiriのインストール

```
gem install nokogiri
```


```
$ gem install nokogiri
Fetching: mini_portile2-2.2.0.gem (100%)
Successfully installed mini_portile2-2.2.0
Fetching: nokogiri-1.8.0.gem (100%)
ERROR:  Error installing nokogiri:
        nokogiri requires Ruby version >= 2.1.0.


$ ruby -v
ruby 2.0.0p648 (2015-12-16) [x86_64-linux]
```

現在2017年9月時点ではruby2.1以上が必要。現環境centos7デフォのyumで入れられるのは2.0のみ

そもそもyumで言語のバージョン管理するのがよくない

http://qiita.com/Fendo181/items/d14ebfb148223c8e5ecb

上記を参考にrbenvを使う


今回は2.4.1を入れる

```
rbenv install 2.4.1 
```

あらためてnokogiriをインストール

```
gem install nokogiri
```

## nokogiriのサンプル

### ページの全htmlを取得

```
#!/usr/bin/env ruby
 
require "open-uri"
require "nokogiri"
 
url = "http://www.yahoo.co.jp/"
charset = nil
 
html = open(url) do |page|
  charset = page.charset
  page.read
end
 
doc = Nokogiri::HTML.parse(html,nil,charset)
 
puts doc
```

### タイトルのみを取得する場合は最後のdocをdoc.titleにする

```
puts doc.title
```

実行するとこんな感じ

```
ruby noko2.rb 
"Yahoo! JAPAN"
```


### 画像のすべてのパスを表示(imgタグのsrcを取得）

```
doc.xpath('//img').each do |node|
   p node.attribute('src').value
end
```

実行するとこんな感じ

```
ruby noko.rb
"//s.yimg.jp/images/clear.gif"
"//s.yimg.jp/images/clear.gif"
"//s.yimg.jp/images/top/sp/cgrade/logo-mh-160929.gif"
"//s.yimg.jp/images/clear.gif"
"//s.yimg.jp/images/clear.gif"
"//s.yimg.jp/images/clear.gif"
"//s.yimg.jp/images/clear.gif"
"//s.yimg.jp/images/clear.gif"
～以下略
```


