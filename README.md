# README
![トップページ](https://user-images.githubusercontent.com/62546632/81910490-01f51d80-9607-11ea-9a1a-a94e09952d99.png)

# このアプリについて
・メルカリクローンのフリーマーケットアプリ
<br>
・TECH CAMP 73期短期メンバー５名にて作成
<br>
・制作期間:2020年　4月21日〜5月13日
<br>
・使用Gemや機能、各担当箇所は以下に記載

# 制作メンバー
<a href="https://github.com/ryuso0720"><h2>ryuso0720</h2></a>
<h3>実装内容</h3>
ER図作成
<br>
README編集
<br>
テーブル作成
<br>
アソシエーション記述
<br>
商品詳細ページ_マークアップ
<br>
商品一覧表示_マークアップ&サーバーサイド
<br>
商品カテゴリー機能_マークアップ&サーバーサイド



# freemarket_sample_73g DB設計

## usersテーブル
|Column|Type|Options|
|------|----|-------|
|family_name|string|null: false|
|first_name|string|null: false|
|family_name_kana|string|null: false|
|first_name_kana|string|null: false|
|birthday|date|-|
|nickname|string|null: false|
|icon_image|string||
|email|string|false, unique: true|
|password|string|null: false|
|profile|text||
|phone_authy|string|null: false, unique: true, limit: 11|
### Association
- has_one: address, dependent: :destroy
- has_one: card, dependent: :destroy
- has_many: comments
- has_many :likes
- has_many :items, foreign_key: "saler_id", class_name: "Item"
- has_many :items, foreign_key: "buyer_id", class_name: "Item"
- has_many: orders

## addressesテーブル
|Column|Type|Options|
|------|----|-------|
||user_id|references|null: false, foreign_key: true|
|postal_code|string|null: false|
|prefecture|string|null: false|
|city|string|null: false|
|block|string|-|
|building|string|-|
|phone|string|-|
## Association
- belongs_to :user

## cardsテーブル(gem 'payjp'を使用)
|Column|Type|Options|
|------|----|-------|
||user_id|references|null: false, foreign_key: true|
|customer_id|string|null: false|
|card_id|string|null: false|
## Association
- belongs_to :user

## itemsテーブル
|Column|Type|Options|
|------|----|-------|
|seller_id|references|foreign_key: true|
|name||string|null: false, unique: true, index:true|
|category_id|references|null: false, foreign_key: true|
|brand_id|references|null: false, foreign_key: true|
|price|integer|null: false|
|description|text|null: false|
|condition|string|null: false|
|delivery_charge|string|null: false|
|delivery_method|string|null: false|
|region|string|null: false|
|delivery_day|string|null: false|
|size|string|null: false|
|state|boolean|default: false,null: false|
|buyer_id|referencesforeign_key: true|
|shipping_method|string|null: false|
### Association
- has_many: comment
- has_many: likes, dependent: :destroy
- has_many: images, dependent: :destroy
- belongs_to :buyer, class_name: "User"
- belongs_to :seller, class_name: "User"
- belongs_to :category
- belongs_to :brand


## imagesテーブル
|Column|Type|Options|
|------|----|-------|
|image|strig|null: false|
|item_id|references|foreign_key: true|
### Association
- belongs_to :item

## categoriesテーブル
|Column|Type|Options|
|------|----|-------|
|name|strig|null: false|
### Association
- has_many: items
- has_many: category_brands
- has_many: brands, through: category_brands

## brandsテーブル
|Column|Type|Options|
|------|----|-------|
|name|strig|null: false|
### Association
- has_many: items
- has_many: category_brands
- has_many: categories, through: :category_brands

## category_brandsテーブル
|Column|Type|Options|
|------|----|-------|
|category_id|references|null: false, foreign_key: true|
|brand_id|references|null: false, foreign_key: true|
### Association
belongs_to :category
belongs_to :brand

## likesテーブル
|Column|Type|Options|
|------|----|-------|
|item_id|references|null: false, foreign_key: true|
|user_id|references|null: false, foreign_key: true|
### Association
belongs_to :item
belongs_to :user

## commentsテーブル
|Column|Type|Options|
|------|----|-------|
|text|text|null: false|
|item_id|references|null: false, foreign_key: true|
|user_id|references|null: false, foreign_key: true|
### Association
belongs_to :item
belongs_to :user

## ordersテーブル
|Column|Type|Options|
|------|----|-------|
|item_id|references|null: false, foreign_key: true|
|user_id|references|null: false, foreign_key: true|
### Association
belongs_to :user