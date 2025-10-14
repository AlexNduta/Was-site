+++
title = "Building A LAMP"
date = "2025-10-14T12:42:47+03:00"
#dateFormat = "2006-01-02" # This value can be configured for per-post date formatting
author = "Alex Kinyanjui"
authorTwitter = "" #do not include @
cover = ""
tags = ["LAMP", "Linux", "Apache", "DevOps", "SysAdmin"]
keywords = ["LAMP", "Linux", "Apache", "DevOps", "SysAdmin"]
description = ""
showFullContent = false
readingTime = false
hideComments = false
+++

# LAMP
LAMP stands for Linux, Apache, Mysql, Php

# Scenario

    >X-FusionCorp Industries is planning to host a WordPress website on their infra in Stratos Datacenter.
    They have already done infrastructure configuration—for example, 
    on the storage server they already have a shared directory /vaw/www/html that is mounted on each app host under /var/www/html directory. 
    Please perform the following steps to accomplish the task:



    a. Install httpd, php and its dependencies on all app hosts.


    b. Apache should serve on port 6200 within the apps.


    c. Install/Configure MariaDB server on DB Server.


    d. Create a database named kodekloud_db5 and create a database user named kodekloud_top identified as password YchZHRcLkL. 
    Further make sure this newly created user is able to perform all operation on the database you created.


    e. Finally you should be able to access the website on LBR link,by clicking on the App button on the top bar. 
    You should see a message like App is able to connect to the database using user kodekloud_top

## solution
*considering that I am working on CentOs, all commands will apply ot all RHEL flavours*
1. install http and mysql in all the servers
```sh
 $ sudo dnf install httpd php php-mysqlnd -y
```

2. change the listening port to 6200 as speciifed
```sh
# /etc/http/conf/httpd.conf
# change the line
Listen 80
# to 
Listen 6200
```
3. Restart the httpd server
```sh
$ systemctl restart http
$ sysstemctl enable htttpd
# confirm which port the httpd server is listening on
$ sudo ss -tunlp | grep -i httpd
```

3. install `mariadb` in the DB Server
```sh
$ sudo dnf install mariadb-sever
# restart the server and make it persistent over reboots
$ systemctl restart mariadb-server
$ systemctl enable mariadb-server
```

4. Create mysql database specified and the user and grant the user all the privileges of our table
```sh
$ mysql
$ CREATE DATABASE kodekloud_db5;
$ CREATE USER 'kodekloud_top'@'%' IDENTIFIED BY 'YchZHRcLkL';
$ GRANT AL PRIVILEGES ON kodekloud_db5.* TO kodekloud_top;
$ FLUSH PRIVILEGES;
$ EXIT;
```

5 Setup a loadbalancer to distribute traffic all through our three servers
```sh
$ sudo dnf install haproxy
$ systemctl restart haproxy
$ systemctl enable haproxy
```

6. setup the config file of the haproxy
```cfg
# /etc/haproxy/haproxy.cfg
global
  maxconn 4096
  pidfile /var/run/haproxy.pid
defaults
  mode http
  retries 3
  option redispatch
  maxconn 2000
  timeout connect 5000
  timeout check 5000
  timeout client 30000
  timeout server 30000

frontend myAppFrontEnd
  bind *:${BIND_PORT}
  use_backend webservers
backend webservers
    balance roundrobin
    server web1 ${WEB_APP1}:${WEB_APP1_PORT} check
    server web2 ${WEB_APP2}:${WEB_APP2_PORT} check
    server web3 ${WEB_APP3}:${WEB_APP3_PORT} check
```

Using the URL of the loadbalancer, we will be able to get contents of each web-server
