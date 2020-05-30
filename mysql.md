## mysql

### mysql 8.0.20安装

-  下载mysql-8.0.20-winx64.zip

- 解压压缩包

- 在解压目录下创建my.ini文件

  ```shell
  [mysqld]
  # 设置3306端口
  port=3306
  # 设置mysql的安装目录
  basedir=C:\Program Files\MySQL
  # 设置mysql数据库的数据的存放目录
  datadir=C:\Program Files\MySQL\Data
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

- 在管理员命令行中进入mysql安装目录的bin目录

- 输入下面命令

```shell
mysqld --initialize --console
# 成功会有一个root用户的临时密码，记住
# A temporary password is generated for root@localhost: &%s5n=nfkwyW

# 安装mysql服务
mysqld --install

# 开启服务
net start mysql

# 登录mysql
mysql -u root -p
# 输入刚刚的临时密码（不包含开头的空格）

# 修改密码
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';

# 退出
exit
```

