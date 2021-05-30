# 规划

必看：离散数学 算法设计与实现 编译原理

## 选择1：

基础数学、计算数学、概率论与数理统计、应用数学、运筹学与控制论

| 学科           | 1    | 2    | 3    | 4    | 5    | 6    | 备注     |
| -------------- | ---- | ---- | ---- | ---- | ---- | ---- | -------- |
| 数学分析       | √    | √    | √    |      |      |      | 初试     |
| 高等代数       | √    | √    |      |      |      |      | 初试     |
| 解析几何       | √    |      |      |      |      |      |          |
| 近世代数       |      |      | √    |      |      |      | 复试     |
| 微分方程       |      |      | √    |      |      |      |          |
| 概率论         |      |      | √    |      |      |      | 复试(必) |
| 数理统计       |      |      |      | √    |      |      | 复试(必) |
| 复变函数       |      |      |      | √    |      |      |          |
| 拓扑学         |      |      |      | √    |      |      |          |
| 随机过程       |      |      |      |      | √    |      |          |
| 实变函数       |      |      |      |      | √    |      | 复试     |
| 泛函分析       |      |      |      |      |      | √    |          |
| 数值分析A      |      |      |      | √    |      |      | 复试(必) |
| 运筹学基础     |      |      | √    |      |      |      |          |
| 抽象代数       |      |      |      | √    |      |      | 基础数学 |
| 微分几何       |      |      |      | √    |      |      | 基础数学 |
| 偏微分方程     |      |      |      | √    |      |      | 基础数学 |
| 现代分析       |      |      |      | √    |      |      | 基础数学 |
| 数据分析与挖掘 |      |      |      | √    |      |      | 信息处理 |
| 应用模糊数学   |      |      | √    |      |      |      | 信息处理 |
|                |      |      |      |      |      |      |          |
|                |      |      |      |      |      |      |          |
|                |      |      |      |      |      |      |          |
|                |      |      |      |      |      |      |          |

|      |                  |      |
| ---- | ---------------- | ---- |
|      |                  |      |
|      |                  |      |
|      |                  |      |
|      |                  |      |
| 7    | 上海交通大学     | A    |
| 8    | 中国科学技术大学 | A    |
| 9    | 西安交通大学     | A    |
| 10   | 吉林大学         | A-   |
|      |                  |      |
| 12   | 同济大学         | A-   |
|      |                  |      |
| 14   | 南京大学         | A-   |
|      | 北京工业大学34   |      |
|      | 北京科技大学22   |      |
|      | 天津大学37       |      |
|      | 东北大学30       |      |
|      | 哈工程48         |      |
|      | 南航南理         |      |
|      | 合肥工业大学     |      |

## 选择2：

云计算运维：Linux以及Shell编程变量运用等，企业级大型网站搭建、自动化运维、企业级防火墙。每天学一点，记笔记。

大数据：flink spark

# JavaScript

## 箭头函数 与 匿名函数（函数表达式）的区别

​	this：箭头函数的this始终是声明时的外部环境对象（绑定作用域，因为没有this），声明式函数则改变

​	构造：没有`constructor`, 所以不能使用箭头函数创建对象.

共同点：function声明提升，var不能提升，而function为最高级。

## 闭包

```javascript
function createFunction(){
    var a=new Array[];
    for(var i=0;i<=10;i++){
        a[i]=function(sum){
            //return sum; 同下
            return function(){
                return sum;
            }
        }(i)
    }
}
```

# Vue

# go

## 作用域

```
:=,只是局部作用域，无法初始化，全局作用域。
解决：通过err来得到错误信息。
```

## 类型转换

### int转成str形式

```go
func FormatInt(i int64, base int) string {
	if fastSmalls && 0 <= i && i < nSmalls && base == 10 {
        //todo 切片
		return small(int(i))
	}
    //todo 转换成进制
	_, s := formatBits(nil, uint64(i), base, i < 0, false)
	return s
}
```

### str转成int

