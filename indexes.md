# Indexes in MySQL

## Create index

### On one column

```sql
CREATE INDEX `created_at` ON claims(created_at);
```

### On several columns

```sql
CREATE INDEX `created+crm_category+cc_id` ON claims_cc(created, crm_category, cc_id);
```

### Creating index time

Creating index will lock your table for insert and update. Lock time depends on the size of table.

| Records num | Table size | Index creating time |   |   |
|-------------|------------|---------------------|---|---|
| 600 000     | 57 Mb      | 5 sec               |   |   |
| 13 000 000  | 20 Gb      | 12 min              |   |   |
|             |            |                     |   |   |

## Drop index

```sql
DROP INDEX `created_at` ON claims;
```

### Drop unique index

- Define name of unique key:
```sql
SHOW CREATE TABLE article;
```
- Drop index by name:
```sql
ALTER TABLE article DROP INDEX UNIQ_23AOR66534345B45;
```

## Find unused index in MySQL

For MySQL >= 5.6

```sql
SELECT * FROM sys.schema_unused_indexes;
```

## Using indexes

### Quotes in value matters

Suppose that we have table `claims`:

```sql
CREATE TABLE `claims` (
	`id` INT(11) UNSIGNED NOT NULL AUTO_INCREMENT,
	`crm_task_id` VARCHAR(100) NOT NULL,

	PRIMARY KEY (`id`),
	INDEX `crm_task_id` (`crm_task_id`),
)
ENGINE=InnoDB;
```

Similar queries:

```sql
-- Index on 'crm_task_id' is NOT USED
SELECT * FROM claims c WHERE crm_task_id IN ( 1465110897 )

-- Index on 'crm_task_id' is used
SELECT * FROM claims c WHERE crm_task_id IN ( '1465110897' )
```

Tested on *10.1.37-MariaDB* server.
