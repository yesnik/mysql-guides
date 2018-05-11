# MySQL logs

## Log all queries into table

```sql
-- Execute under 'root' user:
SET GLOBAL general_log = 'ON';
SET global log_output = 'table';
```

After that you can see logs with query:

```sql
SELECT * FROM mysql.general_log WHERE event_time > DATE_SUB(NOW(), INTERVAL 1 minute);
```

**Important!** Don't forget to turn off this setting after debugging:

```sql
SET GLOBAL general_log = 'OFF';
```
