---
layout: post
title: '安装部署堡垒机-Jumpserver'
tags: [linux]
---
## 准备安装环境
>1.关闭安装机制

~~~bash
setenforce 0
~~~
>2.查看系统字符集是否是utf-8

~~~bash
locale
~~~

~~~cp
LANG=zh_CN.UTF-8
LC_CTYPE="zh_CN.UTF-8"
LC_NUMERIC="zh_CN.UTF-8"
LC_TIME="zh_CN.UTF-8"
LC_COLLATE="zh_CN.UTF-8"
LC_MONETARY="zh_CN.UTF-8"
LC_MESSAGES="zh_CN.UTF-8"
LC_PAPER="zh_CN.UTF-8"
LC_NAME="zh_CN.UTF-8"
LC_ADDRESS="zh_CN.UTF-8"
LC_TELEPHONE="zh_CN.UTF-8"
LC_MEASUREMENT="zh_CN.UTF-8"
LC_IDENTIFICATION="zh_CN.UTF-8"
LC_ALL=
不是utf-8修改方法如下
localedef -c -f UTF-8 -i zh_CN zh_CN.UTF-8
export LC_ALL=zh_CN.UTF-8
echo 'LANG="zh_CN.UTF-8"' > /etc/locale.conf
~~~
## 准备python3和python3虚拟环境
>1.安装依赖包

~~~yum
yum -y install wget sqlite-devel xz gcc automake zlib-devel openssl-devel epel-release git
~~~
> 2. 安装python3

~~~wget
wget https://www.python.org/ftp/python/3.6.1/Python-3.6.1.tar.xz
tar xvf Python-3.6.1.tar.xz  && cd Python-3.6.1
./configure && make && make install
~~~
> 3. 创建python3虚拟环境
因为 CentOS 6/7 自带的是 Python2，而 Yum 等工具依赖原来的 Python，为了不扰乱原来的环境我们来使用 Python 虚拟环境

~~~cd
cd /opt && python3 -m venv py3 && source /opt/py3/bin/activate
# 看到下面的提示符代表成功，以后运行 Jumpserver 都要先运行以上 source 命令，以下所有命令均在该虚拟环境中运行
(py3) [root@localhost py3]
~~~
> 4. 自动载入python虚拟环境配置

~~~cd
cd /opt && git clone https://github.com/kennethreitz/autoenv.git 
echo 'source /opt/autoenv/activate.sh' >> ~/.bashrc && source ~/.bashrc
~~~
##安装Jumpserver
>1.下载或clone项目
项目提交较多 git clone 时较大，你可以选择去 Github 项目页面直接下载zip包。

~~~git
cd /opt &&  git clone https://github.com/jumpserver/jumpserver.git && cd jumpserver && git checkout master
echo "source /opt/py3/bin/activate" > /opt/jumpserver/.env  # 进入 jumpserver 目录时将自动载入 python 虚拟环境
~~~
>2. 安装依赖RPM包

~~~yum
cd /opt/jumpserver/requirements # 首次进入 jumpserver 文件夹会有提示，按 y 即可 # Are you sure you want to allow this? (y/N) y
yum -y install $(cat rpm_requirements.txt)  # 如果没有任何报错请继续
~~~
>3. 安装python依赖

~~~pip
pip install -r requirements.txt
如出现一下错误
You are using pip version 9.0.1, however version 18.1 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
升级pip
pip install -U pip
pip –version
pip 18.1 from /opt/py3/lib/python3.6/site-packages/pip (python 3.6)
~~~
>4. 安装 Redis, Jumpserver 使用 Redis 做 cache 和 celery broke

~~~yum
yum -y install redis && systemctl enable redis && systemctl start redis
~~~
>5. 安装MySQL

~~~yum
yum -y install mariadb mariadb-devel mariadb-server
systemctl enable mariadb && systemctl start mariadb
~~~
>6. 创建数据库 Jumpserver 并授权

~~~mysql
mysql && create database jumpserver default charset 'utf8';
grant all on jumpserver.* to 'jumpserver'@'127.0.0.1' identified by '666666';
flush privileges;
~~~
>7. 修改 Jumpserver 配置文件

~~~vim
cd /opt/jumpserver && cp config_example.py config.py
vi config.py
注意对齐，修改内容如下: 
# DB_ENGINE = 'sqlite3'
# DB_NAME = os.path.join(BASE_DIR, 'data', 'db.sqlite3')
DB_ENGINE = os.environ.get("DB_ENGINE") or 'mysql'
DB_HOST = os.environ.get("DB_HOST") or '127.0.0.1'
DB_PORT = os.environ.get("DB_PORT") or 3306
DB_USER = os.environ.get("DB_USER") or 'jumpserver'
DB_PASSWORD = os.environ.get("DB_PASSWORD") or '666666'
DB_NAME = os.environ.get("DB_NAME") or 'jumpserver'
~~~
>8. 生成数据库表结构和初始化数据

~~~bash
cd /opt/jumpserver/utils && bash make_migrations.sh
~~~
>9. 运行 Jumpserver

