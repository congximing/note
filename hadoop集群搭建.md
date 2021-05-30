# Linux

## rpm

```shell
rpm -qa | grep -i mysql
rpm -ev mysql.*
```



## ssh

```shell
#安装
yum list installed | grep openssh-server
yum install openssh-server
#修改配置
vim /etc/ssh/sshd_config
Port 22
ListenAddress 0.0.0.0
ListenAddress ::
PermitRootLogin yes
PasswordAuthentication yes
#开启服务
service sshd start
#检查
ps -e|grep sshd
netstat -an | grep 22
#自开启
systemctl enable sshd
systemctl is-enabled sshd
systemctl list-unit-files | grep sshd
```

## firewall

```shell
#查看防火墙某个端口是否开放
firewall-cmd --query-port=9870/tcp
#开放防火墙端口9870
firewall-cmd --zone=public --add-port=9870/tcp --permanen
#关闭80端口
firewall-cmd --zone=public --remove-port=80/tcp --permanent  
#查看开放的端口列表
firewall-cmd --zone=public --list-ports
#配置立即生效
firewall-cmd --reload 


#关闭firewall ,start status
systemctl stop firewalld
```



# vim

```
gg
v
shift+g
=
```



# 配置JAVA环境

```shell
export JAVA_HOME=/opt/module/jdk1.8.0_212
export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin
```

# Hadoop

## 网卡配置

```shell
#修改hostname,只是临时修改的，没什么用
vim /etc/hostname

#修改hostname
vim /etc/sysconfig/network
#修改hosts
vim hosts

#修改为静态网卡
vim /etc/udev/rules.d/70-persistent-net.rules#去掉前一个克隆多余的网卡
vim /etc/sysconfig/network-scripts/ifcfg-eth0#修改HWADDR

TYPE=“Ethernet”
PROXY_METHOD=“none”
BROWSER_ONLY=“no”
BOOTPROTO=“static” # 设置为static为静态
DEFROUTE=“yes”
IPV4_FAILURE_FATAL=“no”
IPV6INIT=“yes”
IPV6_AUTOCONF=“yes”
IPV6_DEFROUTE=“yes”
IPV6_FAILURE_FATAL=“no”
IPV6_ADDR_GEN_MODE=“stable-privacy”
NAME=“eth0”
UUID=“3d9407ec-c449-4d84-822b-9b1ffa986d97”
DEVICE=“eth0”
ONBOOT=“yes” # 设置开机启动设置
IPADDR=“192.168.32.184” # 该虚拟机的静态IP，这边要跟win10的保持同一网段
GATEWAY=“192.168.32.177” # win10的ip地址
NETMASK=“255.255.255.240” # win10的默认网关
DNS1=“192.168.32.177”
#restart
systemctl restart NetworkManager

```

## 集群分发脚本

```shell
#集群分发脚本
#!/bin/bash
#1 获取输入参数个数，如果没有参数，直接退出
pcount=$#
if((pcount==0)); then
echo no args;
exit;
fi

#2 获取文件名称
p1=$1
fname=`basename $p1`
echo fname=$fname

#3 获取上级目录到绝对路径
pdir=`cd -P $(dirname $p1); pwd`
echo pdir=$pdir

#4 获取当前用户名称
user=`whoami`

#5 循环
for((host=129; host<131; host++)); do
        echo ------------------- hadoop$host --------------
        rsync -rvl $pdir/$fname $user@hadoop$host:$pdir
done
```

## ssh免密登录

```shell
#ssh免密登录
#ssh 172.16.40.129
ssh-keygen -t rsa#id_rsa（私钥）、id_rsa.pub（公钥）
ssh-copy-id hadoop128
ssh-copy-id hadoop129
ssh-copy-id hadoop130
```

## 配置Hadoop

### core-site.xml

```xml
<!-- 指定HDFS中NameNode的地址 -->
<property>
		<name>fs.defaultFS</name>
      <value>hdfs://hadoop128:9000</value>
</property>

<!-- 指定Hadoop运行时产生文件的存储目录 -->
<property>
		<name>hadoop.tmp.dir</name>
		<value>/opt/module/hadoop-3.1.3/data/tmp</value>
</property>
<!-- 配置HDFS网页登录使用的静态用户为cxm -->
    <property>
        <name>hadoop.http.staticuser.user</name>
        <value>cxm</value>
</property>

<!-- 配置该cxm(superUser)允许通过代理访问的主机节点 -->
    <property>
        <name>hadoop.proxyuser.cxm.hosts</name>
        <value>*</value>
</property>
<!-- 配置该cxm(superUser)允许通过代理用户所属组 -->
    <property>
        <name>hadoop.proxyuser.cxm.groups</name>
        <value>*</value>
</property>
<!-- 配置该cxm(superUser)允许通过代理的用户-->
    <property>
        <name>hadoop.proxyuser.cxm.groups</name>
        <value>*</value>
</property>
```

### HDFS

hadoop-env.sh

```shell
#yarn-env.sh > hadoop-env.sh > hard-coded defaults
export JAVA_HOME=/opt/module/jdk1.8.0_212
```

hdfs-site.xml

