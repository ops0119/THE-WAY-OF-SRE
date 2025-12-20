# mysql授权root可以远程登录

### mysql5.7版本

```sql
mysql -u root -p
---------------------------------------------------------------------------------------------------------
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'your_password' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

### mysql8.0
<font style="color:rgb(44, 44, 54);">在MySQL 8.0中，因为默认的身份验证插件由</font><font style="color:rgb(44, 44, 54);">mysql_native_password</font><font style="color:rgb(44, 44, 54);">更改为</font><font style="color:rgb(44, 44, 54);">caching_sha2_password</font>

```sql
mysql -u root -p
---------------------------------------------------------------------------------------------------------
create user 'root'@'%' identified by 'PASSWD';
grant all privileges on *.* to 'root'@'%';

或者是
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Admin@1234567';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'Admin@1234567' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

### 查看是否开启
```sql
mysql> SELECT User, Host FROM mysql.user WHERE User = 'root';
+------+-----------+
| User | Host      |
+------+-----------+
| root | %         |
| root | localhost |
+------+-----------+
2 rows in set (0.01 sec)
```

