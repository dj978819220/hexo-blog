---
title: linux基本使用
date: 2018-04-01 20:59:16
tags:
    - linux
---

## 命令的用法

### linux支持

locale -a | grep zh_CN

### source .

```shell
source .bash_profile
. .bash_profile
```

### 参数读入

```shell
echo "please input a number: "
read line #输入
echo $line  #读取
```

### 比较

```shell
who am i | grep root
if [ $? -ne 0 ]
then
echo 'hello'
fi
```

`-eq     等于,如:if ["$a" -eq "$b" ]`
`-ne     不等于,如:if ["$a" -ne "$b" ]`
`-gt     大于,如:if ["$a" -gt "$b" ]`
`-ge    大于等于,如:if ["$a" -ge "$b" ]`
`-lt      小于,如:if ["$a" -lt "$b" ]`
`-le      小于等于,如:if ["$a" -le "$b" ]`
`<  小于(需要双括号),如:(("$a" < "$b"))`
`<=  小于等于(需要双括号),如:(("$a" <= "$b"))`
`>  大于(需要双括号),如:(("$a" > "$b"))`
`>=  大于等于(需要双括号),如:(("$a" >= "$b"))`

### chkconfig

在CentOS或者RedHat其他系统下，如果是后面安装的服务，如httpd、mysqld、postfix等，安装后系统默认不会自动启动的。就算手动执行/etc/init.d/mysqld start启动了服务，只要服务器重启后，系统仍然不会自动启动服务。
在这个时候，我们就需要在安装后做个设置，让系统自动启动这些服务，避免不必要的损失和麻烦。
其实命令很简单的，使用chkconfig即可。比如要将mysqld设置为开机自动启动：
* chkconfig mysqld on

同理，要取消掉某个服务自动启动，只需要将最后的参数“on”变更为“Off”即可。比如要取消postfix的自动启动：
* chkconfig postfix off

值得注意的是，如果这个服务尚未被添加到chkconfig列表中，则现需要使用–add参数将其添加进去：
* chkconfig –add postfix

从系统启动项列表删除一个服务，使用–del选项从启动列表删除它：
* chkconfig --del ip6tables

如果要查询当前所有自动启动的服务，可以输入：
* chkconfig –list

但是这样显示东西太多了，看起来很晕。如果只想看指定的服务怎么办呢？这个时候只需要在“–list”之后加上服务名就好了，比如查看httpd服务是否为自动启动，就输入：
* chkconfig –list httpd

这个时候输出的结果：
httpd           0:off   1:off   2:off   3:off   4:off   5:off   6:off

此时0~6均为off，则说明httpd服务不会在系统启动的时候自动启动。我们输入chkconfig httpd on后，再次检查输出结果变为：
httpd           0:off   1:off   2:on    3:on    4:on    5:on    6:off

这个时候2~5都是on，就表明会自动启动了。

### 查找

1. find 从文件实时查找
2. locate 查找一个数据库（/var/lib/locatedb）信息非实时更新
3. which 查找命令是否存在 #which ls
4. whereis 命令只能用于搜索程序名，而且只搜索二进制文件（参数-b）、man说明文件（参数-m）和源代码文件（参数-s）。如果省略参数，则返回所有信息。
5. type 命令用来区分某个命令到底是由shell自带的，还是由shell外部的独立二进制文件提供的。如果一个命令是外部命令，那么使用-p参数，会显示该命令的路径，相当于which命令。

###  vim 安装

`yum -y install vim*`

### crontab

显示当前用户的定时任务： `crontab -l`
编辑当前用户的定时任务： `crontab -e`

### 几个基本符号及其含义

/dev/null 表示空设备文件
0 表示stdin标准输入
1 表示stdout标准输出
2 表示stderr标准错误

### > 与 >>

`>` 覆盖源文件 `ps -ef | grep tcp > test.txt`
`>>` 在结尾追加 `ps -ef | grep tcp >> test.txt`
`<` 将默认从键盘输入改成从文件输入 `grep pid < pid.txt`

`59 23 * * * /bin/sh /home/gbase/sql/backup.sh >>/home/informix/gbase/sql/backup.log 2>&1`
分析: &1是将 1（标准输出）做为引用输入进去

### 管理用户及用户组

* 用户组目录 `cat /etc/group`

![img/linux-group.png](img/linux-group.png "Optional title")

* 用户信息 `cat /etc/passwd`

![img/linux-group.png](img/linux-user.png "Optional title")

* 管理用户

