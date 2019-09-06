# MySQL CRUD operations

## Copy data in backup table

```sql
TRUNCATE TABLE products_backup;
INSERT INTO products_backup SELECT * FROM products;
```

## Select count on condition

Show amount on different conditions:

```sql
SELECT SUM(alias = 'base') AS base, COUNT(*) AS total FROM products_forms;
```
```
+------+-------+
| base | total |
+------+-------+
|   22 |   104 |
+------+-------+
```

## Group records by hour

```sql
SELECT count_interval_start, sum(claims_amount), sum(fast_claims_amount)
FROM claims_amount
GROUP BY HOUR(count_interval_start)
```

Instead of `HOUR` we can use: `DAY()`, `WEEK()`, `MONTH()`, `QUARTER()`, `YEAR()`

*Note:* Column `count_interval_start` has `TIMESTAMP` type here.

## Concatinate many records into one row

```sql
SELECT person_id, GROUP_CONCAT(hobbies SEPARATOR ', ')
FROM peoples_hobbies
GROUP BY person_id;
```

See [stackoverflow answer](https://stackoverflow.com/a/276949/1921272).

## Show orphan rows

```sql
SELECT a.id FROM articles a
LEFT JOIN users u ON u.id = a.user_id
WHERE u.id IS NULL
```

This query will select ID of articles that don't have authors in corresponding table.

## Delete orphan rows

```sql
DELETE a.* FROM articles a
LEFT JOIN users u ON u.id = a.user_id
WHERE u.id IS NULL
```

## Ignore insert

If you want to ignore insert that violates unique constraint in table:

```sql
INSERT IGNORE INTO products (title, weight) VALUES ('Cup', 2);
```
