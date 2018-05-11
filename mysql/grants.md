# MySQL Grants

## Show user's grants

```sql
SHOW GRANTS FOR 'kenny'@'%';
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
