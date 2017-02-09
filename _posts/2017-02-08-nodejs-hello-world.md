---
layout: post
title: Node.jsでHello World
categories: app
tags: javascript node.js
---

以下、解説は全て[前記事](/environment/2017/02/08/setup-vagrant-windoes-env.html)で作成したVirtualbox環境(CentOS6.5)での説明となります。


## Node.jsのインストール

```
sudo yum install nodejs npm
```

リポジトリが見つからない場合↓

sudo yum install nodejs npm --enablerepo=epel


## コンソールでHello World

```
vi helloworld.js
```

```
console.log("Hello World");
```

```
node helloworld.js
```

Hello Worldと出力されます。

そういう意味ではNode.jsというのはJavaScriptの実行環境なんですね。

## ブラウザでHello World

ブラウザに表示する場合はこうです。

```server.js
var http = require("http");

http.createServer(function(request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello World");
  response.end();
}).listen(8888);
```

```
node server.js
```


なおVagrant環境で、外部（Windows環境）の仮想マシン（Linux）の間でネットワーク通信を行う場合は、Vagrantfileの修正が必要です。

コメントアウトされたconfig.vm.networkの項目があると思います。
下記の内容でコメントを外してください。

```
config.vm.network "forwarded_port", guest: 8888, host: 8888
```

Windowsのいつも使っているブラウザで

<http://localhost:8888/>

にアクセスしてみましょう。Hello Worldと表示されるはずです。


参考

<http://www.nodebeginner.org/index-jp.html>
