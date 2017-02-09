---
layout: post
title: curlコマンドにセッションID(cookie)を指定する方法
categories: web
tags: curl session
---

## やりたいこと

- 自分で管理しているWEBアプリケーションに対してcurlコマンドで任意の通信を行いたい。

- 環境
  - ユーザーIDとパスワードでログインしてから色々遊べるWEBアプリである。
  - PHPで作られている。
  - ログイン後は基本的にセッションIDで認証をしている。
  - セッションID以外にPOSTパラメータでトークンを毎回渡している。
  - ログイン処理以外はSSLは使用していない。

- ツール化するというよりも単発で簡単に試したい。
  - ログイン処理とかCookieの保存とかはせずにブラウザで認証したあとに、そのセッションIDをcurlで利用したい。

- 要はセッションハイジャック的なことを試したい。

## やったこと

- Firefoxで対象のWEBアプリにログインする。
- HTTPFoxで任意の通信のCookieタブとPOSTタブの中身を見る。
  - もちろんchromeのdeveloper toolsとかでもよい。
- どんな値になっているかメモしておく。
- curlコマンドで今回使うオプション
  - -b, --cookie COOKIE データCOOKIEをcookieとして送信する
  - -d, --data PARAM... POSTリクエストとしてフォームを送信する

- サンプル。条件は下記。
  - cookieがXXXXX
  - postするパラメータがキー名token、値がYYYYY
  - ホスト名はexample.com、ファイル名はhoge.php


```
curl --cookie "PHPSESSID=XXXXX" --data "token=YYYYY" example.com/hoge.php
```
