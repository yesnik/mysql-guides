# MySQL triggers

## Show triggers

```sql
SHOW TRIGGERS;
```

or

```sql
SELECT trigger_schema, trigger_name, action_statement
FROM information_schema.triggers;
```

## Create trigger

*Important*: Creating trigger doesn't lock the table. Tested on *10.1.37-MariaDB MariaDB Server*.
- table `claims_settings` (2.5 Gb, 12 mln rows), took 0 sec.
- table `claims_cc` (1 Gb, 1 mln rows), took 0 sec.

*Important*: But on *MariaDB 5.5.36* creating trigger locked the table. It took *48 sec.* to create trigger on the table with 13 mln rows.

### Trigger that makes insert in another table on condition

```sql
DROP TRIGGER IF EXISTS claims_log__after_update;

DELIMITER //
CREATE TRIGGER claims_log__after_update AFTER UPDATE ON claims 
FOR EACH ROW
BEGIN
  IF NEW.status = 150 THEN
    INSERT INTO claims_log (claim_id, submitted_at)
    VALUES (NEW.id, NOW());
  END IF;
END //
DELIMITER ;
```

### Throw exception in trigger

```sql
DELIMITER //
CREATE TRIGGER claims_log_create AFTER UPDATE ON claims 
FOR EACH ROW
BEGIN
  IF EXISTS (SELECT 1 FROM claims_log WHERE claim_id = NEW.id) THEN
    SET @message_text = CONCAT('claims_log was already created for claim.id = ', NEW.id);
    SIGNAL SQLSTATE '45000' SET message_text = @message_text;
  END IF;
     
  INSERT INTO claims_log (claim_id, submitted_at)
  VALUES (NEW.id, NOW());
END //
DELIMITER ;
```

This trigger will throw our custom error `Error SQL (1644): claims_log was already created for claim.id = 1985` if record for passed claim.id already exists.

We use `CONCAT` to add `claim.id` value to error message.

### Trigger for created_at, updated_at

These triggers will help you if MySQL version is 5.5.

```sql
CREATE TRIGGER claims_calls__before_insert
BEFORE INSERT ON claims_calls
FOR EACH ROW SET NEW.created_at = NOW(), NEW.updated_at = NOW();

CREATE TRIGGER claims_calls__before_update
BEFORE UPDATE ON claims_calls
FOR EACH ROW SET NEW.created_at = OLD.created_at, NEW.updated_at = NOW();
```

If your MySQL version is 5.6 and higher, you can get the same result easier:

```sql
CREATE TABLE claims_calls (
  id INT NOT NULL AUTO_INCREMENT,
  claim_id INT NULL,
  call_number INT NULL,
  created_at TIMESTAMP NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMP NOT NULL DEFAULT NOW() ON UPDATE now(),
  PRIMARY KEY(id)
);
```

### Trigger for logging column changes

```sql
CREATE TRIGGER `claims_settings__after_update` AFTER UPDATE ON `claims_settings` FOR EACH ROW BEGIN
    IF NOT (OLD.`queue` <=> NEW.`queue`) THEN
        INSERT INTO `claims_audit`(`claim_id`, `user`, `table`, `field`, `value_old`, `value_new`) 
        VALUES (OLD.`claim_id`,  USER(), 'claims_settings', 'queue', OLD.`queue`, NEW.`queue`);
    END IF;
END
```
It uses `<=>` - [NULL-safe equal operator](https://mariadb.com/kb/en/null-safe-equal/).

## Drop trigger

*Important*: Dropping trigger doesn't lock the table. Tested on *10.1.37-MariaDB MariaDB Server*.
- table `claims_settings` (2.5 Gb, 12 mln rows), took 0 sec.
- table `claims_cc` (1 Gb, 1 mln rows), took 0 sec.

*Important*: But on *MariaDB 5.5.36)* dropping trigger locked the table. It took *38 sec.* to drop trigger on the table with 13 mln rows.

```sql
DROP TRIGGER IF EXISTS claims_log_create;
```

## Variables in trigger

```sql
DELIMITER //
CREATE TRIGGER claims_before_update BEFORE UPDATE ON claims 
FOR EACH ROW
BEGIN
  SET @product_type = (SELECT `type` FROM products WHERE id = NEW.product_id);

  INSERT INTO claims_log (claim_id, product_type, submitted_at)
  VALUES (NEW.id, @product_type, NOW());
END //
DELIMITER ;
```
As we can see, to declare variable in MySQL trigger we can use expression:

```sql
SET @product_type = 123;
```
