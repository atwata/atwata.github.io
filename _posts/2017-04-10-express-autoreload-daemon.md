---
layout: post
title: expressオートリロードとデーモン化
categories: middle
tags: express node.js
---

## オートリロード

ソースファイル変更時に自動でリロードしたい場合はnode-devを使用する。

- node-devインストール

```
npm install -g node-dev
```

nodeコマンドでなくnode-devでアプリを起動することで自動的に変更が反映される。

- node-devでの起動方法

```
node-dev ./bin/www
```


## デーモン化

node.jsアプリをデーモンとして起動したい場合はforeverを使用する。

- インストール

```
npm install forever -g
```

- 起動

```
cd /path/to/project
forever start app.js
```
⇒動かない。これはexpress3系

express4は

```
forever start ./bin/www
```

さらにexpress4でforever経由でnode-devを使用する場合

```
forever start -c node-dev ./bin/www
```

foreverはデフォルトでnodeコマンドを実行する。-cでコマンドをnode-devコマンドを実行するように指定。


- 確認

```
forever list
```

- 停止

```
forever stopall
```

console出力はhome配下の.forever配下のログにはいてくれる。

具体的なパスはforever listで表示。

```
forever list
info:    Forever processes running
data:        uid  command                                     script  forever pid   id logfile                    uptime        
data:    [0] JTyj /home/xxx/.nvm/versions/node/v6.5.0/bin/node bin/www 14337   14343    /home/xxx/.forever/JTyj.log 0:0:10:11.773 
```






