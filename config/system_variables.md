# MySQL System Variables

We can set these values in the file `/etc/my.cnf.d/server.cnf`.

### max_allowed_packet

Max packet length to send to or receive from the server.

Default: 16777216 bytes (16.8 Mb)

```
# 50 Mb
max_allowed_packet=50777216
```
