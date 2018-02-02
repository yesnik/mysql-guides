# MySQL triggers

## Create trigger

### Trigger that makes insert in another table on condition

```sql
DROP TRIGGER IF EXISTS claims_log_create;

DELIMITER //
CREATE TRIGGER claims_log_create AFTER UPDATE ON claims 
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

## Drop trigger

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
