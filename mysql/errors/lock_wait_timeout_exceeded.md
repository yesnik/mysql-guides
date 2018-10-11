# SQLSTATE[HY000]: General error: 1205 Lock wait timeout exceeded; try restarting transaction

This error occures when we try to access data that has been locked by another process.
MySQL waits some time for the lock to be removed and then throws this error.

Something is blocking the execution of our query. 
Most likely another query updating, inserting or deleting from one of the tables in our query. 

## Debug tools

### Look at current processes

```sql
SHOW FULL processlist
```

One of these transactions could be blocking. Pay special attention to:

- processes with big `Time`
- processes in `Sleep` state

### Show current transactions

```sql
SHOW ENGINE INNODB STATUS;
```

### Get value of innodb_lock_wait_timeout

```sql
SHOW VARIABLES LIKE 'innodb_lock_wait_timeout';
```

Default value is 50.

## Reproduce lock wait timeout exceeded error

*Note:* In this example we are using *10.1.33-MariaDB MariaDB Server*, innodb version: *5.6.39*.

1. In your test MySQL database create table `test` and populate it with several records:

```sql
CREATE TABLE `test` (
	`id` INT(11) NOT NULL,
	PRIMARY KEY (`id`)
)
ENGINE=InnoDB;

INSERT INTO test (`id`) values (1), (2), (3);
```

2. Open *first* terminal window and connect to database. Open transaction and execute query on deleting records. 
Don't close transaction yet.

```sql
BEGIN;
DELETE FROM test WHERE id >= 2;
```

This command locked rows `2, 3` of table `test`.

3. Open *second* terminal window and connect to database. Try to select rows:

```sql
> select * from test;
+----+
| id |
+----+
|  1 |
|  2 |
|  3 |
+----+
3 rows in set (0.00 sec)
```

As we can see, `SELECT` on locked table works.

4. Try to INSERT, UPDATE, DELETE record. Commands will hang for 50 sec and then throws error:

```sql
insert into test (id) values (4);
-- ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction

update test set id = 11 where id = 1;
-- ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction

delete from test where id = 2;
-- ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction
```

*Note:* But we **can delete** not locked row:

```sql
delete from test where id = 1;
-- Query OK, 1 row affected (0.00 sec)
```

5. Let's see process list. In our second terminal execute query:

```sql
show full processlist;
+-------+------+-----------+-------+---------+------+-------+-----------------------+----------+
| Id    | User | Host      | db    | Command | Time | State | Info                  | Progress |
+-------+------+-----------+-------+---------+------+-------+-----------------------+----------+
| 46979 | root | localhost | sales | Sleep   |  950 |       | NULL                  |    0.000 |
| 47160 | root | localhost | sales | Query   |    0 | init  | show full processlist |    0.000 |
+-------+------+-----------+-------+---------+------+-------+-----------------------+----------+
2 rows in set (0.00 sec)
```

As we can see process `46979` is in `Sleep` and it lasts 950 seconds. 
We know that it's a process of MySQL connection in our *first* terminal.

Execute query to kill blocking process:

```sql
kill 46979;
```

6. Records in table were not touched by killed process:

```sql
select * from test;
+----+
| id |
+----+
|  2 |
|  3 |
+----+
```
