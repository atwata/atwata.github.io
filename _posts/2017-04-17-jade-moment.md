---
layout: post
title: jadeでmomentを使って日付をフォーマットする方法
categories: app
tags: jade express
---

### momentをインストール

```
npm install moment
```

### 基本的な使い方サンプル

ルーティングの設定。momentを読み込みテンプレートに現在日付とmomentを渡す。

routes/index.js

```
var express = require('express');
var router = express.Router();

router.get('/date', function(req, res, next) {
  var moment = require('moment');
  res.locals.moment = moment;
  res.render('date', { title: 'moment format', now: new Date });
});
```

formatで表示可能。

views/date.jade

```
extends layout

block content
  h1= title
  p= moment(now).format('YYYY/MM/DD')
```

### 1日前を取得

```
var yesterday = moment().subtract(1, 'days');
```

### 明日をDateオブジェクトで取得

```
var dayTommorow = moment().add(1,'days').toDate();
```















