---
layout: post
title: '安装部署LNMP'
tags: [linux]
---

## 安装环境配置

> 关闭firewalld防火墙
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
> 配置centos系统yum源
~~~wget
>1.cnetos 源
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
~~~
>2.epel源
~~~wget
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
~~~
>3.php源
~~~rpm
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm 
~~~
## 安装LAMP
>1.安装nginx
~~~yum
yum install nginx -y 
~~~

~~~cp
正在解决依赖关系
--> 正在检查事务
---> 软件包 nginx.x86_64.1.1.12.2-2.el7 将被 安装
--> 正在处理依赖关系 nginx-filesystem = 1:1.12.2-2.el7，它被软件包 1:nginx-1.12.2-2.el7.x86_64 需要
--> 正在处理依赖关系 nginx-all-modules = 1:1.12.2-2.el7，它被软件包 1:nginx-1.12.2-2.el7.x86_64 需要
--> 正在处理依赖关系 nginx-filesystem，它被软件包 1:nginx-1.12.2-2.el7.x86_64 需要
--> 正在处理依赖关系 libprofiler.so.0()(64bit)，它被软件包 1:nginx-1.12.2-2.el7.x86_64 需要
--> 正在检查事务
---> 软件包 gperftools-libs.x86_64.0.2.6.1-1.el7 将被 安装
---> 软件包 nginx-all-modules.noarch.1.1.12.2-2.el7 将被 安装
--> 正在处理依赖关系 nginx-mod-stream = 1:1.12.2-2.el7，它被软件包 1:nginx-all-modules-1.12.2-2.el7.noarch 需要
--> 正在处理依赖关系 nginx-mod-mail = 1:1.12.2-2.el7，它被软件包 1:nginx-all-modules-1.12.2-2.el7.noarch 需要
--> 正在处理依赖关系 nginx-mod-http-xslt-filter = 1:1.12.2-2.el7，它被软件包 1:nginx-all-modules-1.12.2-2.el7.noarch 需要
--> 正在处理依赖关系 nginx-mod-http-perl = 1:1.12.2-2.el7，它被软件包 1:nginx-all-modules-1.12.2-2.el7.noarch 需要
--> 正在处理依赖关系 nginx-mod-http-image-filter = 1:1.12.2-2.el7，它被软件包 1:nginx-all-modules-1.12.2-2.el7.noarch 需要
--> 正在处理依赖关系 nginx-mod-http-geoip = 1:1.12.2-2.el7，它被软件包 1:nginx-all-modules-1.12.2-2.el7.noarch 需要
---> 软件包 nginx-filesystem.noarch.1.1.12.2-2.el7 将被 安装
--> 正在检查事务
---> 软件包 nginx-mod-http-geoip.x86_64.1.1.12.2-2.el7 将被 安装
---> 软件包 nginx-mod-http-image-filter.x86_64.1.1.12.2-2.el7 将被 安装
--> 正在处理依赖关系 gd，它被软件包 1:nginx-mod-http-image-filter-1.12.2-2.el7.x86_64 需要
--> 正在处理依赖关系 libgd.so.2()(64bit)，它被软件包 1:nginx-mod-http-image-filter-1.12.2-2.el7.x86_64 需要
---> 软件包 nginx-mod-http-perl.x86_64.1.1.12.2-2.el7 将被 安装
---> 软件包 nginx-mod-http-xslt-filter.x86_64.1.1.12.2-2.el7 将被 安装
---> 软件包 nginx-mod-mail.x86_64.1.1.12.2-2.el7 将被 安装
---> 软件包 nginx-mod-stream.x86_64.1.1.12.2-2.el7 将被 安装
--> 正在检查事务
---> 软件包 gd.x86_64.0.2.0.35-26.el7 将被 安装
--> 正在处理依赖关系 libXpm.so.4()(64bit)，它被软件包 gd-2.0.35-26.el7.x86_64 需要
--> 正在检查事务
---> 软件包 libXpm.x86_64.0.3.5.12-1.el7 将被 安装
--> 解决依赖关系完成
依赖关系解决
======================================================================================================================================
 Package                                       架构                     版本                             源                      大小
======================================================================================================================================
正在安装:
 nginx                                         x86_64                   1:1.12.2-2.el7                   epel                   530 k
