---
layout: post
title: ソースでインストールしたPHPにあとからphp_ldapのライブラリを追加する
categories: middle
tags: php ldap
---

ソースからインストールしたPHPにあとからphp_ldapのライブラリを追加する

下記がPHPをインストールしたときのソースのパス

```
/home/vagrant/php-5.6.22
```

ソースがあるディレクトリでコンパイル

```
$cd /home/vagrant/php-5.6.22/ext/ldap

$ phpize
Configuring for:
PHP Api Version: 20131106
Zend Module Api No: 20131226
Zend Extension Api No: 220131226

$ ./configure --with-ldap
checking for grep that handles long lines and -e... /bin/grep
checking for egrep... /bin/grep -E
～中略～
checking for stdint.h... (cached) yes
checking for unistd.h... (cached) yes
configure: error: Cannot find ldap libraries in /usr/lib.
```

エラーになったので、openldap関連のパッケージをいれ、
さらに/usr/libの一部のライブラリにシンボリックリンク作成

```
$ sudo yum install openldap openldap-devel openldap-clients openldap-servers

$ sudo ln -s /usr/lib64/libldap.so /usr/lib/libldap.so
$ sudo ln -s /usr/lib64/libldap<em>r.so /usr/lib/libldap</em>r.so

$ ./configure --with-ldap
checking for grep that handles long lines and -e... /bin/grep
～中略～
configure: creating ./config.status
config.status: creating config.h

$ make
$ sudo make install
Installing shared extensions: /usr/local/lib/php/extensions/no-debug-non-zts-20131226/
```

php.iniの指定を変更

```
$ sudo vi /usr/local/lib/php.ini
;extension=php_ldap.dll
↓
extension=ldap.so
```

ライブラリが追加されていることを確認

```
$ php -m
```










