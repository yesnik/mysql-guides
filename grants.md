# MySQL Grants

## Show grants

**For current user**

```sql
SHOW GRANTS;
```

**For user kenny**

```sql
SHOW GRANTS FOR 'kenny'@'%';
```

**For all users**

```sql
SELECT * FROM information_schema.user_privileges;
```

## Grant SELECT

```sql
-- Grant to whole DB sales
GRANT SELECT ON `sales`.* TO 'kenny'@'localhost';
-- Grant to only one table
GRANT SELECT ON sales.products TO 'kenny'@'%';
FLUSH PRIVILEGES;
```

## Grant ALTER

Allow user to alter all tables of *sales* database:

```sql
GRANT ALTER on sales.* to 'kenny'@'%';
FLUSH PRIVILEGES;
```

## Grant UPDATE

Allow user 'kenny' to update only 2 columns in table `products`:

```sql
GRANT UPDATE(status, processed_at) ON products TO 'kenny'@'%';
```

If you want to allow user to update one more column:

```sql
GRANT UPDATE(delivered_at) ON products TO 'kenny'@'%';
```

## Grant PROCESS

This allows to view server processes. This privilege enables use of `SHOW PROCESSLIST` 
to see threads belonging to other accounts.

```sql
GRANT PROCESS ON *.* TO 'kenny'@'%';
```

## Grant TRIGGER

```sql
GRANT TRIGGER ON mydb.* TO 'kenny'@'%';
```

## Grant ALL

### For all databases

```sql
-- Without password definition
GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'%' WITH GRANT OPTION;

-- With password definition
GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'%' IDENTIFIED BY 'some_password' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

### For one database

```sql
-- Without password definition
GRANT ALL PRIVILEGES ON sales.* TO 'myuser'@'%' WITH GRANT OPTION;

-- With password definition
GRANT ALL PRIVILEGES ON sales.* TO 'myuser'@'%' IDENTIFIED BY 'some_password' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

## Remove grants

**Remove SELECT grant**

```sql
REVOKE SELECT ON sales.call_results FROM 'kenny'@'%';
```

**Remove all privileges**

```sql
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'kenny'@'%';
```

## Allow connections

## Remote connections from any IP

```sql
-- Without password definition
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;

-- With password definition
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

## Local connections only

```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
