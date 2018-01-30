---
layout: post
title: go言語のインストールからhello worldまで
categories: app
tags: golang centos7
---

## インストール

centos7環境でgo言語はyumでインストールできる。

```
$ sudo yum install golang
```

インストールできたのでバージョンを確認

```
$ go version
go version go1.8.3 linux/amd64
```

## Hello Worldの確認

`hello.go` ファイルを作成する

日本語の確認もしたかったので、公式ドキュメントにあるソースをコピペ。

公式→ [http://golang-jp.org/](http://golang-jp.org/)

```
package main

import "fmt"

func main() {
	fmt.Println("Hello, 世界")
}

実行する。

$ go hello.go 
go: unknown subcommand "hello.go"
```

`go ファイル名` ではなく `go run ファイル名` らしい

```
$ go run hello.go 
Hello, 世界
```

実行ファイルを作ることもできる（スクリプト言語と異なり、golangがない環境でも生成されたバイナリを実行可能）


```
$ go build hello.go 
$ ls
hello  hello.go
$ ./hello  
Hello, 世界
```

Linux上でWindows向けにコンパイルすることもできる

※-oで出力ファイル名を指定

```
$ GOOS=windows GOARCH=amd64 go build -o ./hello.exe
$ ls
hello  hello.exe  hello.go
```

このhello.exeとhelloをWindows環境に持っていって実行してみる

```
c:\tmp>dir
2018/01/30  18:38    <DIR>          .
2018/01/30  18:38    <DIR>          ..
2018/01/30  17:56         1,551,539 hello
2018/01/30  18:37         1,627,136 hello.exe

c:\tmp>hello
Hello, 世界

c:\tmp>hello.exe
Hello, 世界
```

あれ？helloもhello.exeも両方実行できる。なんでだろう。
helloのほうはエラーになることを期待してたのに




