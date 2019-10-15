# MySQL view

## Create or replace view

```sql
CREATE OR REPLACE VIEW claims_calls 
AS
SELECT
  id,
  cc_id,
  claim_id,
  crm_status,
  time_start_talk,
  time_end_talk,
  operator_cc
FROM claims_calls
  WHERE cc_id = 13 
WITH CHECK OPTION
```

## Drop view

```sql
DROP VIEW IF EXISTS claims_calls;
```

## Grant access to view

This query will allow user 'foo' to do following actions with view `claims_calls`:

- SELECT every row
- INSERT new records
- UPDATE fields time_start_talk, time_end_talk

```
GRANT SELECT, INSERT, UPDATE(time_start_talk, time_end_talk) ON mydb.claims_calls TO 'foo'@'%'
```
