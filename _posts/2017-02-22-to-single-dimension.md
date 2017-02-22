---
layout: post
title: PHPで多次元配列を1次元配列にする(RecursiveIteratorIterator使用)
categories: app
tags: php
---

## 多次元配列を.で連結して1次元配列にしたい

やりたいことはこんな感じです。

多段の連想配列（普通の配列でも可）を、それぞれのキーを.で順に連結して一意の1次元配列にします。

### インプット

```
Array
(
    [parent] => Array
        (
            [childA] => A
            [childB] => B
            [childC] => Array
                (
                    [grandchildX] => X
                    [grandchildY] => Y
                )

        )

)
```

### アウトプット

```
Array
(
    [parent.childA] => A
    [parent.childB] => B
    [parent.childC.grandchildX] => X
    [parent.childC.grandchildY] => Y
)
```



## ソースコード

```
<?php

// 多次元配列のキーを.で連結して1次元配列にする。
function toSingleDimension(array $multiArray)
{
    $singleArray = array();

    $iterator = new RecursiveIteratorIterator(new RecursiveArrayIterator($multiArray), RecursiveIteratorIterator::SELF_FIRST);

    for ($iterator; $iterator->valid(); $iterator->next()) {
        $key = $iterator->key();
        $value = $iterator->current();
        $depth = $iterator->getDepth();
        $keymap[$depth] = $key;

        if (is_array($value)) {
            continue;
        }

        if ($depth > 0) {
            $ancestorKeys = array();
            foreach (range(0, $depth) as $depthKey) {
                $ancestorKeys[] = $keymap[$depthKey];
            }
            $key = implode('.', $ancestorKeys);
        }

        $singleArray[$key] = $value;
    }

    return $singleArray;
}

        
$data = array('parent' => array('childA' => 'A', 'childB' => 'B', 'childC' => array('grandchildX' => 'X', 'grandchildY' => 'Y')));

print_r($data);
print_r(toSingleDimension($data));
```

## やっていること

- RecursiveIteratorIteratorを使って、全ての配列の要素を順になめます。
- その際配列のキーを保存しておきます
- 要素が配列でなかったら（葉ノード）だったら保存しておいたキーを.で連結してキーにして新たな配列に詰めます


RecursiveIteratorIteratorの公式ドキュメント

[http://php.net/manual/ja/class.recursiveiteratoriterator.php]

コンストラクタのオプション

- RecursiveIteratorIterator::LEAVES_ONLY - デフォルト。イテレーションで葉ノードだけを取り上げる
- RecursiveIteratorIterator::SELF_FIRST - イテレーションで葉と親を (親から先に) 取り上げる
- RecursiveIteratorIterator::CHILD_FIRST - イテレーションで葉と親を (葉から先に) 取り上げる