```xml
<!-- nn web端访问地址-->
<property>
       <name>dfs.namenode.http-address</name>
       <value>hadoop102:9870</value>
</property>
<!-- 2nn web端访问地址 -->
<property>
      <name>dfs.namenode.secondary.http-address</name>
      <value>hadoop130:50090</value>
</property>
<!-- 测试环境指定HDFS副本的数量3 -->
<property>
		<name>dfs.replication</name>
		<value>3</value>
</property>
```

### Yarn

yarn-env.sh

```shell
export JAVA_HOME=/opt/module/jdk1.8.0_212
```

yarn-site.xml

```xml
<!-- Reducer获取数据的方式 -->
<property>
		<name>yarn.nodemanager.aux-services</name>
		<value>mapreduce_shuffle</value>
</property>

<!-- 指定YARN的ResourceManager的地址 -->
<property>
		<name>yarn.resourcemanager.hostname</name>
		<value>hadoop129</value>
</property>

<!-- 环境变量的继承 -->
<property>
<name>yarn.nodemanager.env-whitelist</name>    			     <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
</property>

<!-- yarn容器允许分配的最大最小内存 -->
<property>
    <name>yarn.scheduler.minimum-allocation-mb</name>
    <value>512</value>
</property>
<property>
    <name>yarn.scheduler.maximum-allocation-mb</name>
    <value>4096</value>
</property>

<!-- yarn容器允许管理的物理内存大小 -->
<property>
    <name>yarn.nodemanager.resource.memory-mb</name>
    <value>4096</value>
</property>

<!-- 关闭yarn对物理内存和虚拟内存的限制检查 -->
<property>
    <name>yarn.nodemanager.pmem-check-enabled</name>
    <value>false</value>
</property>
<property>
    <name>yarn.nodemanager.vmem-check-enabled</name>
    <value>false</value>
</property>

<!-- 开启日志聚集功能 -->
<property>
    <name>yarn.log-aggregation-enable</name>
    <value>true</value>
</property>

<!-- 设置日志聚集服务器地址 -->
<property>  
    <name>yarn.log.server.url</name>  
    <value>http://hadoop128:19888/jobhistory/logs</value>
</property>

<!-- 设置日志保留时间为7天 -->
<property>
    <name>yarn.log-aggregation.retain-seconds</name>
    <value>604800</value>
</property>    
```

### MapReduce

mapred-env.sh

```shell
export JAVA_HOME=/opt/module/jdk1.8.0_212
```

mapred-site.xml

```xml
<!-- 指定MR运行在Yarn上 -->
<property>
		<name>mapreduce.framework.name</name>
		<value>yarn</value>
</property>
<!-- 历史服务器端地址 -->
<property>
    <name>mapreduce.jobhistory.address</name>
    <value>hadoop128:10020</value>
</property>

<!-- 历史服务器web端地址 -->
<property>
    <name>mapreduce.jobhistory.webapp.address</name>
    <value>hadoop128:19888</value>
</property>
```

### 格式化NameNode

```shell
#一定要先停止上次启动的所有namenode和datanode进程，然后再删除data和log数据
hadoop namenode -format
```

### 集群启动

```shell
#hdfs
sbin/start-dfs.sh

HDFS_DATANODE_USER=root
HADOOP_SECURE_DN_USER=hdfs
HDFS_NAMENODE_USER=root
HDFS_SECONDARYNAMENODE_USER=root
#yarn
sbin/start-yarn.sh

YARN_RESOURCEMANAGER_USER=root
HADOOP_SECURE_DN_USER=yarn
YARN_NODEMANAGER_USER=root

#---略nameNode 3*dataNode
hadoop-daemon.sh start namenode
hadoop-daemon.sh start datanode

yanr-daemon.sh start resourcemanager
yarn-daemon.sh start nodemanager

```

### 集群关闭

```shell
sbin/stop-yarn.sh
sbin/stop-dfs.sh
```

### 集群时间同步

```shell
#检查ntp是否安装
rpm -qa|grep ntp

#添加wlnmp源
rpm -ivh http://mirrors.wlnmp.com/centos/wlnmp-release-centos.noarch.rpm
#安装ntp服务
yum install wntp
#时间同步
ntpdate ntp1.aliyun.com

#查看所有节点ntpd服务状态和开机自启动状态
systemctl status ntpd
systemctl is-enabled ntpd
#关闭ntp服务和自启动
systemctl stop ntpd
systemctl disable ntpd
```

### 历史服务器

```shell
# 启动Hadoop 历史服务器
mr-jobhistory-daemon.sh start historyserver
insert into table student values(1,'abc');
```

### 日志排错

```
yarn logs -applicationId [JobID]
```



### web页面

```
//HDFS
http://172.16.40.128:9870/
//SecondaryNameNode
http://172.16.40.130:9868/
```

### 测试

```shell
#hadoop3.1.3/
mkdir wcinput
cd wcinput
vim wc.input
cd ..
#wordcount
hadoop jar \
share/hadoop/mapreduce/hadoop-mapreduce-examples-3.1.3.jar wordcount \
/input /output
```

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

# zookeeper

```shell
#启动 stop,status
zkServer.sh start
#启动客户端
zkCli.sh
```





