# MongoDB
# MongoDB踩坑之路

<!-- 安装篇 -->

安装时出现：
"service 'mongodb server' failed to start. verify that you have sufficient privileges to start system services"
解决办法：
直接点Ignore（忽略），然后进入安装MongoDB的目录下，进入data，新建一个文件夹然后命名为db。
注意：路径最好是全英文。

配置数据库和连接数据库：
进入安装MongoDB的文件夹
进入bin文件夹
进入命令行窗口后
$ mongod -dbpath d:\'这是你安装mongoDB的文件夹，根据你自己的命名来写'\data\db

继续在bin文件夹里打开命令行窗口，
$ mongo
此时就成功连接数据库了。



<!-- 启动篇 -->

 启动Mongodb服务有两种方式，前台启动或者Daemon方式启动，前者启动会需要保持当前Session不能被关闭，后者可以作为系统的fork进程执行，下文中的path是mongodb部署的实际地址。

1. 最简单的启动方式，前台启动，仅指定数据目录，并且使用默认的27107端口，cli下可以直接使用./mongo连上本机的mongodb，一般只用于临时的开发测试。
mongod --dbpath=/path/mongodb

2. 启动绑定固定的IP地址、端口，这就mongo在连接mongod的时候就需要指定IP和端口了。
mongo 10.10.10.10:12345
 
3. daemon后台运行，简单的是命令后面加“&”。
./mongod --dbpath=/path/mongodb --bind_ip=10.10.10.10 --port=12345 & 
或者使用mongod自带的--fork参数，此时必须指定log的路径。
./mongod --dbpath=/path/mongodb --fork=true logpath=/path/mongod.log

# 4. （推荐）以配置文件形式保存配置。
<!-- 以下为mongod.conf代码： -->
port=12345  
bind_ip=10.10.10.10  
logpath=/path/mongod.log  
pidfilepath=/path/mongod.pid  
logappend=true  
fork=true
然后启动mongod时引入配置文件：./mongod -f /path/mongod.conf  

# 下面是mongod启动的常用参数详细说明：
<!-- 参数	说明	取值示例 -->
dbpath	mongodb数据文件存储路径	/data/mongodb
logpath	mongod的日志路径	/var/log/mongodb/mongodb.log
logappend	日志使用追加代替覆盖	true
bind_ip	绑定的IP	10.10.10.10
port	绑定的端口	27107
journal	write操作首先写入“日记”，是一个数据安全的设置，具体参考官方文档。	true

5 Mongodb开机启动
在/etc/rc.local文件末尾添加下面的代码
#add mongodb service
rm -rf /data/mongodb_data/* && /usr/local/mongodb/bin/mongod --dbpath=/data/mongdb_data/ --logpath=/data/mongdb_log/mongodb.log --logappend &
