---
layout: post
title: Symfony2で開発/本番(dev,prod)環境の切り替え
categories: app
tags: symfony2 php
---

アクセスするURLで切り替えることができる。

prod環境で処理をしたい場合

```
http://localhost/demo/hello/Fabien
```

dev環境で動かしたい場合

```
http://localhost/app_dev.php/demo/hello/Fabien
```

上記のようにapp_dev.phpをはさむ。



