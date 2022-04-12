# MySQL datetime functions

## DATE_SUB

Query selects records created during the last 5 minutes:

```sql
SELECT * FROM products WHERE created_at > DATE_SUB(NOW(), INTERVAL 5 minute);
```

*Note:* Instead of `minute` you can use: `second`, `hour`, `day`, `week`, `month`, `quarter`, `year`

## Timestamp to datetime

```sql
SELECT from_unixtime(1501651076); -- 2017-08-02 05:17:56
```

## Select from 1 day ago

```sql
SELECT * FROM claims WHERE created_at > (NOW() - INTERVAL 1 day);
```

## Show date in 1 month
  ```sql
  SELECT CURDATE() + interval 1 MONTH;
  -- 2022-04-10
  ```
