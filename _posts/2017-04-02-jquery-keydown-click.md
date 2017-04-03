---
layout: post
title: inputボックスでEnterキーを押したらボタンのclickイベントを呼ぶ
categories: app
tags: jquery javascript
---

## jQueryを使って実装する方法

```
$("#input-hoge").keydown(function() {
  if( event.keyCode == 13 ) {
    $("#button-piyo").click();
  }
});
```




