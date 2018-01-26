---
title: 'Linux/CentOS 下开启MySQL远程连接,远程管理数据库'
date: 2018-01-04 13:53:17
tags: [数据库,Linux,MySQL,CentOS]
categories: [数据库,Linux]
---

> 当服务器没有运行PHP、没装phpMyAdmin的时候，远程管理MySQL就显得有必要了。

<!-- more -->


## 第一步：开启MySQL用户的远程访问权限
```mysql
mysql -u root -p mysql # 第1个mysql是执行命令，第2个mysql是系统数据名称
```
mysql -u root -p mysql # 第1个mysql是执行命令，第2个mysql是系统数据名称
在MySQL控制台执行:
```mysql
grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;
# root是用户名，%代表任意主机，'123456'指定的登录密码（这个和本地的root密码可以设置不同的，互不影响）
flush privileges; # 重载系统权限
exit;
```
grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;
> root是用户名，%代表任意主机，'123456'指定的登录密码（这个和本地的root密码可以设置不同的，互不影响）
flush privileges; # 重载系统权限
exit;
如果想允许用户root从ip为192.168.137.99的主机连接到MySQL服务：

```mysql
grant all privileges on *.* to 'root'@'192.168.137.99' identified by '123456' with grant option;
flush privileges;

```
grant all privileges on *.* to 'root'@'192.168.137.99' identified by '123456' with grant option;
flush privileges;

## 第二步：设置防火墙，让 3306 端口对外可访问
```mysql
iptables -I INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT
# 查看规则是否生效
iptables -L -n # 或者: service iptables status

# 此时生产环境是不安全的，远程管理之后应该关闭端口，删除之前添加的规则
iptables -D INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT
```
iptables -I INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT
### 查看规则是否生效
iptables -L -n # 或者: service iptables status

### 此时生产环境是不安全的，远程管理之后应该关闭端口，删除之前添加的规则
iptables -D INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT
注意：上面iptables添加/删除规则都是临时的，如果需要重启后也生效，需要保存修改:
```mysql
service iptables save # 或者: /etc/init.d/iptables save
```
service iptables save # 或者: /etc/init.d/iptables save
另外，
```mysql
vi /etc/sysconfig/iptables # 加上下面这行规则也是可以的
-A INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT
```
vi /etc/sysconfig/iptables # 加上下面这行规则也是可以的
-A INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT

远程管理数据库的软件，Windows系统下可以使用SQLyog，用了几种远程软件，感觉这个用起来蛮不错的。
