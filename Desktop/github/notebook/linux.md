# 压缩命令

## zip 格式

zip 压缩文件名  源文件 #压缩文件
zip -r 压缩文件名 源目录 #压缩目录
unzip 压缩文件 #解压缩.zip文件

## gz 格式

gzip 源文件 #压缩为.gz格式的压缩文件，源文件会消失
gzip -c 源文件 > 压缩文件 #压缩为.gz格式,源文件保留
gzip -r 目录 #压缩目录下所有的子文件，但是不能压缩目录
gzip -d 压缩文件 #解压缩文件
gunzip 压缩文件 #解压缩文件

## bz2格式

bzip2 源文件 #压缩为.bz2格式。不保留源文件
bzip2 -k 源文件 #压缩之后保留源文件
bzip 不能压缩目录
bzip2 -d 压缩文件 #解压缩，-k保留压缩文件
bunzip2 压缩文件 #解压缩，-k保留压缩文件

## 打包命令

tar -cvf 打包文件名 源文件 # -c：打包; -v:显示过程；-f：指定打包的文件名
tar -xvf 打包文件名 #-x:解打包
.tar.gz 压缩格式
tar -zcvf 压缩包名.tar.gz 源文件 # -z :压缩为.tar.gz 格式
tar -zxvf 压缩包名.tar.gz # -x :解压缩.tar.gz格式
.tar.bz2压缩格式
tar -jcvf 压缩包名.tar.bz2 源文件 #-z : 压缩为.tar.bz2格式
tar -jxvf 压缩包名.tar.bz2 #-x 解压缩.tar.bz2格式

# 系统资源查看

## vmstat 命令监控系统资源

语法：vmstat [刷新延时 刷新次数]
例如：vmstat 1 3

## dmesg命令开机时内核检测信息

例如：dmesg | grep cpu

## free命令查看内存使用状态

语法：free [-b | -k | -m | -g]
-b: 以字节为单位显示
-k: 以KB为单位显示，默认就是以KB为单位显示
-m: 以MB为单位显示
-g: 以GB为单位显示

## 查看CPU信息

命令：cat /proc/cpuinfo
显示CPU相关信息，比如cpu核数是cpu cores，cpu型号是model name，缓存大小cache size等等

## uptime命令

 显示系统的启动时间和平均负载，也就是 `top` 命令的第一行。`w` 命令也可以看到这个数据。`top` 命令相对比较耗费资源，如果只需要看平均负载就可以使用该命令。 

## uname查看系统与内核相关信息

 语法：`uname [选项]`
`-a:` 查看系统所有相关信息
`-r:` 查看内核版本
`-s:` 查看内核名称 

## 判断当前系统位数

 file /bin/ls 

## 查看当前系统linux发行版本

 版本详情：`lsb_release -a`
版本信息：`cat /etc/redhat-release` 

## lsof命令 列出进程调用或打开的文件信息

语法：`lsof [选项]`
 `-c 字符串:` 只列出以字符串开头的进程打开的文件
 `-u 用户名:` 只列出某个用户的进程打开的文件
 `-p pid:` 列出某个PID进程打开的文件
 `-i :port:` 列出谁在使用某个端口

##  查看当前所有环境变量的方法和命令  

env

## 查看当前主机NTP配置情况的命令  

service ntpd status

## 查看当前主机名的命令  

hostname

##  查询当前登录系统用户情况的命令  

id:w

# 进程管理

## 进程查看命令-ps

###### ps aux

查看系统中所有进程，使用BSD操作系统格式

###### ps -le / ps -ef

