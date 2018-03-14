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

## Drop index

```sql
DROP INDEX `created_at` ON claims;
```

## Find unused index in MySQL

For MySQL >= 5.6

```sql
SELECT * FROM sys.schema_unused_indexes;
```
