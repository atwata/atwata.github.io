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

## ファイル修正からリモートにpushまで

### ステータス確認

```
git status
```

### コミット前の差分確認

```
-- 行単位で差分を表示
git diff
```

```
-- 単語単位で色分けして差分を表示（変更内容によってはこちらのほうが見やすい）
git diff --color-words
```


### 変更をまとめてadd

```
git add .
```

### コメントを指定してcommit

```
git commit -m "add express files"
```

### masterにpush

```
git push origin master
```

## ブランチ関連

### ブランチの作成

今チェックアウトしているブランチを元に作成される

```
git branch <branch_name>
```



### 不要なローカルブランチを削除

```
git branch -d <branch_name>
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












