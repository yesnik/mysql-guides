# MySQL users

## Create user with remote access

```sql
CREATE USER 'you_user'@'%' IDENTIFIED BY 'some_password';
```

## Show users with remote access

```sql
SELECT User, Host FROM mysql.user WHERE Host <> 'localhost';
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