```go
strconv.parseInt("123",10,64)//10进制标示，64位
strconv.parseUint()
```

### 各种Print

```go
Fprintf()
Printf()
Sprintln()
Println()
```

## 常量

### 无类型：

不仅要维持高精度，还要比已确定的变量（左边）写进更多表达式，防止类型转换。

布尔、整数、浮点数、复数、文字符号、字符串=> 0，0.0，0i，‘\u0000’

rune等同于int32,unicode utf8

1.无类型常量赋值给声明类型的变量：

```go
var f float64 = 3 + 0i// 无类型复数 => float64
fmt.Println(f)//3
f = 'a'// 无类型常量 赋值 给类型明确的变量，进行隐式转换为变量类型
f = float64('a') //显式转换
fmt.Println(f)//97
```

2.短声明变量，转换成默认类型

```go
i := 0 //int
j := 0.0 //float64
k := 0i //complex64
l := '0' //rune 

```

3.'0' 、'\000'、'\u0000'的区别：

```go

fmt.Printf("%T\n",'\000')// int32
fmt.Printf("%T\n",'0')//int32
fmt.Printf("%T\n",'\u0000')//int32
```

int类型不确定，可能有8，16，32，64

总结：

​	声明的变量是不是有类型的，无类型常量根据这个来进行转换

​	如果没有声明变量的类型，默认转换

​	否则，转换成变量类型

## 复合类型

### 数组

```go
//声明
var a [3]int
//初始化
var a [3]int = [3]int{1,2,3}
a := [...]int{1,2,3}

r := [...]int{9:-1}//10长度，r[9]=-1,9为索引
```

省类型，省var，省长度为... ，要不要再省个{} ？？



函数参数为值传递，采用的是副本引用机制，传入一个较大数组时效率会很低。

```go
//利用指针清空数组
func zero(ptr *[32]int){
    for i := range ptr {
        ptr[i] = 0
    }
}
//2种
func zero2(ptr *[32]int ){
    *ptr = [32]int{0}
}
```

### slice 

这个比较重要(有点不太了解)

相当于一个轻量级的数据结构，可以对数组进行操作。

```go
//slice 
type slice struct{
    len int
    data *type_ptr //指向数组的某一个
    cap int 
}
//初始化数组
s := []int{2 3 4 5 0 1}
```

#### append()

可能导致内存分配的系统调用，类似redis中的string结构。

```go
var x []int 
x = append(x,1,1,1,2,3,4,5,6)//一次可以添加多个值
```

#### slice()

可以实现去除“”功能，也利用append()

实现一个栈，利用slice

### map

```go
//create
ages := make(map[string]int)
//init
ages := map[string]int{
    "alice":31,
    "charlice":34
}
//create null
ages := map[string]int{}
//delete
delete(ages,"alice")

//零值 nil
fmt.Print(ages == nil) //true
//设置map
arges["ss"]= 1//必须先初始化
//访问map
value，boolean = arges["alice"]//值，key是否存在，为了判别value==0时，这个键是否存在。
```

判断2个map是否相等

```go
func equal(x,y map[string]int) bool{
    if len(x) != len(y){
        return false
    }
    for k,xv := range x{
        if yv,ok := y[k];!ok || xv!=yv {
            return false
        }
    }
    return true
}
```

### 结构体

```go
type Point struct {
    x,y int
}
type Circle struct{
    Point
    Raduis int
}
type Wheel struct{
    Circle	//匿名结构体
    Spokes int
}
```

### Json

将结构体转化成Json，原理是反射

```go
type Movies struct{
    Title string
    //成员定义标签 json:"released"
    Year int `json:"released"` //编译期确定,Year=>released
    Color bool `json:"color,omitempty"`// Color=>colr,可省略
    Actors []string
}
func main(){
    json.Marshal(movies)
    //前缀，缩进
	json.MarshalIndent(movies,"","    ")
}
```

将Json逆解析成结构体

