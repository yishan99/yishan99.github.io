---
title: Linux-yum出错
date: 2021-01-24 21:23:22
tags:
- Linux
---

### yum 出错
<!--more-->

```shell
Loaded plugins: fastestmirror, refresh-packagekit, security Loading mirror speeds from cached
```

### yum安装软件出错

- 1.备份现有repo仓库]

- 2.下载并使用阿里云仓库repo
- 3.更新yum
- 4.用yum安装软件



日志信息：Loaded plugins: fastestmirror, refresh-packagekit, security
Loading mirror speeds from cached hostfile
Setting up Install Process
No package gcc available.
Error: Nothing to do
*出现这个错误一般是没有正确配置镜像文件，或者是用163镜像，由于12 月 8 日，CentOS 开发团队在其官博宣布，CentOS 8 将在 2021 年底结束支持，CentOS 7 由于用户基数与用户贡献较多，因此会按照计划维护至生命周期结束即 2024 年 6 月 30 日，接下来一年会把重心放到 CentOS Stream 上。好多镜像都已经不能访问了*

建议以前有镜像的或者是新安装的系统都配置阿里镜像
配置过程如下：

## 1.备份现有repo仓库

```shell
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

## 2.下载并使用阿里云仓库repo

```shell
curl -o /etc/yum.repos.d/CentOS-Base.repo https://www.xmpan.com/Centos-6-Vault-Aliyun.repo
```

## 3.更新yum

```shell
yum clean all
```

```shell
yum makecache
```

## 4.用yum安装软件

```shell
yum -y install 软件名称
```

