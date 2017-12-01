## CentOS7 离线安装mysql-5.7.16

#### 1 . 安装新版mysql前，需将系统自带的mariadb-lib卸载
    [root@slave mytmp]# rpm -qa|grep mariadb
    mariadb-libs-5.5.44-2.el7.centos.x86_64
    [root@slave mytmp]# rpm -e --nodeps mariadb-libs-5.5.44-2.el7.centos.x86_64
#### 2 . 解压mysql
    [root@slave mytmp]# tar -zxf mysql-5.7.16-1.el7.x86_64.rpm-bundle.tar
    [root@slave mytmp]# ls
    mysql-5.7.16-1.el7.x86_64.rpm-bundle.tar                 mysql-community-libs-5.7.16-1.el7.x86_64.rpm
    mysql-community-client-5.7.16-1.el7.x86_64.rpm           mysql-community-libs-compat-5.7.16-1.el7.x86_64.rpm
    mysql-community-common-5.7.16-1.el7.x86_64.rpm           mysql-community-minimal-debuginfo-5.7.16-1.el7.x86_64.rpm
    mysql-community-devel-5.7.16-1.el7.x86_64.rpm            mysql-community-server-5.7.16-1.el7.x86_64.rpm
    mysql-community-embedded-5.7.16-1.el7.x86_64.rpm         mysql-community-server-minimal-5.7.16-1.el7.x86_64.rpm
    mysql-community-embedded-compat-5.7.16-1.el7.x86_64.rpm  mysql-community-test-5.7.16-1.el7.x86_64.rpm
    mysql-community-embedded-devel-5.7.16-1.el7.x86_64.rpm
#### 3 . 使用rpm -ivh命令依次进行安装
    rpm -ivh mysql-community-common-5.7.16-1.el7.x86_64.rpm

    rpm -ivh mysql-community-libs-5.7.16-1.el7.x86_64.rpm

    rpm -ivh mysql-community-client-5.7.16-1.el7.x86_64.rpm
-

    安装mysql-community-server-5.7.16-1.el7.x86_64.rpm 前需要安装libaio-0.3.107-10.el6.x86_64.rpm

    下载地址：
    http://mirror.centos.org/centos/6/os/x86_64/Packages/libaio-0.3.107-10.el6.x86_64.rpm

    安装libaio库：
    rpm -ivh libaio-0.3.107-10.el6.x86_64.rpm（若在有网情况下可执行yum install libaio）

    安装mysql-community-server：
    rpm -ivh mysql-community-server-5.7.16-1.el7.x86_64.rpm

#### 4 . 初始化数据库

```java
// 指定datadir, 执行后会生成~/.mysql_secret密码文件
[root@slave mytmp]# mysql_install_db --datadir=/var/lib/m

// 初始化，执行生会在/var/log/mysqld.log生成随机密码
[root@slave mytmp]# mysqld --initialize
```

#### 5 . 更改mysql数据库目录的所属用户及其所属组，并启动mysql数据库

    [root@slave mytmp]# chown mysql:mysql /var/lib/mysql -R
    [root@slave mytmp]# systemctl start mysqld.service

#### 6 . 登录到mysql，更改root用户的密码

    // password 通过 cat ~/.mysql_secret 命令可以查看初始密码

    [root@slave mytmp]# mysql -uroot -p
    Enter password:

    mysql> set password=password('1234');

## 常见问题

### 一、MySQL ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)
1.解决思路，设置skip-grant-tables免密登录

2.修改密码
```sql
mysql>use mysql ;
mysql> UPDATE user SET authentication_string=PASSWORD('newpassword') where USER='root' and host='root' or host='localhost';//把空的用户密码都修改成非空的密码就行了。
mysql> FLUSH PRIVILEGES;
```
3.skip-grant-tables免密登录取消，重启mysql.


## 参考文献
    http://blog.csdn.net/zz657114506/article/details/53553845
    http://www.mysql.com
    https://www.cnblogs.com/kerrycode/p/4368312.html
