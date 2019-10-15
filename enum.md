# MySQL ENUM field

## Add ENUM field

### Add to existing table

```sql
ALTER TABLE call_results ADD COLUMN crm_status2 ENUM('','E0004','E0005') NOT NULL;
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

#### Table call_results

| id | crm_status | title | 
| ---: | --- | --- | 
| 1 |  | Hello | 

## Add value to existing ENUM field

```sql
ALTER TABLE call_results MODIFY COLUMN crm_status enum('','E0004','E0005') NOT NULL;
```