为依赖而安装:
 gd                                            x86_64                   2.0.35-26.el7                    base                   146 k
 gperftools-libs                               x86_64                   2.6.1-1.el7                      base                   272 k
 libXpm                                        x86_64                   3.5.12-1.el7                     base                    55 k
 nginx-all-modules                             noarch                   1:1.12.2-2.el7                   epel                    16 k
 nginx-filesystem                              noarch                   1:1.12.2-2.el7                   epel                    17 k
 nginx-mod-http-geoip                          x86_64                   1:1.12.2-2.el7                   epel                    23 k
 nginx-mod-http-image-filter                   x86_64                   1:1.12.2-2.el7                   epel                    26 k
 nginx-mod-http-perl                           x86_64                   1:1.12.2-2.el7                   epel                    36 k
 nginx-mod-http-xslt-filter                    x86_64                   1:1.12.2-2.el7                   epel                    26 k
 nginx-mod-mail                                x86_64                   1:1.12.2-2.el7                   epel                    54 k
 nginx-mod-stream                              x86_64                   1:1.12.2-2.el7                   epel                    76 k

事务概要
======================================================================================================================================
安装  1 软件包 (+11 依赖软件包)

总下载量：1.2 M
安装大小：3.9 M
Downloading packages:
(1/12): gd-2.0.35-26.el7.x86_64.rpm                                                                            | 146 kB  00:00:04     
(2/12): nginx-all-modules-1.12.2-2.el7.noarch.rpm                                                              |  16 kB  00:00:00     
(3/12): libXpm-3.5.12-1.el7.x86_64.rpm                                                                         |  55 kB  00:00:01     
(4/12): nginx-filesystem-1.12.2-2.el7.noarch.rpm                                                               |  17 kB  00:00:00     
(5/12): gperftools-libs-2.6.1-1.el7.x86_64.rpm                                                                 | 272 kB  00:00:06     
(6/12): nginx-1.12.2-2.el7.x86_64.rpm                                                                          | 530 kB  00:00:02     
(7/12): nginx-mod-http-geoip-1.12.2-2.el7.x86_64.rpm                                                           |  23 kB  00:00:01     
(8/12): nginx-mod-http-image-filter-1.12.2-2.el7.x86_64.rpm                                                    |  26 kB  00:00:01     
(9/12): nginx-mod-http-perl-1.12.2-2.el7.x86_64.rpm                                                            |  36 kB  00:00:01     
(10/12): nginx-mod-mail-1.12.2-2.el7.x86_64.rpm                                                                |  54 kB  00:00:00     
(11/12): nginx-mod-stream-1.12.2-2.el7.x86_64.rpm                                                              |  76 kB  00:00:00     
(12/12): nginx-mod-http-xslt-filter-1.12.2-2.el7.x86_64.rpm                                                    |  26 kB  00:00:02     
--------------------------------------------------------------------------------------------------------------------------------------
总计                                                                                                  119 kB/s | 1.2 MB  00:00:10     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在安装    : gperftools-libs-2.6.1-1.el7.x86_64                                                                               1/12 
  正在安装    : 1:nginx-filesystem-1.12.2-2.el7.noarch                                                                           2/12 
  正在安装    : libXpm-3.5.12-1.el7.x86_64                                                                                       3/12 
  正在安装    : gd-2.0.35-26.el7.x86_64                                                                                          4/12 
  正在安装    : 1:nginx-mod-mail-1.12.2-2.el7.x86_64                                                                             5/12 
  正在安装    : 1:nginx-mod-http-geoip-1.12.2-2.el7.x86_64                                                                       6/12 
  正在安装    : 1:nginx-mod-http-xslt-filter-1.12.2-2.el7.x86_64                                                                 7/12 
  正在安装    : 1:nginx-mod-http-perl-1.12.2-2.el7.x86_64                                                                        8/12 
  正在安装    : 1:nginx-mod-stream-1.12.2-2.el7.x86_64                                                                           9/12 
  正在安装    : 1:nginx-1.12.2-2.el7.x86_64                                                                                     10/12 
  正在安装    : 1:nginx-mod-http-image-filter-1.12.2-2.el7.x86_64                                                               11/12 
  正在安装    : 1:nginx-all-modules-1.12.2-2.el7.noarch                                                                         12/12 
  验证中      : libXpm-3.5.12-1.el7.x86_64                                                                                       1/12 
  验证中      : 1:nginx-filesystem-1.12.2-2.el7.noarch                                                                           2/12 
  验证中      : gd-2.0.35-26.el7.x86_64                                                                                          3/12 
  验证中      : gperftools-libs-2.6.1-1.el7.x86_64                                                                               4/12 
  验证中      : 1:nginx-1.12.2-2.el7.x86_64                                                                                      5/12 
  验证中      : 1:nginx-mod-mail-1.12.2-2.el7.x86_64                                                                             6/12 
  验证中      : 1:nginx-all-modules-1.12.2-2.el7.noarch                                                                          7/12 
  验证中      : 1:nginx-mod-http-geoip-1.12.2-2.el7.x86_64                                                                       8/12 
  验证中      : 1:nginx-mod-http-xslt-filter-1.12.2-2.el7.x86_64                                                                 9/12 
  验证中      : 1:nginx-mod-http-image-filter-1.12.2-2.el7.x86_64                                                               10/12 
  验证中      : 1:nginx-mod-http-perl-1.12.2-2.el7.x86_64                                                                       11/12 
  验证中      : 1:nginx-mod-stream-1.12.2-2.el7.x86_64                                                                          12/12 

