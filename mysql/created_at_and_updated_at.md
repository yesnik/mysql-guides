# created_at and updated_at fields

If you want to create a table with 2 columns `created_at` and `updated_at`:

```sql
CREATE TABLE `claims_processed` (
  `id` INT(11) NOT NULL AUTO_INCREMENT,
  
  `created_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB;
```

If you insert value in this table, MySQL will fill these fields with current time value.
If you update a row, MySQL will update time in `updated_at` column.
