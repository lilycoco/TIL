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

## BEGIN/COMMIT <a id="section-title"></a>

コードの先頭で`BEGIN`文を使って**トランザクション**を開始し，末尾で`COMMIT`文を使って**変更を適用**する

```sql
BEGIN;
INSERT INTO countries (id, name) VALUES (5, 'Germany');
INSERT INTO users (id, name, country_id) VALUES (7, 'jonas', 5);
COMMIT;
```

## BEGIN/ROLLBACK <a id="section-title"></a>

コードの先頭で`BEGIN`文を使って**トランザクション**を開始し，末尾で`ROLLBACK`文を使って**変更を破棄**する

```sql
BEGIN;
INSERT INTO countries (id, name) VALUES (5, 'Germany');
INSERT INTO users (id, name, country_id) VALUES (7, 'jonas', 5);
ROLLBACK;
```

## FOREIGN KEY\(\) <a id="section-title"></a>

`users`テーブルを作り直して，`users.country_id`と`countries.id`を関連付ける**外部キー**\(`FOREIGN KEY`\)を追加する

```sql
CREATE TABLE users (
  (中略),
  FOREIGN KEY (country_id) REFERENCES countries (id)
);
```

## ON UPDATE RESTRICT <a id="section-title"></a>

参照操作を`RESTRICT`に指定すると，レコード更新時に参照整合性を満たさない操作を行うことを禁じます。`NO ACTION`は，ほとんど`RESTRICT`と同じ挙動をします。

ひとつのトランザクション内において，`RESTIRCT`の場合は影響する文の実行後に整合性の確認が行われますが，`NO ACTION`の場合はトランザクション後まで先延ばしできます。  
ただし，MySQLにおいては`NO ACTION`は`RESTRICT`と同じになります。

参照操作のデフォルト設定は，MySQLでは`RESTRICT`，SQLiteでは`NO ACITON`となります。

```sql
PRAGMA foreign_keys = ON;

-- users テーブルを作り直す
DROP TABLE users;
CREATE TABLE users (
 id integer PRIMARY KEY AUTOINCREMENT,
 name text NOT NULL,
 country_id integer,
 FOREIGN KEY (country_id) REFERENCES countries (id)
  ON UPDATE RESTRICT -- / NO ACTION / SET NULL
);

-- 動作確認用
INSERT INTO countries (id, name) VALUES (1, 'Japan');
INSERT INTO users (id, name, country_id) VALUES (1, 'mitsuru', 1);

-- わざと存在しないcountries.idを参照するよう変更する
-- エラー"Error: near line 19 FOREIN KEY constraint failed"が表示される
UPDATE countries SET id = 2 WHERE id = 1;
SELECT * FROM users LEFT OUTER JOIN countries ON countries.id = users.country_id;
```