已安装:
  nginx.x86_64 1:1.12.2-2.el7                                                                                                         

作为依赖被安装:
  gd.x86_64 0:2.0.35-26.el7                                             gperftools-libs.x86_64 0:2.6.1-1.el7                          
  libXpm.x86_64 0:3.5.12-1.el7                                          nginx-all-modules.noarch 1:1.12.2-2.el7                       
  nginx-filesystem.noarch 1:1.12.2-2.el7                                nginx-mod-http-geoip.x86_64 1:1.12.2-2.el7                    
  nginx-mod-http-image-filter.x86_64 1:1.12.2-2.el7                     nginx-mod-http-perl.x86_64 1:1.12.2-2.el7                     
  nginx-mod-http-xslt-filter.x86_64 1:1.12.2-2.el7                      nginx-mod-mail.x86_64 1:1.12.2-2.el7                          
  nginx-mod-stream.x86_64 1:1.12.2-2.el7                               
~~~

>2.安装mysql
~~~yum
yum -y install mariadb 
~~~

~~~cp
已加载插件：fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.aliyun.com
 * extras: mirrors.aliyun.com
 * updates: mirrors.aliyun.com
 * webtatic: uk.repo.webtatic.com
正在解决依赖关系
--> 正在检查事务
---> 软件包 mariadb.x86_64.1.5.5.60-1.el7_5 将被 安装
--> 正在处理依赖关系 mariadb-libs(x86-64) = 1:5.5.60-1.el7_5，它被软件包 1:mariadb-5.5.60-1.el7_5.x86_64 需要
--> 正在检查事务
---> 软件包 mariadb-libs.x86_64.1.5.5.56-2.el7 将被 升级
---> 软件包 mariadb-libs.x86_64.1.5.5.60-1.el7_5 将被 更新
--> 解决依赖关系完成

依赖关系解决

======================================================================================================================================
 Package                          架构                       版本                                   源                           大小
======================================================================================================================================
正在安装:
 mariadb                          x86_64                     1:5.5.60-1.el7_5                       updates                     8.9 M
为依赖而更新:
 mariadb-libs                     x86_64                     1:5.5.60-1.el7_5                       updates                     758 k

事务概要
======================================================================================================================================
安装  1 软件包
升级           ( 1 依赖软件包)

总下载量：9.6 M
Downloading packages:
Delta RPMs reduced 758 k of updates to 73 k (90% saved)
(1/2): mariadb-libs-5.5.56-2.el7_5.5.60-1.el7_5.x86_64.drpm                                                    |  73 kB  00:00:02     
(2/2): mariadb-5.5.60-1.el7_5.x86_64.rpm                                                                       | 8.9 MB  00:00:23     
--------------------------------------------------------------------------------------------------------------------------------------
总计                                                                                                  391 kB/s | 9.0 MB  00:00:23     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在更新    : 1:mariadb-libs-5.5.60-1.el7_5.x86_64                                                                              1/3 
  正在安装    : 1:mariadb-5.5.60-1.el7_5.x86_64                                                                                   2/3 
  清理        : 1:mariadb-libs-5.5.56-2.el7.x86_64                                                                                3/3 
  验证中      : 1:mariadb-libs-5.5.60-1.el7_5.x86_64                                                                              1/3 
  验证中      : 1:mariadb-5.5.60-1.el7_5.x86_64                                                                                   2/3 
  验证中      : 1:mariadb-libs-5.5.56-2.el7.x86_64                                                                                3/3 

已安装:
  mariadb.x86_64 1:5.5.60-1.el7_5                                                                                                     
作为依赖被升级:
  mariadb-libs.x86_64 1:5.5.60-1.el7_5                                                                                                

完毕！
~~~

~~~yum
yum -y install mariadb-devel
~~~

