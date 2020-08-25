# Advanced

## JOIN

下記のように`JOIN`句を用いて`users`テーブルと`countries`テーブルを結合します。`WHERE`句をあわせて指定することで`countries.name`が`'USA'`のデータのみを抽出します。

```sql
SELECT users.name as user_name, countries.name as country_name
FROM users
JOIN countries ON users.country_id = countries.id
WHERE countries.name = 'USA';
```

## 複数テーブルのJOIN

`JOIN`句を複数回使うことで，3つ以上のテーブルを結合することができます。

```sql
SELECT *
FROM a
JOIN b ON b.id = a.b_id
JOIN c ON c.id = a.c_id;
```

## FROM

`FROM`句でサブクエリを使うことで，サブクエリの取得結果を一時的なテーブルとして扱うことができます。  
サブクエリの取得結果には別名を付ける必要がありますので，注意してください。

```sql
SELECT *
FROM (
 SELECT u.name as username, c.name as country_name
 FROM users as u
 JOIN countries as c ON c.id = u.country_id
) as sub
WHERE country_name = 'Japan';
```

## 取得カラムのサブクエリ <a id="section-title"></a>

```sql
SELECT
 id, name,
 (
  SELECT COUNT(*)
  FROM users 
  WHERE country_id = c.id
 ) as user_count
 FROM countries as c;
```

## WHERE X = サブクエリ <a id="section-title"></a>

`WHERE`句と一緒に使うことで，サブクエリの取得結果を使って絞り込みができます。

```sql
SELECT *
FROM users
WHERE country_id = (
  SELECT id
  FROM countries
  WHERE name = 'Japan'
);
```

## \# COUNT\(\)

```sql
SELECT c.name as country_name, (
  SELECT COUNT(u.id)
  FROM users as u
  WHERE u.country_id = c.id
) as users_count
FROM countries as c;
```

## WHERE EXISTS <a id="section-title"></a>

`EXISTS`はサブクエリの取得結果が1件以上存在するかのみを見るため，サブクエリで取得する値として`1`を指定することがあります。  
`0`や`*`，`c.id`，`NULL`でも結果は変わりません。

```sql
SELECT *
FROM users as u
WHERE EXISTS (
  SELECT 1
  FROM countries as c
  WHERE c.id = u.country_id AND c.name = 'Japan'
);
```

## WHERE X in <a id="section-title"></a>

`in`とサブクエリを一緒に使うことで，指定したカラムの値がサブクエリの取得結果に含まれるデータを取得することができます。

```sql
SELECT * FROM テーブル名
WHERE カラム名 in (
  SELECT文のサブクエリ
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

`SET NULL`は，参照先のレコードが存在しなくなるような操作が行われた際に，参照するカラムの値を`NULL`に変更します。  
`NOT NULL`制約が付与されている場合はエラーとなります。

`ON UPDATE CASCADE`は，参照先の値が更新されたら，参照元の値も更新されます。  
`ON UPDATE DELETE`は，参照先のレコードが削除されたら，参照元のレコードも削除されます。

```sql
PRAGMA foreign_keys = ON;

-- users テーブルを作り直す
DROP TABLE users;
CREATE TABLE users (
 id integer PRIMARY KEY AUTOINCREMENT,
 name text NOT NULL,
 country_id integer,
 FOREIGN KEY (country_id) REFERENCES countries (id)
  ON UPDATE RESTRICT -- / NO ACTION / SET NULL / CASCADE / ON DELETE SET NULL
);

-- 動作確認用
INSERT INTO countries (id, name) VALUES (1, 'Japan');
INSERT INTO users (id, name, country_id) VALUES (1, 'mitsuru', 1);

-- わざと存在しないcountries.idを参照するよう変更する
-- エラー"Error: near line 19 FOREIN KEY constraint failed"が表示される
UPDATE countries SET id = 2 WHERE id = 1;
SELECT * FROM users LEFT OUTER JOIN countries ON countries.id = users.country_id;
```

