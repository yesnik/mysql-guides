# MySQL users

## Create user 

- With local access
  ```sql
  CREATE USER 'kenny'@'localhost' IDENTIFIED BY 'some_password';
  ```
- With remote access
  ```sql
  CREATE USER 'kenny'@'%' IDENTIFIED BY 'some_password';
  ```
- With the password marked expired so that the user must choose a new one at the first connection to the server
  ```sql
  CREATE USER 'kenny'@'localhost' IDENTIFIED BY '12345' PASSWORD EXPIRE;
  ```

*See*: [give privileges to a user](grants.md)

## Drop user

```sql
DROP USER 'kenny'@'%';
```

## Show users with remote access

```sql
SELECT User, Host FROM mysql.user WHERE Host != 'localhost';
```

```
+----------------+--------------------+
| User           | Host               |
+----------------+--------------------+
| sales-dev      | %                  |
| root           | 127.0.0.11         |
| ksvuser        | 192.12.10.16       |
| vo             | vp.loop.com        |
+----------------+--------------------+
```

## Update root user password

```bash
mysqladmin -u root -pMyCurrentPassword password
```
