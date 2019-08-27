# MySQL Stored Procedures

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
    IN `claim_id_value` INT(11),
    IN `call_result_id_value` INT(11)
)
BEGIN
    UPDATE claims_cc SET call_result_id = call_result_id_value WHERE claim_id = claim_id_value;
END //
DELIMITER ;
```

**Notes:** 

- We have carefully choose name for input variables. If we had chosen name `claim_id`, the call of this procecedure would *update ALL records* in table `claims_cc`.

## Call stored procedure

```sql
CALL claims_log_insert(1230, '2018-02-01 10:00:00');
```

## Drop procedure if exists

```sql
DROP PROCEDURE IF EXISTS claims_log_insert;
```
