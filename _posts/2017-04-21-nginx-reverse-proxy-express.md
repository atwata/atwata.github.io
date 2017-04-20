---
layout: post
title: Nginxのリバースプロキシでサブディレクトリを作ってNode.js(express)を動かす
categories: middle
tags: centos nginx
---

### やりたいこと

http://ホスト名/アプリ名/

をルートにしてWEBサービスを動かしたい

いろいろ方法はありそうだが、そのうちの1つとして。

今回はサブディレクトリはmyappとした場合について説明する。

### expressアプリの設定

あらかじめ下記のURLでアプリが動くようにしておく。

```
http://localhost:3000/myapp/
```

### app.jsの修正

静的ファイル（アセット）を指定

```
app.use('/myapp', express.static(path.join(__dirname, 'public')));
```

アプリケーションのルートを指定

```
app.use('/myapp', routes);
```

- 静的ファイル（アセットの読み込み）やリンク、formのaction先を相対パスにする

例

```
link(rel='stylesheet', href='stylesheets/style.css')
script(type='text/javascript', src='javascripts/myapp.js')
```

絶対パスにする場合はサブディレクトリmyappまで指定する

### nginxにリバースプロキシ設定

専用の設定ファイルを作成し、

```
/etc/nginx/conf.d/myapp.conf
```

下記のように記述する。

```
server {
    listen 80;
    server_name [ホスト名];
    location /myapp/ {
        proxy_pass http://127.0.0.1:3000/myapp/;
    }
}
```





























