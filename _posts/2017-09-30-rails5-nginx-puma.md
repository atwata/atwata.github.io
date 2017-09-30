---
layout: post
title: Rails5でnginx+pumaでproduction環境を構築する
categories: middle
tags: rails nginx puma
---

## nginxのインストール

過去に書いたのこちらを参考

http://blog.atwata.com/middle/2017/04/21/centos-nginx.html

## rails側puma設定

rails5ならデフォルトで入っている

rails serverで起動しているのもpuma(webrickではない)

とりあえずpumaをproduction設定で起動

```
bundle exec puma -t 5:5 -e production -C config/puma.rb
```

### secret_key_base未設定エラー

すると、こういうエラーがでる

```
RuntimeError: Missing `secret_key_base` for 'production' environment, set this value in `config/secrets.yml
```

secret_key_baseがセットされていないというエラー

対応は下記のようにsecretを生成して、環境変数にセットする

```
bundle exec rake secret
```

を実行すると長いランダム文字列が生成させる

これはconfig/secret.ymlで管理しているのだが、productionは下記のようになっている

```
production:
  secret_key_base: <%= ENV["SECRET_KEY_BASE"] %>
```

cookieの生成に使うらしいので、セキュリティ上ファイル化は好ましくないらしい。

都度生成し、環境変数として渡す仕組みになっている

```
export SECRET_KEY_BASE=hogehoge
```

のようにする

改めて起動

```
bundle exec puma -t 5:5 -e production -C config/puma.rb
```

### assets:precompile未実施エラー

起動ログにエラーは出ていないが画面にアクセスするとエラー。
productionはデフォルトでは起動したコンソールにエラーは出さず、ログファイルに出力される。

`log/production.log` を確認すると下記の表示。

```
ActionView::Template::Error (The asset "application.css" is not present in the asset pipeline.):
```

確認するとproduction環境では自動でcssやjsをコンパイルしてくれないので（デフォルト設定がそうなっているので）
自分でプリコンパイルする必要がある。

```
bundle exec rake assets:precompile RAILS_ENV=production
```

しかし

### yarnがインストールされていないエラー

下記のエラーが発生

```
Yarn executable was not detected in the system.
Download Yarn at https://yarnpkg.com/en/docs/install
Download Yarn at https://yarnpkg.com/en/docs/install
```

yarnがないというエラー。rails5では使うらしい。

yarnとはjavascriptのパッケージマネージャ。npmの後継として利用され始めている。

下記のようにインストール。

```
npm install yarn -g
```

起動して画面が表示されるようになった、

### 静的ファイル(public)にアクセスできない

しかしcssが効かない。画像も表示されない。

確認すると

`config/environments/production.rb` が下記のような設定になっているために読み込みができなくなっていた

```
# config.public_file_server.enabled = ENV['RAILS_SERVE_STATIC_FILES'].present?
config.public_file_server.enabled = true
```

上記のように強制的にtrueにする。

ついにproduction環境のpumaでrailsアプリが起動するようになった。

## 次はnginxとのつなぎ込み

例として下記のように設定ファイルを記述する。

/etc/nginx/conf.d/myapp.conf

```
upstream puma_myapp {
    server  unix:/path/to/myapp/tmp/puma_myapp.sock;
}

server {
    listen       80;
    server_name  [ホスト名];

    access_log  /var/log/nginx/access.log;
    error_log   /var/log/nginx/error.log;

    root /path/to/myapp/;

    client_max_body_size 100m;
    error_page  404              /404.html;
    error_page  500 502 503 504  /500.html;

    location / {
        proxy_pass http://127.0.0.1:3000/;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
    }
}
```

nginx再起動

```
sudo systemctl restart nginx
```

これで無事に80番ポートでnginx経由でrailsアプリにアクセスできるようになりました。
