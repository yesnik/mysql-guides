# MySQL administration

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
