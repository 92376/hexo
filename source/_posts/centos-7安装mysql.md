---
title: centos-7安装mysql
date: 2019-02-01 13:49:29
categories: mysql
tags: [yum, mysql]
---



# 1.安装mysql

## 1.添加mysql yum存储库

1.访问<https://dev.mysql.com/downloads/repo/yum/>上的mysql yum存储库下载页面

2.选择对应的发行包

```shell
rpm -Uvh mysql80-community-release-el7-2.noarch.rpm
```

## 2.选择安装版本

1.查看mysql版本列表

```shell
yum repolist all | grep mysql
```

2.安装最新版本(8.0)，无需进行任何配置

(安装5.7配置): 禁用8.0,启用5.7

1) 使用yum-config-manager命令

```shell
yum-config-manager --disable mysql80-community
yum-config-manager --enable mysql57-community
```

2) 手动编辑配置文件

```shell
vi /etc/yum.repos.d/mysql-community.repo 
```

找到如下配置, 然后编辑该`enabled`选项. `enabled=0`禁用8.0，或 `enabled=1`启用5.7。

```shell
[mysql80-community]
name=MySQL 8.0 Community Server
baseurl=http://repo.mysql.com/yum/mysql-8.0-community/el/6/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

# Enable to use MySQL 5.7
[mysql57-community]
name=MySQL 5.7 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/6/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
```

## 3.安装mysql

```shell
yum install mysql-community-server
```

## 4.启动mysql

```shell
service mysqld start
或者
systemctl start mysqld.service
```

## 5.查看mysql运行状态

```shell
service mysqld status
或者
systemctl status mysqld.service
```

## 6.查看mysql初始化root账户密码

```shell
grep 'temporary password' /var/log/mysqld.log
```

## 7.登录修改生成的密码(不修改, 不能正常使用)

注意: MySQL的 [validate_password](https://dev.mysql.com/doc/refman/8.0/en/validate-password.html) 插件默认安装。这将要求密码包含至少一个大写字母，一个小写字母，一个数字和一个特殊字符，并且密码总长度至少为8个字符 

```shell
mysql -uroot -p

ALTER USER 'root'@'localhost' IDENTIFIED BY 'Admin123.';
```
## 8.设置mysql远程登录

修改user表, 刷新权限

```mysql
use mysql;
update user set host = '%' where user = 'root';
flush privileges;
```

查看监听端口

```shell
netstat -apn|grep 3306
```

如果存在127.0.0.1, 否则跳过下面3步

```shell
vi /etc/my.cnf
```

添加配置

```shell
bing-address=0.0.0.0
```

重启mysql

```shell
 systemctl restart mysqld.service
```

## 9.开启防火墙3306端口

```shell
添加
firewall-cmd --zone=public --add-port=3306/tcp --permanent
（--permanent永久生效，没有此参数重启后失效）
重新载入
firewall-cmd --reload
查看
firewall-cmd --zone=public --query-port=3306/tcp
删除
firewall-cmd --zone=public --remove-port=3306/tcp --permanent
```

## 10.sql_mode配置(解决insert语句Err1055)

```shell
vi /etc/my.cnf
添加配置
sql_model=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
然后重启mysql
systemctl restart mysqld.service
```

