# Installation

## Centos

Install packets:

```bash
sudo yum install mariadb-server
```

Enable service:

```bash
sudo service mariadb start
```

Run config script:

```bash
sudo mysql_secure_installation
```

### Connect to database

```bash
mysql -u root -pSoMePassw -P 9310 -h 127.0.0.1 -D mydb_name
```
