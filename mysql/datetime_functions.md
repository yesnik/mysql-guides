# MySQL datetime functions

## DATE_SUB

Query selects records created during the last 5 minutes:

```sql
SELECT * FROM products WHERE created_at > DATE_SUB(NOW(), INTERVAL 5 minute);
```
