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

### Create table dump

```
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
