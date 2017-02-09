---
layout: post
title: 新しい技術とかを試してみたいけどWindowsローカル環境を汚したくない
categories: environment
tags: vagrant
---

手持ちのPCはWindows8です。

たとえばNode.jsでちょっと遊んでみたいなど。

<https://github.com/joyent/node/wiki/Installation>

Node.jsの公式ページにWindows用の.msiや.exeはあるのですがこれは使いたくない。

レジストリをいじられたり、余計な環境変数とかが増えるのは避けたいからです。

解決策は色々ありますが、今回は仮想環境でVagrantを使用します。

 ## VirtualBoxのインストール

<https://www.virtualbox.org/wiki/Downloads>

実行するとインストーラーが立ち上がるので次へ次へと進むだけです。

## vagrantのインストール

インストール説明（英語）

<http://docs.vagrantup.com/v2/installation/index.html>

インストール説明（日本語）

<http://lab.raqda.com/vagrant/installation/index.html>

Windows版をダウンロードします。

<http://www.vagrantup.com/downloads>

こちらも実行するとインストーラーが立ち上がるので次へ次へと進むだけです。

## 仮想環境はわかるけどなぜvagrantを使用するか

### vagrantを使わない場合

- VirtualBoxインストール後、OSのディスクイメージをダウンロードしてマウントしてインストールして～あれこれ～
- OSインストール後も、開発に必要なミドルウェアなどをインストール
- 他のメンバ環境構築をする場合は、が前の人が残した「手順書」に従って同じように初期セットアップ作業を1つ1つ実施

### vagrantを使う場合
- vagrant initとvagrant upの2コマンドを実行するだけで、すぐ使えるLinux環境が手に入る
- OSのバージョンやソフトウェアのインストール状態のいろんな仮想環境設定用ファイルが用意されている。
- 一度作成した仮想環境をパッケージ化し、他のメンバに配布すれば、まったく同じ環境を簡単に構築できる。



## セットアップと起動

- コマンドプロンプトで任意のフォルダに移動

```
cd C:\
mkdir MyVagrant
cd MyVagrant
mkdir sample01
cd sample01
```

- 設定ファイル作成（公式ページでの記載））

```
vagrant init precise32 http://files.vagrantup.com/precise32.box
```

- 設定ファイルの作成（CentOSを入れたい場合）

```
vagrant init centos65 https://github.com/2creatives/vagrant-centos/releases/download/v6.5.3/centos65-x86_64-20140116.box
```

指定しているURLをどこで見つけたのかというと、たとえばこちら。
vagrant boxが一覧になっています。

<http://www.vagrantbox.es/>

- 仮想マシン起動・インストール

```
vagrant up
```

- ログイン

```
vagrant ssh
```

でログインできるのが本来なのですがWindowsのコマンドプロンプトはSSHに対応していないためエラーになる。
Git Bash（GitHub for windowsをインストールすると使える）などの他のツールを利用するか、SSHクライアントを利用する必要があります。

```
C:\MyVagrant\sample01>vagrant ssh
`ssh` executable not found in any directories in the %PATH% variable. Is an
SSH client installed? Try installing Cygwin, MinGW or Git, all of which
contain an SSH client. Or use your favorite SSH client with the following
authentication information shown below:

Host: 127.0.0.1
Port: 2222
Username: vagrant
Private key: C:/MyVagrant/sample01/.vagrant/machines/default/virtualbox/private_key
```

私はTeratermでログインしています。
（必要な情報は上記に出ている＋パスワードはvagrantです）

- 終了

```
vagrant halt
```
