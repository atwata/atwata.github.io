---
layout: post
title: mongodb環境構築メモ
categories: middle
tags: mongodb
---

## リポジトリ準備

`/etc/yum.repos.d/mongodb.repo`を下記のように設定。


```
[mongodb]
name=MongoDB Repository
baseurl=http://downloads-distro.mongodb.org/repo/redhat/os/x86_64/
gpgcheck=0
enabled=1
```

## インストール

```
yum install mongodb-org
```

## サービス起動

```
/etc/rc.d/init.d/mongod start
```

## 自動起動

```
chkconfig mongod on
```






