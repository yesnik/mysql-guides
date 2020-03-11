# MySQL administration

## MySQL dump

### Create database dump

*Create .gz archive*

```
mysqldump -u root -p123 DATABASE | gzip > dump.sql.gz
```

*Create schema only (without data):*

```
mysqldump --no-data -u root -p123 sales > dump.sql
```

### Create table or many tables dump

**With drop, create table**

This command will create dump for 2 tables:

```bash
# Create .sql file
mysqldump -u USER -pPASSWORD mydbname --tables table_one table_two > dump.sql

# Create .gz archive
mysqldump -u USER -pPASSWORD mydbname --tables table_one table_two | gzip > dump.sql.gz
```

This dump has `DROP TABLE IF EXISTS` and `CREATE TABLE` statements for each table.

**Only data - without drop, create table**

If we want to exclude `DROP TABLE` and `CREATE TABLE` queries from dump, we need use `--skip-add-drop-table`, `--no-create-info` options.

```bash
mysqldump -u USER -pPASSWORD mydbname --skip-add-drop-table --no-create-info --tables products_point_sales products_to_point_sales > dump.sql
```

**Only data - with WHERE condition**

```bash
mysqldump -u USER -pPASSWORD mydbname --skip-add-drop-table --no-create-info --tables products --where="id > 10000" > products_delta_dump.sql
```

### Import database dump

- **.gz archive**
```
gunzip < /path/to/outputfile.sql.gz | mysql -u USER -pPASSWORD DATABASE
```
or
```
zcat /path/to/outputfile.sql.gz | mysql -u USER -pPASSWORD DATABASE
```
- **.zip archive**
```
unzip -p db-dump.zip | mysql -u USER -pPASSWORD DATABASE
```
Here `-p` flag pipes the output.

## Reset MySQL password for root

*Note:* We are talking about MariaDB

1. Stop mysql service
```
service mariadb stop
```

2. Start service ignoring grant tables
```
sudo mysqld_safe --skip-grant-tables &
```

3. Log in to database as root
```
mysql -u root
```

4. Execute queries
```sql
USE mysql;
UPDATE user SET password=PASSWORD("123") WHERE User='root';
flush privileges;
```

5. Kill mysql process that we started recently
```
sudo kill `cat /var/run/mariadb/mariadb.pid`
```

* Start mysql service in usual way
```
service mariadb start
```

## Shrink table

### Get tables that have unused space

This query shows tables that have more than 100Mb of unused free space 
(`data_free` is the number of allocated but unused bytes).

```sql
SELECT TABLE_NAME, ROUND(data_length/1024/1024) AS data_length_mb, ROUND(data_free/1024/1024) AS data_free_mb
FROM information_schema.tables
WHERE ROUND(data_free/1024/1024) > 100
ORDER BY data_free_mb;
```

### Optimize table

[OPTIMIZE TABLE](https://dev.mysql.com/doc/refman/8.0/en/optimize-table.html) query reorganizes the physical storage of table data and associated index data, to reduce storage space.

```sql
OPTIMIZE TABLE products;
```

**Important:** This query locks table for read and write.

## Using mysqlcheck

[mysqlcheck](https://mariadb.com/kb/en/library/mysqlcheck/) is a maintenance tool that allows you to check, repair, analyze and optimize multiple tables from the command line.

```bash
# Check tables in database
mysqlcheck --database mydatabase -u root -pMyPass

# Optimize tables
mysqlcheck --optimize --database mydatabase -u root -pMyPass

# Analyze tables
mysqlcheck --analyze --database mydatabase -u root -pMyPass
```

InnoDb tables doesn't support `optimize`, so `recreate + analyze` will be executed instead.

## Duplicate table

### Copy with indexes and triggers

```sql
DROP TABLE IF EXISTS newtable;
CREATE TABLE newtable LIKE oldtable; 
INSERT newtable SELECT * FROM oldtable;
```

### Copy just structure and data

```sql
CREATE TABLE newtable AS SELECT * FROM oldtable;
```

This will not copy indexes and triggers as the SELECT result is a temporary table and does not "carry" the metadata of the source table.

## Show lock status of tables

```sql
SHOW OPEN TABLES;
```

## Show MySQL variables

```sql
SHOW VARIABLES;
```

## Show statistics

```sql
SHOW CLIENT_STATISTICS;
SHOW USER_STATISTICS;
SHOW INDEX_STATISTICS;
SHOW TABLE_STATISTICS;
```

## Show MySQL processes

```sql
SHOW FULL PROCESSLIST;
```
*Note:* Your user must have permission to view processes.

## Kill all MySQL processes

```bash
killall mysqld mysqld_safe
```

Wait for 10 seconds atleast so that it shut down cleanly. Run this command to check whether still there are some mysqld process remained or not ?

```bash
ps aux | grep mysqld
```

If you still able to see more than run this command:

```bash
killall -9 mysqld mysqld_safe
```

**Note:** This command will kill all `mysqld` processes. If you start `mysql` it will try to rollback all transactions that were running when you killed `mysqld` processes.
