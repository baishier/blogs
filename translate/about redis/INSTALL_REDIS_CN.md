## redis 安装
使用下面的命令，下载、解压、编译Redis:

    $ wget http://download.redis.io/releases/
    $ tar xzf redis-x.x.x.tar.gz
    $ cd redis-x.x.x
    $ make

编译好的二进制文件可以在 src 文件夹中找到. 使用如下命令，运行 Redis:

    $ src/redis-server
使用redis内置的客户端，命令如下:

    $ src/redis-cli
    redis> set foo bar
    OK
    redis> get foo
    "bar"


### 提示：

    那个`~`符号代表的是『使用者的家目录』的意思，他是个『变量！』
    这相关的意义我们会在后续的章节依序介绍到。举例来说， root
    的家目录在 /root， 所以 `~` 就代表 /root 的意思，而 dmtsai
    的家目录在 /home/dmtsai， 所以如果您以 dmtsai 登入时，他看
    到的 `~` 就会等于 /home/dmtsai 喔！
    至于提示字符方面，在 Linux 当中，预设 root 的提示字符为 `#` ，
    而一般身份使用者的提示字符为 `$` 。

## 附，安装常见问题：
### 1.redis后台运行

1、我们要把src目录下面的redis-cli、redis-server、redis-sentinel移到/usr/bin目录下面
    # cd src  
    # mv redis-cli redis-server redis-sentinel /usr/bin/  
    # cd ..  
    # mkdir -p /etc/redis/  
    # cp redis.conf /etc/redis/redis.conf
2、现在打开文件/etc/redis/redis.conf， 找到‘daemonize no’改为‘daemonize yes‘，然后启动它！  
    # redis-server /etc/redis/redis.conf  

### 2.【CentOS】make cc Command not found，make: *** [adlist.o] Error 127”
在linux系统上对redis源码进行编译时提示提示“make cc Command not found，make: * [adlist.o] Error 127”。这是由于系统没有安装gcc环境，因此在进行编译时才会出现上面提示，当安装好gcc后再进行编译时，上面错误提示将消失。

#### 一，rpm包下载
http://vault.centos.org/7.0.1406/os/x86_64/Packages/
里面有Centos7 所有rpm包
下载如下的包，每个包之间都有互相依赖

    cpp-4.8.2-16.el7.x86_64.rpm
    gcc-4.8.2-16.el7.x86_64.rpm
    glibc-2.17-55.el7.x86_64.rpm
    glibc-common-2.17-55.el7.x86_64.rpm
    glibc-devel-2.17-55.el7.x86_64.rpm
    glibc-headers-2.17-55.el7.x86_64.rpm
    glibc-static-2.17-55.el7.x86_64.rpm
    glibc-utils-2.17-55.el7.x86_64.rpm
    kernel-headers-3.10.0-123.el7.x86_64.rpm
    libmpc-1.0.1-3.el7.x86_64.rpm
    mpfr-3.1.1-4.el7.x86_64.rpm

#### 二，安装
将下载好的包传到Linux中后，使用命令进行安装

```
~ rpm -Uvh *.rpm --nodeps --force
```

#### 三，完成
用命令验证
```
~ gcc -v
```

### 更多的关于 /usr/local/redis/etc/redis.conf 的配置信息
1、daemonize 如果需要在后台运行，把该项改为yes

2、pidfile 配置多个pid的地址 默认在/var/run/redis.pid

3、bind 绑定ip，设置后只接受来自该ip的请求

4、port 监听端口，默认是6379

5、loglevel 分为4个等级：debug verbose notice warning

6、logfile 用于配置log文件地址

7、databases 设置数据库个数，默认使用的数据库为0

8、save 设置redis进行数据库镜像的频率。

9、rdbcompression 在进行镜像备份时，是否进行压缩

10、dbfilename 镜像备份文件的文件名

11、Dir 数据库镜像备份的文件放置路径

12、Slaveof 设置数据库为其他数据库的从数据库

13、Masterauth 主数据库连接需要的密码验证

14、Requriepass 设置 登陆时需要使用密码

15、Maxclients 限制同时使用的客户数量

16、Maxmemory 设置redis能够使用的最大内存

17、Appendonly 开启append only模式

18、Appendfsync 设置对appendonly.aof文件同步的频率（对数据进行备份的第二种方式）

19、vm-enabled 是否开启虚拟内存支持 （vm开头的参数都是配置虚拟内存的）

20、vm-swap-file 设置虚拟内存的交换文件路径

21、vm-max-memory 设置redis使用的最大物理内存大小

22、vm-page-size 设置虚拟内存的页大小

23、vm-pages 设置交换文件的总的page数量

24、vm-max-threads 设置VM IO同时使用的线程数量

25、Glueoutputbuf 把小的输出缓存存放在一起

26、hash-max-zipmap-entries 设置hash的临界值

27、Activerehashing 重新hash  

## 参考文献
    http://blog.csdn.net/yangjjuan/article/details/70244935
    https://redis.io
    http://blog.csdn.net/lk10207160511/article/details/50364088
    http://blog.csdn.net/baidu_30000217/article/details/51476712  
    《鸟哥的Linux私房菜》