查看系统中所有进程,使用Linux标准命令格式/全格式

 ![img](https://upload-images.jianshu.io/upload_images/14795543-8994b78cade6b23e.png?imageMogr2/auto-orient/strip|imageView2/2/w/620/format/webp) 

 ![img](https://upload-images.jianshu.io/upload_images/14795543-d4eafaef68397a5a.png?imageMogr2/auto-orient/strip|imageView2/2/w/620/format/webp) 

## 查看进程树-pstree

###### pstree [选项]

`-p:` 显示进程的PID(显示所有子进程)
`-u:` 显示进程的所属用户

## 杀死进程

###### kill命令

语法：`kill [信号] 进程ID(PID)`
 `kill -1 进程ID`  #重启进程(平滑重启)
 `kill -9 进程ID`  #强制杀死进程

## 进程信号

`kill -l`  #查看可用的进程信号

## killall命令

`killall [选项][信号] 进程名`  #按照进程名杀死进程
 选项:
 `-i:` 交互式，询问是否要杀死某个进程
 `-I:` 忽略进程名的大小写
 `killall -9 httpd`  #杀死所有的apache进程
 `killall -i -9 httpd`  #每杀死一个进程询问一次，按y同意

##  查看优先级 

 `ps -le` #可以显示所有进程的优先级 

## nice命令

`nice [选项] 命令`  #nice命令可以给新执行的命令直接赋予NI值，但是不能修改已经存在进程的NI值
 `-n NI值:` 给命令赋予NI值
 `nice -n -5 service httpd start`  #启动Apache服务并修改其NI值为-5

## renice命令

```english
renice [优先级] PID`  #renice命令是修改已经存在进程的NI值的命令
 例如：`renice -10 2125
```

# 用户与用户组

## 用户组命令

 使用命令后可以对比查看用户组配置文件内容：`cat /etc/group` 

 ![img](https://upload-images.jianshu.io/upload_images/14795543-a87cdb97a2f4f885.png?imageMogr2/auto-orient/strip|imageView2/2/w/620/format/webp) 

## 用户命令

 ![img](https://upload-images.jianshu.io/upload_images/14795543-36f99fc5bfad5cdb.png?imageMogr2/auto-orient/strip|imageView2/2/w/900/format/webp) 

## 修改密码

普通用户直接输入 `passwd` 命令修改自己的密码，不能加用户名修改别的用户密码

普通用户设置密码过于简单会不通过，必须符合密码原则，root用户则可以任意设置

 ![img](https://upload-images.jianshu.io/upload_images/14795543-ff8175c7ee5e2623.png?imageMogr2/auto-orient/strip|imageView2/2/w/620/format/webp) 

## 登录说明

`useradd -s /bin/false` 是最严格的禁止登录，一切服务都不能用
 `useradd -s /sbin/nologin` 只是不允许进行系统登录，可以使用其他ftp等服务
 `touch /etc/nologin`  执行该命令会禁止除了root用户以外的其他用户登录服务器
 只要是创建该文件即可，应用场景比如服务器维护时需要暂时禁止普通用户登录。

## 锁定用户

passwd -l 用户名

## 解锁用户

passwd -u 用户名

## 清除用户密码

passwd -d 用户名

##  主要组和附属组 

 ![img](https://upload-images.jianshu.io/upload_images/14795543-bb483e89d940ae93.png?imageMogr2/auto-orient/strip|imageView2/2/w/620/format/webp) 

## 切换用户身份

 `su [用户名]` #切换用户身份
如果不带参数，则为切换到root身份，除了root用户，其他用户切换身份操作都需要输入密码。
使用完成后切换回来执行 `exit` 命令即可 

```
1、su - USERNAME 切换用户后，同时切换到新用户的工作环境中。
2、su USERNAME 切换用户后，不改变原用户的工作目录，及其他环境变量目录。
```

## 显示当前用户名

whoami

## 显示用户当前组

groups

# 文件搜索命令

## locate

 `locate`命令优点是搜索速度更快，耗费系统资源更少，缺点是只能按文件名搜索
`locate`命令所搜索的后台数据库：`/var/lib/mlocate` 

 ![img](https://upload-images.jianshu.io/upload_images/14795543-0a034b895a18cc8e.png?imageMogr2/auto-orient/strip|imageView2/2/w/620/format/webp) 

#### /etc/updatedb.conf配置文件

## whereis

搜索命令所在路径及帮助文档所在位置，只能搜索命令
 注意：某些命令比如 `cd` 命令是找不到可执行文件的，因为是shell自带的命令
 语法：`whereis [选项] 命令名`
 选项：`-b:` 只查找可执行文件;   `-m:` 只查找帮助文件

## which

 搜索命令所在路径及别名定义(有则显示)
语法：`which 命令名` 

## find

用于在指定目录下搜索符合条件的文件，如果需要模糊查询，使用通配符匹配，通配符是完全匹配。
 `find`命令非常强大，也相对比较耗费系统资源，所以应该尽量避免大范围的搜索，比如直接搜索根目录：`find / ...`
 语法：`find 路径 [选项] [搜索内容]`

 ![img](https://upload-images.jianshu.io/upload_images/14795543-bb89b62e323f3c08.png?imageMogr2/auto-orient/strip|imageView2/2/w/620/format/webp) 

 ![img](https://upload-images.jianshu.io/upload_images/14795543-aa21e3ac793a9a8d.png?imageMogr2/auto-orient/strip|imageView2/2/w/620/format/webp) 

## grep

在指定文件当中匹配符合条件的字符串
语法：`grep [选项] 字符串 文件名`
选项：`-i:` 忽略大小写； `-v:` 排除指定字符串； `-n:` 显示匹配的行号



# Linux登录与关机命令,登录的用户信息,登录时间

## 关机命令

`shutdown -h now`

##  重启命令

`shutdown -r now`、`reboot`

##  退出登录

`logout`

##  查看当前登录用户信息

`w`、`who`

##  查询当前登录和过去登录的用户信息

`last` -n

##  查看所有用户的最后一次登录时间

`lastlog`

# 文件目录常用命令

## ls

 用于显示指定工作目录下之内容(不加参数则列出当前目录下的内容)
语法：`ls [选项] [文件或目录]` 

 ![img](https://upload-images.jianshu.io/upload_images/14795543-edf2094ac2eb6ada.png?imageMogr2/auto-orient/strip|imageView2/2/w/620/format/webp) 

## mkdir 创建目录

 命令英文原意 make directories，创建目录，`-p` 递归创建，用于创建多级目录
语法：`mkdir -p [目录名]` 

## 显示工作目录所在位置

pwd,命令英文原意 print working directory，显示工作目录所在位置(显示绝对路径)

## rmdir,删除空目录

命令英文原意 remove empty directories，删除空目录(很少用)
语法：`rmdir [目录]`

## rm 删除文件

 命令英文原意 remove，删除文件或目录
语法：`rm -rf [文件或目录]`
选项：-r 删除目录；-f 强制执行 

## cp 复制

 命令英文原意 copy，复制文件或目录
语法：`cp [选项] [原文件或目录] [目标目录]` 

 ![img](https://upload-images.jianshu.io/upload_images/14795543-7f477914132414da.png?imageMogr2/auto-orient/strip|imageView2/2/w/620/format/webp) 

## mv，移动

命令英文原意 move，剪切或改名，如果原文件和目标目录在同一目录下，则为改名，否则为剪切
语法：`mv [原文件或目录] [目标目录]`

## cat 打印

 连接文件并打印到标准输出设备上，主要用来显示文件的内容
语法：`cat 文件1 文件2...` 

## touch 创建文件

 创建新的文件，如果文件已存在，则会更新文件的最后修改时间为当前系统时间，不会更改内容
语法：`touch 文件名` `touch 目录/文件名` 

## ln，建立链接

 命令英文原意 link，为某一个文件(目录)在另外一个位置建立一个同步的链接文件，`-s` 代表创建软链接
语法：`ln -s [原文件] [目标文件]` 

# 挂载命令

## 查询与自动挂载

`mount` #查询系统中已经挂载的设备
`mount -a` #依据配置文件/etc/fstab的内容，进行自动挂载

#### /etc/fstab 自动挂载配置文件

- 该文件修改需要谨慎，一旦有错则系统不能正常启动。
- 光盘、u盘最好不要写入`/etc/fstab`进行自动挂载，否则系统一旦检测到没有光盘或者u盘，系统便不能正常启动。
- 文件部分内容如图，可以参照文件内容的指定格式添加一条新的分区或设备自动挂载配置，然后执行 `mount -a` 命令进行自动挂载

## 卸载命令

使用完光盘等存储设备后必须卸载，执行卸载命令时必须保证不在设备挂载的目录下，否则会提示设备正在使用。
 `umount 设备文件名或挂载点`  #卸载命令
 `umount /mnt/cdrom`   #卸载光盘

# 工作管理

## 把进程放入后台

通过 `&` 符号 或者 `ctrl + z`，以tar命令为例：
 `tar -zcf redis1.tar.gz &`  #把命令放入后台，并在后台执行
 `tar -zcf redis2.tar.gz`   #按下ctr+z快捷键,放在后台暂停

## 查看后台的工作

`jobs [-l]`  # -l代表显示工作的PID
 `"+"` 号代表最后一个放入后台的工作，也是工作恢复时，默认恢复的工作
 `"-"` 号代表倒数第二个放入后台的工作，第三个工作就不会有符号显示
 `[1]` 这里的1代表工作号

## 将后台暂停的工作恢复到前台执行

`fg %工作号`
注: %号可以省略，但是注意工作号和PID的区别，不带参数默认恢复"+"号的工作

## 将后台暂停的工作恢复到后台执行

 `bg %工作号`
后台恢复执行的命令，是不能和前台有交互的，否则不能恢复到后台执行，不带参数默认恢复"+"号的工作 

## 脱离终端常用方法

1. 把需要后台执行的命令加入 `/etc/rc.local` 文件(推荐)
2. 使用系统定时任务，让系统在指定的时间执行某个后台命令
3. 标准方法是使用nohup命令(推荐)：`nohup [命令] &`

# 权限管理

##  修改权限的方式 

`chmod [选项] 模式 文件名`  #用来变更文件或目录的权限

- 选项：-R表示递归，模式有两种表示方式：
- [ugoa] [+-=] [rwx] 权限字母表示法
- [mode=421] 权限数字表示法(推荐)
- 权限数字：r - 4，w - 2，x - 1，无权限 - 0
- 常用组合: 777最高权限，644普通文件权限，755执行权限

## 修改文件的所有者

`chown 用户名 文件名` #修改文件所有者
`chown 所有者:所属组 文件名` #同时改变所有者和所属组
选项-R：递归，处理指定目录以及其子目录下的所有文件

## 修改文件的所属组

格式：`chgrp 组名 文件名`
例如：`chgrp user1 abc`

## 文件默认权限

文件的默认权限即当我们新建文件时所拥有的初始权限。对于windows而言其默认权限是从上级目录继承而来的，而linux则是通过umask权限设定的。

对于root用户而言，文件的默认权限是644，目录的默认权限是755

对于普通用户而言，文件的默认权限是664，目录的默认权限是775

##  查看与修改 默认权限

 `umask` #查看默认权限 

 `umask val` #临时修改默认权限
`vi /etc/profile` #永久修改 

## ACL权限

## 查看分区ACL权限是否开启

`dumpe2fs -h /dev/sda5`  #查看根分区超级块信息
 dumpe2fs命令是查询指定分区详细文件系统信息的命令
 -h选项： 仅显示超级块中信息，而不显示磁盘块组的详细信息

##  开启分区ACL权限 

1. 临时开启分区ACL权限
    `mount -o remount,acl /`  #重新挂载根分区，并挂载加入acl权限
2. 永久开启分区ACL权限



```bash
vi /etc/fstab
UUID=c2ca6f57-b15c-43ea-bca0-f239083d8bd2 / ext4 defaults,acl 11  #加入acl
#保存文件后，重新挂载文件系统或重启动系统，使修改生效
mount -o remount /  
```

# 定时任务

## crontab循环定时任务

```csharp
chkconfig --list | grep crond  #确认是否安装该服务
service crond status  #确认服务是否开启
chkconfig crond on  #设置自启动
yum -y install vixie-cron crontabs  #安装cron服务
```

## crontab命令

语法：`crontab [选项]`
`-e:` 编辑crontab定时任务
`-l:` 查询crontab任务
`-r:` 删除当前用户所有的crontab任务

##  crontab格式说明 

 ![img](https://upload-images.jianshu.io/upload_images/14795543-6088131dee23e32f.png?imageMogr2/auto-orient/strip|imageView2/2/w/620/format/webp) 

 ![img](https://upload-images.jianshu.io/upload_images/14795543-ee45ae0eb8aee6a4.png?imageMogr2/auto-orient/strip|imageView2/2/w/620/format/webp) 

 ![img](https://upload-images.jianshu.io/upload_images/14795543-fd35bcb3fe112519.png?imageMogr2/auto-orient/strip|imageView2/2/w/620/format/webp) 

# 网络管理

## wirte 给用户发信息

语法：write <用户名>

以Ctrl+D 保存结束

## ping 测试网路连通性

语法： ping 选项 IP地址 -c 指定发送次数

## ifconfig 查看和设置网卡信息

语法：ifconfig 网卡名称 IP地址

## mail 查看发送电子邮件

mail [用户名]

## traceroute 路由跟踪，显示数据包到主机间的路径

范例： traceroute www.baidu.com

## netstat 显示网络相关信息

netstat [选项]

选项：

-t : TCP协议

-u : UDP协议

-l : 监听

-r :路由

-n : 显示IP地址和端口号

示例：netstat -tlun

## setup 配置网络

# 文件系统常用命令

## 文件系统查看命令 df

df [选项] [挂载点]

-   -a，--all：全部文件系统列表
      -B, --block-size=SIZE：指定分区块大小
      -h：人类可阅读的方式显示
      -i：以inode模式来显示磁盘使用情况
      -k：区块为1024字节
      -m：区块为1048576字节
      -l：只显示本地文件系统
      --no-sync：忽略sync命令
      -P：输出格式为POSIX
      --sync：在取得磁盘信息前，先执行sync命令
      -T：文件系统类型
      -t<文件系统类型>只显示指定类型文件系统的磁盘信息
      -x<文件系统类型>不示指定类型文件系统的磁盘信息
  df -hl 查看磁盘剩余空间

## 统计目录或文件大小du

du [选项] [目录或文件名]

-a或-all 显示目录中个别文件的大小。 

 -h或--human-readable 以K，M，G为单位，提高信息的可读性。 

 -s或--summarize 仅显示总计 ，统计分区

## 显示磁盘状态命令dumpe2fs

dumpe2fs 分区设备文件名

# 示例试卷

**Linux****常用命令知识考试**

**考试要求：**

1， 本次考试要求半开卷考试，可以查阅集成文档、纸质文档、电脑上文档和登录相关linux主机查询验证。不允许使用internet查询，包括百度，bing和google等。

2， 时间为60分钟。

 

一，某linux系统（以redhat7&CentOS7为例），基本信息如下：

   

问题：

1， 请给出查看当前系统发行版本所有信息命令。(2分)

 cat /proc/version --- cat /etc/redhat-release

2， 请给出查看root用户目录（默认/root）下所有文件详细信息（涵隐藏文件），要求文件按照时间顺序倒叙排列（从新到旧）。并给出/root目录统计文件总数量大小命令。(3分)

ls -a -lh -t /root

3， 请给出在/var目录下查找名字为：”messages”文件的命令。(2分)

find /var -name messages

4， 请给出最近50个用户登录的记录命令。(2分)

last -n 50

5， 请给出22监听端口下所有进程的启动情况命令。(3分)

 lsof -i:22

6， 请给出查看内存使用情况的命令。(2分)

free -h

7， 请给出立即关机并重启主机的命令。(3分)

reboot shutdown lint6 ---shutdown -r now或者reboot

8， 请给出查看/var目录下每个文件/文件夹的大小的命令。(4分)

ls -lh /var

9， 请给出查看当前所有环境变量的方法和命令。(2分)

env

10，      请给出查看linux内核所有信息的命令。(2分)

uname -a

11，      请给出查看当前系统时间和日期的命令，并给出查看当前主机NTP配置情况的命令。(4分)

date

chronyc sources -v(在已经配好NTP条件下有效) -- service ntpd status

12，      请给出查看当前网卡和网卡配置ip地址的情况。(3分)

Ifconfig  ifconfig -a或者ip addr

13，      请给出查看当前默认网关和其它路由表的信息的命令。(3分)

route -n 或 route -nr

14，      请给出查看/etc/ntp.conf文件里面的信息，要求过滤掉所有以‘#’开头的注释行。(3分)

cat /etc/ntp.conf |grep -v "#"

15，      请给出查询”cat”命令全路径的命令。(2分)

which cat

16，      请给出查询当前登录系统用户情况的命令。(2分)

w  ---id

17，      请给出查看当前主机名的命令。(2分)

hostname

18，      请给出查看当前主机cpu情况的方法和命令。(3分)

top

19，      请给出查询从当前主机到达远端主机8.8.8.8的路由跟踪情况的命令。(3分)

traceroute 8.8.8.8

20，      请给出查询当前root用户自动执行任务的配置情况的命令。(3分)

grep /var/log/cron root 并不确定   ---crontab -l

Crontab -u root

21，      请给出”su oracle” 和”su - oracle”的区别。(4分)

```
1、su - USERNAME 切换用户后，同时切换到新用户的工作环境中。
2、su USERNAME 切换用户后，不改变原用户的工作目录，及其他环境变量目录。
```

 

22，      请给出一个用户下面，”.profile”和”.bash_profile”的作用，如果都存在时，以哪个生效。(5分)

作用：定义用户的环境变量

Bash_profile对单一用户有效，profile对所有用户有效，bash_profile 生效

23，      请给出查询当前主机进程带”sshd”关键字的进程信息，包括进程用户，进程号，进程启动时间，进程CMD等信息。(3分)

ps aux | grep -n sshd

24，      如果想修改当前命令行显示以如下为准，应该如何修改，请给出修改方法：

“oracle@drocsdb1:[/oracle]%” (4分)

25，      请给出创建一个组名test命令，要求组id为3033.

groupadd -g 3033 test

  请给出创建一个用户名test命令，要求所属组为test，默认目录为/home/test.

useradd -g 3033 -d /home/tests tests

  给出修改用户test密码的命令，要求密码为test. (6分)

Passwd test <ENTER> 新密码 <ENTER>  新密码

26，      请给出将dba组添加给已存在用户test命令，并移除用户test当前所属的test组。(4分)

usermod -  G bda test   usermod -g dba test

27，      请给出删除组test的命令，并给出删除用户test的命令。(4分)

groupdel test 

uesrdel test 

28，      请给出查看当前主机分区表情况. (3分)

lsblk

29，      请给出将test1，test2，test3三个文件打包成test.tar的文件的命令，并给出查看test.tar打包信息的命令，并给出解压test.tar文件的命令。(6分)

tar -cv test1,test2,test3 -f ./test.tar  ----tar -cv test1 test2 test3 -f ./test.tar

Tar -tv ./test.tar   ---tar -tvf test.tar

Tar -xv ./test.tar  ---tar -xvf ./test.tar

 

30，      请给出将“sleep 1500“命令放在后台执行的命令，并给出查询该后台命令Pid，然后kill掉该命令的方法和命令。(8分)

sleep 1500 > sleep.file 2>&1 &

ps -l              -- ps -ef|grep sleep

kill -9 11710  

 

 

答案，请填写到如下表格：

 

| 题目序号 | 题目答案 | 评分 | 备注 |
| -------- | -------- | ---- | ---- |
| 1        |          |      |      |
| 2        |          |      |      |
| 3        |          |      |      |
| 4        |          |      |      |
| 5        |          |      |      |
| 6        |          |      |      |
| 7        |          |      |      |
| 8        |          |      |      |
| 9        |          |      |      |
| 10       |          |      |      |
| 11       |          |      |      |
| 12       |          |      |      |
| 13       |          |      |      |
| 14       |          |      |      |
| 15       |          |      |      |
| 16       |          |      |      |
| 17       |          |      |      |
| 18       |          |      |      |
| 19       |          |      |      |
| 20       |          |      |      |
| 21       |          |      |      |
| 22       |          |      |      |
| 23       |          |      |      |
| 24       |          |      |      |
| 25       |          |      |      |
| 26       |          |      |      |
| 27       |          |      |      |
| 28       |          |      |      |
| 29       |          |      |      |
| 30       |          |      |      |
| 总分合计 |          |      |      |

 

 

 