~~~cp
已加载插件：fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.aliyun.com
 * extras: mirrors.aliyun.com
 * updates: mirrors.aliyun.com
 * webtatic: uk.repo.webtatic.com
正在解决依赖关系
--> 正在检查事务
---> 软件包 mariadb-devel.x86_64.1.5.5.60-1.el7_5 将被 安装
--> 解决依赖关系完成

依赖关系解决

======================================================================================================================================
 Package                           架构                       版本                                  源                           大小
======================================================================================================================================
正在安装:
 mariadb-devel                     x86_64                     1:5.5.60-1.el7_5                      updates                     754 k

事务概要
======================================================================================================================================
安装  1 软件包

总下载量：754 k
安装大小：3.3 M
Downloading packages:
mariadb-devel-5.5.60-1.el7_5.x86_64.rpm                                                                        | 754 kB  00:00:22     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在安装    : 1:mariadb-devel-5.5.60-1.el7_5.x86_64                                                                             1/1 
  验证中      : 1:mariadb-devel-5.5.60-1.el7_5.x86_64                                                                             1/1 

已安装:
  mariadb-devel.x86_64 1:5.5.60-1.el7_5                                                                                               

完毕！
~~~

~~~yum
yum -y install mariadb-server
~~~

~~~
已加载插件：fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.aliyun.com
 * extras: mirrors.aliyun.com
 * updates: mirrors.aliyun.com
 * webtatic: uk.repo.webtatic.com
正在解决依赖关系
--> 正在检查事务
---> 软件包 mariadb-server.x86_64.1.5.5.60-1.el7_5 将被 安装
--> 正在处理依赖关系 perl-DBI，它被软件包 1:mariadb-server-5.5.60-1.el7_5.x86_64 需要
--> 正在处理依赖关系 perl-DBD-MySQL，它被软件包 1:mariadb-server-5.5.60-1.el7_5.x86_64 需要
--> 正在处理依赖关系 perl(DBI)，它被软件包 1:mariadb-server-5.5.60-1.el7_5.x86_64 需要
--> 正在检查事务
---> 软件包 perl-DBD-MySQL.x86_64.0.4.023-6.el7 将被 安装
---> 软件包 perl-DBI.x86_64.0.1.627-4.el7 将被 安装
--> 正在处理依赖关系 perl(RPC::PlServer) >= 0.2001，它被软件包 perl-DBI-1.627-4.el7.x86_64 需要
--> 正在处理依赖关系 perl(RPC::PlClient) >= 0.2000，它被软件包 perl-DBI-1.627-4.el7.x86_64 需要
--> 正在检查事务
---> 软件包 perl-PlRPC.noarch.0.0.2020-14.el7 将被 安装
--> 正在处理依赖关系 perl(Net::Daemon) >= 0.13，它被软件包 perl-PlRPC-0.2020-14.el7.noarch 需要
--> 正在处理依赖关系 perl(Net::Daemon::Test)，它被软件包 perl-PlRPC-0.2020-14.el7.noarch 需要
--> 正在处理依赖关系 perl(Net::Daemon::Log)，它被软件包 perl-PlRPC-0.2020-14.el7.noarch 需要
--> 正在处理依赖关系 perl(Compress::Zlib)，它被软件包 perl-PlRPC-0.2020-14.el7.noarch 需要
--> 正在检查事务
---> 软件包 perl-IO-Compress.noarch.0.2.061-2.el7 将被 安装
--> 正在处理依赖关系 perl(Compress::Raw::Zlib) >= 2.061，它被软件包 perl-IO-Compress-2.061-2.el7.noarch 需要
--> 正在处理依赖关系 perl(Compress::Raw::Bzip2) >= 2.061，它被软件包 perl-IO-Compress-2.061-2.el7.noarch 需要
---> 软件包 perl-Net-Daemon.noarch.0.0.48-5.el7 将被 安装
--> 正在检查事务
---> 软件包 perl-Compress-Raw-Bzip2.x86_64.0.2.061-3.el7 将被 安装
---> 软件包 perl-Compress-Raw-Zlib.x86_64.1.2.061-4.el7 将被 安装
--> 解决依赖关系完成

依赖关系解决

===================================================================================================================================================================================================================================================
 Package                                                              架构                                                版本                                                          源                                                    大小
===================================================================================================================================================================================================================================================
正在安装:
 mariadb-server                                                       x86_64                                              1:5.5.60-1.el7_5                                              updates                                               11 M
