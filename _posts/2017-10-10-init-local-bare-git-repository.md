---
layout: post
title: ローカル環境のみでgitリポジトリを作り、履歴管理やブランチ操作を行いたい
categories: tool
tags: git
---

# ローカル環境にgitリモートブランチを作る

個人開発のちょっとしたツールやプログラムのソース管理について。

- githubを使ってもよいのだが、公開したくないタイプのソースである
- gitlabをわざわざ立ち上げるのも面倒
- 有料のgithubプライベート版を使うまでもない
- そもそもちょっとした履歴管理とローカルディレクトリ間でのpush、pullができればよい
- 具体的には開発環境で動かして試したソースをpusuして、本番環境でpullしたい
- ちなみに開発も本番も同じサーバ

コマンドのみなら、すべてローカル環境で管理できる。

以下手順。

## 共有リポジトリを作成する

ローカルサーバにあるがリモートリポジトリとして扱われる

```
$ cd /path/to
$ mkdir myapp.git
$ cd myapp.git
```

リモートリポジトリのディレクトリは.gitをつけるのが慣例らしい

```
$ git --bare init --shared
```

リモートリポジトリとして初期化するコマンド

--bareは作業ファイルをもたないpushされるための専用のリポジトリという意味

--shareはこのリポジトリを共有可能にするためのオプション。これがないとpushしてもファイルを作成できない。

## ファイルが作成されていることを確認

```
$ ls
branches  config  description  HEAD  hooks  info  objects  refs
```

## 別の場所にいってcloneする


ちなみにローカルからのcloneとはネットワーク経由での取得なので、
 `ssh localhost` で接続できるようになっている必要がある。
 
公開鍵認証でログインできるようになっていればSSHエージェント転送を有効にするのがラク

ためしにtmpディレクトリに移動しclone

```
$ cd /tmp/
$ pwd
/tmp
$ git clone localhost:/path/to/myapp.git
Cloning into 'myapp'...
warning: You appear to have cloned an empty repository.
$ cd myapp/
$ ls
```

これであとは通常のgithubに対してgit操作するようにソースの管理を行える

# 既存のローカルディレクトリを管理対象にする

ついでにこのリポジトリに対してすでに存在するディレクトリをgit管理対象とした上で、pushする方法

## 既存のファイルがあるディレクトリに移動して初期化、リモートリポジトリ追加

```
cd /path/to/hoge/
git init
git remote add origin localhost:/path/to/myapp.git
```

git fetchでエラーが出なければ正しくリモートリポジトリが設定されている

```
git fetch
```

## 既存のファイルを追加してコミット、プッシュ

```
git add .
git commit
git push origin master
```

