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

### Show tables info

```sql
SHOW TABLE STATUS
```

## Alter table

### Add column

```sql
ALTER TABLE products ADD COLUMN category_id INT(11) UNSIGNED NOT NULL;

ALTER TABLE claims_info ADD COLUMN products_point_sales_id INT(11) UNSIGNED AFTER city_name;

ALTER TABLE claims_params ADD COLUMN double_claim_id INT(11) UNSIGNED 
COMMENT 'ID of double claim'
AFTER office_sap_id;
```

**Important:** If MySQL version >= 5.6 you can add column [without locking](https://dev.mysql.com/doc/refman/5.6/en/innodb-online-ddl-operations.html#online-ddl-column-operations) the whole table

```sql
ALTER TABLE tbl_name ADD COLUMN column_name column_definition, ALGORITHM=INPLACE, LOCK=NONE;
```
Concurrent DML is not permitted when adding an auto-increment column. Data is reorganized substantially, making it an expensive operation.

Some [people say](https://medium.com/practo-engineering/mysql-zero-downtime-schema-update-without-algorithm-inplace-fd427ec5b681) that INPLACE algorithm is not working with big tables.

**Our experience**

*Example 1*

We have *10.1.37-MariaDB MariaDB Server*, this query allowed us to add new column to table (size 1.4Gb, 10.5 mln rows) without locking read, update, insert operations:

```sql
ALTER TABLE claims_cc ADD COLUMN call_result_id INT(11) UNSIGNED AFTER crm_status;
-- Query OK, 0 rows affected (4 min 10.59 sec)
```

*Example 2*

We added new column to the table without locking read, update, insert operations:

```sql
-- Table size: 2.5 Gb, 11 mln rows
ALTER TABLE claims_params ADD COLUMN double_claim_id INT(11) UNSIGNED COMMENT 'ID of double claim' AFTER office_sap_id;
```

```sql
-- Table size: 2.5 Gb (before), 256 Mb (after the column was added), 1.5 mln rows
ALTER TABLE claims_settings ADD COLUMN `double` INT(11) NOT NULL AFTER rule;
-- Query OK, 0 rows affected (41.80 sec)
```

```sql
-- Table size: 1.8 Gb, 1.4 mln rows
ALTER TABLE claims ADD COLUMN utm_content VARCHAR(255) NOT NULL AFTER ldg_utm_medium;
-- Query OK, 0 rows affected (2 min 16.18 sec)
```

**Important:** Internally MySQL creates the copy of the table and then renames it. So ensure that there is *enough free space* on the server.

### Change / Modify column

`CHANGE` is a MySQL extension to standard SQL. `MODIFY` is a MySQL extension for Oracle compatibility.

```sql
ALTER TABLE t1 CHANGE col_old col_new BIGINT NOT NULL;
```

`MODIFY` is more convenient to change the definition without changing the name because it *requires the column name only once*:

```sql
ALTER TABLE products MODIFY COLUMN type_id INT(11) UNSIGNED COMMENT 'ID of product type';
```

### Drop column

```sql
ALTER TABLE products DROP COLUMN updated_at
```

## Copy table

### Copy structure, indexes, triggers, without data

```sql
CREATE TABLE products_new LIKE products;
```
Autoincrement value will NOT be copied.

### Set autoincrement

```sql
ALTER TABLE products AUTO_INCREMENT=10;
```

This query sets autoincrement column to 10. It means that if you insert a new record it's ID will be 10.

### Insert data from old table

```sql
INSERT INTO products_new SELECT * FROM products;
```

## Rename table

```sql
RENAME TABLE products TO products_old;
```

## Optimize table

You can use OPTIMIZE TABLE to reclaim the unused space and to defragment the data file.
This operation frees the unused space and updates index statistics. It helps to shrink a table.

```sql
OPTIMIZE TABLE redirect_log;
```

## Truncate table

- This command empties a table completely. 
- For and InnoDB table, if there are no FOREIGN KEY constraints, InnoDB performs fast truncation by dropping the original table and creating an empty one with the same defenition.
- It resets AUTO_INCREMENT to 1.

```sql
TRUNCATE TABLE api_log;
```
Table size was 63 Gb. No foreign keys. This query took 1.9 sec. 

## Collation

### Get default character set and collation for DB

```sql
SELECT @@character_set_database, @@collation_database
```

### Get tables collation

```sql
SELECT TABLE_SCHEMA, TABLE_NAME, TABLE_COLLATION
FROM INFORMATION_SCHEMA.TABLES
```

### Get columns collation

```sql
SELECT TABLE_SCHEMA, TABLE_NAME, COLUMN_NAME, COLLATION_NAME 
FROM INFORMATION_SCHEMA.COLUMNS
```
