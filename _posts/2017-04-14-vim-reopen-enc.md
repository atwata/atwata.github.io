---
layout: post
title: vimで文字コードを変更して開きなおす
categories: tool
tags: vim
---

ファイル開いたら文字化けしてるな、というとき。

ノーマルモードの状態で下記のように文字コードを指定すると再読み込みできる。

```
:e ++enc=utf-8
:e ++enc=euc-jp
:e ++enc=iso-2022-jp
:e ++enc=cp932
```











