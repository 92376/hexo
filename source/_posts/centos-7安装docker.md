---
title:  "centos-7安装docker"
date:   2018-11-10 14:12:26
categories: docker
tag: yum, docker
---





# 1.centos-7安装docker

### 1.root账户, 更新yum

```
yum -y update
```

### 2.安装yum工具

```
yum install -y yum-utils
```

### 3.docker源

```
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

### 4.更新yum索引

```
yum makecache fast
```

### 5.版本列表

```
yum list docker-ce --showduplicates | sort -r
```

### 6.安装docker

```
yum -y install docker-ce
```

### 7.启动并设置开机启动

```
systemctl start docker && systemctl enable docker
```

### 8.验证

```
docker version
```
