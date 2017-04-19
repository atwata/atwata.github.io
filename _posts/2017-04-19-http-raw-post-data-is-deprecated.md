---
layout: post
title: PHPでAutomatically populating $HTTP_RAW_POST_DATA is deprecatedというエラー・警告がでる
categories: app
tags: php
---

表示されるメッセージは下記の通り。

```
Deprecated:  Automatically populating $HTTP_RAW_POST_DATA is deprecated and will be removed in a future version. To avoid this warning set 'always_populate_raw_post_data' to '-1' in php.ini and use the php://input stream instead. in Unknown on line 0

Warning:  Cannot modify header information - headers already sent in Unknown on line 0
```

php.iniの設定を下記のように変更することで警告を抑制できる。
```
;always_populate_raw_post_data = On
↓
always_populate_raw_post_data = -1
```
