为依赖而安装:
 perl-Compress-Raw-Bzip2                                              x86_64                                              2.061-3.el7                                                   base                                                  32 k
 perl-Compress-Raw-Zlib                                               x86_64                                              1:2.061-4.el7                                                 base                                                  57 k
 perl-DBD-MySQL                                                       x86_64                                              4.023-6.el7                                                   base                                                 140 k
 perl-DBI                                                             x86_64                                              1.627-4.el7                                                   base                                                 802 k
 perl-IO-Compress                                                     noarch                                              2.061-2.el7                                                   base                                                 260 k
 perl-Net-Daemon                                                      noarch                                              0.48-5.el7                                                    base                                                  51 k
 perl-PlRPC                                                           noarch                                              0.2020-14.el7                                                 base                                                  36 k

事务概要
===================================================================================================================================================================================================================================================
安装  1 软件包 (+7 依赖软件包)

总下载量：12 M
安装大小：62 M
Downloading packages:
(1/8): perl-Compress-Raw-Bzip2-2.061-3.el7.x86_64.rpm                                                                                                                                                                       |  32 kB  00:00:03     
(2/8): perl-Compress-Raw-Zlib-2.061-4.el7.x86_64.rpm                                                                                                                                                                        |  57 kB  00:00:03     
(3/8): perl-DBD-MySQL-4.023-6.el7.x86_64.rpm                                                                                                                                                                                | 140 kB  00:00:03     
(4/8): perl-IO-Compress-2.061-2.el7.noarch.rpm                                                                                                                                                                              | 260 kB  00:00:06     
(5/8): perl-DBI-1.627-4.el7.x86_64.rpm                                                                                                                                                                                      | 802 kB  00:00:10     
(6/8): perl-Net-Daemon-0.48-5.el7.noarch.rpm                                                                                                                                                                                |  51 kB  00:00:03     
(7/8): perl-PlRPC-0.2020-14.el7.noarch.rpm                                                                                                                                                                                  |  36 kB  00:00:03     
(8/8): mariadb-server-5.5.60-1.el7_5.x86_64.rpm                                                                                                                                                                             |  11 MB  00:01:20     
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
总计                                                                                                                                                                                                               155 kB/s |  12 MB  00:01:20     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在安装    : perl-Compress-Raw-Bzip2-2.061-3.el7.x86_64                                                                                                                                                                                     1/8 
  正在安装    : 1:perl-Compress-Raw-Zlib-2.061-4.el7.x86_64                                                                                                                                                                                    2/8 
  正在安装    : perl-IO-Compress-2.061-2.el7.noarch                                                                                                                                                                                            3/8 
  正在安装    : perl-Net-Daemon-0.48-5.el7.noarch                                                                                                                                                                                              4/8 
  正在安装    : perl-PlRPC-0.2020-14.el7.noarch                                                                                                                                                                                                5/8 
  正在安装    : perl-DBI-1.627-4.el7.x86_64                                                                                                                                                                                                    6/8 
  正在安装    : perl-DBD-MySQL-4.023-6.el7.x86_64                                                                                                                                                                                              7/8 
  正在安装    : 1:mariadb-server-5.5.60-1.el7_5.x86_64                                                                                                                                                                                         8/8 
  验证中      : 1:mariadb-server-5.5.60-1.el7_5.x86_64                                                                                                                                                                                         1/8 
  验证中      : perl-Net-Daemon-0.48-5.el7.noarch                                                                                                                                                                                              2/8 
  验证中      : perl-DBD-MySQL-4.023-6.el7.x86_64                                                                                                                                                                                              3/8 
  验证中      : perl-IO-Compress-2.061-2.el7.noarch                                                                                                                                                                                            4/8 
  验证中      : 1:perl-Compress-Raw-Zlib-2.061-4.el7.x86_64                                                                                                                                                                                    5/8 
  验证中      : perl-DBI-1.627-4.el7.x86_64                                                                                                                                                                                                    6/8 
  验证中      : perl-Compress-Raw-Bzip2-2.061-3.el7.x86_64                                                                                                                                                                                     7/8 
  验证中      : perl-PlRPC-0.2020-14.el7.noarch                                                                                                                                                                                                8/8 

已安装:
  mariadb-server.x86_64 1:5.5.60-1.el7_5                                                                                                                                                                                                           

作为依赖被安装:
  perl-Compress-Raw-Bzip2.x86_64 0:2.061-3.el7   perl-Compress-Raw-Zlib.x86_64 1:2.061-4.el7   perl-DBD-MySQL.x86_64 0:4.023-6.el7   perl-DBI.x86_64 0:1.627-4.el7   perl-IO-Compress.noarch 0:2.061-2.el7   perl-Net-Daemon.noarch 0:0.48-5.el7  
  perl-PlRPC.noarch 0:0.2020-14.el7             

