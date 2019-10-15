# MySQL Stored Procedures

## Show stored procedures

This query will show stored procedures where Definer is current user:

```sql
SHOW PROCEDURE STATUS;
```

## Show stored procedure source code

To display source code of a particular stored procedure:

```sql
SHOW CREATE PROCEDURE stored_procedure_name;
```

## Create stored procedure

*Example 1*

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

*Example 2*

```sql
DROP PROCEDURE IF EXISTS updateCallResultId;

DELIMITER //
CREATE PROCEDURE updateCallResultId(
	IN `$claim_id` INT(11),
	IN `$call_result_id` INT(11)
)
BEGIN
  IF $call_result_id IS NOT NULL THEN
  
    SET @crm_status = (SELECT crm_status FROM call_results WHERE id = $call_result_id);
  
    UPDATE claims_cc SET call_result_id = $call_result_id, crm_status = @crm_status
    WHERE claim_id = $claim_id;
  END IF;
END //
DELIMITER ;
```

**Notes:** 

- We have carefully choose name for input variables. If we had chosen name `claim_id`, the call of this procecedure would *update ALL records* in table `claims_cc`. It's because the SQL would be:
```sql
UPDATE claims_cc SET call_result_id = call_result_id, crm_status = @crm_status
WHERE claim_id = claim_id;
```
- The mode `IN` is the default mode for procedure's parameters. If you don't provide the parameter mode, it will be `IN`.

## Call stored procedure

```sql
CALL claims_log_insert(1230, '2018-02-01 10:00:00');
```

## Drop procedure if exists

```sql
DROP PROCEDURE IF EXISTS claims_log_insert;
```
