---
layout: post
title: PHP/Vagrant/NetBeans環境で デバッグしてもブレークポイントで止まらない原因はパスマッピングだった
categories: app
tags: php vagrant NetBeans
---

php.iniでxdebugが有効になっていると、phpの実行時にxdebug.remote_hostのxdebug.remote_portに接続してくる。

```
xdebug.remote_host = 10.0.2.2
xdebug.remote_port = 9000
```

そこで、NetBeans側でデバッグを実行して接続を受け付ける状態にする。

ブラウザで対象の画面を開くがブレークポイントで止まらない。

「最初の行で停止」のチェックをONにすると一応停止はする。（でも再開してもブレークポイントで止まらない）

その原因は、パスマッピングがされていないことだった。

eclipseならダイアログがでてきて、パスマッピングを聞いてくれる。
パスマッピングというのはサーバ上のPHPのソースとローカルのPHPのソース、それぞれどう対応するのかを教えてあげることである。

NetBeansでパスマッピングを指定する方法は、

プロジェクトのプロパティ→実行構成→詳細ボタン

ここでサーバパスとプロジェクトパスを指定する。

サーバパスというのはサーバ上のソースがおいてあるパス。

具体例は下記。

```
/var/www/hoge
```

プロジェクトパスというのはgitやsvnで落としてきたソースのディレクトリ

具体例は下記。

```
C:\repositoryt\hoge
```