完毕！
~~~

>3.安装php

~~~yum
yum -y install php72w php72w-cli php72w-common php72w-devel php72w-embedded php72w-fpm php72w-gd php72w-mbstring php72w-mysqlnd php72w-opcache php72w-pdo php72w-xml
~~~

~~~cp
已加载插件：fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.aliyun.com
 * extras: mirrors.aliyun.com
 * updates: mirrors.aliyun.com
 * webtatic: uk.repo.webtatic.com
正在解决依赖关系
--> 正在检查事务
---> 软件包 mod_php72w.x86_64.0.7.2.10-1.w7 将被 安装
--> 正在处理依赖关系 libargon2.so.0()(64bit)，它被软件包 mod_php72w-7.2.10-1.w7.x86_64 需要
---> 软件包 php72w-cli.x86_64.0.7.2.10-1.w7 将被 安装
---> 软件包 php72w-common.x86_64.0.7.2.10-1.w7 将被 安装
---> 软件包 php72w-devel.x86_64.0.7.2.10-1.w7 将被 安装
---> 软件包 php72w-embedded.x86_64.0.7.2.10-1.w7 将被 安装
---> 软件包 php72w-fpm.x86_64.0.7.2.10-1.w7 将被 安装
---> 软件包 php72w-gd.x86_64.0.7.2.10-1.w7 将被 安装
---> 软件包 php72w-mbstring.x86_64.0.7.2.10-1.w7 将被 安装
---> 软件包 php72w-mysqlnd.x86_64.0.7.2.10-1.w7 将被 安装
---> 软件包 php72w-opcache.x86_64.0.7.2.10-1.w7 将被 安装
---> 软件包 php72w-pdo.x86_64.0.7.2.10-1.w7 将被 安装
---> 软件包 php72w-xml.x86_64.0.7.2.10-1.w7 将被 安装
--> 正在检查事务
---> 软件包 libargon2.x86_64.0.20161029-2.el7 将被 安装
--> 解决依赖关系完成

依赖关系解决

======================================================================================================================================
 Package                             架构                       版本                               源                            大小
======================================================================================================================================
正在安装:
 mod_php72w                          x86_64                     7.2.10-1.w7                        webtatic                     3.1 M
 php72w-cli                          x86_64                     7.2.10-1.w7                        webtatic                     3.1 M
 php72w-common                       x86_64                     7.2.10-1.w7                        webtatic                     1.3 M
 php72w-devel                        x86_64                     7.2.10-1.w7                        webtatic                     2.8 M
 php72w-embedded                     x86_64                     7.2.10-1.w7                        webtatic                     1.5 M
 php72w-fpm                          x86_64                     7.2.10-1.w7                        webtatic                     1.6 M
 php72w-gd                           x86_64                     7.2.10-1.w7                        webtatic                     140 k
 php72w-mbstring                     x86_64                     7.2.10-1.w7                        webtatic                     586 k
 php72w-mysqlnd                      x86_64                     7.2.10-1.w7                        webtatic                     198 k
 php72w-opcache                      x86_64                     7.2.10-1.w7                        webtatic                     244 k
 php72w-pdo                          x86_64                     7.2.10-1.w7                        webtatic                      89 k
 php72w-xml                          x86_64                     7.2.10-1.w7                        webtatic                     123 k
为依赖而安装:
 libargon2                           x86_64                     20161029-2.el7                     epel                          23 k

事务概要
======================================================================================================================================
安装  12 软件包 (+1 依赖软件包)

