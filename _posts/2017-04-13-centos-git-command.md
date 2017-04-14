---
layout: post
title: centos上でコマンドラインgit操作
categories: tool
tags: git
---

## 初回githubからclone

```
git clone https://github.com/[username]/[repositoryname]
```

## 開発中によく使うコマンド

ステータス確認

```
git status
```

変更をまとめてadd

```
git add .
```

コメントを指定してcommit

```
git commit -m "add express files"
```

masterにpush

```
git push origin master
```

## push時にユーザー名の入力を省略したい

```
cd /path/to/project
$ vi .git/config
```

URLにユーザー名を指定する。

ユーザー名の@はURLエンコードし%40とする

```
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[remote "origin"]
        url = https://[username]@github.com/[account]/[repository]
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
        remote = origin
        merge = refs/heads/master
```












