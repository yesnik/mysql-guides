# Varchar

See [doc](https://mariadb.com/kb/en/varchar/)

## varchar(500)

```sql
CREATE TABLE `urls` (
    `url` VARCHAR(500) NOT NULL COLLATE 'utf8_general_ci'
);
```

Insert big string:

```sql
INSERT INTO urls (url) VALUES ('... String with length 501 chars ...');
-- Query OK, 1 row affected, 1 warning (0.00 sec)
```
*Warning:* Data truncated for column 'url' at row 1

## varchar(65536)

```sql
CREATE TABLE `urls` (
    `url` VARCHAR(65536) NOT NULL COLLATE 'utf8_general_ci'
);
-- Query OK, 0 rows affected, 1 warning (0.01 sec)
```
*Warning:* Converting column 'url' from VARCHAR to TEXT
