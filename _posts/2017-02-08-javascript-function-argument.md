---
layout: post
title: JavaScriptで関数を引数で渡せるのがどうにもしっくりこない
categories: app
tags: javascript
---

理解可能はところから少しずつ進めてみます。


まずは単純に。

```javascript
// hello01.js
console.log("Hello World");
```

実行するとhelloと表示される。

```
$ node hello01.js
hello
```

次に関数化。これも慣れた表現なのでわかります。

```javascript
//hello02.js
function print_word(word){
    console.log(word);
}

print_word("hello");
```

こちらも実行するとhelloです

```
$ node hello02.js
hello
```

Functionコンストラクタというもので関数を定義することができるらしいです。

```javascript
//hello03.js
var print_word = new Function("word", "console.log(word)");

print_word("hello");
```

この時点で違和感しかありません。
Functionに渡している引数がダブルクオートで囲われています。
これは文字列なのだから、変数というイメージをもてないし、ましてや命令文とみなすことができません。

これはFunctionコンストラクタを用いて、Functionオブジェクトを作成し、変数print_wordに保存しています。

変数に関数オブジェクトを保存？そう、これがしっくりこないんですね。

```
$ node hello03.js
hello
```

上記のhello03.jsは（関数オブジェクトが代入されている）変数をいきなり呼び出しています。
変数を呼び出すという作法に問題がないのでしょうか。
ちょっとずれますが、関数オブジェクトを代入していない変数を実行してみましょう。

```javascript
// hello04.js
var word = "hello";

word;
```

実行してみます。

```
$ node hello04.js
$
```

なにも起きません。エラーにもなりません。

それなら宣言すらしていない変数（というか宣言をしていなければそれは単なる文字の羅列）ならどうでしょうか。


```javascript
// hello05.js
var word = "hello";

word;

word2;
```

これはさすがにエラーになりました。

```
node hello05.js

/home/vagrant/tmp/test_jsfunc/hello05.js:5
word2;
^
ReferenceError: word2 is not defined
    at Object.<anonymous> (/home/vagrant/tmp/test_jsfunc/hello05.js:5:1)
    at Module._compile (module.js:456:26)
    at Object.Module._extensions..js (module.js:474:10)
    at Module.load (module.js:356:32)
    at Function.Module._load (module.js:312:12)
    at Function.Module.runMain (module.js:497:10)
    at startup (node.js:119:16)
    at node.js:906:3
```

ちょっと脱線しましたが、先ほどのhello03.jsは下記のように書くことも出来るそうです。

```javascript
//hello06.js
var print_word = function(word){
    console.log(word);
}

print_word("hello");
```

このほうがまだ違和感が少ないですね。
パッと見、関数の戻り値を変数に保存しているように見間違えますが、これは関数定義（関数オブジェクト）自体を変数に入れてます。

```
$ node hello06.js
hello
```

ここまでで違和感は残るものの、

- 関数を変数に保存できる
- 変数をそのまま実行できる（変数に関数が保存されている場合は、その関数が実行される）

ことがわかりました。

関数を変数に代入できるのなら、その関数を関数の引数に渡すのも問題ない気もしてきます。

実際にやってみましょう。


```javascript
// hello07.js
var print_word = function(word){
    console.log(word);
}

var exec_func = function(var_func){
    var_func;
}

exec_func(print_word("hello"));
```

「関数を保存した変数」を実行する関数を新たに定義してみました。（exec_func関数）
このexec_func経由でhelloと表示させて見ます。

```
$ node hello07.js
hello
```
ちゃんと動きますね。だんだんこういうものかと見えてきました。

では最後に上記のサンプルをこのように書き換えてみましょう。

```javascript
// hello08.js
function print_word(word){
    console.log(word);
}

function exec_func(var_func){
    var_func;
}

exec_func(print_word("hello"));
```


うーん。関数定義自体はすごく普通なのですが、関数名を関数の引数にしているところにまだ違和感が残ります。

しかしhello07.jsで示したように、関数名をつけて関数を定義した時点で、その関数名の変数に関数オブジェクトを保存したと考えれば納得できます。

念のため実行してみましょう。

```
$ node hello08.js
hello
```

問題ないですね。

以上、まだしっくりこないところもありますが慣れれば問題なくなってきそうです。

関数を引数で渡せるのがしっくりこないというのは、関数定義をした時点で、その関数の中身が定義した関数名で変数に保存されたと考えればよさそうです。
