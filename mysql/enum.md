# MySQL ENUM field

We can create ENUM field in `CREATE TABLE` statement:

```sql
CREATE TABLE `call_results` (
  id INT(11) PRIMARY KEY NOT NULL AUTO_INCREMENT,
  crm_status ENUM('','E0005','E0006') NOT NULL,
  title VARCHAR(255) NOT NULL
);
```

In this case enum field is not required. If you omit the value of ENUM field in the query the inserted value will be `''` (empty string):

```sql
INSERT INTO call_results (title) VALUES ('Hello');
```

#### Table call_results

| id | crm_status | title | 
| ---: | --- | --- | 
| 1 |  | Hello | 
