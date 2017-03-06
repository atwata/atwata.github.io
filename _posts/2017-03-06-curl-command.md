---
layout: post
title: curlコマンドでJSON形式のリクエストをPOSTする方法
categories: app
tags: curl
---

## curlコマンド

いつも忘れるのでメモ。

```
curl -v -H "Accept: application/json" -H "Content-type: application/json" -X POST -d '{"info":{"system":"1"},"data":{"no":"123","code":"000","name":"hoge"}}'  http://localhost/api/info
```

