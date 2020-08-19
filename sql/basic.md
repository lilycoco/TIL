# Basic

## INSERT INTO

テーブルにデータを追加

```sql
INSERT INTO songs ('id', 'title', 'artist', 'genre', 'play_count')
VALUES (1, 'Frozen Island', 'Sandy Ryan', 'Pops', 214);
```

## SELECT

```sql
SELECT title, artist FROM songs;
```

## WHERE

`SELECT`文に条件を追加するには`WHERE`句を用います。

```sql
SELECT * from songs
WHERE play_count >= 1000 or play_count <= 500;
```

## LIKE

`SELECT`文において，文字列検索を行うには`LIKE`句を用います。この検索文字列にワイルドカードを使用することで「~から始まる」のような条件を設定できます。この問題の場合は「`'The'` から始まる」を検索したいので，`The%`を検索文字列として指定します。

```sql
SELECT * FROM songs
WHERE title LIKE 'The%';
```

## ORDER BY

`SELECT`文において，データを並び替えるために `ORDER BY` 句を用い，降順で取得するために `DESC` を指定します。  
また，先頭8件のデータなので，`LIMIT`句を用います。

```sql
SELECT * FROM songs
ORDER BY play_count DESC
LIMIT 8;
```

## COUNT\(\)

`SELECT`文において，データをグループ化するために `GROUP BY` 句を用い，データ数を集計するために `COUNT()` 関数を用います。

```sql
SELECT genre, COUNT(id) FROM songs
GROUP BY genre;
```

