 # Docker安装Mysql
**使用Docker安装mysql:**

1. 下载镜像：
    docker pull mysql:8.0.16
    
2. 配置文件：
    1.1. 创建文件夹：
      mkdir -p /opt/docker-mysql/conf.d
    1.2. 增加并修改配置文件：
      config-file.cnf
    1.3. config-file.cnf 内容：
      [mysqld]
      #表名不区分大小写
      lower_case_table_names=1 
      #server-id=1
      datadir=/var/lib/mysql
      #socket=/var/lib/mysql/mysqlx.sock
      #symbolic-links=0
      # sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES 
      [mysqld_safe]
      log-error=/var/log/mysqld.log
      pid-file=/var/run/mysqld/mysqld.pid
3. 增加数据文件夹
    mkdir -p /opt/docker-mysql/var/lib/mysql
4. 创建容器
  docker run --name mysql \
    --restart=always \
    -p 3306:3306 \
    -v /opt/docker-mysql/conf.d:/etc/mysql/conf.d \
    -v /opt/docker-mysql/var/lib/mysql:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=123456-abc \
    -d mysql:8.0.16
 5. 常用命令
  进入容器：docker exec -it mysql bash
  查看日志：docker logs -f mysql
  备份数据：docker exec mysql sh -c 'exec mysqldump --all-databases -uroot -p"123456-abc"' > /some/path/on/your/host/all-databases.sql
  恢复数据：docker exec -i mysql sh -c 'exec mysql -uroot -p"123456-abc"' < /some/path/on/your/host/all-databases.sql
