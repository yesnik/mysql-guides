# Default Character Set UTF-8

To change the default character set from `latin1` to `UTF-8`, edit config file at `/etc/my.cnf` or config files in the directory `/etc/my.cnf.d/`.

### utf8

```
[client]
default-character-set=utf8

[mysqld]
collation-server = utf8_general_ci
init-connect='SET NAMES utf8'
character-set-server = utf8
```

### utf8mb4

```
[client]
default-character-set=utf8mb4

[mysql]
default-character-set=utf8mb4

[mysqld]
collation-server = utf8mb4_unicode_ci
init-connect='SET NAMES utf8mb4'
character-set-server = utf8mb4
```

## Change database character set

```sql
ALTER DATABASE sales COLLATE = 'utf8_general_ci';
```
