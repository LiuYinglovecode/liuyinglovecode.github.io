---
title: upgrade mysql
date: 2020-1-8 17:30:10
tags:
- mysql
categories:
- sql
cover: ../../images/articles/mysql.png
---
### mysql upgrade(from 5.6 to 8.0.18)
1. find installation location
* open cmd type mysql,and then
``` bash
show variables like "%char%";
+--------------------------+---------------------------------------------------------+
| Variable_name            | Value                                                   |
+--------------------------+---------------------------------------------------------+
| character_set_client     | gbk                                                     |
| character_set_connection | gbk                                                     |
| character_set_database   | latin1                                                  |
| character_set_filesystem | binary                                                  |
| character_set_results    | gbk                                                     |
| character_set_server     | latin1                                                  |
| character_set_system     | utf8                                                    |
| character_sets_dir       | D:\Program Files\MySQL\MySQL Server 5.6\share\charsets\ |
+--------------------------+---------------------------------------------------------+
```
2. remove old version service
``` bash
D:\Program Files\MySQL\MySQL Server 5.6\bin>mysqld --remove mysql
Failed to remove the service because the service is running
Stop the service and try again
D:\Program Files\MySQL\MySQL Server 5.6\bin>net stop mysql
MySQL 服务正在停止.
MySQL 服务已成功停止。
D:\Program Files\MySQL\MySQL Server 5.6\bin>mysqld --remove mysql
Service successfully removed.
```
3. install new
``` bash
D:\Program Files\MySQL\MySQL Server 8.0.18\bin>mysqld --install mysql
Service successfully installed.
D:\Program Files\MySQL\MySQL Server 8.0.18\bin>mysqld --initialize-insecure
# then a new fold data is created
D:\Program Files\MySQL\MySQL Server 8.0.18\bin>net start mysql
mysql 服务正在启动 .
mysql 服务已经启动成功。
```
4. upgrade
``` bash
D:\Program Files\MySQL\MySQL Server 8.0.18\bin>mysql_upgrade -uroot -p123
mysql_upgrade: [Warning] Using a password on the command line interface can be insecure.
The mysql_upgrade client is now deprecated. The actions executed by the upgrade client are now done by the server.
To upgrade, please start the new MySQL binary with the older data directory. Repairing user tables is done automatically. Restart is not required after upgrade.
The upgrade process automatically starts on running a new MySQL binary with an older data directory. To avoid accidental upgrades, please use the --upgrade=NONE option with the MySQL binary. The option --upgrade=FORCE is also provided to run the server upgrade sequence on demand.
It may be possible that the server upgrade fails due to a number of reasons. In that case, the upgrade sequence will run again during the next MySQL server start. If the server upgrade fails repeatedly, the server can be started with the --upgrade=MINIMAL option to start the server without executing the upgrade sequence, thus allowing users to manually rectify the problem.

D:\Program Files\MySQL\MySQL Server 8.0.18\bin>net stop mysql
...
D:\Program Files\MySQL\MySQL Server 8.0.18\bin>net start mysql
...
```
5. use
``` bash
C:\Users\user>mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.0.18 MySQL Community Server - GPL
```
6. ERROR 1045 (28000): Access denied for user 'ODBC'@'localhost' 
this error pop up when type just "mysql". That's because ODBC is the default username under win system.
