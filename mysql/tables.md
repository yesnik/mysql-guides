# Tables

## Alter table

### Drop column

```sql
ALTER TABLE products DROP COLUMN updated_at
```

### Set autoincrement

```sql
ALTER TABLE products AUTO_INCREMENT=10;
```

This query sets autoincrement column to 10. It means that if you insert a new record it's ID will be 10.
