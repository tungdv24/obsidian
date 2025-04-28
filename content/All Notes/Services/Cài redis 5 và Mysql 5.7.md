# Cài redis 5 và Mysql 5.7
16-04-2025
Tags: #services 

## Install redis 5:

```bash
dnf module -y install redis:5
```

- Change password redis 5
```bash
nano /etc/redis.conf
```
Thay đổi dòng dưới bằng password:
```bash
requirepass password
```

```bash
sudo systemctl restart redis
```
Test password:
```bash
redis-cli
AUTH password
```
Để thay đổi folder của redis change
```bash
sudo mkdir -p /data/redis
sudo chown -R redis:redis /data/redis
```

```
dir /var/lib/redis -> dir /data/redis
```
## Install Mysql 5.8

```bash
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
sudo yum localinstall https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
sudo yum install mysql-community-server
```
Đổi folder data của mysql
```bash
sudo mkdir -p /data/mysql
sudo chown -R mysql:mysql /data/mysql
sudo rsync -av /var/lib/mysql/ /data/mysql/
```
Edit file config mysql
```bash
nano /etc/my.cnf
```

```
[mysqld]
symbolic-links                  = 0
log_timestamps                  = SYSTEM
innodb_buffer_pool_size         = 5072M

### replication ###
bind-address                    = {{ ansible_ssh_host }}
server-id                       = 1
expire_logs_days                = 7
log_bin                         = batbai
log_bin_index                   = batbai.log.index
relay_log                       = batbai-relay-bin
relay_log_index                 = batbai-relay-bin.index
binlog_format                   = MIXED
max_binlog_size                 = 200M

### basic ###
datadir                         = /data/mysql
user                            = mysql
socket                          = /var/lib/mysql/mysql.sock
port                            = 3306
skip_name_resolve               = 1
innodb_buffer_pool_size         = 2048M
max_allowed_packet              = 64M

### timeout backend to db###
wait_timeout                    = 30
interactive_timeout             = 60
connect_timeout                 = 30

### query cache no cache###
query_cache_size                = 0
query_cache_type                = 0

### Redo log innodb storage####
innodb_log_files_in_group       = 2
innodb_log_file_size            = 64M

### temporary tables ###
tmp_table_size                  = 512M
max_heap_table_size             = 512M
tmpdir                          = /tmp

### open cache
table_open_cache                = 1000

### thread cache ###
thread_cache_size               = 8
max_connections                 = 512

### log ###
log_output                      = FILE
log_error                       = /var/log/mysqld/mysql_error.log
slow_query_log                  = 1
log_queries_not_using_indexes   = 0
log_warnings                    = 2
long_query_time                 = 2
slow_query_log_file             = /var/log/mysqld/mysql_slow.log
general_log                     = 0
general_log_file                = /var/log/mysqld/mysql_general.log
```
Trong một số version cách để lấy password root mặc định là:
```
sudo grep 'temporary password' /var/log/mysqld.log
```

```sql
mysql -u root -p
ALTER USER 'root'@'localhost' IDENTIFIED BY 'password';
FLUSH PRIVILEGES;
```

# References
- https://kdata.vn/cam-nang/cai-dat-mysql-57-centos-7
- https://www.server-world.info/en/note?os=CentOS_Stream_8&p=redis5&f=1

z8tHSc55N662fmqpQwyWbkXq7-CcjViB8N8IPFiBaETKT4GZKmdetJrCfil7FxVKTRMVGj8oKQZgCJhYrB_kTQ==

z8tHSc55N662fmqpQwyWbkXq7-CcjViB8N8IPFiBaETKT4GZKmdetJrCfil7FxVKTRMVGj8oKQZgCJhYrB_kTQ==