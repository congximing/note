# MySQL安装

```shell
#卸载自带的Mysql-libs
rpm -qa | grep -i -E mysql\|mariadb | xargs -n1 sudo rpm -e --nodeps
#安装mysql
rpm -ivh 01-05...
#启动
systemctl start mysqld
#查看密码
cat /var/log/mysqld.log | grep password

bin/mysqld --initialize --user=mysql --basedir=/opt/module/mysql --datadir=/opt/module/mysql/data
```

错误

```shell
#centos8,版本过高
ln -s libtinfo.so.5 /usr/lib/libncurses.so.5.9
dnf install ncurses-compat-libs\

#no digest 
rpm -ivh --nodigest --nofiledigest 

#not support authentication	--Navicat
ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER;
alter user 'root'@'localhost' identified with mysql_native_passwor by '123456';

#1130 is not allowed to connect to this mysql
update mysql.user set host='%' where user='root';
flush privileges;
```

删除

```sh
rm -fr /usr/lib/mysql
rm -fr /usr/include/mysql
rm -f /etc/my.cnf
rm -fr /var/lib/mysql
```

# 