```shell
useradd 注：添加用户
adduser 注：添加用户
passwd 注：为用户设置密码
usermod 注：修改用户命令，可以通过usermod 来修改登录名、用户的家目录等等；
usermod -a -G groupA user //将user用户加到groupA用户组
pwcov 注：同步用户从/etc/passwd 到/etc/shadow
pwck 注：pwck是校验用户配置文件/etc/passwd 和/etc/shadow 文件内容是否合法或完整；
pwunconv 注：是pwcov 的立逆向操作，是从/etc/shadow和 /etc/passwd 创建/etc/passwd ，然后会删除 /etc/shadow 文件；
finger 注：查看用户信息工具 id 注：查看用户的UID、GID及所归属的用户组 chfn 注：更改用户信息工具
su 注：用户切换工具 sudo 注：sudo 是通过另一个用户来执行命令（execute a command as another user），su 是用来切换用户，然后通过切换到的用户来完成相应的任务，
但sudo 能后面直接执行命令，比如sudo 不需要root 密码就可以执行root 赋与的执行只有root才能执行相应的命令；但得通过visudo 来编辑/etc/sudoers来实现；
visudo 注：visodo 是编辑 /etc/sudoers 的命令；也可以不用这个命令，直接用vi 来编辑 /etc/sudoers 的效果是一样的；
sudoedit 注：和sudo 功能差不多；
```

* 管理用户组

```shell
groupadd 注：添加用户组；
groupdel 注：删除用户组；
groupmod 注：修改用户组信息
groups 注：显示用户所属的用户组
grpck grpconv 注：通过/etc/group和/etc/gshadow 的文件内容来同步或创建/etc/gshadow ，如果/etc/gshadow 不存在则创建；
grpunconv 注：通过/etc/group 和/etc/gshadow 文件内容来同步或创建/etc/group ，然后删除gshadow文件；
```

### 后台运行

 1. command & ： 后台运行，你关掉终端会停止运行
 2. nohup command & ： 后台运行，你关掉终端也会继续运行

### fsck

> fsck命令被用于检查并且试图修复文件系统中的错误。当文件系统发生错误四化，可用fsck指令尝试加以修复。

-a：自动修复文件系统，不询问任何问题；
-A：依照/etc/fstab配置文件的内容，检查文件内所列的全部文件系统；
-N：不执行指令，仅列出实际执行会进行的动作；
-P：当搭配"-A"参数使用时，则会同时检查所有的文件系统；
-r：采用互动模式，在执行修复时询问问题，让用户得以确认并决定处理方式；
-R：当搭配"-A"参数使用时，则会略过/目录的文件系统不予检查；
-s：依序执行检查作业，而非同时执行；
-t<文件系统类型>：指定要检查的文件系统类型；
-T：执行fsck指令时，不显示标题信息； -V：显示指令执行过程。

1. fsck
2. 在随后的多个确认对话框中输入:y
3. 结束后同样使用reboot命令重启系统这样就好了！


### 查看端口

`lsof -i:7007`

### scp 远程拷贝

1. 对拷文件夹 (包括文件夹本身)
`scp -r   /home/wwwroot/www/charts/util root@192.168.1.65:/home/wwwroot/limesurvey_back/scp`

2. 对拷文件夹下所有文件 (不包括文件夹本身)
`scp   /home/wwwroot/www/charts/util/* root@192.168.1.65:/home/wwwroot/limesurvey_back/scp`

3. 对拷文件并重命名
`scp   /home/wwwroot/www/charts/util/a.txt root@192.168.1.65:/home/wwwroot/limesurvey_back/scp/b.text`

### 查看内存

    df -lh 查看硬盘使用率
    free -g 查看内存使用率

### 分配权限

* `chmod 777`

### vim编辑器的使用

* vim快捷键示意图
* ![img/vim-hotkey.jpg](img/vim-hotkey.jpg "Optional title")
* `:s/old/new/` 改变当前行第一个的old变为new
* `:%s/old/new/` 改变所有行的第一个old为new
* `:%s/old/new/g` 改变所有行的所有的old为new
* `gg` 跳到最开始
* `G` 跳到最后

### 查看硬盘使用量

ll -lh

### less

* `gg` 跳到开头
* `G` 跳到结尾
* `/` 查找
* `f` 向下翻页
* `b` 往回翻页

### 启动服务

* 进入服务 `/etc/rc.d/init.d`
* 停止服务 `./DmServiceDMSERVER stop `
* 启动服务: `./DmServiceDMSERVER start`
* 查看服务： `chkconfig --list`

### 运行脚本

`sudo chmod +x test.bin sudo ./test.bin`

### 目录递归拷贝

`cp '/home/dj/Desktop/jdk1.6.0_43' ./ -r`

### 文件删除

* rm -rf file  删除文件
    r //递归
    f //强制


### 查看cpu占用

* `top` 查看cup

### 如何在linux启动时执行命令

 在 `/etc/rc.local` 中添加代码即可

### 查看进程

* ps 命令用于查看当前正在运行的进程。
grep 是搜索
例如： ps -ef | grep java
表示查看所有进程里 CMD 是 java 的进程信息
* ps -aux | grep java
-aux 显示所有状态
ps
* kill 命令用于终止进程
例如： kill -9 [PID]
-9 表示强迫进程立即停止
通常用 ps 查看进程 PID ，用 kill 命令终止进程

 ### yum命令
*  yum -y list grep  yum显示安装包
*  yum list installed java* 显示已安装的java文件
*  安装无线网络
    1. yum install epel-release 安装下载源
    2. yum install wireless-tools 下载无线工具

