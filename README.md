# MongoDB
MongoDB安装踩坑之路

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
