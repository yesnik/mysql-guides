# MySQL Foreign Keys

## Add foreign key

### With create table query

```sql
CREATE TABLE books (
  id INT(11) NOT NULL AUTO_INCREMENT,
  author_id INT(11) UNSIGNED NOT NULL,
  -- ... ,
  CONSTRAINT fk_books_author_id FOREIGN KEY (author_id) REFERENCES authors (id) ON DELETE CASCADE
);
```

### With alter table query

```sql
ALTER TABLE books
  ADD CONSTRAINT fk_books_author_id` 
  FOREIGN KEY (author_id) REFERENCES authors (id) 
  ON DELETE CASCADE;
```

This query adds foreign key to `books` table, on column `author_id`.
Statement `ON DELETE CASCADE` says that record in `books` table will be deleted if we delete author.

## Change foreign key

We need to do this in two steps: delete existing key and then create it.

```sql
ALTER TABLE `claims_log` DROP FOREIGN KEY `fk_claims_log_claim_id`;

ALTER TABLE `claims_log`
  ADD CONSTRAINT `fk_claims_log_claim_id` FOREIGN KEY (`claim_id`) 
  REFERENCES `claims` (`id`) ON DELETE CASCADE;
```

## Remove foreign key

We need to know foreign key's name to remove it from table:

```sql
ALTER TABLE claims_indeces DROP FOREIGN KEY claims_indeces_ibfk_1;
```

*Note:* This query will help you to know the foreign key's name:

```sql
SHOW CREATE TABLE table_name;
```
