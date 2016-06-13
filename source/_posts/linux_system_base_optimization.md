---
title: Linux系统的基础优化
date: 2016-06-13 00:07:34
tags: [系统, linux]
categories: [系统]
---

> 晚上在群里看到有朋友问linux的基本优化具体有哪些？
这个问题嘛！因为我有总结过，所以就不是很难回答。
linux系统的优化有很多，我简单阐述下我经常优化的方针：


# 一清、一精、一增
# 两优、四设、七其他

<!--more-->
## 一清： 
	- 定时清理日志/var/spool/clientsqueue
## 一精： 
	- 精简开机启动服务
## 一增： 
	- 增大文件描述符
## 两优： 
	- linux内核参数的优化
	- yum源优化
## 四设：
	- 设置系统的字符集
	- 设置ssh登录限制
	- 设置开机的提示信息与内核信息
	- 设置block的大小
## 七其他：
	1. 文件系统优化
	2. sync数据同步写入磁盘
	3. 不更新时间戳
	4. 锁定系统关键文件
	5. 时间同步
	6. sudo集权管理
	7. 关闭防火墙和selinux


# 简单说明

### 1. sync 数据同步写入磁盘

```bash
async sync
```

### 2. 不更新时间戳

```bash
noatime
```

### 3. 文件系统优化

```bash
禁止ext3、ext4日志功能*(针对数据不太重要的业务)*
```

### 4. 设置block的大小，一般为4K

```bash
mkfs -t ext3 -b 4096 /dev/sda1
```

### 5. 锁定系统的关键文件

```bash
chattr +/-i /etc/passwd
```

### 6. linux系统的内核调优（参数调优略）

### 7. 设置开机的提示信息，以及系统信息

```bash
/etc/motd /etc/issue
```

### 8. 搭建系统的yum源，以及进行优化（upgrade）
```bash
/etc/yum.repos.d/
```

### 9. 时间同步；服务器在50-100台之间可以搭建时间同步服务器ntpserver
```bash
/usr/sbin/ntpdate time.windows.com
```

### 10. 设置系统的字符集

```bash
/etc/sysconfig/i18n
```

### 11. 利用sudo工具来对用户进行集权管理

```bash
visudo
```

### 12. 限制ssh的登录设置，比如更改端口，禁止root登录，禁止无密码登录等等。

```bash
/etc/ssh/sshd.conf
```

### 13. 增大文件描述符

```bash
echo '* - nofile 65535 ' >>/etc/security/limits.conf
```

### 14. 定时清理/var/spool/clientsqueue/

```bash
写脚本，放在定时任务里面定时清理
```

### 15. 精简开机启动服务

```bash
a) setup,勾选开机启动的服务
b) 终端输入ntsysv
c) 脚本编写
cat /server/scripts/chkinfo.sh
#setup sys start server or process
for i in `chkconfig --list |grep 3:on|awk '{print $1}'`;do chkconfig --level 3 $i off;done
/bin/sh /server/scripts/chkinfo.sh
```