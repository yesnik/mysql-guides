# Fields `created_at`, `updated_at`

- If we insert a value in the table, MySQL will fill `created_at`, `updated_at` fields with the current timestamp.
- If we update a row in the table, MySQL will update the `updated_at` column.

## Modern versions

```sql
CREATE TABLE `call_results` (
    `id` INT(11) UNSIGNED NOT NULL AUTO_INCREMENT,
    `result` VARCHAR(20) NOT NULL,

    `created_at` TIMESTAMP NOT NULL DEFAULT NOW(),
    `updated_at` TIMESTAMP NOT NULL DEFAULT NOW() ON UPDATE NOW(),
    PRIMARY KEY (`id`)
);
```

## Legacy versions

- It works for MySQL version >= 5.6
- It was tested on Ver 15.1 Distrib 5.5.46-MariaDB

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
