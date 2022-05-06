# Varchar

See [doc](https://mariadb.com/kb/en/varchar/)

## varchar(500)

```sql
CREATE TABLE `urls` (
    `url` VARCHAR(500) NOT NULL COLLATE 'utf8_general_ci'
);
```

### Long string will be truncated

```sql
INSERT INTO urls (url) VALUES ('... String with length 501 chars ...');
-- Query OK, 1 row affected, 1 warning (0.00 sec)
```
*Warning:* Data truncated for column 'url' at row 1

## varchar(21844)

`utf8` characters can require up to three bytes per character, 
so a `VARCHAR` column that uses the `utf8` character set can be declared to be a **maximum of 21 844** characters.

```sql
CREATE TABLE `urls` (
    `url` VARCHAR(21844) NOT NULL COLLATE 'utf8_general_ci'
);
-- Query OK, 0 rows affected (0.00 sec)
```

```sql
CREATE TABLE `urls` (
    `url` VARCHAR(21845) NOT NULL COLLATE 'utf8_general_ci'
);
-- Query OK, 0 rows affected, 1 warning (0.01 sec)
```
*Warning:* Converting column 'url' from VARCHAR to TEXT
