# Profile queries in MySQL

## Disable query output in MySQL console

If your query returns many information, you'd probably 
want to disable output of this data to mysql console's STDOUT:

```
mysql> pager cat > /dev/null 
mysql> select * from products;
5014 rows in set (1.06 sec)

mysql> nopager
```
