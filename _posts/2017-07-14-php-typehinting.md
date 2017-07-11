---
layout: post
title: PHPのタイプヒンティング
categories: app
tags: php
---

## 型指定とデフォルト値の関係

これにnullを渡してもエラーにならないが、

```
function someMedthod(array $var = null){
// doSomething;
}
```

これにnullを渡すとエラーにある

```
function someMedthod(array $var){
// doSomething;
}
```













