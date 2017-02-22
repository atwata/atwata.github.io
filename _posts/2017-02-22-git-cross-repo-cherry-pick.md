---
layout: post
title: gitで異なるリポジトリ間でcherry-pickをする
categories: tool
tags: git
---

## cherry-pickを異なるリポジトリ間で行う

準備

github上で下記2つのリポジトリを作成する。

- cross-repo-common(共通リポジトリ)
- cross-repo-a(Aリポジトリ)

commonリポジトリのcommonディレクトリ配下の内容をリポジトリAに取り込む手順。

cloneする。

```
git clone git@github.com:atwata/cross-repo-common.git
git clone git@github.com:atwata/cross-repo-a.git
```

```
$ cd cross-repo-common/
$ mkdir common
$ cd common/
```

common.txtを編集して下記を入力

```
hello common1
hello common2
```

```
$ cat common.txt
hello common1
hello common2
```

```
$ git add .
$ git commit -m "add common.txt"
$ git push
```

共通リポジトリをリモートに追加

```
$ git remote add cross-repo-common git@github.com:atwata/cross-repo-common.git
$ git remote
cross-repo-common
origin
```

fetchする

```
$ git fetch cross-repo-common
warning: no common commits
remote: Counting objects: 4, done.
remote: Total 4 (delta 0), reused 4 (delta 0), pack-reused 0
Unpacking objects: 100% (4/4), done.
From github.com:atwata/cross-repo-common
 * [new branch]      master     -> cross-repo-common/master
 ```


branchの状態

```
$ git branch -a
* master
  remotes/cross-repo-common/master
  remotes/origin/master
```



リモートのログを確認

```
$ git log cross-repo-common/master
commit a38778b0b93ad2ec95e720970ce3b2ee65a8583f
- 略 -
    add common.txt
```

ハッシュを確認し、それをcherry-pickする。

```
$ git cherry-pick a38778b0b93ad2ec95e720970ce3b2ee65a8583f
[master 49ab301] add common.txt
 Date: Wed Feb 22 22:36:46 2017 +0900
 1 file changed, 2 insertions(+)
 create mode 100644 common/common.txt
```

cross-repo-commonに対して実行した変更をcross-repo-aに取り込めた。

```
cat common/common.txt
hello common1
hello common2
```

pushしてリモートのcross-repo-aにも反映

```
$ git push
```

異なるリポジトリ間であっても、共通のファイル群などがある場合に効果的な方法。

異なるリポジトリのcommonディレクトリ配下をまとめてマージなどはできないが、
commonリポジトリの「特定のコミット」をリポジトリAに反映させることはできる。

共通のディレクトリ配下だけを反映したいという場合は、共通のディレクトリ配下だけを
追加・更新したコミットを作り、そのコミットを別のリポジトリにcherry-pickすれば良い。