### 查看系统端口

    netstat -anp
    netstat -anlp|grep 8080

### 查看系统信息

    uname -a
    cat /etc/issue

### 查看文件

    find / -name grep 在所有文件中查询 find / -name "name"
    whereis name


### 赋予执行权限

chmod +x filename.bin

### 压缩与解压

*tar*
tar czvf filename.tar dir gzip压缩
tar zxvf filname.tar gzip解压
`tar zxvf filename.tar.gz -C dir` 解压到指定目录
*zip*
unzip 压缩命令



##命令的运用

### 启动网站卡

ifup ifcfg-eth0
service network restart 重启网络


### 查看项目安装情况

ls -ltr /usr/bin/java

### 时间区域设置

* `tzselect` 设置时区
* `date -s "2017-10-23 13:19:09"` 时间设置
* 设置时区
*  `rm -rf /etc/localtime` 删除默认时区
*  `ln -s /usr/share/zoneinfo/UTC /etc/localtime` 将时区设置为UTC格式
*  hwclock -r (查看时间) -w(写入时区进入bios)
*  UTC标准时区（比上海时区少8个小时） CST上海时区
*  date -R `显示时区的格式`

### linux安装

1. 安装linux系统

a. vmlinuz initrd=initrd.img linux dd quiet
b. vmlinuz initrd=initrd.img inst.stage2=hd:/dev/sdb4


注意： 如果遇到uefi 启动必须改成legacy

/var 5G
/home 10G
/usr 4G
/ 39G
2. 修改40_custom 重建引导

$ sudo vi /etc/grub.d/40_custom

#!/bin/sh
exec tail -n +3 $0
# This file provides an easy way to add custom menu entries.  Simply type the
# menu entries you want to add after this comment.  Be careful not to change
# the 'exec tail' line above.
menuentry 'Windows7'{
set root=(hd0,1)
chainloader +1
}

$ grub2-mkconfig -o /boot/grub2/grub.cfg
$ reboot

### linux设置开机启动顺序

```shell
1. 安装linux系统

a. vmlinuz initrd=initrd.img linux dd quiet
b. vmlinuz initrd=initrd.img inst.stage2=hd:/dev/sdb4


注意： 如果遇到uefi 启动必须改成legacy

/var 5G
/home 10G
/usr 4G
/ 39G
2. 修改40_custom 重建引导

$ sudo vi /etc/grub.d/40_custom

#!/bin/sh
exec tail -n +3 $0
# This file provides an easy way to add custom menu entries.  Simply type the
# menu entries you want to add after this comment.  Be careful not to change
# the 'exec tail' line above.
menuentry 'Windows7'{
set root=(hd0,1)
chainloader +1
}

$ grub2-mkconfig -o /boot/grub2/grub.cfg
$ reboot
```

### 防火墙查看

`iptables -L -n` | `service iptables status` 查看防火墙

### 设置临时IP

`ifconfig eth0 192.168.39.200 netmask 255.255.255.0`


### iptables的运用

 端口禁用

关闭防火墙 -## 打开22端口 -## 关闭forward端口 关闭input端口 -## 保存防火墙规则 -## 启动防火墙
打开需要打开的端口 : 3525(dmserver) 3307(mysql) 19999(realTimeserver) 8081 8443 8015 8019(tomcat) 7007(weblogic)

* 禁用所有端口

```shell
iptables -P INPUT DROP
iptables -P OUTPUT DROP
iptables -P FORWARD DROP
```

*打开所有端口*
```shell
iptables -P INPUT ACCEPT
iptables -P OUTPUT ACCEPT
iptables -P FORWARD ACCEPT
```

* 打开端口

```shell
/sbin/iptables -I INPUT -p tcp --dport 3525 -j ACCEPT
/etc/rc.d/init.d/iptables save
/etc/init.d/iptables restart
/sbin/iptables -L -n
```

* 关闭端口

```shell
/sbin/iptables -I OUTPUT -p tcp --dport 22 -j DROP
```

* 清除所有规则

```shell
iptables -F (flush 清除所有的已定规则)

iptables -X (delete 删除所有用户“自定义”的链（tables）)

iptables -Z （zero 将所有的chain的计数与流量统计都归零）

/etc/rc.d/init.d/iptables save
ervice iptables restart
```

*开放连续端口*

```shell
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 30000 -j ACCEPT

这样就搞定了，一句就可以了，下面再多讲几句iptables防火墙一些规则。

一、 700:800  表示700到800之间的所有端口

二、 :800   表示800及以下所有端口

三、 700:   表示700以及以上所有端口
```


### telnet退出

后来找到了正确的命令 `ctrl+]`  然后在telnet 命令行输入 `quit`  就可以退出了
顶

### linux盘符更改 - 目录永久更改挂载

`vim /etc/fstab` lv 挂载更改 修改参数

### linux报 “Device eth0 does not seem to be present”解决办法