总下载量：15 M
安装大小：65 M
Downloading packages:
(1/13): libargon2-20161029-2.el7.x86_64.rpm                                                                    |  23 kB  00:00:02     
警告：/var/cache/yum/x86_64/7/webtatic/packages/php72w-common-7.2.10-1.w7.x86_64.rpm: 头V4 RSA/SHA1 Signature, 密钥 ID 62e74ca5: NOKEY
php72w-common-7.2.10-1.w7.x86_64.rpm 的公钥尚未安装
(2/13): php72w-common-7.2.10-1.w7.x86_64.rpm                                                                   | 1.3 MB  00:00:09     
(3/13): php72w-embedded-7.2.10-1.w7.x86_64.rpm                                                                 | 1.5 MB  00:00:09     
(4/13): mod_php72w-7.2.10-1.w7.x86_64.rpm                                                                      | 3.1 MB  00:00:20     
(5/13): php72w-devel-7.2.10-1.w7.x86_64.rpm                                                                    | 2.8 MB  00:00:32     
(6/13): php72w-gd-7.2.10-1.w7.x86_64.rpm                                                                       | 140 kB  00:00:00     
(7/13): php72w-opcache-7.2.10-1.w7.x86_64.rpm                                                                  | 244 kB  00:00:00     
(8/13): php72w-pdo-7.2.10-1.w7.x86_64.rpm                                                                      |  89 kB  00:00:00     
(9/13): php72w-mbstring-7.2.10-1.w7.x86_64.rpm                                                                 | 586 kB  00:00:01     
(10/13): php72w-mysqlnd-7.2.10-1.w7.x86_64.rpm                                                                 | 198 kB  00:00:01     
(11/13): php72w-xml-7.2.10-1.w7.x86_64.rpm                                                                     | 123 kB  00:00:00     
(12/13): php72w-cli-7.2.10-1.w7.x86_64.rpm                                                                     | 3.1 MB  00:00:45     
(13/13): php72w-fpm-7.2.10-1.w7.x86_64.rpm                                                                     | 1.6 MB  00:00:23     
--------------------------------------------------------------------------------------------------------------------------------------
总计                                                                                                  270 kB/s |  15 MB  00:00:55     
从 file:///etc/pki/rpm-gpg/RPM-GPG-KEY-webtatic-el7 检索密钥
导入 GPG key 0x62E74CA5:
 用户ID     : "Webtatic EL7 <rpms@webtatic.com>"
 指纹       : 830d b159 6d9b 9b01 99dc 24a3 e87f d236 62e7 4ca5
 软件包     : webtatic-release-7-3.noarch (installed)
 来自       : /etc/pki/rpm-gpg/RPM-GPG-KEY-webtatic-el7
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在安装    : php72w-common-7.2.10-1.w7.x86_64                                                                                 1/13 
  正在安装    : libargon2-20161029-2.el7.x86_64                                                                                  2/13 
  正在安装    : php72w-cli-7.2.10-1.w7.x86_64                                                                                    3/13 
  正在安装    : php72w-pdo-7.2.10-1.w7.x86_64                                                                                    4/13 
  正在安装    : php72w-mysqlnd-7.2.10-1.w7.x86_64                                                                                5/13 
  正在安装    : php72w-devel-7.2.10-1.w7.x86_64                                                                                  6/13 
  正在安装    : php72w-fpm-7.2.10-1.w7.x86_64                                                                                    7/13 
  正在安装    : php72w-embedded-7.2.10-1.w7.x86_64                                                                               8/13 
  正在安装    : mod_php72w-7.2.10-1.w7.x86_64                                                                                    9/13 
  正在安装    : php72w-xml-7.2.10-1.w7.x86_64                                                                                   10/13 
  正在安装    : php72w-opcache-7.2.10-1.w7.x86_64                                                                               11/13 
  正在安装    : php72w-mbstring-7.2.10-1.w7.x86_64                                                                              12/13 
  正在安装    : php72w-gd-7.2.10-1.w7.x86_64                                                                                    13/13 
  验证中      : php72w-fpm-7.2.10-1.w7.x86_64                                                                                    1/13 
  验证中      : php72w-embedded-7.2.10-1.w7.x86_64                                                                               2/13 
  验证中      : php72w-devel-7.2.10-1.w7.x86_64                                                                                  3/13 
  验证中      : php72w-xml-7.2.10-1.w7.x86_64                                                                                    4/13 
  验证中      : php72w-pdo-7.2.10-1.w7.x86_64                                                                                    5/13 
  验证中      : mod_php72w-7.2.10-1.w7.x86_64                                                                                    6/13 
  验证中      : php72w-common-7.2.10-1.w7.x86_64                                                                                 7/13 
  验证中      : libargon2-20161029-2.el7.x86_64                                                                                  8/13 
  验证中      : php72w-mysqlnd-7.2.10-1.w7.x86_64                                                                                9/13 
  验证中      : php72w-cli-7.2.10-1.w7.x86_64                                                                                   10/13 
  验证中      : php72w-opcache-7.2.10-1.w7.x86_64                                                                               11/13 
  验证中      : php72w-mbstring-7.2.10-1.w7.x86_64                                                                              12/13 
  验证中      : php72w-gd-7.2.10-1.w7.x86_64                                                                                    13/13 

