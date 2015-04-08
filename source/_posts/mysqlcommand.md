title: "MySQL常用命令"
date: 2015-04-08 16:43:46
tags:
- MySQL
- 技术
categories: 技术
---

### 常用命令

```

## 修改表字段
alter table test add column A varchar(36)
alter table test modify column A int(11)
alter table test drop column A 
alter table test add unique('phone')	添加唯一键

## 赋权
grant select,insert,update,delete on {database}.{table or *} to '{username}'@'{127.0.0.1}' identified by "{password}";
select user,host from mysql.user; 
flush privileges;


## 导出结果到CSV文件中
/*
FIELDS TERMINATED BY ---- 字段终止字符 
OPTIONALLY ENCLOSED BY ---- 封套符 
LINES TERMINATED BY ---- 行终止符
*/
select * from test into outfile '/tmp/test.csv' 
fields terminated by ',' 
optionally enclosed by '"' escaped by '"'   
lines terminated by '\r\n'; 



```


### Master-Slave配置（如果master在使用，则需要锁表，不能按照以下步骤操作）

```
1、进入Master MySQL
GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO '{username}'@'%' IDENTIFIED BY '{password}'; 
select user,host from mysql.user; 
flush privileges;

2、读取主服务器上当前的二进制日志名和偏移量值(master) 
mysql> show master status; 
*************************** 1. row *************************** 
File: binlog.000003 
Position: 785 
Binlog_Do_DB: 
Binlog_Ignore_DB: 
1 row in set (0.00 sec) 
File列显示日志名，而Position显示偏移量。以后设置从服务器时需要使用这些值。它们表示复制坐标，从服务器应从该点开始从主服务器上进行新的更新。 

3、设置从服务器(slave) 
sql>CHANGE MASTER TO 
MASTER_HOST='192.168.0.1', 
MASTER_USER='replication_user_name', 
MASTER_PASSWORD='replication_password', 
MASTER_LOG_FILE='recorded_log_file_name', 
MASTER_LOG_POS='recorded_log_position'; 

sql> 
change master to 
master_host="192.168.0.1", 
master_user='{username}', 
master_password='{password}', 
master_port=3306, 
master_connect_retry=60, 
master_log_file='binlog.000009', 
master_log_pos=120; 

4、启动从库复制(slave) 
sql>START SLAVE; 

5、检查read_only参数(每次重启mysql后都要重新设置) 
set global read_only=on; 
show variables like 'read_only';
```