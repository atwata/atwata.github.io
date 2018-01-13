---
layout: post
title: centos7へのpostgresqlのインストール・初期設定
categories: middle
tags: postgresql centos7
---


## インストール

yumでインストールする。

```
yum install postgresql-server
```

yumでインストールすると

- postgresql-server
- postgresql
- postgresql-lib

がまとめてインストールされる

## データベース初期化

```
postgresql-setup initdb
```

/var/lib/pgsql/dataに設定ファイルが作成される。

## 自動起動設定

```
systemctl enable postgresql.service
```

## postgresql起動

```
systemctl start postgresql.service
```

##  postgresqlにログイン

postgresqlのログイン方法はいろいろあるが、ここではOSユーザpostgresqlでログインしてみる。postgresユーザならpostgresqlに対するパスワードの認証なしでDBにアクセスすることができる。

```
su - postgres
psql
```

