---
title:  "centos-7配置静态IP"
date:   2018-11-5 15:15:27
categories: centos-7
tag: [network, ifcfg-ens33]
---





# 1.centos-7配置静态IP

## 1.安装VMware

```
...
```

## 2.安装Centos7 64位

### 1.静态网络配置

```
vi /etc/sysconfig/network-scripts/ifcfg-ens33
```

```
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
# 注释自动分配
#BOOTPROTO=dhcp
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=e96a96be-c256-4ad9-b611-2ff65450f7d5
DEVICE=ens33
# 开机启动
ONBOOT=yes

# 静态设置于VMnet8网络对应net模式网关192.168.112.2
BOOTPROTO=static
# 自定义虚拟机ip
IPADDR=192.168.112.132
NETMASK=255.255.255.0
GATEWAY=192.168.112.2
DNS1=192.168.112.2
```

### 2.重启网络

```
/etc/init.d/network restart 
或者是
/etc/init.d/networking restart
或者重启网络服务
service network restart
```
