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

It works for MySQL version >= 5.6:

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

On Ver 15.1 Distrib 5.5.46-MariaDB this query produced an error:
*SQLSTATE[Hy000]: General error: 1293 Incorrect table definition; there can be only one TIMESTAMP column with CURRENT_TIMESTAMP in DEFAULT or ON UPDATE clause*.

For this legacy DB version we can only add one field:

```sql
CREATE TABLE `articles` (
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```
On insert and update timestamp in this field will be updated.
