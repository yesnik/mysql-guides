# created_at and updated_at fields

If you want to create a table with 2 columns `created_at` and `updated_at`:

```sql
CREATE TABLE `call_results` (
  `id` INT(11) NOT NULL AUTO_INCREMENT,
  
  `created_at` TIMESTAMP NOT NULL DEFAULT NOW(),
  `updated_at` TIMESTAMP NOT NULL DEFAULT NOW() ON UPDATE NOW(),
  PRIMARY KEY (`id`)
);
```

**Important:** It works for MySQL version >= 5.6

If you insert value in this table, MySQL will fill these fields with current time value.
If you update a row, MySQL will update time in `updated_at` column.

See [article at Medium](https://medium.com/@bengarvey/use-an-updated-at-column-in-your-mysql-table-and-make-it-update-automatically-6bf010873e6a)
