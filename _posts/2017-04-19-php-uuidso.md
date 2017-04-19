---
layout: post
title: phpでuuid.soを追加でインストール
categories: middle
tags: php
---

```
# yum -y install libuuid-devel
# pecl install uuid
```

php.iniに下記を追加

```
extension=uuid.so
```














