---
layout: post
title: HTTPリクエストのContent-type
categories: web
tags: http
---

POSTメソッドでデータをリクエスト時に送信する場合にそのデータの形式を指定する。

下記はChromeAppのAdvanced Rest Client Applicationでデフォルトで用意されているものの一覧とその簡単な説明

|Content-type|説明|
|-|-|
|application/x-www-form-urlencoded|HTMLフォームでinputタグ等で送信する場合に指定。パラメータの名前と値の組を送信する。|
|application/json|データをJSON形式で送信する場合に指定。REST APIなどでよく使用される。|
|application/xml|データをXML形式で送信する場合に指定|
|application/atom+xml|データをAtom+Xmlで送信する場合に指定。AtomとはWEBページのタイトルや要約といったメタデータを管理するためのフォーマット。ブログ等でRSS的な形で使われる。|
|application/base64|データがbase64でエンコードされている場合に指定。ちなみにbase64はバイナリをテキストに変換する方式の1つ。メールの添付ファイルで利用される。|
|application/octet-stream|データがoctet-stream（8ビットバイナリ）の場合に指定。|
|application/javascript|データがjavascriptの場合に指定。|
|multipart/form-data|複数のデータ形式が混在している場合に指定。特にHTMLフォームからファイル送信を行う場合に利用。multipartは複数の形式が混じったデータを送るよ、という意味でしかなく、データの中で個別にform-dataやapplication/octet-streamに指定する必要がある。|
|multipart/alternative|複数のデータ形式が混在している場合に指定。メールでテキストとHTMLを切り替えられるようにしている場合に使用されたりする。|
|multipart/mixed|複数のデータ形式が混在している場合に指定。|
|text/plain|プレーンテキストの場合に指定。|
|text/css|CSSの場合に指定|
|text/html|HTMLの場合に指定|