已安装:
  mod_php72w.x86_64 0:7.2.10-1.w7             php72w-cli.x86_64 0:7.2.10-1.w7              php72w-common.x86_64 0:7.2.10-1.w7         
  php72w-devel.x86_64 0:7.2.10-1.w7           php72w-embedded.x86_64 0:7.2.10-1.w7         php72w-fpm.x86_64 0:7.2.10-1.w7            
  php72w-gd.x86_64 0:7.2.10-1.w7              php72w-mbstring.x86_64 0:7.2.10-1.w7         php72w-mysqlnd.x86_64 0:7.2.10-1.w7        
  php72w-opcache.x86_64 0:7.2.10-1.w7         php72w-pdo.x86_64 0:7.2.10-1.w7              php72w-xml.x86_64 0:7.2.10-1.w7            

作为依赖被安装:
  libargon2.x86_64 0:20161029-2.el7                                                                                                   

完毕！
~~~

>4.配置nginx

~~~vim
vim /etc/nginx/nginx.conf
user                nginx nginx;
worker_processes    16;

error_log           /opt/nginx/logs/nginx_error.log crit;
pid                 /opt/nginx/logs/nginx.pid;
worker_rlimit_nofile 655350;

events {
    use                 epoll;
    worker_connections  655350;
}

http {
    include                 mime.types;
    default_type        application/octet-stream;
    charset         utf-8;
    server_tokens       off;

    server_names_hash_bucket_size   128;
    client_header_buffer_size       32k;
    large_client_header_buffers     4 32k;
    client_max_body_size            8m;

    sendfile                on;
user                nginx nginx;
worker_processes    16;

error_log           /opt/nginx/logs/nginx_error.log crit;
pid                 /opt/nginx/logs/nginx.pid;
user                nginx nginx;
worker_processes    16;

error_log           /opt/nginx/logs/nginx_error.log crit;
pid                 /opt/nginx/logs/nginx.pid;
worker_rlimit_nofile 655350;

events {
    use                 epoll;
    worker_connections  655350;
}

http {
    include                 mime.types;
    default_type        application/octet-stream;
    charset         utf-8;
    server_tokens       off;

    server_names_hash_bucket_size   128;
    client_header_buffer_size       32k;
    large_client_header_buffers     4 32k;
    client_max_body_size            8m;

    sendfile                on;
    tcp_nopush              on;
    keepalive_timeout       60;
    tcp_nodelay             on;

    proxy_connect_timeout   5;
    proxy_read_timeout      60;
    proxy_send_timeout      5;
    proxy_buffer_size       32k;
    proxy_buffers           4 64k;
    proxy_busy_buffers_size 128k;
    proxy_temp_file_write_size 128k;

    gzip                on;
    gzip_min_length     1k;
    gzip_buffers        4 16k;
    gzip_http_version   1.0;
    gzip_comp_level     2;
    gzip_types          text/plain application/x-javascript text/css application/xml;
    gzip_vary           on;

    proxy_temp_path     /opt/nginx/cache_temp;
    proxy_cache_path    /opt/nginx/cache_dir levels=1:2 keys_zone=cache_one:200m inactive=1d max_size=30g;

    include     /opt/nginx/conf/vhosts/*.conf;

    log_format  access  '$remote_addr - $remote_user [$time_local] "$request" '
                        '$status $body_bytes_sent "$http_referer" '
                        '"$http_user_agent" $http_x_forwarded_for';
    access_log  logs/access.log  access;
}
server {
        listen 80;
        server_name loclhost;

        root   html;
        index  index.html index.htm index.php;
        location ~ \.php$ {
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  /opt/html/php$fastcgi_script_name;
            include        fastcgi_params;
        }
        access_log  logs/php.log;
        }
include /opt/nginx/config.d/*; 
~~~

重启nginx

~~~restart
nginx –s reload
~~~

##配置测试页面

~~~echo
mkdir -p /var/www/html/
vim /var/www/html/index.php
<?php
phpinfo();
?>
~~~

重启nginx
~~~restart
nginx -s reload
~~~

~~~netstat
netstat -anput
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      1600/nginx: master
~~~

重启mysql

~~~restart
systemctl restat mariadb
~~~

~~~netstat
netstat -anput
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      1600/nginx: master  
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      964/sshd            
tcp        0      0 0.0.0.0:3306            0.0.0.0:*               LISTEN      3411/mysqld   
~~~

重启php-fpm

~~~restart
php-fpm
~~~

~~~netstat
netstat -anput
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      1600/nginx: master  
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      964/sshd            
tcp        0      0 127.0.0.1:9000          0.0.0.0:*               LISTEN      3448/php-fpm: maste 
~~~

>测试结果
![](/images/linux/2018-10-16.php.png)

