# MySQL Grants

## Show grants

**For current user**

```sql
SHOW GRANTS FOR 'kenny'@'%';
```

**For all users**

```sql
SELECT * FROM information_schema.user_privileges;
```

## Grant permission to do everything

### For all databases

*Without password definition:*

```sql
GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

*With password definition:*

```sql
GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'%' IDENTIFIED BY 'some_password' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

### For one database

*Without password definition:*

```sql
GRANT ALL PRIVILEGES ON sales.* TO 'myuser'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

*With password definition:*

```sql
GRANT ALL PRIVILEGES ON sales.* TO 'myuser'@'%' IDENTIFIED BY 'some_password' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

## Grant SELECT

```sql
GRANT select ON products TO 'kenny'@'%';
```

## Grant permission to update some columns

Allow user 'kenny' to update only 2 columns in table `products`:

```sql
GRANT UPDATE(status, processed_at) ON products TO 'kenny'@'%';
```

## Grant permission to see other account's server processes

The `PROCESS` privilege pertains to display of information about the threads 
executing within the server. The privilege enables use of `SHOW PROCESSLIST` 
to see threads belonging to other accounts.

```sql
GRANT PROCESS ON *.* TO 'kenny'@'%';
```

## Grant permission to view processes

```sql
GRANT PROCESS ON database.* TO user@'localhost';
```

## Allow remote connections

```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

If we want to allow user `root` to connect locally:

```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
