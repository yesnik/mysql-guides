# Work with strings in MySQL

### CONCAT(str1, str2)

```sql
SELECT CONCAT(first_name, ' ' , last_name) FROM claims_info where id = 1;
-- Returns: 'Depp Johny'
```

### SUBSTRING(str, start, length)

| id | title    |
|----|----------|
| 1  | New York |

```sql
SELECT SUBSTRING(title, 1, 3) FROM cities WHERE id = 1;
-- Returns: 'New'

SELECT SUBSTRING(title, 6) FROM cities WHERE id = 1;
-- Returns: 'ork'
```
