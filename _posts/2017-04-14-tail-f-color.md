---
layout: post
title: tail -fの結果を色づけ
categories: os
tags: linux
---

xxxログをtail -fしたとき、キーワードyyyの文字を赤色で表示する。

```
tail -f xxx.log | perl -pe 's/yyy/\e[31m$&\e[m/g'
```

意味はこんな感じ。

```
tail -f ログ名 | perl -pe 's/キーワード/\e[カラーコードm$&\e[m/g'
```

カラーコードとは、

2桁あって1桁目の3は文字色の意味、2桁目は下記

```
0 Black
1 Red
2 Green
3 Yellow
4 Blue
5 Magenta
6 Cyan
7 White
```

したがって最初の例の31だとキーワードの文字色がが赤で表示される。








