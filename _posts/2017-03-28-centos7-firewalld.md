---
layout: post
title: centos7のfirewalldで特定のポート開放
categories: os
tags: centos firewalld
---

### 今回はexpressのテストで使用されるTCP3000番を対象にコマンドを実行

TCPポート3000を許可

```
firewall-cmd --zone=public --add-port=3000/tcp --permanent
```

設定再読み込み

```
firewall-cmd --reload
```

現在の設定を確認

```
firewall-cmd --list-all --zone=public
```

TCPポート3000を拒否

```
firewall-cmd --zone=public --remove-port=3000/tcp --permanent
```

一連のコマンドを実行する。

```
$ firewall-cmd --list-all --zone=public
public (default, active)
  interfaces: eth0
  sources:
  services: dhcpv6-client ssh
  ports:
  masquerade: no
  forward-ports:
  icmp-blocks:
  rich rules: 

$ sudo firewall-cmd --zone=public --add-port=3000/tcp --permanent
success
$ sudo firewall-cmd --reload
success
$ firewall-cmd --list-all --zone=public
public (default, active)
  interfaces: eth0
  sources:
  services: dhcpv6-client ssh
  ports: 3000/tcp
  masquerade: no
  forward-ports:
  icmp-blocks:
  rich rules: 

$
```


