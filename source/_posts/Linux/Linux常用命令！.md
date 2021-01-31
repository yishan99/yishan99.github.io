---
title: Linux常用命令！
date: 2021-01-21 09:10:12
tags:
- Linux
---
### Linux常用命令

<!--more-->

#### Linux账号管理

##### 添加用户

```shell
useradd 选项 用户名
```

##### 切换用户

```shell
su 用户名
```

##### 用户口令

```shell
passwd 选项 用户名
```

##### 修改用户

```shell
usermod 选项 用户名
```

##### 删除用户

```shell
userdel 选项 用户名
```

##### 显示用户

```shell
logname
```

##### 查看当前用户的详细信息（用户id,群组id,所属组）

```shell
id 用户名
```

##### 提高普通用户的操作权限

```shell
sudo 参数选项
```

#### Linux用户组

##### 创建用户组

```shell
groupadd 选项 用户组名
```

##### 修改用户组

```shell
groupmod 选项 用户组名
```

##### 查询所属用户组

```shell
groups 用户名
```

##### 删除用户组

```shell
groupdel 用户组名
```

#### 将用户添加到组

```shell
gpasswd -a 组名
```

##### 将用户从组中删除

```shel
gpasswd -d 组名
```

##### 查看用户组下所有用户

```shell
grep '组名' /etc/group
```

#### 日期管理

```shell
date 参数选项
```

#### 进程命令

##### 实时显示所有的进程信息

```shell
top
```

##### 实时显示所有的进程信息（显示完整命令）

```shell
top -c
```

##### 实时显示指定进程的信息

```shell
top -p PID
```

##### 查看进程信息（部分）

```shell
ps -A
```

##### 显示指定用户进程

``` shell
ps -u
```

##### 显示所有进程信息（完整）

```shell
ps -ef
```

##### 杀死指定进程

```shel
kill 进程PID
```

##### 彻底杀死指定进程

```shel
kill -9 进程PID
```

杀死指定用户的所有进程

```shell
kill -9 (ps -ef | grep 用户名)
```

```shell
killall -u 用户名
```

#### 关机重启命令

##### 立刻关机

```shell
shutdown -h now
```

##### 一分钟之后关机，并出现警告信息

```shell
shutdown +1 "警告信息"
```

##### 一分钟之后重启，并出现警告信息

```shell
shutdown -r +1 "警告信息"
```

##### 取消当前关机操作

```shell
shutdown -c
```

##### 立刻重启命令

```shell
reboot
```

#### who命令

##### 显示当前登录系统的用户

```shell
who
```

##### 显示明细（标题）信息

```shell
who -H
```

#### timedatectl命令

##### 校正服务器时间、时区

```shell
timedatectl status
```

##### 查看所有可用的时区

```shell
timedatectl list-timezones
```

#####  设置本地时区

```shell
timedatectl set-timezone "Asia/Shanghai“ 设置本地时区
```

##### 禁用时间同步

```shell
timedatectl set-ntp false
```

##### 启动时间同步

```shell
imedatectl set-ntp true
```

##### 设置时间

```shell
timedatectl set-time “2019-03-11 20:45:00“ 
```

#### 目录管理

##### 显示不隐藏的文件与文件夹

```shell
ls
```

##### 显示不隐藏的文件与文件夹的详细信息

```shell
ls -l
```

##### 显示所有文件与文件夹的详细信息

```shell
ls -al
```

##### 查看当前所在目录

```shell
pwd -P
```

#### 切换目录

```shell
cd
```

##### 创建目录

```shell
mkdir 文件夹名
```

##### 创建多级目录

```shell
mkdir -p aaa/bbb
```

##### 删除目录

```shell
rmdir 文件夹名
```

##### 删除ccc,如果删完之后bbb也是空的，bbb也一起删除

```shell
rmdir -p bbb/ccc
```

##### 删除文件

```shell
rm 文件路径
```

##### 删除目录和目录里面所有的内容

```shell
rm -r 目录路径
```

##### 将aaa文件夹中的a.txt文件拷贝到ccc文件夹中

```shell
cp aaa/a.txt ccc
```

#### 将aaa文件夹中所有内容拷贝到ccc文件夹中

```shell
cp -r aaa/* ccc
```

##### 改名、移动

```shell
mv 数据源 目的地
```

#### Linux文件基本属性

![](https://i.loli.net/2021/01/22/dCRtoeqfEjrZuNm.png)

##### 更改属组（将aaa的属组改为root）

```shell
chgrp -v root aaa
```

##### 将aaa的属主改为root

```shell
chown root aaa
```

##### 将bbb的属主和属组改为root

```shell
chown root:root bbb
```

##### 将aaa文件夹和里面所有的属主和属组改为root

```shell
chown -R root:root aaa
```

##### 符号权限

```shell
chmod 参数选项 符号权限 文件或目录
```

##### 查看文件

```shell
cat 文件名
```

##### 查看文件（显示行号）

```shell
cat -n 文件名
```

##### 查看大文件

```shell
less 文件名
```

##### 查看大文件（显示行号）

```shell
less -N 文件名
```

##### 显示文件的最后3行（查看日志）

```shell
tail -3 文件名 
```

##### 动态显示最后10行

```shell
tail -f 文件名
```

##### 动态显示最后4行

```shell
tail -4f 文件名
```

##### 显示文件的内容，从第二行至文件末尾

```shell
tail -n+2 文件名
```

##### 把包含关键字的行展示出来

```shell
grep 关键字 文件名
```

##### 把包含关键字的行展示出来切加上行号

```shell
grep -n 关键字 文件名
```

##### 把包含关键字的行展示出来，搜索时忽略大小写

```shell
grep -i 关键字 文件名
```

##### 把不包含关键字的行展示出来

```shell
grep -v 关键字 文件名
```

##### 查找指定的进程信息，包含grep进程

```shell
ps-ef | gerp 关键字
```

##### 查找指定的进程信息，不包含grep进程

```shell
ps-ef | gerp 关键字 | grep -v "grep"
```

##### 查找进程个数

```shell
ps-ef | grep -c sshd
```

##### 查看文件时光标位置位于第十行

```shell
vim 文件名 +10
```

##### 展示文本

```shell
echo 字符串
```

##### 将字符串写到文件中（覆盖文件中的内容）

```shell
echo 字符串 > 文件名
```

##### 将字符串写到文件中（不覆盖文件中的内容）

```shell
echo 字符串 >> 文件名
```

##### 将命令的失败结果追加 error.log文件的后面

```shell
cat 不存在的目录 &>> error.log
```

#### 创建软连接（快捷方式）

```shell
ln -s 需要指定的路径 快捷方式
```

#### Find查找文件

##### 将目前及其子目录下所有的.txt文件查询出来

```shell
find . -name "*.txt"
```

##### 将目前及其子目录下最近一天内更新过的文件查询出来

```shell
find . -ctime -1
```

#### Linux备份压缩

##### 压缩文件

```shell
gizp 文件名
```

##### 压缩当前目录下所有文件

```shell
gzip*
```

##### 压缩文件并列出详细信息

```shell
gzip -clv*
```

##### 解压文件

```shell
gunzip 文件名
```

##### tar 命令

##### 打包压缩

```shell
tar -czvf 压缩文件名 文件名/文件夹名 压缩文件或者文件夹并指定压缩文件名
```

##### 解压

```shell
tar -xzvf 压缩文件名
```

##### 查看压缩文件中有哪些文件（查看）

```shell
tar -tzvf 压缩文件名
```

