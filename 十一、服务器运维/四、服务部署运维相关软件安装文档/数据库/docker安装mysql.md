# docker 安装mysql

## 一、下载镜像命令

```shell
 docker search mysql
 docker pull mysql:5.7
 docker images
```



## 二、创建master启动实例

```shell
docker run -p 3307:3306 --name mysql-master \
-v /mydata/mysql/master/log:/var/log/mysql \
-v /mydata/mysql/master/data:/var/lib/mysql \
-v /mydata/mysql/master/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7
```



## 三、修改配置文件

```cnf
[client]
default-character-set=utf8
 
[mysql]
default-character-set=utf8
 
[mysqld]
init_connect='SET collation_connection = utf8_unicode_ci'
init_connect='SET NAMES utf8'
character-set-server=utf8
collation-server=utf8_unicode_ci
skip-character-set-client-handshake
skip-name-resolve
```



## 四、创建Slave实例并启动

```shell
docker run -p 3316:3306 --name mysql-slaver-01 --restart=always \
-v /mydata/mysql/slaver/01/log:/var/log/mysql \
-v /mydata/mysql/slaver/01/data:/var/lib/mysql \
-v /mydata/mysql/slaver/01/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7

#修改slave基本配置
[client]
default-character-set=utf8
 
[mysql]
default-character-set=utf8
 
[mysqld]
init_connect='SET collation_connection = utf8_unicode_ci'
init_connect='SET NAMES utf8'
character-set-server=utf8
collation-server=utf8_unicode_ci
skip-character-set-client-handshake
skip-name-resolve
```



## 五、添加master主从复制部分配置

```cnf
vim /mydata/mysql/master/conf/my.cnf

server_id=1
log-bin=mysql-bin
read-only=0
binlog-do-db=gmall_ums
binlog-do-db=gmall_pms
binlog-do-db=gmall_oms
binlog-do-db=gmall_sms
binlog-do-db=gmall_cms


replicate-ignore-db=mysql
replicate-ignore-db=sys
replicate-ignore-db=information_schema
replicate-ignore-db=performance_schema
重启容器
```



## 六、添加Slave主从复制部分配置

```cnf
server_id=2
log-bin=mysql-bin
read-only=1
binlog-do-db=gmall_ums
binlog-do-db=gmall_pms
binlog-do-db=gmall_oms
binlog-do-db=gmall_sms
binlog-do-db=gmall_cms


replicate-ignore-db=mysql
replicate-ignore-db=sys
replicate-ignore-db=information_schema
replicate-ignore-db=performance_schema
重启容器
```



## 七、为master授权用户来他的同步数据

```shell
docker exec -it 4fdd7f265228 /bin/bash
mysql -u root -p
#授权root可以远程访问（ 主从无关，为了方便我们远程连接mysql）
grant all privileges on *.* to 'root'@'%' identified by 'root' with grant option;
flush privileges;
#添加用来同步的用户
GRANT REPLICATION SLAVE ON *.* to 'backup'@'%' identified by '123456';
#查看数据库的状态
show master status\G;

#进入从库mysql数据库
#授权root可以远程访问（ 主从无关，为了方便我们远程连接mysql）
grant all privileges on *.* to 'root'@'%' identified by 'root' with grant option;
flush privileges;
#设置主库连接
change master to master_host='192.168.137.133',master_user='backup',master_password='123456',master_log_file='mysql-bin.000001',master_log_pos=0,master_port=3307;
#启动从库同步
start slave;
#查看从库状态
show slave status\G;

总结：

1）、主从数据库在自己配置文件中声明需要同步哪个数据库，忽略哪个数据库等信息。并且server-id不能一样

2）、主库授权某个账号密码来同步自己的数据

3）、从库使用这个账号密码连接主库来同步数据

```



## 八、Too many connections, 解决方法是修改 mysql 里的连接数。

```mysql
show variables like "%conn%";


# cat my.cnf 
[mysqld]
local-infile=0
datadir=/var/lib/mysql
user=mysql
symbolic-links=0
max_connections = 1000
```

