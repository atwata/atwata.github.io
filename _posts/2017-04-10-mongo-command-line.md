---
layout: post
title: mongoコマンドライン
categories: db
tags: mongodb
---

### ローカルのmongodbに接続

```
$ mongo
```

### DB一覧表示

```
> show dbs
admin  (empty)
local  0.078GB
```

### DBのsampleに接続（存在しない場合は作成する）

```
> use sample
switched to db sample
```

### コレクション一覧表示

```
> show collections;
```

### CRUD基本

```
> db.sample.insert({id:1, name:'john'});
WriteResult({ "nInserted" : 1 })

> db.sample.insert({id:2, name:'mike'});
WriteResult({ "nInserted" : 1 })

> db.sample.find();
{ "_id" : ObjectId("57f5001cfd9ae2813134b019"), "id" : 1, "name" : "john" }
{ "_id" : ObjectId("57f50043fd9ae2813134b01a"), "id" : 2, "name" : "mike" }

> db.sample.update({id:2},{$set:{name:'pochi'}});
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

> db.sample.find();
{ "_id" : ObjectId("57f5001cfd9ae2813134b019"), "id" : 1, "name" : "john" }
{ "_id" : ObjectId("57f50043fd9ae2813134b01a"), "id" : 2, "name" : "pochi" }

> db.sample.remove({id:2});
WriteResult({ "nRemoved" : 1 })

> db.sample.find();
{ "_id" : ObjectId("57f5001cfd9ae2813134b019"), "id" : 1, "name" : "john" }
```

### すべて削除

```
> db.sample.remove({});
```

### 全件更新（カラムがあれば更新、なければカラム追加）

```
db.sample.update({}, {$set:{col1:"value1", col2:"value2"}}, { multi:true });
```

第1引数で条件を指定しないことにより全件抽出。デフォルトだと1件しか更新されないのでmulti:trueオプションを付与。

### upsert(データがあればupdate、なければinsert)

```
> db.option.find();
> db.option.update({"key" : 123}, {"key" : 123, "value1" : 100, "value2" : 200});
WriteResult({ "nMatched" : 0, "nUpserted" : 0, "nModified" : 0 })
> db.option.update({"key" : 123}, {"key" : 123, "value1" : 100, "value2" : 200}, true);
WriteResult({
        "nMatched" : 0,
        "nUpserted" : 1,
        "nModified" : 0,
        "_id" : ObjectId("582e630a6ef9bd2f11d14fb7")
})
> db.option.find();
{ "_id" : ObjectId("582e630a6ef9bd2f11d14fb7"), "key" : 123, "value1" : 100, "value2" : 200 }
> 
```

updateの第3引数をtrueに設定する。









