---
layout: post
title: postgresqlにログインできない（Ident authentication failedエラー）
categories: db
tags: postgresql
---


## 下記のエラーが出てログインできない場合の対処法

```
psql: FATAL:  Ident authentication failed for user "postgres";
```

通常イメージするユーザ名とパスワードでログインできるようにする。

### パスワードを設定する

まずはOSのpostgresユーザにスイッチし、postgres上のパスワードを設定する

```
$ su - postgres
```

```
$ psql
```

```
postgres=# alter role postgres with password 'postgres';
```

### ログイン周りの設定をきちんとする

```
sudo vi /var/lib/pgsql/data/pg_hba.conf
```



```
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
#local   all             all                                     peer
local   all             all                                     md5
# IPv4 local connections:
#host    all             all             127.0.0.1/32            ident
host    all             all             127.0.0.1/32            md5
```

上記のMETHODを適宜変更する

- md5だと通常イメージするパスワード認証
- trustだとOS認証（OSユーザで認証済みならパスワードなしでログインできる）

### TYPEがlocalのところはこうやってログインするときの設定

```
psql -U postgres
```

### TYPEがhostのところはこうやってログインするときの設定

```
psql -U postgres -h 127.0.0.1
```

### postgresql再起動

設定ファイルを変更したらpostgresqlを再起動しないと反映されない

```
$ sudo service postgresql restart
```



