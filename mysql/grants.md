# MySQL Grants

## Grant permission to see other account's server processes

The `PROCESS` privilege pertains to display of information about the threads 
executing within the server. The privilege enables use of `SHOW PROCESSLIST` 
to see threads belonging to other accounts.

```sql
GRANT PROCESS ON *.* TO 'kenny'@'%';
```
