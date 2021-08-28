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

```
cd D:\Tools\mysql-8.0.26\bin
```

2） 初始化数据库

```
mysqld --initialize --console
```

3） 安装并启动mysql

```
mysqld install
```

```
net start mysql
```

##### 4 忘记密码的情况下清空密码

1） 停止mysql服务

```
net stop mysql
```

2） 无密码启动

```
mysqld --console --skip-grant-tables --shared-memory
```

3） 打开另一个cmd窗口就可以无密码登录

```
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

