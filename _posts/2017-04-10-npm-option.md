pppp---
layout: post
title: npmのオプション
categories: app
tags: node.js npm
---

## npm install

パッケージをインストールする

```
-g
```

グローバルインストール扱いとなり、どこからでもコマンドを利用可能となる。

コマンドから直接利用するタイプのパッケージはこちらを利用

アプリから呼び出すタイプのパッケージは-gなしとする

```
--save
```

package.json の dependencies に追記される。

## npm list

パッケージの一覧を表示する

npm list時に、下記のエラーが出た場合はそのパッケージがpakage.jsonに追加されていない。
不要なら削除し、必要ならnpm install パッケージ --save(--save-dev)をする。

```
npm ERR! extraneous: moment@2.15.1 /home/xxx/node_modules/moment
npm ERR! extraneous: request@2.78.0 /home/xxx/node_modules/request
```




