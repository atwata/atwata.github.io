---
layout: post
title: JavaScriptのオブジェクトの定義の仕方2種類
categories: app
tags: javascript
---

### 1つ目

```
var book = {};
book.title = 'sample';
book.price = 1000;
book.getInfo = function(){
  return book.title + ' ' + book.price;
}
console.log(book.getInfo()); //sample 1000
```

`{}`で変数を生成するとオブジェクトとなり、
あとからメンバ変数や関数を.ポコポコくっつけられるのが面白い。

### 2つ目

```
var book = {
  title: 'sample',
  price: 1000,
  getInfo: function(){
    return book.title + ' ' + book.price;
  }
}
console.log(book.getInfo()); //sample 1000
```

2つ目は一見JavaやPHPにおけるクラスのように見えるが、
各項目はコロンでkey/valueを指定し、項目間はカンマ区切りのJSON形式である。






















