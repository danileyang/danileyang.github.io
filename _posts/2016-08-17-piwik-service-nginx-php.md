---
layout: post
title: piwik
date: 2016-08-17 10:13:26
disqus: y
---
##写在开始
&emsp;&emsp;**piwik**，是一个开源的应用统计程序，其基于php&mysql但并不限于mysql，用户可以指定数据存储，当然如果你是高级用户也可以享受数据云端存储。
\n
<p></p>
&emsp;&emsp;那么piwik能做什么呢？它可以统计网页浏览人数，访问最多的页面，甚至每次访问的时间，分块的用户事件，搜索引擎关键词等等
<p><font color=red>&emsp;&emsp;更重要的是</font>通过自己搭建piwik服务器，统计的元数据可以自己存放，而不是仅仅得到一个统计报表。对于希望利用原始统计数据进行用户跟踪，用户行为分析等的开发者来说，这无疑是非常重要的部分。</p>
<p></p>
&emsp;&emsp;*我们将从系统搭建开始一步一步记录piwik服务器搭建，piwik系统安装，统计代码以及业务数据分析的整个过程。那么还等什么*
<p></p>
##环境准备
<p>&emsp;&emsp;piwik是基于php的，那么我们需要一台可以运行php的服务器，笔者特地花了108大洋在阿里云购买了一台ECS(单核)。需要准备的环境包括nginx+php+mysql</p>
####nginx安装
<p>服务器系统为centos6.5，因为nginx等包需要添加额外的源，所以我们可以先进行添加</p>
```
1. vim /etc/yum.repos.d/nginx.repo
2.添加源：
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=0
enabled=1
3.yum install nginx //进行nginx安装
```
安装好以后可以进行服务的启动和停止
<p></p>
```
service nginx start //启动
service nginx stop //停止
service nginx -s reload //重启
```
####php安装
<p>添加remi源</p>
```
rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
```
安装php
<p></p>
```
yum --enablerepo=remi,remi-php55 install php-fpm php-common php-devel php-mysqlnd php-mbstring php-mcrypt
```
安装过程中可能会遇到各种包缺少的情况，可以通过搜索相关yum进行安装，安装好后即可对服务进行相关操作了
<p></p>
```
php -v //查看Php版本
service php-fpm start //启动php
service php-fpm stop //停止php
```
####mysql安装
&emsp;&emsp;由于笔者在使用阿里云的RDS，因此并不涉及mysql的安装过程，入需要请yum自行安装
####nginx-php配置
<p>&emsp;&emsp;nginx和php都已经准备就绪，那么我们需要nginx和php配合起来工作，nginx的配置</p>
```
user  admin;
worker_processes  1;

error_log  /home/admin/logs/nginx/error.log;

pid       /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {

    include       mime.types;
    default_type  application/octet-stream;

    log_format timed_combined '$http_host - $remote_addr [$time_local] '
                              '"$request" $status $body_bytes_sent '
   		              '"$http_referer" "$http_user_agent" '
	    		      '$request_time $upstream_response_time $http_x_forwarded_for';

    access_log /home/admin/logs/nginx/access.log timed_combined;

    sendfile        on;
    tcp_nopush      on;

    keepalive_timeout  0;

    server {
        listen 80;
        server_name youdomainname;
	location ~ \.php$ {
    		root           /usr/share/nginx/html;
    		fastcgi_pass   127.0.0.1:9000;
    		fastcgi_index  index.php;
    		fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
		include        fastcgi_params;
	}
}


server {
        listen 80;
		server_name youapachedomainname;

        location / {
                proxy_pass http://127.0.0.1:8080;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
                proxy_set_header   X-Forwarded-proto https;
        }
	}

}
```
如上配置，php默认服务端口为9000未做修改，php根目录指定为html目录。
<p></p>
那么现在我们可以写一个简单的php页面来测试一下nginx->php的转发服务了
<p></p>
```
cd /usr/local/ngnix/html

echo "<?php phpinfo();" > index.php
```
现在请求youdomain/index.php
<p></p>
页面已经正确的解析展现了
<img src="http://7fvcu1.com1.z0.glb.clouddn.com/phpinfo.jpeg" width = "390" height = "500" alt="图片名称" align=center />
至此，nginx+php的环境已经准备就绪，下一篇我们将进行piwik的安装和配置。