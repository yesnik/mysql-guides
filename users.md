# MySQL users

## Create user 

- with local access

```sql
CREATE USER 'kenny'@'localhost' IDENTIFIED BY 'some_password';
```

- with remote access

```sql
CREATE USER 'kenny'@'%' IDENTIFIED BY 'some_password';
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