```go
func main(){
    var titles []struct{ Title string}
    //只解析Title字段，其他扔掉
    if err := json.Unmarshal(data,&titles); err !=  nil{
        log.Fatalf("faild case: %s",err)
    }
}
```

## 方法

### 方法声明

```go
type Point struct{
	x float64
	y float64
}
//声明类型，注意不是变量
type Path []Point

func (p*Point) ScaleBy(factory float64){
    p.X *= factory
    p.Y *= factory
}
func main(){
    //1.call
    r := &Point{1,2}//create
    r.ScaleBy(2)
    //2.call
    p := Point{1,2}
    (&p).ScaleBy(2)
    p.ScaleBy(2)//隐式转化，必须能取地址，例如Point{1,2}不能取字面量地址
}
```

## 接口

### 类型判断

```go
//检查动态类型
x.(T)
//接口实例化
var w io.writer
w = os.Stdout
f := w.(*os.File) // f == os.Sdout
```

### 类型分支

```go
//第一种风格 io.Reafer	io.Writer fmt.Stringer sort.Interface
```

## 包

```shell
#GOROOT  GOPATH
go env 
go get #D:\GoProject\pkg\mod\github.com\jinzhu\gorm@v1.9.16\dialects\mysql
go get [package]@v1.0.0
go env -w GOPROXY=https://mirrors.aliyun.com/goproxy
go build
go list
go run
```

### 打包

```shell
#GitHub托管
#拉取
go get github.com/congximing/corm 
go mod init github.com/congximing/corm #拉取并初始化一个go.mod文件
```

### mod

```shell
go env -w GO111MODULE=on #开启module
export GO111MODULE=on

go env -w GOENV="C:\Users\congximing\AppData\Roaming\go\env" #配置文件 /etc/profile

#初始化项目
mkdir Gone
cd Gone
go mod init Gone
#go.mod文件内容
module Gone
require	exclude	replace
#go.sum文件,记录 dependency tree

```



## 测试

```shell
go test
go test -v #名称+运行时间
go test -v -run="正则"
go test -bench=.

go test -cpuprofile=cpu.out
go test -blockprofile=block.out
go test -memprofile=mem.out

#实例
go test -run=NONE -bench=ClientServerParallelTLS64  -cpuprofile=cpu.log net/http #生成CPU日志文件
go tool pprof -text -nodecount=10 ./http.test cpu.log #查看日志文件
    
```



## 标准库

### http

```go
//http.Get(string)
func Get(url string) (resp *Response, err error) {}
//Response结构体
type Response struct{
    Body io.ReadCloser
}
```

### io

```go
type Reader interface {
	Read(p []byte) (n int, err error)
}
```

### encoding/json

```go
json.NewDecoder(resp.Body).Decode(&result)
//NewDecoder(resp.Body)
func NewDecoder(r io.Reader) *Decoder {
	return &Decoder{r: r}
}
//Decode(&result)
func (dec *Decoder) Decode(v interface{}) error {
	if dec.err != nil {
		return dec.err
	}
	if err := dec.tokenPrepareForDecode(); err != nil {
		return err
	}
	if !dec.tokenValueAllowed() {
		return &SyntaxError{msg: "not at beginning of value", Offset: dec.InputOffset()}
	}
	// Read whole value into buffer.
	n, err := dec.readValue()
	if err != nil {
		return err
	}
	dec.d.init(dec.buf[dec.scanp : dec.scanp+n])
	dec.scanp += n
	// Don't save err from unmarshal into dec.err:
	// the connection is still usable since we read a complete JSON
	// object from it before the error happened.
	err = dec.d.unmarshal(v)
	// fixup token streaming state
	dec.tokenValueEnd()
	return err
}

```

# git

```shell
git checkout dev
git merge main
git reset head~1
git rebase -i head~1
git commit --amend
git 
```

# 书籍推荐

影响力、怪诞行为学、思考:快与慢

经济学原理(曼昆)、国富论、资本论

理财：小狗钱钱、穷查理得宝典

怪诞行为学、看不见的大猩猩、影响力

