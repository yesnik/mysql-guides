# MySQL Stored Procedures

## Create stored procedure

```sql
DROP PROCEDURE IF EXISTS claims_log_insert;

DELIMITER //
CREATE PROCEDURE claims_log_insert (
	IN `claim_id` INT(11),
	IN `submitted_at` TIMESTAMP
)
BEGIN
  INSERT INTO claims_log (claim_id, submitted_at)
  VALUES (claim_id, submitted_at);
END //
DELIMITER ;
```

**Notes:** 

- We have to define `DELIMITER ;` and not `DELIMITER;`
- We have to define `DELIMITER //` and not `DELIMITER//`

## Call stored procedure

```sql
CALL claims_log_insert(1230, '2018-02-01 10:00:00');
```

## Drop procedure if exists

```sql
DROP PROCEDURE IF EXISTS claims_log_insert;
```
