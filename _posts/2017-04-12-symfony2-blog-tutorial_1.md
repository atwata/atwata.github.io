---
layout: post
title: symfony2.8でブログチュートリアルを試す その1
categories: app
tags: php symfony
---

## 2017年にsymfony2.8で日本語マニュアルを元にブログチュートリアルをやってみる

symfony2のアプリを保守することになったが、重厚なフレームワークであるため、どこがどう動いているのか把握するのが辛い。

symfonyマジわからないからブログチュートリアルをやってみる。

[日本語チュートリアル](http://docs.symfony.gr.jp/symfony2/sf2-blog-tutorial/)


まずはこちらから。

[blogチュートリアル(1) Symfony2のダウンロードとインストール](http://docs.symfony.gr.jp/symfony2/sf2-blog-tutorial/01-introduction.html)

環境構築はいくつか種類があるようなので一番上の[Symfony2 の全体像](http://docs.symfony.gr.jp/symfony2/quick_tour/the_big_picture.html)
にそってやってみる

composerでのインストールパターンもあったので、下記を実行。

都合により2系でやる必要があるので2.8を指定。

```
php composer.phar create-project symfony/framework-standard-edition Symfony 2.8.0
```

apacheのバーチャルホストで動かす。設定は下記の通り。


```
/etc/httpd/conf.d

$ cat symfony2_blog.conf 
Listen 8008
<VirtualHost *:8008>
    DocumentRoot /vagrant_repos/tmp/symfony2_blog/Symfony/web
</VirtualHost>
```

接続先はこうなる。

http://localhost:8008/app_dev.php

ブラウザでアクセスすると下記のエラー。

```
You are not allowed to access this file. Check app_dev.php for more information.
```

ローカルからのみアクセスできるよう制限がかかっているよう。

仮想環境経由なので、コメントアウト。

app_dev.phpの

```
    header('HTTP/1.0 403 Forbidden');
    exit('You are not allowed to access this file. Check '.basename(__FILE__).' for more information.');
```

configの確認ができるらしい下記ページにもアクセス。

```
http://localhost:8008/config.php
```

同様のエラー発生。

```
This script is only accessible from localhost.
```

こちらも下記をコメントアウト。

```
    header('HTTP/1.0 403 Forbidden');
    exit('This script is only accessible from localhost.');
```

チュートリアルにある

```
http://localhost:8008/app_dev.php/demo/hello/Fabian
```

にアクセスしてみるが `ResourceNotFoundException` バージョン2.8では存在しないのか？

チュートリアルでは、`app/config/routing_dev.yml` にデモのルーティングがあるようだけど落としてきた2.8にはないように見える。

ここまで。バージョンが異なる4年半前の翻訳なので、日本語マニュアルの通りに全然進まない。

デモは見れなかったが、環境のセットアップはできたので次へ進む。


### データベースの設定

次はこちらの
[blogチュートリアル(2) データベースの設定](http://docs.symfony.gr.jp/symfony2/sf2-blog-tutorial/02-create-database.html)

phpは入っていたがDB環境がないのでmysqlをローカルで動作させる。

Linux環境なので下記でインストール、起動。

```
$ sudo yum install mysql-server
$ sudo /etc/rc.d/init.d/mysqld start
$ sudo chkconfig mysqld on
```

データベースの作成、権限付与。

```
$ mysql -u root
mysql> CREATE DATABASE `blogsymfony2` DEFAULT CHARACTER SET 'utf8';
mysql> GRANT ALL ON `blogsymfony2`.* TO 'blogsymfony2'@localhost IDENTIFIED BY 'blogsymfony2';
```

symfony2のデータベースの設定を記述するファイルは
`app/config/parameters.yml`
書き換える。

```
# This file is auto-generated during the composer install
parameters:
    database_host: 127.0.0.1
    database_port: null
    database_name: blogsymfony2
    database_user: blogsymfony2
    database_password: blogsymfony2
    mailer_transport: smtp
    mailer_host: 127.0.0.1
    mailer_user: null
    mailer_password: null
    secret: ThisTokenIsNotSoSecretChangeIt
```

完了。

### バンドルの作成

[blogチュートリアル(3) バンドルの作成](http://docs.symfony.gr.jp/symfony2/sf2-blog-tutorial/03-create-bundle.html)

```
php app/console generate:bundle --namespace=My/BlogBundle --format=yml
```

色々聞かれるが、全てデフォルトのままEnterキーを押して進める。

作成されたバンドルの構成ファイル

```
src/My
└── BlogBundle
    ├── Controller
    │   └── DefaultController.php
    ├── MyBlogBundle.php
    ├── Resources
    │   ├── config
    │   │   ├── routing.yml
    │   │   └── services.yml
    │   └── views
    │      └── Default
    │           └── index.html.twig
    └── Tests
        └── Controller
            └── DefaultControllerTest.php

```

上記のバンドルを作成して

```
http://localhost:8008/app_dev.php/
```

にアクセスすると

```
Hello World!
```

と表示された。デフォルトのルーティングは/(ルート)になっているのか。

デフォルトのルーティング設定はこんな感じ。

```
cat src/My/BlogBundle/Resources/config/routing.yml 
my_blog_homepage:
    path:     /
    defaults: { _controller: MyBlogBundle:Default:index }
```




### テーブルスキーマとエンティティクラス

[blogチュートリアル(4) テーブルスキーマとエンティティクラス](http://docs.symfony.gr.jp/symfony2/sf2-blog-tutorial/04-schema.html)

他の多くのフレームワークが可能なように、symfony2でも

- Entityクラスを作成
- そのEntityクラスを元にテーブルを生成

が、できるらしい。

下記のコマンドでクラスを生成。

```
$ php app/console generate:doctrine:entity --entity=MyBlogBundle:Post --format=annotation --fields="title:string(255) body:text createdAt:datetime updatedAt:datetime"
```


実際に実行してみるとエラー発生。

```
php app/console generate:doctrine:entity --entity=MyBlogBundle:Post --format=annotation --fields="title:string(255) body:text createdAt:datetime updatedAt:datetime" 
                                             
  Welcome to the Doctrine2 entity generator  

This command helps you generate Doctrine2 entities.

First, you need to give the entity name you want to generate.
You must use the shortcut notation like AcmeBlogBundle:Post.

The Entity shortcut name [MyBlogBundle:Post]: 
                                                         
  [Doctrine\DBAL\Exception\DriverException]              
  An exception occured in driver: could not find driver  

  [Doctrine\DBAL\Driver\PDOException]  
  could not find driver                

  [PDOException]         
  could not find driver  

doctrine:generate:entity [--entity ENTITY] [--fields FIELDS] [--format FORMAT] [-h|--help] [-q|--quiet] [-v|vv|vvv|--verbose] [-V|--version] [--ansi] [--no-ansi] [-n|--no-interaction] [-s|--shell] [--process-isolation] [-e|--env ENV] [--no-debug] [--] <command>
```

そういえばmysqlは入れたけどmysql-pdoを入れていなかった。

詳細は省くが、その後いろいろと試行錯誤したけど、エラーが出てmysql-pdoをインストールできなかった。

あきらめる。

テストをしている環境ではoracleにはつながるような設定を過去に行ったので、それをそのまま利用することにする。

ドライバを指定しているのは
`app/config/parameters.yml`
である。

初期設定はmysql用になっている。

```
doctrine:
    dbal:
        driver:   pdo_mysql
```

↓

```
doctrine:
    dbal:
        driver:   oci8
```

と変更する

`app/config/parameters.yml`
も利用環境に応じた値に書き換える。

再度実行。

```
$ php app/console generate:doctrine:entity --entity=MyBlogBundle:Post --format=annotation --fields="title:string(255) body:text createdAt:datetime updatedAt:datetime" 
```


無事生成完了。下記2つのファイルが作成された。

```
> Generating entity class src/My/BlogBundle/Entity/Post.php: OK!
> Generating repository class src/My/BlogBundle/Repository/PostRepository.php: OK!
```

この段階ではEntityとRepositoryクラスが作成されるのみで実際にスキーマ（テーブル）は作成されない。

実際のDBに反映するには下記コマンドを実行する。

```
php app/console doctrine:schema:create
```

実行結果

```
php app/console doctrine:schema:create
ATTENTION: This operation should not be executed in a production environment.

Creating database schema...
Database schema created successfully!
```

成功！

sqlplusでログインして見てみる

```
SQL> desc POST
 名前                                    NULL?    型
 ----------------------------------------- -------- ----------------------------
 ID                                        NOT NULL NUMBER(10)
 TITLE                                     NOT NULL VARCHAR2(255)
 BODY                                      NOT NULL CLOB
 CREATEDAT                                 NOT NULL TIMESTAMP(0)
 UPDATEDAT                                 NOT NULL TIMESTAMP(0)

SQL> 
```

POSTというテーブルが作成されている。

一旦ここまで。



