# MySQL administration

## MySQL dump

### Create database dump

```
mysqldump -u USER -pPASSWORD DATABASE | gzip > /path/to/outputfile.sql.gz
```

### Import database dump

- Way 1
```
gunzip < /path/to/outputfile.sql.gz | mysql -u USER -pPASSWORD DATABASE
```

- Way 2
```
zcat /path/to/outputfile.sql.gz | mysql -u USER -pPASSWORD DATABASE
```

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
SHOW OPEN TABLES
```
