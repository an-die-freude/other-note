#### 一  Windows系统

##### 1 mysql下载

https://dev.mysql.com/downloads/mysql/

##### 2 解压后在根目录创建文件my.ini，内容根据需求更改

```
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=D:\\Tools\\mysql-8.0.26
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。
max_connect_errors=10
# 服务端使用的字符集默认为utf8mb4
character-set-server=utf8mb4
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证
#mysql_native_password
default_authentication_plugin=mysql_native_password
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8mb4
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8mb4
```

##### 3 启动mysql

1） 以管理员身份打开cmd并且切换到mysql的bin目录下

```shell
cd D:\Tools\mysql-8.0.26\bin
```

2） 初始化数据库

```shell
mysqld --initialize --console
```

3） 安装并启动mysql

```shell
mysqld install
```

```shell
net start mysql
```

##### 4 忘记密码的情况下清空密码

1） 停止mysql服务

```shell
net stop mysql
```

2） 无密码启动

```shell
mysqld --console --skip-grant-tables --shared-memory
```

3） 打开另一个cmd窗口就可以无密码登录

```shell
mysql -u root
```

4） 清空密码

```sql
update mysql.user SET authentication_string='' where user='root' and host='localhost'
```

##### 5 忘记密码的情况下更改密码

1） 停止mysql服务

2） 在任意目录创建txt文件，这里以D盘的根目录下的123456.txt为例，内容如下

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY '新密码'
```

3） 通过cmd切换到mysql的bin目录下并执行以下指令

```
mysqld --init-file=D:\123456.txt --console
```

#### 二 Ubuntu系统

##### 1 更新系统并安装mysql

```shell
apt update
apt install mysql-server
```

##### 2 切换到root用户并初始化

```shell
su root
mysql_secure_installation
```

##### 3 登录数据库

```shell
mysql -u root -p
```

#### 三 Ubuntu系统docker部署

##### 1 拉取镜像

```shell
docker pull mysql
```

##### 2 在任意目录下创建如下目录结构

```
mysql/
├ conf/
├ mysql/
├ log/
├ mysql-files/
├ docker-compose.yml
```

docker-compose.yml

```yaml
version: '3.1'
services:
    mysql:
        container_name: mysql #容器名
        network_mode: host #网络模式
        environment:
            MYSQL_ROOT_PASSWORD: 123456 #root 用户密码
            MYSQL_USER: mysql # 其他用户的用户名
            MYSQL_PASS: 123456 # 其他用户的密码
        command:
            --default-authentication-plugin=mysql_native_password
            --character-set-server=utf8mb4
            --collation-server=utf8mb4_general_ci
            --explicit_defaults_for_timestamp=true
            --lower_case_table_names=1
        image: mysql #使用的镜像，镜像不存在会自动下载
        restart: always # 启动方式always是自动启动
        ports:
            - 3306:3306 #映射端口，前面的是宿主机端口
        volumes: # 环境值
         	- /home/mu/program/docker-container/mysql/mysql-files:/var/lib/mysql-files
            - /home/mu/program/docker-container/mysql/mysql:/var/lib/mysql
            - /home/mu/program/docker-container/mysql/conf:/etc/mysql
            - /home/mu/program/docker-container/mysql/log:/var/log/mysql
```

##### 3 运行容器

切换到/home/mu/program/docker-container/mysql目录下

```shell
docker-compose --compatibility up -d
```

```shell
docker ps
```

```shell
docker exec -it <container ID> /bin/bash
```

```shell
mysql -u root -p
```

如果失败可以查看启动日志

```shell
docker logs --tail 50 --follow --timestamps <container name>
```

授权远程访问

```sql
select host, user, authentication_string, plugin from user;
GRANT ALL ON *.* TO 'root'@'%';
flush privileges;
```

#### 四 搭建主从配置

##### 1 修改主数据库的配置文件并重启mysql

添加如下内容

```
[mysqld]
server-id=1
binlog-do-db=<需要同步的数据库名>	#可设置多个
binlog-ignore-db=<不同步的数据库名>
```

##### 2 在主数据库新建一个账号用来复制数据并授权

```sql
CREATE USER `slave`@`%` IDENTIFIED WITH mysql_native_password BY '123456'
```

```sql
GRANT REPLICATION SLAVE ON *.* to 'slave'@'192.168.137.7';
FLUSH PRIVILEGES
```

##### 3 查看主数据库当前二进制日志名和偏移量

```sql
SHOW MASTER STATUS
```

![image-20210926235022893](https://gitee.com/k864197/img/raw/master/mysql_install_1.png)

这个操作的目的是为了在从数据库启动后，从这个点开始进行数据的恢复

##### 4 修改从数据库的配置文件并重启mysql

添加如下内容：

```
[mysqld]
server-id=2	#区别主数据库的server-id
```

##### 5 从数据库执行

```sql
CHANGE MASTER TO master_host = "192.168.1.10",
master_user = "slave",
master_password = "123456",
master_log_file = "binlog.000010",
master_log_pos = 370
```

master_host即主数据库ip，master_user、master_password即主数据库创建的那个用户和密码，master_log_file、master_log_pos即主数据库的二进制日志名和偏移量

```sql
START SLAVE
```

```sql
SHOW SLAVE STATUS
```

![image-20210926235022894](https://gitee.com/k864197/img/raw/master/mysql_install_2.png)