```shell
用ifconfig查看发现缺少eth0，只有lo；用ifconfig -a查看发现多出了eth1的信息。

解决办法1：
# mv /etc/sysconfig/network-scripts/ifcfg-eth0  /etcsysconfig/network-scripts/ifcfg-eth1
将eth0的mac地址改为eth1的mac地址，同时改变其DEVICE名称为eth1，再重启网络即可。

解决办法2：
# rm -rf /etc/udev/rules.d/70-persistent-net.rules
# reboot
```

### 硬盘扩容 步骤

```shell
vgs lvs
linux -> 扩展 -> linux lvm -> pv -> vg -> lv ->  lvresize -> resize2fs

    在磁盘sdb上创建新分区
    命令：fdisk /dev/sdb
    输入 p 打印现有分区情况（还没有分区）
    输入 n 新建分区
    输入 p 为建立主分区（此时的p是在n后的，不是打印）
    输入 2 为建立第二个主分区
    分区起始位置可以直接回车，默认是651(如何是1则先建一个分区)
    分区最后位置可以直接回车，默认为 1305
    输入 p 打印分区情况，发现已建立一个分区 /dev/sdb2，但是 此分区为 Linux 格式
    由于分区 /dev/sdb2 为 Linux 格式，我们需要改变系统标识符为Linux LVM格式：
    输入 t 改变分区的属性
    输入 2 表示改变第二个分区的属性
    输入 8e 改变分区1为 Linux LVM格式
    输入 p 打印分区情况，发现建立的分区 /dev/sdb1 为 Linux LVM 格式
    输入 w 保存分区
    使kernel重新读取分区表
    命令：partprobe
    但是出现了一些关于sdb的警告，重启系统
    命令：reboot
    再次使用 fdisk -l 查看系统内磁盘情况发现 /dev/sdb上已有一个 Linux LVM 格式的 /dev/sdb2分区
    创建PV:
    创建PV：pvcreate /dev/sdb2
    查看系统PV：pvscan
    这样我们就创建了一个 5.02G的PV
    增加 VG容量:
    增加VG：vgextend vg_test /dev/sdb2
    查看VG：vgdisplay
    这样我们就将vg_test增加了 5.02G（1284 个Free PE，要记住这个数字）
    增加LV容量:
    增加LV：lvresize -l +1284  /dev/vg_test/lv_test（1284是VG中Free PE的个数）
    查看LV：lvdisplay
    这样我们就将 lv_test 的容量增加至9.99G
    增加文件系统的容量：
    命令：resize2fs /dev/vg_test/lv_test
    文件系统lv_test已经由 4.9G 增加至 9.9G
    至此，大功告成！

```


### linux sftp服务器搭建

由于采用明文传输用户名和密码，FTP协议是不安全的。在同一机房中只要有一台服务器被攻击者控制，它就可能获取到其它服务器上的FTP密码，从而控制其它的服务器。

当然，很多优秀的FTP服务器都已经支持加密。但如果服务器上已经开了SSH服务，我们完全可以使用SFTP来传输数据，何必要多开一个进程和端口呢？

下面，我就从账户设置、SSH设置、权限设置这三个方面来讲讲如何使用SFTP完全替代FTP。

范例

本文要实现以下功能：

SFTP要管理3个目录：

homepage
blog
pay
权限配置如下：

账户www，可以管理所有的3个目录；
账户blog，只能管理blog目录；
账户pay，只能管理pay目录。
web服务器需求：

账户blog管理的目录是一个博客网站，使用apache服务器。apache服务器的启动账户是apache账户，组为apache组。
账户blog属于apache组，它上传的文件能够被apache服务器删除。同样的，它也能删除在博客中上传的文件（就是属于apache账户的文件）。
账户设置

SFTP的账户直接使用Linux操作系统账户，我们可以用useradd命令来创建账户。

首先建立3个要管理的目录：

mkdir /home/sftp/homepage
mkdir /home/sftp/blog
mkdir /home/sftp/pay
创建sftp组和www、blog、pay账号，这3个账号都属于sftp组：

groupadd sftp
useradd -M -d /home/sftp -G sftp www
useradd -M -d /home/sftp/blog -G sftp blog
useradd -M -d /home/sftp/pay -G sftp pay

# 将blog账户也加到apache组
useradd -M -d /home/sftp/blog -G apache blog

#设置3个账户的密码密码
passwd www
passwd blog
passwd pay
至此账户设置完毕。

SSH设置

首先要升级OpenSSH的版本。只有4.8p1及以上版本才支持Chroot。

CentOS 5.4的源中的最新版本是4.3，因此需要升级OpenSSH。

指定新的源：

vim /etc/yum.repos.d/test.repo
#输入如下内容
[centalt] name=CentALT Packages for Enterprise Linux 5 – $basearch
baseurl=http://centos.alt.ru/repository/centos/5/$basearch/
enabled=0
gpgcheck=0
# wq保存
执行升级：

yum –enablerepo=centalt update -y openssh* openssl*
# 重启服务
service sshd restart
# 重看版本
ssh -V
# OpenSSH_5.8p1, OpenSSL 0.9.8e-fips-rhel5 01 Jul 2008
升级成功后，设置sshd_config。通过Chroot限制用户的根目录。