思考，快与慢（最好原著，神作！！！，翻译烂+不好懂！）



人类简史、老人与海

围城、活着

一个女人一生中的24小时

# 微观经济学

人们面临权衡

某事的成本是为了得到它而放弃的其他东西

理性人在边际处思考

# 哲理

没有人愿意慢慢变富。

开发的替代性很强，学的越底层越不容易替代，越是需要经验的，越不容易替代。

大数据工程师，只需要写几个SQL就可以，替代性强。

Java，需要熟悉各种业务场景。

# =====任务======

springBoot,vue

spark+sql

# LeetCode

```go
//input nums = [1,1,1,2,2,3]
//逻辑地址和物理地址
func removeDuplicates(nums []int) int {
	slow, fast := 2, 2
    for fast < n {
        if nums[slow-2] != nums[fast] {
            nums[slow] = nums[fast]
            slow++
        }
        fast++
    }
}
```



# 编译原理

简单模式：扫描程序->前端->抽象语法树->后端->目标生成代码

所有语言只需要，跳转和赋值这两个操作即可。



# 架构整洁之道



# 运维

```
useradd chmod chown 
ifconfig top ps iftop atop 
awk sed cat grep find more 

NFS ftp nginx+keepalived lvs+keepalived

mysql主从	tomcat zabbix redis 

shell 

系统内核
```



# shell

```shell
~/.bash_history #历史命令
alias lm='ls -al' #会话范围别名
```

## 变量

$#：参数数量

$@：以单个字符存储所有参数，相当于数组

```shell
for param in "$@"
do
	echo "\$@ Parameter === $param"
done
```

$*：一个整字符串存储所有参数

$?：最后一次执行的命令的返回状态

## Read

```shell
#提示7秒内，读取控制台输入的名称
read -t 7 -p "Enter your name in 7 seconds 	" NAME
```



## 脚本

```shell
#磁盘监控
du -Sh / |
sort -rn |
sed '{11,$D;=}' |
sed 'N;s/\n/ /' |
gawk '{printf $1 ":" "\t" $2  "\t" $3 "\n"}'

#备份
DATE=`date +%y%m%d`
FILE=archive$DATE.tar.gz
CONFIG_FILE=/opt/module/shell/File_To_Backup
DESTINATION=/opt/module/shell/$FILE

if [ -f $CONFIG_FILE ]
then 
	echo 
else 
	echo "$CONFIG_FILE is not exit"
	exit
fi

FILE_NO=1
exec < $CONFIG_FILE #14
read FILE_NAME #读一行

while [ $? -eq 0 ]
do
	if [ -f $FILE_NAME -o -d $FILE_NAME ] #13.6
	then 
		FILE_LIST="$FILE_LIST $FILE_NAME"
	else
		echo
		echo "$FILE_NAME,does not exit"
		echo
	fi
	FILE_NO=$[ $FILE_NO + 1]
	read FILE_NAME
done
	
tar -zcvf $DESTINATION $FILE_LIST 2 > /dev/null
                
```



```shell
#检验文件是否存在
if [ -f $file ]
	echo "file exit"
else
	echo "not exit"
fi
```



## 正则表达式

```shell
wget http://linux.vbird.org/linux_basic/0330regularex/regular_express.txt
#1. 搜索特定字符串
grep -n 'the' regular_express.txt
#反向选择
grep -vn 'the' regular_express.txt
#ingore大小写
grep -in 'the' regular_express.txt

#2. []搜索taste test
grep -n 't[ae]st' regular_express.txt
#oo前没有g
grep -n '[^g]oo' regular_express.txt
#oo前没有小写 [:lower:] == a-z
grep -n '[^a-z]oo' regular_express.txt 

#3. 行首行尾 ^$
#the 开头
grep -n '^the' regular_express.txt
#a-z 开头
grep -n '^[a-z]' regular_express.txt
#非字母开头的 [:alpha:] == a-zA-Z
grep -n '^[^a-zA-Z]' regular_express.txt
#以.为结尾的字符
grep -n '\.$' regular_express.txt
#空行
grep -n '^$' regular_express.txt

#4. 任意字符. 与重复字符 *
#查找g与d之间只有2个字符
grep -n 'g..d' regular_express.txt
#查找至少含有2个o以上的字符 oo和o*,第三个o可有可无
grep -n 'ooo*' regular_express.txt
#开头结尾g，中间任意
grep -n 'g.*g' regular_express.txt

#5. 字符范围限定{}
#查找含有2个O的字符串
grep -n 'o\{2\}' regular_express.txt
#查找含有2,5个O的字符串 go\{2,5\}g ,go\{2,\}g

#总结
# $、*、{} 字符放在此之前
# ^ 字符放在此之后

\"value\":\"[0-9]\.[0-9]* FIL\"
```

