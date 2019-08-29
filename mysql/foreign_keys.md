# MySQL Foreign Keys

## Add foreign key

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

**Add foreign key without locking tables (in some cases)**

```sql
set FOREIGN_KEY_CHECKS=0;

ALTER TABLE claims_import ADD CONSTRAINT fk_claims_import_call_result_id FOREIGN KEY (call_result_id) REFERENCES call_results (id);
-- Query OK, 0 rows affected (0.00 sec)

set FOREIGN_KEY_CHECKS=1;
```

This setting disables foreign key checks for current session only.
Setting [FOREIGN_KEY_CHECKS](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_foreign_key_checks): If set to `1` (the default), foreign key constraints for InnoDB tables are checked. If set to `0`, foreign key constraints are ignored, with a couple of exceptions.

**Note:** We tried to add foreign key to another table (that has 20 inserts per minute), but got an error:
*ERROR 1823 (HY000): Failed to add the foreign key constraint 'sales/fk_claims_sms_call_result_id' to system tables*

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

**Notes:** 

1. This query will help you to know the foreign key's name:
```sql
SHOW CREATE TABLE table_name;
```
2. It took 0 sec to drop trigger on the table with size 220 Mb, 0.5 mln rows
