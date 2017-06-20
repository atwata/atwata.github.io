---
layout: post
title: imgでなくcssのbackgroundの画像を使い、それをリンクにする
categories: app
tags: css
---

## cssで画像を表示して、さらにそれをリンクにしたい

### サンプルcss

```
#hoge-id a {
  width: 100px;
  background: url("path/to/img.png") no-repeat;
  height: 50px;
  border: none;
  cursor: pointer;
  display:block;
  margin: 0 auto;
}
#hoge-id a:focus {
	outline: 0;
}
#hoge-id a:hover {
	opacity: 0.6;
}

#hoge-id span {
  visibility: hidden;
}
```

### サンプル html


```
<div id="hoge-id">
  <a  href="hoge.html"><span>back</span></a>
</div>
```


### ポイントは

- aタグのbackgroundで画像ファイルを指定
- aタグ内にはspanをおき、そのvisibililyをhiddenにする