vim /etc/ssh/sshd_config
#注释原来的Subsystem设置
Subsystem sftp /usr/libexec/openssh/sftp-server
#启用internal-sftp
Subsystem sftp internal-sftp
#限制www用户的根目录
Match User www
ChrootDirectory /home/sftp
ForceCommand internal-sftp
#限制blog和pay用户的根目录
Match Group sftp
ChrootDirectory %h
ForceCommand internal-sftp
完成这一步之后，尝试登录SFTP：

sftp www@abc.com
#或者
ssh www@abc.com
#如果出现下面的错误信息，则可能是目录权限设置错误，继续看下一步
#Connection to abc.com closed by remote host.
#Connection closed
权限设置

要实现Chroot功能，目录权限的设置非常重要。否则无法登录，给出的错误提示也让人摸不着头脑，无从查起。我在这上面浪费了很多时间。

目录权限设置上要遵循2点：

ChrootDirectory设置的目录权限及其所有的上级文件夹权限，属主和属组必须是root；
ChrootDirectory设置的目录权限及其所有的上级文件夹权限，只有属主能拥有写权限，也就是说权限最大设置只能是755。
如果不能遵循以上2点，即使是该目录仅属于某个用户，也可能会影响到所有的SFTP用户。

chown root.root /home/sftp /home/sftp/homepage /home/sftp/blog /home/sftp/pay
chmod 755 /home/sftp /home/sftp/homepage /home/sftp/blog /home/sftp/pay
由于上面设置了目录的权限是755，因此所有非root用户都无法在目录中写入文件。我们需要在ChrootDirectory指定的目录下建立子目录，重新设置属主和权限。以homepage目录为例：

mkdir /home/sftp/homepage/web
chown www.sftp /home/sftp/homepage/web
chmod 775 /home/sftp/homepage/web
要实现web服务器与blog账户互删文件的权限需求，需要设置umask，让默认创建的文件和目录权限为775即可。将下面的内容写入.bashrc中：

umask 0002
至此，我们已经实现了所有需要的功能。

