# Profile queries in MySQL

## Disable query output in MySQL console

If your query returns many information, you'd probably 
want to disable output of this data to mysql console's STDOUT:

```
mysql> pager cat > /dev/null 
mysql> select * from products;
5014 rows in set (1.06 sec)

mysql> nopager
```

## Log all queries to table

Under root user execute queries:

```sql
SET GLOBAL general_log = 'ON';
SET global log_output = 'table';
```

After that we can see executed queries:

```sql
SELECT * FROM mysql.general_log;
SELECT * FROM mysql.general_log WHERE event_time > DATE_SUB(NOW(), INTERVAL 1 minute) ORDER BY event_time DESC;
```

**Important!** Don't forget to turn it off after debugging:

```sql
SET GLOBAL general_log = 'OFF';
```
