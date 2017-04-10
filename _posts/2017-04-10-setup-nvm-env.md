---
layout: post
title: nvmを利用してnodeをインストールするまでの設定
categories: middle
tags: nvm node.js
---

下記のような流れ。

- nvm.gitを~/.nvmとしてclone
- .bash.rcにnvm.shを追加
- nvm install

インストールログ。

```
$ git clone git://github.com/creationix/nvm.git ~/.nvm
Cloning into '/home/xxx/.nvm'...
remote: Counting objects: 5484, done.
remote: Total 5484 (delta 0), reused 0 (delta 0), pack-reused 5484
Receiving objects: 100% (5484/5484), 1.55 MiB | 572.00 KiB/s, done.
Resolving deltas: 100% (3321/3321), done.
$ echo . ~/.nvm/nvm.sh >> ~/.bashrc
$ . .bashrc
$ nvm --version
0.32.0
$ nvm ls-remote
        v0.1.14
        v0.1.15
        v0.1.16
        v0.1.17
        v0.1.18
～～中略～～
         v4.4.5   (LTS: Argon)
         v4.4.6   (LTS: Argon)
         v4.4.7   (LTS: Argon)
         v4.5.0   (LTS: Argon)
         v4.6.0   (Latest LTS: Argon)
         v5.0.0
         v5.1.0
～～中略～～
         v6.4.0
         v6.5.0
         v6.6.0
         v6.7.0
$ nvm install stable
################################################################## 100.0%
Computing checksum with sha256sum
Checksums matched!
Now using node v6.7.0 (npm v3.10.3)
Creating default alias: default -> stable (-> v6.7.0)
$
```