—————————–
umask 0002
—————————–
一、团队构建环境，文件读写共享
项目代码位于/svn/prj下，通过svn up更新代码，调用ant来编译、部署。那么，prj这个目录，对于每个人都是需要可读写的。
我 们知道，用什么用户登录，新创建的文件宿主，就是当前用户。而默认的文件权限是644（-rw-r–r–），张三从代码仓库中update的文件，或 者编译后生成的class文件，李四是没法删除的。执行ant clear必然不成功，每次都用chmod去修改相应文件，总不是个事，那怎么办呢？
目标很明确：我们希望，开发团队中，每一个开发人员之间的权限是平等的，谁新建的文件都可以被其他人读写。
分解出来是两个事情：
1.目录/svn/prj应该属于开发团队，即一个用户组。这很简单，建立一个组，比如叫dev,使用chown即可
#gruopadd dev
#useradd zhangshan
#useradd lisi
#useradd zhangsan -G dev -g dev
#useradd lisi -G dev -g dev
#chown -R :dev /svn/prj
这里要特别说明一下，-g和-G是有区别的。-G是大家自然理解的，把一个用户加到一个组或者多个组（逗号分隔）里面去。-g呢，则是
设置用户的gid。也就是用户登陆后初始group(initial group)。
使用id zhangsan命令，可以看到，uid=zhangsantest,gid=dev,groups=zhangsan,dev。或者使用groups zhangsan，结果是zhangsan dev
要注意，创建一个用户，默认会创建一个同名的组，如果不加-g参数，gid就是那个组的id，新建文件，组属也是用户同名组。所以在这里，-g和-G都是缺一不可的。
2.更改文件创建的默认权限为664（-rw-rw-r–）。
这里涉及到一个知识，就是umask，umask主要用来控制默认创建文件或目录的权限。可以使用umask命令直接修改。在我们的linux环境中，默认的umask是022。
umask：设置哪位为1，则哪位就没有权限。放开哪位，哪位有权限。但文件例外，最高到666（默认没有执行权限）。目录则可以到777
比如设置umask为022，则目录最高可以到755，umask为002，则最高目录可以到775
解决思路：每个用户登录都会执行一些初始化脚本，可以在脚本中修改用户的umask。
脚本片段如下：
USERGROUP=`/usr/bin/id -Gn $USER`
echo $USERGROUP | grep -q dev
if [ $? -eq 0 ]; then
umask 0002
fi
意思很简单，这里不赘述。要注意的是，Linux中，应该放在/etc/bashrc里面，而不是/etc/profile中。
登录shell配置文件执行顺序
/etc/profile–>/etc/profile.d/*.sh–>~/.bash_profile–>~/.bashrc–>/etc/bashrc
我们应该把这个设置放在最后执行的文件/etc/bashrc的末尾位置，以防止设置被覆盖（实际上，linux的/etc/bashrc文件开头就有一段类似的umask设置）。
要说明一点：控制用户对某个目录的默认读写权限，是没有直接支持的。在实际中，暂时也没必要，如果真有特殊需要，可以通过crontab设置监控进程定时进行修改，也很简单，在此不做说明。
二、普通用户的特权身份
OK，在第一部分中，我们解决了多人文件共享读写，该运行服务器了。不就是tomcat吗，startup一下。事情没想象那么简单，Tomcat运行过 程中，会写日志文件，一开始，简单的把logs目录组属划分给dev，但后来陆续又遇到一系列不同的权限问题。于是反思一下：与其一点点修改运行 Tomcat涉及的那么多文件权限，不如把自己身份临时换一下？这就是我们要说的sudo。
sudo命令就是sudoer用来执行root操作的。sudoer配置，通过visudo来编辑。
visudo实际上就是vi /etc/sudoers的包装版。但用这个命令的最大好处是，它有语法检查。
%dev ALL =NOPASSWORD: /usr/local/tomcat/bin/startup.sh
%dev ALL =NOPASSWORD: /usr/local/tomcat/bin/shutdown.sh
百分号表示组，如果是多个组，用%dev,%dev2
ALL为所有主机。如果要指定主机，可换成某个ip地址。
NOPASSWORD表示不需要sudoer输入密码。
最后为授权执行的命令全路径。
sudoer的配置还有很多，比如可以设置别名等，请读者自行学习。
执行：组员只需要在原有命令前面加上sudo 即可。
如此一来，Tomcat停启问题也解决了。
补充：sudo命令通配符的设置，如果某个目录下的所有命令都可以给sudoers开放，可以使用xxxx/*.sh，但这样一来，使用者必须使用绝对路径执行。而在当前路径也不能使用./xxx.sh。是何原因，待研究。
三、sftp用户的umask设置
似乎万事大吉了。但有一天，发现还是有一些文件没有权限覆盖，为什么呢？后来发现这部分文件，都是使用winscp上传的。
解决办法：
vi /etc/ssh/sshd_config文件，找到SubSystem sftp /usr/libexec/openssh/sftp-server这一行，修改为
SubSystem sftp /usr/libexec/openssh/sftp-server.sh
然后vi /usr/libexec/openssh/sftp-server.sh
添加
umask 0002
/usr/libexec/openssh/sftp-server
chmod 755 /usr/libexec/openssh/sftp-server.sh 即可。
当然，umask 0002这行可以跟上面的策略一致
变成
USERGROUP=`/usr/bin/id -Gn $USER`
echo $USERGROUP | grep -q developers
if [ $? -eq 0 ]; then
umask 0002
fi

四、NFS文件设置问题
A、B 两台服务器，A为NFS服务器，B为挂载服务器。开发中，发现这个目录老是出现权限问题。但查看组属又没什么问题。甚是奇怪。
具体事例：
一 个NFS的源路径，比如是hostA:/share，该目录在hostA上的属于用户组dev,hostB mount了这个目录，看到该目录用户组是一个组号，比如105，其实就是hostA上的dev用户组号。但这个组号，在hostB上并不存在 (hostB上也有一个dev组)，如何让hostB上的用户也能读写该目录？最后，终于发现症结所在：两边的组号不一致，而文件的拥有者和组属，本质是 认id不认name的。修改了哪边，都会让另一边无法写，产生了冲突。
解决办法：把两边的组号修改为一致。
1.首先，保证hostB上没有105号的组，如果有，则需要协调一个两边都不产生冲突的组号，可能需要修改两边的组号。
2.组号确定之后，假设105就行，在hostB上执行：groupmod –g 105 dev。变化可以通过/etc/group查看
3.重新设置改组涉及到的文件的组属。
4.属于该组的用户需要重新登录，这样才会生效。
五、root用户的行为限制
权限问题中，还有root的滥用。如果使用root来编译部署，root产生的文件，dev用户又无权访问了。也就是说，既然已经划分好了小组构建目录，每个用户都应该是dev组成员才对。root用户应该只在授权或普通用户无法解决的时候，再切换使用。

### 网络设置

    重启服务： systemctl restart network.service
    查看网络服务问题： systemctl status network.service

    查看linux版本: cat  /etc/redhat-release


    IPADDR=192.168.2.22
    NETMASK=255.255.255.0
    GATWAY=192.168.2.1


## rar 文件安装

1）  在 http://www.rarsoft.com/download.htm 下载 linux 版本的 rarlinux
     我是64位版本，下载  rarlinux-x64-5.3.0.tar.gz
2）安装rarlinux
     将下载的   rarlinux-x64-5.3.0.tar.gz 上传至 linux服务器
     tar -zvxf  rarlinux-x64-5.3.0.tar.gz
     当前目录下会出现一个 rar 目录
     cd rar
     ls
     make
    你会看到 rar 和 unrar 命令被安装到了 /usr/local/bin 目录中
    执行一下 rar 或者  unrar ，可以看到输出了
3）执行解压缩 rar 文件
   命令的 e 参数，是解压 rar 文件
   e       Extract files without archived paths.
   使用方法为： unrar e RAR文件名
4）其它常用参数
      a       Add files to archive.      向 RAR文件中加入文件
      d       Delete files from archive.   删除RAR文件中的某个文件


### 配置环境变量

    export JAVA_HOME=/home/jdk1.7.0_79
    export PATH=$JAVA_HOME/bin:$PATH
    export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

    export JAVA_HOME=/home/jdk1.7.0_79
    export PATH=$JAVA_HOME/bin:$PATH
    export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar


## linux须知

### 网络的四种模式
1. 桥接可以直接访问，内外都通
2. NAT只能内部访问外部外部不能访问内部
3. 仅主机模式虚拟机内部之间可以互相访问，不能进去也不能出来


### linux PV/VG/LV理解

![img/linux_pv-vg-lv.png](img/linux_pv-vg-lv.png "Optional title")



## centos 部分命令

ip addr 查询网络

## linux快捷键

1. [tab] 键
在linux所有的shell中，[tab]是最常用的也是linux的bash  shell中最棒的功能；它具有命令补全和档案补全的功能。如果不使用[tab]键，那就别说自己懂linux！
举例，命令补全
我想将磁盘格式化成ext3  ，但是不知道命令是什么了，只记得只mk开头的，那我可以输个mk然后连按两下[tab]

2. [ctrl] +c 强制终止当前的进程
如果在linux下，输入了错误的指令或参数，有时候这个命令就会在系统中跑不停，可以用【ctrl+c】来终止

3. [ctrl+d]键
两个功能，一是代表键盘输入的结束；二是用来取代exit命令。例如要离开文字借口，可以直接按下[ctrl+d]键。
当我用ssh客户端去连接linux，此时，我按了[ctrl+d]键就是这个效果
[root@kissing ~]#
[root@kissing ~]# logout
4. [ctrl+L]键
这个 很实用，清屏，跟clear命令式等效的。
5. [ctrl +K]键
删除 从光标到行末的所有字符
6. [ctrl+U]键清除当前行，与[ctrl+K]相反，删除从光标到行首的所有字符
7. [ctrl+A] 键  移动光标到行首
8. [ctrl+E]键  移动光标到行尾
9.  cd ~  进入当前用户的家目录
10. cd  -  返回前一次进入的目录

## 终端窗口快捷键

```shell
Shift+Ctrl+T:新建标签页
Shift+Ctrl+W:关闭标签页
Ctrl+PageUp:前一标签页
Ctrl+PageDown:后一标签页
Shift+Ctrl+PageUp:标签页左移
Shift+Ctrl+PageDown:标签页右移
Alt+1:切换到标签页1
Alt+2:切换到标签页2
Alt+3:切换到标签页3
Shift+Ctrl+N:新建窗口
Shift+Ctrl+Q:关闭终端
终端中的复制／粘贴:
Shift+Ctrl+C:复制
Shift+Ctrl+V:粘贴
终端改变大小：
F11：全屏
Ctrl+plus:放大
Ctrl+minus:减小
Ctrl+0:原始大小
```

## 权限

文件权限管理

三种基本权限

R           读         数值表示为4

W          写         数值表示为2

X           可执行  数值表示为1



如图所示，jdk-7u21-linux-i586.tar.gz文件的权限为-rw-rw-r--

-rw-rw-r--一共十个字符，分成四段。

第一个字符“-”表示普通文件；这个位置还可能会出现“l”链接；“d”表示目录

第二三四个字符“rw-”表示当前所属用户的权限。   所以用数值表示为4+2=6

第五六七个字符“rw-”表示当前所属组的权限。      所以用数值表示为4+2=6

第八九十个字符“r--”表示其他用户权限。              所以用数值表示为2

所以操作此文件的权限用数值表示为662

更改权限

sudo chmod [u所属用户  g所属组  o其他用户  a所有用户]  [+增加权限  -减少权限]  [r  w  x]   目录名

例如：有一个文件filename，权限为“-rw-r----x” ,将权限值改为"-rwxrw-r-x"，用数值表示为765

sudo chmod u+x g+w o+r  filename


## SHELL基本命令

```shell
$history                  显示在当前shell下命令历史
$alias                      显示所有的命令别称
$alias new_command='command'    将命令command别称为new_command
$env                       显示所有的环境变量
$export var=value    设置环境变量var为value
```

## 显示硬盘、分区、CPU、内存信息

```shell
$df -lh                           显示所有硬盘的使用状况
$du -sh *                       显示当前目录下各个目录和文件的大小

$mount                          显示所有的硬盘分区挂载
$mount partition path       挂在partition到路径path
$umount partition            卸载partition
$sudo fdisk -l                  显示所有的分区
$sudo fdisk device            为device(比如/dev/sdc)创建分区表。 进入后选择n, p, w
$sudo mkfs -t ext3 partition  格式化分区patition(比如/dev/sdc1)
                                      修改 /etc/fstab，以自动挂载分区。增加行：
                                      /dev/sdc1  path(mount point) ext3 defaults 0 0
$arch                            显示架构
$cat /proc/cpuinfo          显示CPU信息
$cat /proc/meminfo         显示内存信息
$free                            显示内存使用状况
```


## 文件

```shell
$touch filename    如果文件不存在，创建一个空白文件；如果文件存在，更新文件读取和修改时间。
$rm filename      删除文件
$cp file1 file2    复制file1为file2
$ls -l path        显示文件和文件相关信息
$mkdir dir        创建dir文件夹
$mkdir -p path    递归创建路径path上的所有文件夹
$rmdir dir        删除dir文件夹，dir必须为空文件夹。
$rm -r dir        删除dir文件夹，以及其包含的所有文件
$file filename    文件filename的类型描述
$chown username:groupname filename    更改文件的拥有者为owner，拥有组为group
$chmod 755 filename更改文件的权限为755: owner r+w+x, group: r+x, others: r+x
$od -c filename    以ASCII字符显示文件
$cat filename      显示文件
$cat file1 file2  连接显示file1和file2
$head -1 filename  显示文件第一行
$tail -5 filename  显示文件倒数第五行
$diff file1 file2  显示file1和file2的差别
$sort filename    对文件中的行排序，并显示
$sort -f filename  排序时，不考虑大小写
$sort -u filename  排序，并去掉重复的行
$uniq filename    显示文件filename中不重复的行 (内容相同，但不相邻的行，不算做重复)
$wc filename      统计文件中的字符、词和行数
$wc -l filename    统计文件中的行数
```

## 目录

1. /- 根
每一个文件和目录从根目录开始。
只有root用户具有该目录下的写权限。请注意，/root是root用户的主目录，这与/.不一样

2. /bin中 - 用户二进制文件
包含二进制可执行文件。
在单用户模式下，你需要使用的常见Linux命令都位于此目录下。系统的所有用户使用的命令都设在这里。
例如：ps、ls、ping、grep、cp

3. /sbin目录 - 系统二进制文件
就像/bin，/sbin同样也包含二进制可执行文件。
但是，在这个目录下的linux命令通常由系统管理员使用，对系统进行维护。例如：iptables、reboot、fdisk、ifconfig、swapon命令

4. /etc - 配置文件
包含所有程序所需的配置文件。
也包含了用于启动/停止单个程序的启动和关闭shell脚本。例如：/etc/resolv.conf、/etc/logrotate.conf
hosts：设备名称（或域名）到ip地址的解析，相当于本地存在的dns功能。见下图：

5. /dev - 设备文件
包含设备文件。
这些包括终端设备、USB或连接到系统的任何设备。例如：/dev/tty1、/dev/usbmon0
6. /proc - 进程信息
包含系统进程的相关信息。
这是一个虚拟的文件系统，包含有关正在运行的进程的信息。例如：/proc/{pid}目录中包含的与特定pid相关的信息。
这是一个虚拟的文件系统，系统资源以文本信息形式存在。例如：/proc/uptime
7. /var - 变量文件
var代表变量文件。
这个目录下可以找到内容可能增长的文件。
这包括 - 系统日志文件（/var/log）;包和数据库文件（/var/lib）;电子邮件（/var/mail）;打印队列（/var/spool）;锁文件（/var/lock）;多次重新启动需要的临时文件（/var/tmp）;
8. /tmp - 临时文件
包含系统和用户创建的临时文件。
当系统重新启动时，这个目录下的文件都将被删除。
9. /usr - 用户程序
包含二进制文件、库文件、文档和二级程序的源代码。
/usr/bin中包含用户程序的二进制文件。如果你在/bin中找不到用户二进制文件，到/usr/bin目录看看。例如：at、awk、cc、less、scp。
/usr/sbin中包含系统管理员的二进制文件。如果你在/sbin中找不到系统二进制文件，到/usr/sbin目录看看。例如：atd、cron、sshd、useradd、userdel。
/usr/lib中包含了/usr/bin和/usr/sbin用到的库。
/usr/local中包含了从源安装的用户程序。例如，当你从源安装Apache，它会在/usr/local/apache2中。
10. /home - HOME目录
所有用户用home目录来存储他们的个人档案。
例如：/home/john、/home/nikita
11. /boot - 引导加载程序文件
包含引导加载程序相关的文件。
内核的initrd、vmlinux、grub文件位于/boot下。
例如：initrd.img-2.6.32-24-generic、vmlinuz-2.6.32-24-generic
12. /lib - 系统库
包含支持位于/bin和/sbin下的二进制文件的库文件.
库文件名为 ld*或lib*.so.*
例如：ld-2.11.1.so，libncurses.so.5.7
13. /opt - 可选的附加应用程序
opt代表可选的。
包含从个别厂商的附加应用程序。
附加应用程序应该安装在/opt/或者/opt/的子目录下。
14. /mnt - 挂载目录
临时安装目录，系统管理员可以挂载文件系统。
15. /media - 可移动媒体设备
用于挂载可移动设备的临时目录。
举例来说，挂载CD-ROM的/media/cdrom，挂载软盘驱动器的/media/floppy;
16. /srv - 服务数据
srv代表服务。
包含服务器特定服务相关的数据。
例如，/srv/cvs包含cvs相关的数据。