11，12，14

4，8

## sed

### 替换

```shell
#2-5行被替换
nl /etc/passwd |sed '2,5c No 2-5 number' 
nl /etc/passwd |sed '2,5c \     2-5 number' #对齐
#只展示5-7行
nl /etc/passwd |sed -n '5,7p'

#查看IP，替换
ifconfig | grep 'inet ' | sed 's/^ *inet//g' | sed 's/ *netmask.*$//g'
192.168.0.219
127.0.0.1

#
```

### 新增和删除

```shell
nl /etc/passwd | sed '2,5d' #delete
nl /etc/passwd |sed '2a drink tea' #append
```



# vim

```shell
#格式化
gg #第一行
shift + v #视图
shift + g #全选

#清空
:d
```

# Linux

## 文件管理

## 磁盘管理



## 网络

```shell
# 去掉防火墙
	sudo service iptables stop
    sudo chkconfig iptables off
# 编辑hosts文件，并更改主机名hostname

# 更改静态IP，更改网卡脚本 /etc/sysconfig/network或(network-scripts)
#vim /etc/udev/rules.d/70-persistent-net.rules

# 免密登录
        ssh-keygen -t rsa 
        ssh-copy-id hadoop102 
```


```SHELL
#--- BOND
DEVICE=
NAME= #同DEVICE
BOOTPROTO=static #获取IP
ONBOOT=yes
TYPE=Ethernet

#----网络
IPADDR=
GATWAY=
MASK=

BINDING_MASTER=
BINDING_OPTS=
USERCTL= 	#可选

```

```shell
#---
DEVICE=em3
NAME=em3
BOOTPROTO=none
ONBOOT=yes
TYPE=Ethernet
UUID=

MASTER=bond
SLAVE=yes
```

## 性能检测

```shell
ps -ef | grep java
ps -mp [pid] -o THREAD,tid,time
printf "%x\n" [tid]
jstack [pid] | grep [tid] -A 30#after 之后30行,-B before
```



# Centos

## 系统性能命令

```shell
#内存、磁盘、CPU、进程状态 不能trace
vmstat [-V] [-n] [delay [count]] # [-V] 为可选参数; [delay [count]]
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
4  0      0 2872152   4396 472128    0    0    25     2   60  138  0  0 99  1  0

procs
	r run
	b block
memory
	swpd 到内存交换区的内存大小，kb
	free 空闲页面
	buff 块
	cache 文件
swap
	si	磁盘页面的换入
	so
io
	bi 读磁盘
	bo 写磁盘
system
	in 每秒中断数
	cs 上下文切换
cpu
	us 用户消耗cpu时间百分比
	sy	
	id 空闲状态百分比
	wa io等待cpu
	st steal
```

```shell
#sar 非常强大
sar [option] [-o filename] [interval [count]]
sar -u	cpu
sar -P 1 3 5 指定第一个cpu
sar -d	disk
sar -r	memory
sar -n	net [DEV | EDEV | SOCK | FULL]

```

```shell
#iostat [option] [delay [count]]
iostat -c
iostat -d 

```

```shell
#free 常用的内存监控
free -h g
```

```shell
#uptime命令

```

```
netstat -i #interface
netstat -r #router
```

