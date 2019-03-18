# MySQL my.cnf for 19Gb RAM

Operation stats:

- select: 78%
- update: 9%
- insert: 9%
- Queries: 1 350 000 per hour, 371 per second

Example of `/etc/my.cnf` for mysql *Ver 15.1 Distrib 10.1.37-MariaDB, for Linux (x86_64) using readline 5.1*:

```
[mysqld]
skip-external-locking

max_allowed_packet          = 16M
key_buffer_size             = 16M
innodb_buffer_pool_size     = 8192M
innodb_file_per_table       = 1
innodb_flush_method         = O_DIRECT
innodb_flush_log_at_trx_commit  = 0

max_connections             = 1280

query_cache_size            = 0

slow_query_log              = /var/log/mysql/mysql-slow.log
long_query_time             = 1

expire_logs_days            = 10
max_binlog_size             = 100M
```
