# DB

DBMSはDBの定義や操作制御などの機能をもつミドルウェア

## 正規化

![正規化](http://kanauka.o-oku.jp/4_jyohosystem/visio/seikika.gif)

繰り返し部分の切り出す＝第１正規型

### 関係従属

主キーが決まれば列の値が一意に定まる関係を関数従属という。
主キーに関数従属した表を切り出すと第２正規型
主キー以外に関数従属している表を切り出すと第３正規型となる


## 演算

射影は列を取り出す演算　SELECT
選択は特定のレコードのみを取り出す演算　SELECT + WHERE

## 3層スキーマ

データの独立性を高める目的

- 外部スキーマ　ユーザがアプリケーションから見るデータ群
- 概念スキーマ　DBそのもの、テーブル設計、キー設定、リレーションなど開発者が見る部分
- 内部スキーマ　ディスクにどう格納するかの設計

![schema](https://image.itmedia.co.jp/ait/articles/1703/01/r20_04-01.PNG)
![description](https://image.slidesharecdn.com/09-131209212904-phpapp01/95/09-10-638.jpg?cb=1465285938)

## 主キー

テーブル内で内容が重複しない＋空でないキー
複数の列を組み合わせれば一意になるキーは**複合キー**という

## 外部キー

表と表を関係付けるため、他の表の主キーを参照すること
![foreign_key](https://xtech.nikkei.com/it/members/ITPro/ITBASIC/20000919/1/zu02.gif)

## SQL

```sql

-- HAVING
-- Group化した中から条件に合致するものだけを抽出

select id, name ,avg(value) from table
where in id (1,2,3,4)
group by name
  having avg(value) > 200

```

## トランザクション管理と排他制御

同時にカラムやテーブルを編集してしまったときに不整合が起きないようにする仕組み

### トランザクション

```mermaid
graph LR
在庫数確認
-->在庫数更新
---|一連の流れ:トランザクション|在庫数確認
```
### 排他制御

データをロックする機能
- 共有ロック　他のユーザも読むことはできる
- 専有ロック 読み書きできない

複数のトランザクションでお互いにロックを掛けてしまい、永遠に解除を待ち続ける状態を**デッドロック**という

## ACID特性

- Atomicity
- Consistency
- Isolation
- Durability

![acid](https://www.seplus.jp/dokushuzemi/wp-content/uploads/2018/12/architecture_rdbms_slide_37.png)
