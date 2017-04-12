---
layout: post
title: いい加減apacheの設定項目をちゃんと覚える
categories: middle
tags: apache
---

### たとえばこうなっていた場合どういう意味か

```
Listen 9999  --9999ポートでapacheが待ち受ける
<VirtualHost *:9999>  --このポートでアクセスを受けたら下記のパスのドキュメントを表示
    DocumentRoot /path/sample  --表示したいファイル群を配置するパス
    <Directory "/path/sample">  --このパスには下記の設定が有効になる
        Options FollowSymLinks  -- シンボリック経由でのアクセスを許可する
        AllowOverride All  -- .htaccessでの設定の上書きを有効にする
        Order allow,deny  --アクセス制限をかける順番。許可してから拒否する。
        Allow from All  -- すべてのアクセスを許可する
        EnableMMAP Off  -- メモリマッピングをOFFにする（vagrant環境ではOFFにしないと静的ファイルが更新されない）
        EnableSendfile Off  -- カーネルの sendfile サポートを OFFにする（↑と同様仮想環境ではOFFにする）
    </Directory>
</VirtualHost>
```









