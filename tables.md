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

### Set autoincrement

```sql
ALTER TABLE products AUTO_INCREMENT=10;
```

This query sets autoincrement column to 10. It means that if you insert a new record it's ID will be 10.
