# MySQL ENUM field

## Add ENUM field

### Add to existing table

```sql
ALTER TABLE call_results ADD COLUMN crm_status ENUM('','E0004','E0005') NOT NULL;
```

### Add with `create table` statement

We can create ENUM field in `CREATE TABLE` statement:

```sql
CREATE TABLE `call_results` (
    id INT(11) PRIMARY KEY NOT NULL AUTO_INCREMENT,
    crm_status ENUM('','E0005','E0006') NOT NULL,
    title VARCHAR(255) NOT NULL
);
```

In this case enum field is not required. 
If you omit the value of ENUM field in the query the inserted value will be `''` (empty string), because it's the first value in ENUM value definition:

```sql
INSERT INTO call_results (title) VALUES ('Hello');
```

*Table `call_results`*

| id | crm_status | title | 
| ---: | --- | --- | 
| 1 |  | Hello | 

## Add value to existing ENUM field

```sql
ALTER TABLE call_results MODIFY COLUMN crm_status enum('','E0004','E0005') NOT NULL;
```
**Note:** If you want to add *a new ENUM value* to a big existing table, it's better to add this value to the end of ENUM list.

*Example*

Suppose that table has field `crm_status enum('','E0004','E0005') NOT NULL`.

- This query will *affect 0 rows* (fast query): 
  ```
  ALTER TABLE call_results MODIFY COLUMN crm_status enum('','E0004','E0005','new') NOT NULL;
  ```
- This query will affect *ALL rows* (because we are changing the order of existing values in the list):
  ```
  ALTER TABLE call_results MODIFY COLUMN crm_status enum('','new','E0004','E0005') NOT NULL;
  ```
