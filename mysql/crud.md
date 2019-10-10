# MySQL CRUD operations

## Create

### Copy data in backup table

```sql
TRUNCATE TABLE products_backup;
INSERT INTO products_backup SELECT * FROM products;
```

### Ignore insert

If you want to ignore insert that violates unique constraint in table:

```sql
INSERT IGNORE INTO products (title, weight) VALUES ('Cup', 2);
```

## Read

### Select count on condition

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

### Group records by hour

```sql
SELECT count_interval_start, sum(claims_amount), sum(fast_claims_amount)
FROM claims_amount
GROUP BY HOUR(count_interval_start)
```

Instead of `HOUR` we can use: `DAY()`, `WEEK()`, `MONTH()`, `QUARTER()`, `YEAR()`

*Note:* Column `count_interval_start` has `TIMESTAMP` type here.

### Concatinate many records into one row

```sql
SELECT person_id, GROUP_CONCAT(hobbies SEPARATOR ', ')
FROM peoples_hobbies
GROUP BY person_id;
```

**Note:** The result is *truncated* to the maximum length that is given by the `group_concat_max_len` system variable, which has a default value of 1024. The value can be set higher, although the effective maximum length of the return value is constrained by the value of `max_allowed_packet`. The syntax to change the value of `group_concat_max_len` at runtime:

```sql
SET SESSION group_concat_max_len = 2048;
```

See [stackoverflow answer](https://stackoverflow.com/a/276949/1921272).

### Show orphan rows

```sql
SELECT a.id FROM articles a
LEFT JOIN users u ON u.id = a.user_id
WHERE u.id IS NULL
```

This query will select ID of articles that don't have authors in corresponding table.

## Update

### Update table based on condition

```sql
UPDATE `products_point_sales` SET status = 0
WHERE id IN (
    SELECT p.id
    -- We use SELECT to create virtual table for query
    FROM ( SELECT id, sap_id FROM products_point_sales ) p
    LEFT JOIN `products_to_point_sales` s ON p.sap_id = s.point_sales_id
    WHERE s.id IS NULL
);
```

**Note:** We use this trick with virtual table to avoid similar error - *SQL (1093): Table 'products_point_sales' is specified twice, both as a target for 'UPDATE' and as a separate source for datauery* - in this *invalid query*:

```sql
-- NOTE: this query will produce error
UPDATE `products_point_sales` SET status = 0
WHERE id IN (
    SELECT p.id
    FROM products_point_sales p
    LEFT JOIN `products_to_point_sales` s ON p.sap_id = s.point_sales_id
    WHERE s.id IS NULL
)
```

## Delete

### Delete records by condition

```sql
DELETE FROM products WHERE created_at <= '2019-05-01';
```

**Delete records by batches**

```sql
DELETE FROM claims_emarsys_log WHERE claim_id < 70229600 ORDER BY claim_id LIMIT 1000;
```

If you use `DELETE` with `LIMIT`, you should really use `ORDER BY` to make the query deterministic; not doing so would have strange effects (including breaking replication in some cases)

### Delete orphan rows

```sql
DELETE a.* FROM articles a
LEFT JOIN users u ON u.id = a.user_id
WHERE u.id IS NULL
```
