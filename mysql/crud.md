# MySQL CRUD operations

## Ignore insert

If you want to ignore insert that violates unique constraint in table:

```sql
INSERT IGNORE INTO products (title, weight) VALUES ('Cup', 2);
```
