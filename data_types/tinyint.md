# Tinyint

See [docs](https://mariadb.com/kb/en/tinyint/)

A very small integer. The signed range is -128 to 127. The unsigned range is 0 to 255.

`BOOL` and `BOOLEAN` are synonyms for `TINYINT(1)`.

```sql
CREATE TABLE `students` (
    `score` TINYINT UNSIGNED NOT NULL
);
```

Insert correct value:

```sql
INSERT INTO students (score) VALUES (255);
```

Insert big value:

```sql
INSERT INTO students (score) VALUES (256);
-- Query OK, 1 row affected, 1 warning (0.00 sec)
-- score = 255 will be inserted
```

*Warning*: Out of range value for column 'score' at row 1 
