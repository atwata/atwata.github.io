---
layout: post
title: Symfony2のイベント
categories: app
tags: symfony2
---

## イベント

- Eventクラスを継承する
- イベント自体はデータのみを持つ


## イベントリスナー

- イベントを受け取って何らかの処理を行う（メソッドを呼び出す）


## ディスパッチャー

2つの役割がある

- イベントとリスナーの対応付け
  - addListener(イベント名, [リスナーオブジェクト, リスナーオブジェクト内の呼び出したいメソッド名])
    - イベント名は任意。イベント呼び出し時にも使う識別文字列。
      - callback的に、オブジェクトとメソッド名を指定する）
- イベントの呼び出し
  - dispatch(イベント名, イベントオブジェクト)
  - イベントの呼び出しが行われると対応付けられたメソッドが呼ばれる。

  
- Symfony2ではイベントとリスナーの対応付けを別の方法で行うこともできる。
  - EventSubscriberInterfaceをimplementsする
  - getSubscribedEventsを実装
    - 呼び出したいメソッド名を定義
    - 定義したメソッドの実装


## 参考

<http://tech.quartetcom.co.jp/2015/12/17/event-dispatcher/>

<http://docs.symfony.gr.jp/symfony2/book/internals.html>
「イベントサブスクライバを使う」の箇所




