# Database

## CREATE TABLE

テーブル作成は`CREATE TABLE`文で行います。  
整数型は`integer`で表し，主キーは`primary key`で表します。`primary key`制約を付与すると，データ挿入時にそのカラムの値を省略した際に，自動的に重複しない値\(連番\)が割り振られます。

```sql
	CREATE TABLE accounts (
   id integer primary key,
   subject varchar(64),
   summary text,
   amount integer,
   created_at date
  );
```

## HAVING

グループ化したデータ列に対して絞り込みを行う場合には`HAVING`句を用います。

```sql
SELECT subject, SUM(amount) AS subject_amount
FROM accounts
GROUP BY subject
HAVING subject_amount < 0
ORDER BY subject_amount DESC;
```

## FOREIGN KEY

外部キー制約を付与するには，`FOREIGN KEY ~ REFERENCES`を用いて以下のように記述します。

```sql
CREATE TABLE テーブル1 (
  id integer primary key,
  ...
);

CREATE TABLE テーブル2 (
  ...
  外部キー名 integer,
  FOREIGN KEY(外部キー名) REFERENCES テーブル1(id)
);
```

## JOIN

外部キー制約を付与したテーブルのデータを取得するには，`JOIN`句を用います。

```sql
SELECT accounts.*, subjects.title as subject FROM accounts
JOIN subjects ON accounts.subject_id = subjects.id;
```

## INSERT INTO

`SELECT文`で取得したデータを`INSERT`文で挿入することができます。

```sql
INSERT INTO accounts (subject_id, summary, amount, created_ad)
SELECT id, 'dinner with friends', -2000, '2019-01-10'
FROM subjects
WHERE title = 'food';

-- 確認用クエリ
SELECT * FROM accounts;
```

## UPDATE

以下のように，`UPDATE`文と`WHERE`句を用いることで`title`の値を更新することができます。

```sql
UPDATE subjects 
SET title = 'meal'
WHERE title = 'food';
```

## DELETE

以下のように，`DELETE`文の`WHERE`句でサブクエリを使うことで，指定した科目の`accounts`データを削除することができます。

```sql
DELETE FROM accounts
WHERE subject_id = (
  SELECT id FROM subjects
  WHERE title = 'food'
);
```

## SUM\(\)

外部キーを用いた場合にも`GROUP BY`句を用いてグループ化することができます。

```sql
SELECT subjects.title as sub, SUM(accounts.amount)
FROM accounts
JOIN subjects ON accounts.subject_id = subjects.id
GROUP BY sub;
```

* `subjects`テーブルの`title`でグループ化
* グループ化したデータ群のうち，ひもづいている`accounts`テーブルのデータ数が`3`件**未満**のデータを取得

```sql
SELECT title FROM subjects
JOIN accounts ON accounts.subject_id = subjects.id
GROUP BY subjects.title
HAVING COUNT(accounts.id) < 3;
```

## NOT EXISTS

`subjects`テーブルから`accounts`テーブルに紐付いているデータが存在しないデータの`title`を取得

```sql
SELECT title FROM subjects as sub
WHERE NOT EXISTS (
  SELECT 1 FROM accounts
  WHERE subject_id = sub.id
);
```



