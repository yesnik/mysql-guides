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

## Drop trigger

```sql
DROP TRIGGER IF EXISTS claims_log_create;
```
