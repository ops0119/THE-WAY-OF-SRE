## 01-windows+mysql

https://www.cnblogs.com/sexintercourse/p/19108665

https://developer.aliyun.com/article/1403687

https://downloads.mysql.com/archives/community/

解压后进入，创建data、log目录，创建配置文件

```mysql
[client]
no-beep
socket =0.0
port=3306
[mysqld]
server-id=45
port=3306
character-set-server=utf8mb4
pid-file ="mysql.pid"
socket =0.0
basedir="D:\01-software\14-mysqld\mysql-8.0.36\bin"
datadir="D:\01-software\14-mysqld\mysql-8.0.36\data"
tmpdir="D:\01-software\14-mysqld\mysql-8.0.36\data"
default-storage-engine=INNODB
#=============================[log]==============================
slow-query-log=1
long_query_time=1
slow_query_log_file="D:\01-software\14-mysqld\mysql-8.0.36\log\mysql-slow.log"
##log-bin="D:\MySQL\Log\mysql-bin"
log-error="D:\01-software\14-mysqld\mysql-8.0.36\log\mysql-error.log"

```

进入bin

```
mysqld --defaults-file="D:\01-software\14-mysqld\mysql-8.0.36\my.ini" --initialize --innodb_undo_tablespaces=3 --explicit_defaults_for_timestamp

mysqld --install

net start mysql

```

```sql
mysql -uroot -p


alter user root@"localhost" identified by "new_password";

```

