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

## Ignore insert

If you want to ignore insert that violates unique constraint in table:

```sql
INSERT IGNORE INTO products (title, weight) VALUES ('Cup', 2);
```
