---
layout: post
title: railsでpostgresを使ってテーブルを先に作ってそれを画面に表示
categories: app
tags: rails postgres
---

## ざっくりやること

- database.ymlでのdb設定
- テーブルと対になったModel作成
- Modelを呼び出すコントローラと表示するビュー作成

## db設定をpostgresとしてアプリ作成

```
rails new myapp -d postgresql
```

## database.yml編集

```
config/database.yml
default:
  adapter: postgresql
  encoding: unicode
  pool: 5
  # dbにアクセスするユーザ名/パスワード/ホストを指定
  username: postgres
  password: postgres
  host: 127.0.0.1

# developmentのところにdatabase名を指定
development:
  <<: *default
#  database: myapp_development
  database: postgres
```

## 空のモデル作成

- テーブル名はsampleとする

```
rails generate model sample
```

## テーブル名とPKのカラムを指定

sample.rbが作成されるのでテーブル名を指定

```
app/model/sample.rb
class Sample < ActiveRecord::Base
  self.table_name = 'sample'
end

```
## マイグレーション

これをしないとPengindMigrationErrorが発生する

```
rake db:migration
```

## コントローラ作成

```
rails generate controller samplelist index
```

## コントローラ編集

- app/controllers/samplelist_controller.rb 

```ruby
class SamplelistController < ApplicationController
  def index
    @title = 'Model Sample'
    @datas = Sample.all
  end
end
```

## ビュー編集

- app/views/samplelist/index.html.erb 

```ruby
<h2><%= @title %></h2>
<table>
<% @datas.each do |data| %>
<tr>
  <td><%= data.column1 %></td>
  <td><%= data.column2 %></td>
  <td><%= data.column3 %></td>
  <td><%= data.column4 %></td>
</tr>
<% end %>
</table>
```

## ブラウザで画面確認

- 下記にアクセス

http://localhost:3000/samplelist/index







