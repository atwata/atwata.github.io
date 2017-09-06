---
layout: post
title: rbenvでrubyをインストールエラー error: no acceptable C compiler found in $PATH
categories: environment
tags: ruby rbenv
---


rbenvを使って、ruby 2.4.1をインストールしようとしたが下記のエラー

```
$ rbenv install 2.4.1
Downloading ruby-2.4.1.tar.bz2...
-> https://cache.ruby-lang.org/pub/ruby/2.4/ruby-2.4.1.tar.bz2
Installing ruby-2.4.1...

BUILD FAILED (CentOS Linux 7 using ruby-build 20170726-9-g86909bf)

Inspect or clean up the working tree at /tmp/ruby-build.20170905165809.22615
Results logged to /tmp/ruby-build.20170905165809.22615.log

Last 10 log lines:
checking for ruby... false
checking build system type... x86_64-pc-linux-gnu
checking host system type... x86_64-pc-linux-gnu
checking target system type... x86_64-pc-linux-gnu
checking for gcc... no
checking for cc... no
checking for cl.exe... no
configure: error: in `/tmp/ruby-build.20170905165809.22615/ruby-2.4.1':
configure: error: no acceptable C compiler found in $PATH
See `config.log' for more details
```


`configure: error: no acceptable C compiler found in $PATH`

とのことなのでCコンパイラ(gcc)をインストール

```
sudo yum install gcc
```

再度試してみるが、今度は違うエラーが発生。

```
$ rbenv install 2.4.1
Downloading ruby-2.4.1.tar.bz2...
-> https://cache.ruby-lang.org/pub/ruby/2.4/ruby-2.4.1.tar.bz2
Installing ruby-2.4.1...

BUILD FAILED (CentOS Linux 7 using ruby-build 20170726-9-g86909bf)

Inspect or clean up the working tree at /tmp/ruby-build.20170905170227.22952
Results logged to /tmp/ruby-build.20170905170227.22952.log

Last 10 log lines:
The Ruby openssl extension was not compiled.
The Ruby readline extension was not compiled.
The Ruby zlib extension was not compiled.
ERROR: Ruby install aborted due to missing extensions
Try running `yum install -y openssl-devel readline-devel zlib-devel` to fetch missing dependencies.

Configure options used:
  --prefix=/home/vagrant/.rbenv/versions/2.4.1
  LDFLAGS=-L/home/vagrant/.rbenv/versions/2.4.1/lib 
  CPPFLAGS=-I/home/vagrant/.rbenv/versions/2.4.1/include 
```

ライブラリが足りないとコマンドまで教えてくれるので、それに従ってインストール

```
sudo yum install -y openssl-devel readline-devel zlib-devel
```

するとruby2.4.1をインストールできました！












