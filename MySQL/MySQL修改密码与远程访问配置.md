# MySQL修改密码与远程访问配置

### 设置root密码

- 设置免密登录（也可通过临时密码登录）

> 在[mysqld]这个条目下加入skip-grant-tables保存退出后重启mysql

- 修改root用户的密码

```mysql
set password for 'root'@'localhost' = password('123456'); # 5.6.44
update mysql.user set authentication_string=password('123456') where user='root'; # 5.7
```

- 取消免密登录

> 在[mysqld]这个条目下删除skip-grant-tables保存退出后重启

### 配置远程访问

在本机使用mysql客户端登录

```mysql
grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;
flush privileges;
```

正常情况下是可以远程登录了。

### 补充

```mysql
INSERT INTO `user` (`Host`, `User`, `Select_priv`, `Insert_priv`, `Update_priv`, `Delete_priv`, `Create_priv`, `Drop_priv`, `Reload_priv`, `Shutdown_priv`, `Process_priv`, `File_priv`, `Grant_priv`, `References_priv`, `Index_priv`, `Alter_priv`, `Show_db_priv`, `Super_priv`, `Create_tmp_table_priv`, `Lock_tables_priv`, `Execute_priv`, `Repl_slave_priv`, `Repl_client_priv`, `Create_view_priv`, `Show_view_priv`, `Create_routine_priv`, `Alter_routine_priv`, `Create_user_priv`, `Event_priv`, `Trigger_priv`, `Create_tablespace_priv`, `ssl_type`, `ssl_cipher`, `x509_issuer`, `x509_subject`, `max_questions`, `max_updates`, `max_connections`, `max_user_connections`, `plugin`, `authentication_string`, `password_expired`, `password_last_changed`, `password_lifetime`, `account_locked`) VALUES
	('localhost', 'root', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', '', _binary '', _binary '', _binary '', 0, 0, 0, 0, 'mysql_native_password', '*10D4483D202FBEB0FF67BEA72BDE209EA98F6564', 'N', '2022-05-07 18:39:53', NULL, 'N'),
	('%', 'root', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', '', _binary '', _binary '', _binary '', 0, 0, 0, 0, 'mysql_native_password', '*10D4483D202FBEB0FF67BEA72BDE209EA98F6564', 'N', '2022-05-07 18:40:37', NULL, 'N');
```