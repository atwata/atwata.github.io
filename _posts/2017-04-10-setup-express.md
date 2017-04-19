---
layout: post
title: expressの初期環境構築
categories: app
tags: node.js express
---

## expressを利用可能になるまでの環境構築

### express-generatorインストール

```
npm install -g express-generator
```

### プロジェクトディレクトリに移動

```
cd xxx_project/
```

### フレームワークのベース作成

```
express
```

### パッケージをインストール

```
cd . && npm install
```

### サーバ起動

```
DEBUG=xxx_project:* npm start
```

#### 違うポートで起動したい場合は環境変数PORTに番号を指定してからnpm startする

```
exporet PORT=3001
```

### ブラウザでアクセスしてみる

```
http://[サーバ名]:3000/
```

上記にアクセスしてwelcomeが表示されればOK



