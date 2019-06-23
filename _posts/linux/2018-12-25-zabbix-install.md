---
layout: post
title: '安装部署zabbix'
tags: [linux]
---
* [1.安装zabbix4.2](#1)

* * [1.1基础环境配置](#1.1)

* * [1.2配置安装软件源](#1.2)

* * [1.3安装所需软件](#1.3)

* * [1.4zabbix数据表导入数据库中](#1.4)

* * [1.5配置zabbix server](#1.5)

* * [1.6浏览器进行访问](#1.6)

* * [1.71.7zabbix 自动发现配置](#1.7)

<h1 id="1">安装zabbix4.2</h1>

<h2 id="1.1">1.基础环境配置</h2>

~~~bash
systemctl stop firewalld
~~~
> 关闭linux安全机制

~~~vim
vim /etc/sysconfig/selinux
SELINUX=enforcing
改为
SELINUX=disabled
~~~
不重启机器生效
~~~bash
setenforce 0
~~~

<h2 id="1.2">1.2配置安装软件源</h2>
>1.cnetos 源

~~~wget
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
~~~
>2.epel源

~~~wget
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
~~~
>3.mysql源

~~~wget
rpm -Uvh http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
~~~
>4.php源

~~~wget
rpm --import http://rpms.remirepo.net/RPM-GPG-KEY-remi
[root@oldboy /]# rpm -ivh https://mirrors.tuna.tsinghua.edu.cn/remi/enterprise/remi-release-6.rpm
sed -i 's/enabled\=0/enabled\=1/g' /etc/yum.repos.d/remi-php73.repo 
sed -i 's/enabled\=0/enabled\=1/g' /etc/yum.repos.d/remi.repo
sed -i 's/enabled\=0/enabled\=1/g' /etc/yum.repos.d/remi-safe.repo
~~~
>5.nginx源

~~~wget
导入 Nginx RPM-GPG-KEY：
rpm --import http://nginx.org/packages/keys/nginx_signing.key
安装nginx源
rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
~~~
清理yum缓冲

~~~bash
yum clean all
~~~
生成yum缓冲

~~~bash
yum makecache
~~~
#配置zabbix源
>6.zabbix源

~~~wget
wget https://repo.zabbix.com/zabbix/4.1/rhel/7/x86_64/zabbix-release-4.1-1.el7.noarch.rpm
rpm -Uvh zabbix-release-4.1-1.el7.noarch.rpm
~~~
<h2 id="1.3">1.3安装所需软件</h2>
#安装mysql软件包

~~~bash
yum -y install mysql-server
~~~
#安装php73软件包

~~~bash
 yum -y install php73-php-fpm php73-php-gd php73-php-json php73-php-mbstring php73-php-mysqlnd php73-php-xml php73-php-xmlrpc php73-php-opcache
~~~
#安装zabbix软件包

~~~bash
yum -y install zabbix-server-mysql zabbix-web-mysql zabbix-agent 
~~~
#配置mysql数据库
启动mysql

~~~bash
systemctl restart mysqld
mysql
mysql>create database zabbix character set utf8 collate utf8_bin;
mysql>grant all privileges on zabbix.* to zabbix@localhost identified by 'zabbix666666';
mysql>quit;
~~~
<h2 id="1.4">1.4zabbix数据表导入数据库中</h2>
将zabbix数据表导入数据库中
~~~bash
cd /usr/share/doc/zabbix-server-mysql-4.2.0
gunzip create.sql.gz
mysql
mysql> use zabbix;
mysql>  source /usr/share/doc/zabbix-server-mysql-4.2.0/create.sql
mysql> flush privileges;
mysql>quit;
~~~
<h2 id="1.5">1.5配置zabbix server</h2>
配置zabbix server

~~~bash
vim /etc/zabbix/zabbix_server.conf 
找到#DBPassword=这一行，把井号去掉改为=zabbix这是数据的密码
DBPassword=zabbix
~~~
编辑Zabbix前端PHP配置,更改时区

~~~bash
vim /etc/httpd/conf.d/zabbix.conf
php_value date.timezone Asia/Shanghai
~~~
#启动php-fpm

~~~bash
cd /opt/remi/php73/root/sbin/
./php-fpm
~~~
访问不了配置httpd

~~~bash
vim /etc/httpd/conf/httpd.conf
修改后如下：
DocumentRoot "/usr/share/zabbix"
#
# Relax access to content within /var/www.
#
<Directory "/usr/share">
    AllowOverride None
    # Allow open access:
    Require all granted
</Directory>
# Further relax access to the default document root:
<Directory "/usr/share/zabbix">
~~~
#配置完成后重启httpd

~~~bash
systemctl restart httpd
~~~
<h2 id="1.6">1.6浏览器进行访问</h2>
#用浏览器进行访问
![](/images/linux/zabbix1-2018-12-25.png)
![](/images/linux/zabbix2-2018-12-25.png)
#到php.ini修改配置

~~~bash
post_max_size = 16M
max_execution_time = 300
max_input_time = 300
~~~
###:配置完成后
![](/images/linux/zabbix3-2018-12-25.png)
![](/images/linux/zabbix4-2018-12-25.png)
![](/images/linux/zabbix5-2018-12-25.png)
![](/images/linux/zabbix6-2018-12-25.png)
![](/images/linux/zabbix7-2018-12-25.png)
![](/images/linux/zabbix8-2018-12-25.png)
zabbix安装完成，登录成功后进行设置中文。
![](/images/linux/zabbix9-2018-12-25.png)
![](/images/linux/zabbix10-2018-12-25.png)
![](/images/linux/zabbix11-2018-12-25.png)
<h2 id="1.7">1.7zabbix 自动发现配置</h2>
#配置自动发现主机 （动作）
![](/images/linux/zabbix17-2018-12-25.png)
![](/images/linux/zabbix13-2018-12-25.png)
![](/images/linux/zabbix14-2018-12-25.png)
![](/images/linux/zabbix15-2018-12-25.png)
![](/images/linux/zabbix16-2018-12-25.png)
![](/images/linux/zabbix17-2018-12-25.png)
![](/images/linux/zabbix18-2018-12-25.png)
#配置自动发现
![](/images/linux/zabbix19-2018-12-25.png)
![](/images/linux/zabbix20-2018-12-25.png)
![](/images/linux/zabbix21-2018-12-25.png)