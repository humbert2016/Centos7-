---------------安装node---------------
mkdir node

wget https://nodejs.org/dist/v10.15.0/node-v10.15.0-linux-x64.tar.xz

tar xvf node-v10.15.0-linux-x64.tar.xz

ln -s /home/node/node-v10.15.0-linux-x64/bin/node /usr/local/bin/node
ln -s /home/node/node-v10.15.0-linux-x64/bin/npm /usr/local/bin/npm



---------------安装nginx---------------
mkdir nginx

yum install gcc gcc-c++

yum install pcre pcre-devel

yum install zlib zlib-devel

yum install openssl openssl-devel

wget https://nginx.org/download/nginx-1.14.2.tar.gz

tar zxf nginx-1.14.2.tar.gz

cd nginx-1.14.2

./configure

make

make install

cd到/usr/local/nginx/sbin/
输入命令./nginx -t 启动

修改/usr/local/nginx/nginx.conf    指向目录 root  /home/web

nginx安装目录地址 -c nginx配置文件地址
/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf

./nginx -s reload



---------------安装mysql5.7---------------
设置mysql5.7安装源
rpm -Uvh  http://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm

安装mysql
yum -y install mysql-community-server

启动mysql
systemctl start mysqld

设置开机启动
systemctl enable mysqld

在/var/log/mysqld.log文件中给root生成了一个默认密码，然后登录mysql进行修改
cat /var/log/mysqld.log | grep "A temporary password is generated for root"
2019-01-03T05:41:47.164940Z 1 [Note] A temporary password is generated for root@localhost: zMnep.TsF3tE

zMnep.TsF3tE便是root密码，修改root密码
mysql -u root -p
zMnep.TsF3tE

设置新密码
ALTER USER 'root'@'localhost' IDENTIFIED BY 'BBmimi.ma';

退出mysql
mysql> exit

设置编码
vim /etc/my.cnf

如下设置
[mysqld]
character_set_server=utf8
init_connect='SET NAMES utf8'

重启mysql
systemctl restart mysqld

服务器端开启远程连接
首先进入mysql数据库，然后输入下面两个命令：
grant all privileges on *.* to 'root'@'%' identified by 'password';
flush privileges;
第一个*是数据库，可以改成允许访问的数据库名称
第二个* 是数据库的表名称，*代表允许访问任意的表
root代表远程登录使用的用户名，可以自定义
%代表允许任意ip登录，如果你想指定特定的IP，可以把%替换掉就可以了
password代表远程登录时使用的密码，可以自定义
flush privileges;这是让权限立即生效

修改my.cnf配置文件
通过vim编辑该文件，找到bind-address = 127.0.0.1这一句，然后在前面加个#号注释掉，保存退出，没有就不用

重启mysql
systemctl restart mysqld

