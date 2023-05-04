
- [1.4.1 Linux版本](#141-linux版本)
- [1.4.2 Linux命令](#142-linux命令)
  - [~查看当前路径](#查看当前路径)
  - [~新建文件夹](#新建文件夹)
  - [~查看当前文件目录](#查看当前文件目录)
  - [~切换当前目录](#切换当前目录)
  - [~创建文件树](#创建文件树)
  - [~删除文件夹](#删除文件夹)
  - [~复制文件夹](#复制文件夹)
  - [~移动/重命名文件夹](#移动重命名文件夹)
  - [~返回上层目录](#返回上层目录)
  - [~输出/输出至文件](#输出输出至文件)
  - [~全部查看文件](#全部查看文件)
  - [~输出至文件（追加）](#输出至文件追加)
  - [~部分查看文件](#部分查看文件)
  - [~查找文件](#查找文件)
  - [~查看文件大小](#查看文件大小)
  - [~统计文件行数](#统计文件行数)
  - [~查找文件内容](#查找文件内容)
  - [~查看磁盘](#查看磁盘)
  - [~查看内存使用](#查看内存使用)
  - [~查看进程](#查看进程)
  - [~查看时间](#查看时间)
  - [~后台进程（python）](#后台进程python)
  - [~查看/退出后台进程](#查看退出后台进程)
  - [~编辑脚本文件](#编辑脚本文件)
  - [~修改文件权限](#修改文件权限)
  - [~运行脚本文件](#运行脚本文件)
  - [~FTP操作](#ftp操作)

### 1.4.1 Linux版本
常用如下：

| 版本名称 | 网址 | 特点 | 软件包管理器 |
| --- | --- | --- | --- |
| Debian Linux | [www.debian.org](http://www.debian.org/) | 开放的开发模式，且易于进行软件包升级 | apt |
| Fedora Core | [www.redhat.com](http://www.redhat.com/) | 拥有数量庞人的用户，优秀的社区技术支持. 并且有许多创新 | up2date（rpm），yum （rpm） |
| CentOS | [www.centos.org](http://www.centos.org/) | CentOS 是一种对 RHEL（Red Hat Enterprise Linux）源代码再编译的产物，由于 Linux 是开发源代码的操作系统，并不排斥样基于源代码的再分发，CentOS 就是将商业的 Linux 操作系统 RHEL 进行源代码再编译后分发，并在 RHEL 的基础上修正了不少已知的漏洞 | rpm |
| SUSE Linux | [www.suse.com](http://www.suse.com/) | 专业的操作系统，易用的 YaST 软件包管理系统 | YaST（rpm），第三方 apt （rpm）软件库（repository） |
| Mandriva | [www.mandriva.com](http://www.mandriva.com/) | 操作界面友好，使用图形配置工具，有庞大的社区进行技术支持，支持 NTFS 分区的大小变更 | rpm |
| KNOPPIX | [www.knoppix.com](http://www.knoppix.com/) | 可以直接在 CD 上运行，具有优秀的硬件检测和适配能力，可作为系统的急救盘使用 | apt |
| Gentoo Linux | [www.gentoo.org](http://www.gentoo.org/) | 高度的可定制性，使用手册完整 | portage |
| Ubuntu | [www.ubuntu.com](http://www.ubuntu.com/) | 优秀已用的桌面环境，基于 Debian 构建 | apt |


### 1.4.2 Linux命令

以下基于空文件夹/root（一般不建议）下实战，一步一步实操：

#### ~查看当前路径

```shell
pwd
# /root
```

#### ~新建文件夹

```shell
mkdir data
```

#### ~查看当前文件目录

```shell
ls -l
# total 4
# drwxr-xr-x 2 root root 4096 Nov 26 14:12 data
```

#### ~切换当前目录

```shell
cd data/
pwd
# /root/data
```

#### ~创建文件树

```shell
mkdir -p f1/f2/f3 e1 e2
ls -l
# total 12
# drwxr-xr-x 2 root root 4096 Nov 26 14:22 e1
# drwxr-xr-x 2 root root 4096 Nov 26 14:22 e2
# drwxr-xr-x 3 root root 4096 Nov 26 14:22 f1
```

#### ~删除文件夹

```shell
rm -rf f1/
ls -l
# total 8
# drwxr-xr-x 2 root root 4096 Nov 26 14:22 e1
# drwxr-xr-x 2 root root 4096 Nov 26 14:22 e2
```

#### ~复制文件夹

```shell
cp -r e1/ e3/
ls -l
# total 12
# drwxr-xr-x 2 root root 4096 Nov 26 14:22 e1
# drwxr-xr-x 3 root root 4096 Nov 26 14:24 e2
# drwxr-xr-x 2 root root 4096 Nov 26 14:24 e3
```

#### ~移动/重命名文件夹

```shell
mv e3/ e1_/
ls -l
# total 12
# drwxr-xr-x 2 root root 4096 Nov 26 14:22 e1
# drwxr-xr-x 2 root root 4096 Nov 26 14:24 e1_
# drwxr-xr-x 3 root root 4096 Nov 26 14:24 e2
```

#### ~返回上层目录

```shell
cd ../
pwd
# /root
```

#### ~输出/输出至文件

```shell
cd data/
echo 123
# 123
echo 123 > A.csv
ls -l
# total 16
# -rw-r--r-- 1 root root    4 Nov 26 14:32 A.csv
# drwxr-xr-x 2 root root 4096 Nov 26 14:22 e1
# drwxr-xr-x 2 root root 4096 Nov 26 14:24 e1_
# drwxr-xr-x 3 root root 4096 Nov 26 14:24 e2
```

#### ~全部查看文件

```shell
cat A.csv
# 123
```

#### ~输出至文件（追加）

```shell
echo 456 >> A.csv
cat A.csv 
# 123
# 456

echo 456 >> A.csv
echo 456 >> A.csv
echo 456 >> A.csv
echo 456 >> A.csv
echo 456 >> A.csv
cat A.csv 
# 123
# 456
# 456
# 456
# 456
# 456
# 456
```

#### ~部分查看文件

```shell
# 前
head -n 2 A.csv 
# 123
# 456

# 后
tail -n 2 A.csv 
# 456
# 456
```

#### ~查找文件

```shell
cat /proc/cpuinfo > B.csv
ls -l|grep .csv
# -rw-r--r-- 1 root root   28 Nov 26 14:38 A.csv
# -rw-r--r-- 1 root root 2818 Nov 26 14:50 B.csv
find . -name B.csv
# ./B.csv
```

#### ~查看文件大小

```shell
ls -l|grep .csv
# total 20
# -rw-r--r-- 1 root root   28 Nov 26 14:38 A.csv
# -rw-r--r-- 1 root root 2818 Nov 26 14:50 B.csv
ls -l -h|grep .csv
# total 20K
# -rw-r--r-- 1 root root   28 Nov 26 14:38 A.csv
# -rw-r--r-- 1 root root 2.8K Nov 26 14:50 B.csv
```

#### ~统计文件行数

```shell
wc -l A.csv 
# 7 A.csv
wc -l B.csv 
# 78 B.csv
```

#### ~查找文件内容

```shell
cat B.csv|grep 'model name' 
# model name	: Intel(R) Core(TM) i7-8850H CPU @ 2.60GHz
# model name	: Intel(R) Core(TM) i7-8850H CPU @ 2.60GHz
# model name	: Intel(R) Core(TM) i7-8850H CPU @ 2.60GHz
```

#### ~查看磁盘

```shell
df -h
# Filesystem      Size  Used Avail Use% Mounted on
# overlay          59G   21G   36G  37% /
# tmpfs            64M     0   64M   0% /dev
# tmpfs           7.9G     0  7.9G   0% /sys/fs/cgroup
# shm              64M     0   64M   0% /dev/shm
```

#### ~查看内存使用

```shell
free -h
#               total        used        free      shared  buff/cache   available
# Mem:            15G        363M         13G        776K        1.5G         15G
# Swap:          1.0G          0B        1.0G
```

#### ~查看进程

```shell
top
# top - 15:00:30 up 7 days, 55 min,  0 users,  load average: 0.11, 0.11, 0.09
# Tasks:   3 total,   1 running,   2 sleeping,   0 stopped,   0 zombie
# %Cpu(s):  0.7 us,  3.8 sy,  0.0 ni, 95.5 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
# KiB Mem : 16398652 total, 14428952 free,   372944 used,  1596756 buff/cache
# KiB Swap:  1048572 total,  1048572 free,        0 used. 15734064 avail Mem 
# 
#   PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
#     1 root      20   0   18504   3272   2912 S   0.0  0.0   0:00.10 bash
#    16 root      20   0   18644   3556   3016 S   0.0  0.0   0:00.32 bash
#   130 root      20   0   38744   3168   2624 R   0.0  0.0   0:00.01 top
```

#### ~查看时间

```shell
date
# Thu Nov 26 15:07:33 UTC 2020
```

#### ~后台进程（python）

```shell
python &
# [1] 136
# Python 3.6.6 (default, Sep 12 2018, 18:26:19) 
# [GCC 8.0.1 20180414 (experimental) [trunk revision 259383]] on linux
# Type "help", "copyright", "credits" or "license" for more information.
# 
# [1]+  Stopped                 python
```

#### ~查看/退出后台进程

```shell
ps -ef|grep python
# root       136    16  0 15:06 pts/1    00:00:00 python
# root       144    16  0 15:11 pts/1    00:00:00 grep --color=auto python

kill -9 136
# [1]+  Killed                  python

ps -ef|grep python
# root       146    16  0 15:11 pts/1    00:00:00 grep --color=auto python
```

#### ~编辑脚本文件

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.4.2.0-000.png" height=400>
</p>

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.4.2.0-001.jpeg" height=400>
</p>

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.4.2.0-002.jpeg" height=400>
</p>

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.4.2.0-003.jpeg" height=400>
</p>

> 详见 [https://www.runoob.com/linux/linux-vim.html](https://www.runoob.com/linux/linux-vim.html)


```shell
vi run.sh
cat run.sh
# echo 1
# echo 2
# echo 3
```

#### ~修改文件权限

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/1.4.2.0-004.png" height=400>
</p>

> 详见 [https://www.runoob.com/linux/linux-comm-chmod.html](https://www.runoob.com/linux/linux-comm-chmod.html)


```shell
ls -l run.sh 
# -rw-r--r-- 1 root root 21 Nov 26 15:22 run.sh
chmod +x run.sh 
ls -l run.sh 
# -rwxr-xr-x 1 root root 21 Nov 26 15:22 run.sh
chmod -x run.sh 
ls -l run.sh 
# -rw-r--r-- 1 root root 21 Nov 26 15:22 run.sh
chmod +x run.sh 
ls -l run.sh 
# -rwxr-xr-x 1 root root 21 Nov 26 15:22 run.sh
```

#### ~运行脚本文件

```shell
sh run.sh
# 1
# 2
# 3
```

#### ~FTP操作

> - help或?- 列出所有可用的FTP命令；
> - cd - 更改远程计算机上的目录；
> - lcd - 更改本地计算机上的目录；
> - ls - 列出当前远程目录中的文件和目录的名称；
> - mkdir - 在当前远程目录中创建一个新目录；
> - pwd - 打印远程计算机上的当前工作目录；
> - delete - 删除当前远程目录中的文件；
> - rmdir- 删除当前远程目录中的目录；
> - get - 将一个文件从远程复制到本地计算机；
> - mget - 将多个文件从远程复制到本地计算机；
> - put - 将一个文件从本地复制到远程计算机；
> - mput - 将一个文件从本地复制到远程计算机；


```shell
ftp 192.168.42.77
# 220---------- Welcome to Pure-FTPd [privsep] [TLS] ----------
# 220-You are user number 1 of 50 allowed.
# 220-Local time is now 21:35\. Server port: 21.
# 220-This is a private system - No anonymous login
# 220-IPv6 connections are also welcome on this server.
# 220 You will be disconnected after 15 minutes of inactivity.
# Name (192.168.42.77:localuser): linuxidc
# Password:
# 230 OK. Current restricted directory is /
# Remote system type is UNIX.
# Using binary mode to transfer files.
# ftp>

ftp > cd ~/ftp_downloads

# 下载
ftp > get backup.zip
# 200 PORT command successful
# 150-Connecting to port 60609
# 150 6516.9 kbytes to download
# 226-File successfully transferred
# 226 2.356 seconds (measured here), 2.70 Mbytes per second
# 6673256 bytes received in 2.55 seconds (2.49 Mbytes/s)

# 上传
ftp > put image.jpg
# 200 PORT command successful
# 150 Connecting to port 34583
# 226-File successfully transferred
# 226 0.849 seconds (measured here), 111.48 Kbytes per second
# 96936 bytes sent in 0.421 seconds (225 kbytes/s)
```

注意：

**1、非特殊情况下，建议不使用root；**

**2、慎用rm命令：**
**一旦你执行了上述“rm -rf /” 或者“rm -rf /*”命令，会删除Linux根目录下的所有文件，直接导致服务器瘫痪；**