~~~bash
cd /opt/jumpserver ./jms start all # 后台运行使用 -d 参数./jms start all -d
~~~
请浏览器访问 http://192.168.244.144:8080/ 默认账号: admin 密码: admin 页面显示不正常先不用处理，继续往下操作，后面搭建 nginx 代理后即可正常访问，原因是因为 django 无法在非 debug 模式下加载静态资源 

## 安装 SSH Server 和 WebSocket Server: Coco

>1. 下载或 Clone 项目

~~~cd
cd /opt && source /opt/py3/bin/activate
git clone https://github.com/jumpserver/coco.git && cd coco && git checkout master
echo "source /opt/py3/bin/activate" > /opt/coco/.env 
 ~~~
>2. 安装依赖

~~~
首次进入 coco 文件夹会有提示，按 y 即可
cd /opt/coco/requirements && yum -y  install $(cat rpm_requirements.txt)
pip install -r requirements.txt
~~~
>3.  修改配置文件并运行

~~~vim
cd /opt/coco &&  mkdir keys logs && cp conf_example.py conf.py 
vim conf.py
注意对齐,修改如下：
NAME = "coco"
CORE_HOST = 'http://127.0.0.1:8080'
LOG_LEVEL = 'WARN'
启动coco
./cocod start  # 后台运行使用 -d 参数./cocod start -d
~~~
启动成功后去Jumpserver 会话管理-终端管理（http://192.168.244.144:8080/terminal/terminal/）接受coco的注册
![](/images/linux/jumpserver1.png)

>4. 安装 Web Terminal 前端: Luna

~~~wget
cd /opt && wget https://github.com/jumpserver/luna/releases/download/1.4.3/luna.tar.gz
tar xvf luna.tar.gz && chown -R root:root luna
~~~
>5. 安装 Windows 支持组件

因为手动安装 guacamole 组件比较复杂，这里提供打包好的 docker 使用, 启动 guacamole
~~~yum
yum remove docker-latest-logrotate docker-logrotate docker-selinux dockdocker-engine
yum install -y yum-utils device-mapper-persistent-data lvm2
~~~
>>5.1增加docker 阿里云的镜像源

~~~yum
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
rpm --import http://mirrors.aliyun.com/docker-ce/linux/centos/gpg
yum makecache fast && yum -y install docker-ce
systemctl start docker && systemctl status docker
~~~
>>5.2 启动 Guacamole

~~~docker
 docker run --name jms_guacamole -d -p 8081:8080 -v /opt/guacamole/key:/config/guacamole/key -e JUMPSERVER_KEY_DIR=/config/guacamole/key -e JUMPSERVER_SERVER=http://192.168.168.172 jumpserver/guacamole:latest
 ~~~
 启动成功后去Jumpserver 会话管理-终端管理（http://192.168.244.144:8080/terminal/terminal/）接受[Gua]开头的一个注册
![](/images/linux/jumpserver2.png)
##配置 Nginx 整合各组件

>1. 安装nginx

~~~yum
yum -y install nginx
~~~
>2.  准备配置文件 修改 /etc/nginx/conf.d/jumpserver.conf

~~~vim
 vim /etc/nginx/conf.d/jumpserver.conf
 server {
    listen 80;  # 代理端口，以后将通过此端口进行访问，不再通过8080端口
    server_name demo.jumpserver.org;  # 修改成你的域名

    client_max_body_size 100m;  # 录像及文件上传大小限制

    location /luna/ {
        try_files $uri / /index.html;
        alias /opt/luna/;  # luna 路径，如果修改安装目录，此处需要修改
    }

    location /media/ {
        add_header Content-Encoding gzip;
        root /opt/jumpserver/data/;  # 录像位置，如果修改安装目录，此处需要修改
    }

    location /static/ {
        root /opt/jumpserver/data/;  # 静态资源，如果修改安装目录，此处需要修改
    }


    location /coco/ {
        proxy_pass       http://localhost:5000/coco/;  # 如果coco安装在别的服务器，请填写它的ip
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        access_log off;
    }

    location /guacamole/ {
        proxy_pass       http://localhost:8081/;  # 如果guacamole安装在别的服务器，请填写它的ip
        proxy_buffering off;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $http_connection;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        access_log off;
    }

    location / {
        proxy_pass http://localhost:8080;  # 如果jumpserver安装在别的服务器，请填写它的ip
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
~~~
>3. 运行 Nginx

~~~bash
systemctl start nginx && systemctl enable nginx
~~~
>4. 开始使用 Jumpserver

~~~bash
cd /opt/jumpserver && ./jms status 
cd /opt/coco && ./cocod status
docker ps # 检查容器是否已经正常运行
~~~
访问 http://192.168.244.144，访问nginx代理的端口，不要再通过8080端口访问
默认账号: admin 密码: admin
>5. 测试连接
liunx

~~~ssh
ssh -p2222 admin@192.168.168.172
~~~
![](/images/linux/jumpserver3.png)
windows

~~~ssh
ssh -p2222 admin@192.168.168.172
~~~
![](/images/linux/jumpserver4.png)






