# MySQL Foreign Keys

## Add foreign key

**Add foreign key without locking tables**

```sql
set FOREIGN_KEY_CHECKS=0;

ALTER TABLE claims_import ADD CONSTRAINT fk_claims_import_call_result_id 
FOREIGN KEY (call_result_id) REFERENCES call_results (id);
-- Query OK, 0 rows affected (0.00 sec)

set FOREIGN_KEY_CHECKS=1;
```

This setting disables foreign key checks for current session only.
Setting [FOREIGN_KEY_CHECKS](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_foreign_key_checks): If set to `1` (the default), foreign key constraints for InnoDB tables are checked. If set to `0`, foreign key constraints are ignored, with a couple of exceptions.

**Important notes:** 

1. Be careful with adding foreign key on production, because it might be a heavy operation for you database. 
For example, we created empty table and tried to create foreign key to existing big table (12 Gb size, 9 mln rows). 
This operation *made our database to shut down*. Here is server info:

- mysql Ver 15.1 Distrib 10.1.37-MariaDB, for Linux (x86_64) using readline 5.1.
- RAM 20Gb

2. We tried to add foreign key on big table `claims_import` (220 Mb, 0.5 mln rows) to small one `call_results` (20 rows):

```sql
ALTER TABLE claims_import 
ADD CONSTRAINT fk_claims_import_call_result_id FOREIGN KEY (call_result_id) 
REFERENCES call_results (id);
-- Query OK, 597870 rows affected (14.43 sec)
```

During running this query *both tables will be locked for modifications*:

- in the big table `claims_import`:
  - we CANNOT insert, update, delete records 
  - we CAN read
- in small table `call_results`:
  - we CANNOT insert, update, delete records 
  - we CAN read

**Note:** We tried to add foreign key to another table (that has 20 inserts per minute), but got an error:
*ERROR 1823 (HY000): Failed to add the foreign key constraint 'sales/fk_claims_sms_call_result_id' to system tables*.

To fix this try to see system table:

```sql
select * from information_schema.INNODB_SYS_FOREIGN;
```

There can be a record for trigger that already exists in this system table. We need to delete it:

```sql
delete from information_schema.INNODB_SYS_FOREIGN where for_name = 'sales/#sql-10d1_3dcfb';
-- ERROR 1044 (42000): Access denied for user 'root'@'localhost' to database 'information_schema'
```

Why? Because `INFORMATION_SCHEMA` is a database within each MySQL instance, the place that stores information about all the other databases that the MySQL server maintains. The `INFORMATION_SCHEMA` database contains several read-only tables. They are actually views, not base tables, so there are no files associated with them, and you cannot set triggers on them. Also, there is no database directory with that name.

Although you can select `INFORMATION_SCHEMA` as the default database with a USE statement, you can only read the contents of tables, not perform INSERT, UPDATE, or DELETE operations on them.

### With create table query

```sql
CREATE TABLE books (
  id INT(11) NOT NULL AUTO_INCREMENT,
  author_id INT(11) UNSIGNED NOT NULL,
  -- ... ,
  CONSTRAINT fk_books_author_id FOREIGN KEY (author_id) REFERENCES authors (id) ON DELETE CASCADE
);
```

### With alter table query

```sql
ALTER TABLE books
  ADD CONSTRAINT fk_books_author_id` 
  FOREIGN KEY (author_id) REFERENCES authors (id) 
  ON DELETE CASCADE;
```

This query adds foreign key to `books` table, on column `author_id`.
Statement `ON DELETE CASCADE` says that record in `books` table will be deleted if we delete author.

## Change foreign key

We need to do this in two steps: delete existing key and then create it.

```sql
ALTER TABLE `claims_log` DROP FOREIGN KEY `fk_claims_log_claim_id`;

ALTER TABLE `claims_log`
  ADD CONSTRAINT `fk_claims_log_claim_id` FOREIGN KEY (`claim_id`) 
  REFERENCES `claims` (`id`) ON DELETE CASCADE;
```

## Remove foreign key

We need to know foreign key's name to remove it from table:

```sql
ALTER TABLE claims_log DROP FOREIGN KEY fk_claims_log_claim_id;
```

This query will help you to get the foreign key's name:

```sql
SHOW CREATE TABLE table_name;
```

**Important:**

[Documentation](https://dev.mysql.com/doc/refman/5.6/en/innodb-online-ddl-operations.html) says that dropping a foreign key can be *performed online* with the `foreign_key_checks` option enabled or disabled. But it's not that simple.

*Try 1*: We executed this query:

```sql
alter table claims_cc drop foreign key fk_claims_cc_products_point_sales_id;

-- ^CCtrl-C -- query killed. Continuing normally.
-- ERROR 1317 (70100): Query execution was interrupted
```
This operation *locked* this table, and clients cannot use our site.
Fortunatelly, we cancelled this query with `Ctrl + C` and site became available again.

Server info:
- Server version: 10.1.37-MariaDB MariaDB Server
- claims_cc - 4 mln rows, size - 1 Gb

*Try 2*: 

1. In [HeidiSQL](https://www.heidisql.com/) client we monitored processlist on MySQL server. We disabled scripts that interacted with this table (background jobs) and ensured that there are no frequent updates or delete operations on our table.

2. In another console window we executed queries:

```sql
set FOREIGN_KEY_CHECKS=0;

alter table claims_cc drop foreign key fk_claims_cc_products_point_sales_id;
-- Query OK, 0 rows affected (0.20 sec)
-- Records: 0  Duplicates: 0  Warnings: 0

set FOREIGN_KEY_CHECKS=1;
```
It was a successful try this time - foreign key has been deleted.

## Show foreign keys

Show info from [innodb_sys_foreign](https://dev.mysql.com/doc/refman/5.7/en/innodb-sys-foreign-table.html) table:

```sql
SELECT * from information_schema.INNODB_SYS_FOREIGN;
```

Show info from [key_column_usage](https://dev.mysql.com/doc/refman/8.0/en/key-column-usage-table.html) table:

```sql
SELECT TABLE_NAME, COLUMN_NAME, REFERENCED_TABLE_SCHEMA, REFERENCED_TABLE_NAME, REFERENCED_COLUMN_NAME from information_schema.KEY_COLUMN_USAGE;
```
