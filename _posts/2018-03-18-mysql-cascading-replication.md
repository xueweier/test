---
layout: post
title: mysql级联复制(pos)
category: tech
tags: mysql
---
![](https://cdn.kelu.org/blog/tags/mysql.jpg)



本文记录 mysql 数据库级联同步的配置步骤。

目前已有3、4两台以pos的方式互为主主。目前将其迁移到新环境1、2，故而先对其进行级联复制，使用4为主库，1为从库，2为子从库，最后再将1和2调整为主主同步。

由于原环境为pos的方式进行同步，故而级联复制也以pos方式进行。

# my.cnf 配置

1. 三个数据库实例修改my.cnf配置文件，

   * 主库4  和 从库1 要打开log-bin


   * 三个server-id不能一样

   ```
   [mysqld]
   server-id=1 #UPDATE //主主或主备节点的ID需要不一样
   log-bin=mysql-bin
   binlog_format=MIXED
   innodb_buffer_pool_size = 8589934592 #update
   max_connections=200 #update
   sync_binlog=1
   slave-skip-errors = 1062,1032,1060
   log-slave-updates
   innodb_flush_log_at_trx_commit=1
   bind-address = 0.0.0.0
   relay-log=mysql-relay-bin
   auto_increment_increment=2 #UPDATE
   auto_increment_offset=1 #UPDATE:master-1, slave-2
   datadir=/var/lib/mysql
   socket=/var/lib/mysql/mysql.sock
   symbolic-links=0
   ```

2. 4上设置从库账号，导出库文件。

   ```
   # mysql-h0.0.0.0 -ppasswd

   mysql> grantreplication slave on *.* to 'slave'@'%' identified by 'slave';
   mysql> flushprivileges;
   mysql> \q;

   # 导出库文件
   mysqldump -h0.0.0.0 -ppasswd --master-data --single-transaction --hex-blob -R --triggers --routines --events -A >/var/lib/mysql/dmflow_all.sql
   ```

3. 获取file 与 pos点 

   ```
   cat /var/lib/mysql/dmflow_all.sql |grep "CHANGE MASTER TO" | head -1
   # 结果显示
   CHANGE MASTER TO MASTER_LOG_FILE='mysql-bin.000341', MASTER_LOG_POS=857418428;
   ```

4. 新建1上的数据库：

   ```
   # 修改 my.cnf 中的相关配置

   # 运行镜像
   sudo docker run --name xxx --restart=unless-stopped -p 3000:3306 -v /app/mysql/my.cnf:/etc/mysql/my.cnf -v /app/mysql/datadir:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=passwd -d percona:5.7
   cp dmflow_all.sql /app/mysql/datadir
   ```

   使用客户端测试可以连通即ok。

5. 数据库1还原，这里包括用户和数据都和源库完全一致

   ```
   docker exec -it xxx /bin/bash
   cd /var/lib/mysql
   mysql -h0.0.0.0 -ppasswd
   mysql> source dmflow_all.sql
   ```

6. 数据库1同步4 

   ```
   mysql> grant replication slave on *.* to 'slave'@'%' identified by 'slave';
   mysql> flush privileges;
   mysql> change master to master_host='xxx', master_port=3000, master_user='slave', master_password='slave', master_log_file='mysql-bin.000341', master_log_pos=857418428;
   mysql> start slave;
   mysql> show slave status\G;
   ```

   主要确认两项：

   （a）Master_Log_File、Read_Master_Log_Pos 与配置的参数一致；

   （b）Slave_IO_Running、Slave_SQL_Running都为YES； 

7. 验证:

   在 4 上的 MySQL 镜像进行测试

   ```
   mysql -h0.0.0.0 -ppasswd

   mysql> create database test;
   mysql> show databases;
   mysql> show slave status\G;
   ```

   同时在 2.3.255.1 中确认已建好数据库：

   ```
   mysql> show databases;
   mysql> show slave status\G;
   ```

   按照同样的方式，导出1的数据文件，将2作为1的slave 。