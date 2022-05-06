# Smallint

See [docs](https://mariadb.com/kb/en/smallint/)

A small integer. The signed range is -32768 to 32767. The unsigned range is 0 to 65535.

If a column has been set to ZEROFILL, all values will be prepended by zeros so that the SMALLINT value contains a number of M digits.

*Note:* If the ZEROFILL attribute has been specified, the column will automatically become UNSIGNED.

```sql
CREATE TABLE `students` (
	`score` SMALLINT UNSIGNED NOT NULL
);
```

Insert allowed value:

```sql
INSERT INTO students (score) VALUES (65535);
-- Query OK, 1 row affected (0.00 sec)
```
Insert too big value:

```sql
INSERT INTO students (score) VALUES (65536);
Query OK, 1 row affected, 1 warning (0.00 sec)
```
*Warning:* Out of range value for column 'score' at row 1