## 防火墙规划

1. 被信任的和不被信任的网
2. 受保护的服务
3. 封包状态

### TCP wrapper

```
/etc/hosts.allow
/etc/hosts.deny
```

### NetFilter

```
考虑顺序，只会过滤第一条规则
```



# Ubuntu

### ssh

```shell
sudo passwd
#开放端口
apt-get install iptables
sudo iptables -I INPUT -p tcp --dport 22 -j ACCEPT
iptables-save
#安装ssh协议
apt-get install openssh-server
vim etc/ssh/sshd_config
apt-get install vim #安装vim
	PermitRootLogin
service ssh restart
```

### 修改网卡脚本

```shell
vim /etc/network/interfaces

#vi /etc/network/interfaces
auto eth0
iface eth0 inet static
	address 192.168.56.101
	netmask 255.255.255.0
	gateway 192.168.56.1
	

```

# Arch Linux

```sh
#1. lsblk 

#2. gdisk /dev/sdc
#--x Expert模式
#--z 清除

#3. cgdisk /dev/sdc 分区

#4. 格式化 
#boot mkfs.fat -F32 /dev/sdc1
#swap mkswap /dev/sdc2
#	  swapon /dev/sdc2
#root mkfs.ext4 /dev/sdc3 
#home

#5. 挂载分区
#mount /dev/sdc3 /mnt
#mkdir /mnt/boot
#mount /dev/sdc1 /mnt/boot
#mkdir /mnt/home
#mount /dev/sdc4 /mnt/home

#6. 修改pacman镜像 /etc/pacman.d/mirrorlist

#7. pacstrap -i /mnt linux linux-headers linux-firmware base base-devel vim amd-ucode 

#8. genfstab -U -p /mnt >> /mnt/etc/fstab

#9. arch-chroot /mnt
#	pacman -Syy

#10. 设置时区
#ln -s /usr/share/zoneinfo/Asia/Shanghai > /etc/localtime
#hwclock --systohw     注:hardwork clock

#11. 语言配置
#vim /etc/locale.gen
#locale-gen
#echo LANG=en_US.UTF-8 > /etc/locale.conf
#export LANG=en_US.UTF-8

#12. 网络配置
#vim /etc/hostname
#vim /etc/hosts
#passwd

#13. 添加用户和权限
#useradd -m -g users -G wheel,stroge,power -s /bin/bash xxxx
#passwd xxxx
#nano visudo

#14. 安装systemd启动器
#bootctl install 
#vim /boot/loader/entries/arch.conf
#echo "options root=PARTUUID=$(blkid -s PARTUUID -o value /dev/sdc3) rw" >> /boot/loader/entries/arch.conf

#15. 开启32位应用
#systemctl enable fstrim.timer
#cat /etc/pacman.conf
#pacman -Syy

#16. 网络、声音、蓝牙、文件管理后台
#pacman -S networkmanager network-manager-applet dialog wpa_supplicant dhcpcd 
#systemctl enable NetworkManager

#17. 显卡
#pacman -S xf86-video-amdgpu vulkan-radeon lib32-vulkan-radeon
#vim /boot/loader/entries/arch.conf

#18. 安装xorg窗口系统
#pacman -S xorg-server xorg-apps xorg-xinit xorg-twm xorg-xclock xterm
#startx

#19. 安装KDE桌面
#pacman -S sddm plasma

#20. 
#exit
#umount -R /mnt 
#
```



# Dockers

## 手写dockers

### 开发环境

```
go 1.7
ubuntu
```

### 资源下载

```shell
git clone -b <branch> https://github.com/xianlubird/mydocker.git
git clone -b code-3.3 https://github.com/xianlubird/mydocker.git
```

## 2.1

UTS(UNIX Time-sharing System):主机名，域名隔离

IPC(Inter-Process Communication):进程间通信

fork()、vfork()、clone()：

fork是最简单的调用，建立一个独立于父进程的子进程

exec()：PID不变，程序的功能改变

# k8S

