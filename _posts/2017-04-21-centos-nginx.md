---
layout: post
title: centos7でNginxを動かす
categories: middle
tags: centos nginx
---

公式

https://www.nginx.com/resources/wiki/start/topics/tutorials/install/

リポジトリ作成

```
/etc/yum.repos.d/nginx.repo
```

```
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/7/x86_64/
gpgcheck=0
enabled=1
```

インストール

```
yum install nginx
```

起動

```
sudo systemctl start nginx.service
```

apacheなど他の80番ポートを使用しているプロセスは停止しておくこと

自動起動

```
sudo  systemctl enable nginx.service
```

firewallで80を開ける

```
sudo  firewall-cmd --permanent --zone=public --add-service=http
sudo  firewall-cmd --reload
```

























