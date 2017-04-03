---
layout: post
title: githubでhttpsでcloneしたリポジトリをsshに変更する
categories: tool
tags: github
---

## 前提条件

- ローカルに秘密鍵が用意されていること
- githubに公開鍵が登録されていること

## 手順

### 現在の設定確認

```
$ git remote -v
origin  https://xxx@github.com/atwata/repository_name.git (fetch)
origin  https://xxx@github.com/atwata/repository_name.git (push)
```

↑ urlがhttpsになっている。これをssh://にしたい。

### 変更

```
$ git remote set-url origin git@github.com:xxx/repository_name.git
```

※ssh用URLはgithubのサイトから取得



