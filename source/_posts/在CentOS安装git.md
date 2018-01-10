---
title: 在CentOS安装git
date: 2018-01-10 14:58:07
tags: [Linux,CentOS,git]
---

## 1.通过winscp将window的git包上传到Linux的/root/目录下

```
下载地址：https://www.kernel.org/pub/software/scm/git/
```

## 2.解压git

```shell
# tar -zxvf git-2.9.0.tar.gz 
```

## 3.安装curl-devel

```shell
//使用yum安装之前，记得挂载
# yum -y install curl-devel
```

## 4.安装git

```shell
# cd git-2.9.0

# ./configure --prefix=/usr/local/git

# make

# make install
```

## 5.将git命令加入到环境变量# vim /etc/profile

```shell
    //将下面这一行放到/etc/profile文件末尾
    export PATH=/usr/local/git/bin:$PATH
```