# Tables

## Show tables

### Show tables and views

```sql
SHOW TABLES;
```

### Show only tables (except views)

```sql
SHOW FULL TABLES WHERE Table_Type = 'BASE TABLE';
```

## Alter table

### Add column

```sql
ALTER TABLE products ADD COLUMN category_id INT(11) UNSIGNED NOT NULL;

ALTER TABLE claims_info ADD COLUMN products_point_sales_id INT(11) UNSIGNED AFTER city_name;
```
**Important:** If MySQL version >= 5.6 you can add column [without locking](https://dev.mysql.com/doc/refman/5.6/en/innodb-online-ddl-operations.html#online-ddl-column-operations) the whole table:

```sql
ALTER TABLE tbl_name ADD COLUMN column_name column_definition, ALGORITHM=INPLACE, LOCK=NONE;
```
Concurrent DML is not permitted when adding an auto-increment column. Data is reorganized substantially, making it an expensive operation.

Some [people say](https://medium.com/practo-engineering/mysql-zero-downtime-schema-update-without-algorithm-inplace-fd427ec5b681) that INPLACE algorithm is not working with big tables.

### Drop column

```sql
ALTER TABLE products DROP COLUMN updated_at
```

### Set autoincrement

```sql
ALTER TABLE products AUTO_INCREMENT=10;
```

This query sets autoincrement column to 10. It means that if you insert a new record it's ID will be 10.
