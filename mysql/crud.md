# MySQL CRUD operations

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
SELECT counted_at, sum(claims_count), sum(fast_claims_count) FROM claims_count
GROUP BY HOUR(counted_at)
```

Instead of `HOUR` we can use: `DAY()`, `WEEK()`, `MONTH()`, `QUARTER()`, `YEAR()`

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
