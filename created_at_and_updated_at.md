# created_at and updated_at fields

If you want to create a table with 2 columns `created_at` and `updated_at`:

```sql
CREATE TABLE `call_results` (
    `id` INT(11) UNSIGNED NOT NULL AUTO_INCREMENT,
    `result` VARCHAR(20) NOT NULL,

    `created_at` TIMESTAMP NOT NULL DEFAULT NOW(),
    `updated_at` TIMESTAMP NOT NULL DEFAULT NOW() ON UPDATE NOW(),
    PRIMARY KEY (`id`)
);
```

```sql
CREATE TABLE `articles` (
    `id` INT(11) UNSIGNED NOT NULL AUTO_INCREMENT,
    `title` VARCHAR(255) NOT NULL,

    `created_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    `updated_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    PRIMARY KEY (`id`)
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB
```

**Important:** It works for MySQL version >= 5.6

If you insert value in this table, MySQL will fill these fields with current time value.
If you update a row, MySQL will update time in the `updated_at` column.
