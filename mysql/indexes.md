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

## Find unused index in MySQL

For MySQL >= 5.6

```sql
SELECT * FROM sys.schema_unused_indexes;
```
