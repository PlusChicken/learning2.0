# Mysql学习(坑)

mysql zip下载的时候在启动的时候会闪退.

下载地址：https://dev.mysql.com/downloads/mysql/

#### 1.配置是my.ini文件

~~~~ini
		 [mysql]
         # 设置mysql客户端默认字符集
         default-character-set=utf8 
         [mysqld]
         #设置3306端口
         port = 3306 
         # 设置mysql的安装目录

         basedir=D:\App\Mysql\mysql-8.0.17-winx64
         # 设置mysql数据库的数据的存放目录
         datadir=D:\App\Mysql\mysql-8.0.17-winx64\\data

         # 允许最大连接数
         max_connections=200
         # 服务端使用的字符集默认为UTF8
         character-set-server=utf8
         # 创建新表时将使用的默认存储引擎
         default-storage-engine=INNODB
~~~~

#### 2.运行

mysqld --initialize 

mysql -initiall

net start mysql 

#### 3.MySQL的初始密码

初始密码在一个叫.err的文件里面在mysql下的data文件夹

使用

~~~~mysql
mysql -h localhost -u root -p
#下面输入密码
~~~~

可以使用Navicat进行修改密码.

#### 4.配置路径



## 删除数据库操作

##### 1.cmd输入 net stop mysql

##### 2.cmd在 bin 中输入sc delete mysql

##### 3.注册表中删除

（1）HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\Eventlog\Application\MySQLD 目录删除
（2）HKEY_LOCAL_MACHINE\SYSTEM\ControlSet002\Services\Eventlog\Application\MySQLD 目录删除
（3）HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Eventlog\Application\MySQLD 目录删除

##### 4.再不行cmd bin里面 mysqld -remove

##### 🆗