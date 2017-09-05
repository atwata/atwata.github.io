---
layout: post
title: LAN内の別マシンからvagrantの仮想マシンにsshでアクセスする方法
categories: environment
tags: vagrant
---

## ブリッジ接続を追加する

Vagrantfileの下記のコメントを外す

```
config.vm.network "public_network"
```

その状態でvagrant upすると自動的にブリッジネットワークが追加される。

私の環境の例

ちなみにvagrantがあるホストマシンのIPは192.168.10.14

仮想マシン上でIPを確認すると192.168.10.13が追加されている。

```
[vagrant@localhost ~]$ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 52:54:00:ad:a0:96 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic eth0
       valid_lft 86377sec preferred_lft 86377sec
    inet6 fe80::5054:ff:fead:a096/64 scope link 
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 08:00:27:bf:fc:d5 brd ff:ff:ff:ff:ff:ff
    inet 192.168.10.13/24 brd 192.168.10.255 scope global dynamic eth1
       valid_lft 86377sec preferred_lft 86377sec
    inet6 fe80::a00:27ff:febf:fcd5/64 scope link 
       valid_lft forever preferred_lft forever
```

この192.168.10.13を指定して、LAN内の別のマシンからsshでアクセスできる！





