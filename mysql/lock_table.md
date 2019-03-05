# MySQL Lock Table

MySQL allows a client session to explicitly acquire a table lock for preventing other sessions from accessing 
the same table during a specific period. 
A client session can acquire or release table locks only for itself. 
It cannot acquire or release table locks for other sessions.

If the session is terminated, either normally or abnormally, MySQL *will release* all the locks implicitly. 

## Read locks

Read lock is *shared* lock which prevent a write lock is being acquired but not other read locks.

```sql
LOCK TABLE products READ;
```

*Our session*:

- can read table
- cannot write

*Other sessions*:

- can read table
- cannot write data to the table until the READ lock is released

## Write locks

Write lock is *exclusive* lock that prevent any other lock.

```sql
LOCK TABLE products WRITE;
```

*Our session*:

- can read table
- can write

*Other sessions*:

- cannot read
- cannot write

... the table until the WRITE lock is released.

### Sources

- MySQL table locking / [www.mysqltutorial.org](http://www.mysqltutorial.org/mysql-table-locking/)
