---
layout: post
title: rbenvでrubyをインストール
categories: environment
tags: ruby rbenv
---


## rbenvのインストール

```
$ git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
$ git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
$ echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
$ source ~/.bash_profile
$ which rbenv
~/.rbenv/bin/rbenv
$ rbenv --version
rbenv 1.1.1-6-g2d7cefe
```

###  zshの場合は.bash_profileではなく.zshrcに下記を追記

```
export PATH="$PATH:$HOME/.rbenv/bin"
eval "$(rbenv init - zsh)"
```

### rbenvのバージョンが上がった時

git cloneしたパスに移動し、git pullすればOK。

## rubyのインストール


### インストール可能なrubyのバージョン一覧表示

```
$ rbenv install -l
```


### インストール

```
$ rbenv install 2.4.2
$ rbenv global 2.4.2
$ rbenv rehash
```

### インストールバージョンの確認

```
$ ruby -v
```
