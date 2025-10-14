+++
title = "Install and configure a web application(Apache)"
date = "2025-10-14T12:42:47+03:00"
#dateFormat = "2006-01-02" # This value can be configured for per-post date formatting
author = "Alex Kinyanjui"
authorTwitter = "" #do not include @
cover = ""
tags = ["", ""]
keywords = ["", ""]
description = ""
showFullContent = false
readingTime = false
hideComments = false
+++

## The problem
XCorp industry requires me as the DevOps Engineer to configure a webserver in their Stratos Data Center in  preparation for the website that is still in progress

*Requiremets*
- Install Apache on the specified server
- The website should be served on port `8086`
- Use the provided templates i.e `cluster` and `ecommerce` to server the files 
- The following endpoints should serve the respective files `curl localhost:8086/cluster` and `curl localhost:8086/ecommerce/`

## The solution
1. **Installing Apace**
- considering that we are using a REHL based system, I will use the default package manager to install Apache(httpd)
```sh
# using dnf
sudo dnf install httpd -y

```
By default, the apache service will be disabled and its the best time to make changes to the config file. 
The Apache config file is found in `etc/http/conf/http.conf`. This is consitsent in all linux flavours.

2. **Changing the port to serve**
A port is like a door to a room in a big appartment that we can use to access a specific service.By default Apache uses port 80 to sever web files. from the requiements, port `8086` is what we should be service from
```sh
# /etc/httpd/conf/httpd.conf
# change from this ..
Listen 80

# to This
Listen 8086
```
At this point in time, we can enable the httpd service just to confirm that its using the port we specified
```sh
$ sudo systemctl start httpd
$ sudo systemctl enable httpd
# confirm the port
$ sudo ss -tunlp | grep -i httpd
# the above should return the port we specified


3. **link URL to a file**
Apache has the capablity of linking files to a URL. This enables us to serve contents of the files using the specified endpoint. In this context we have two folders(`cluster` and `ecommerce`) which containing a simple html file.
```sh
# /etc/httpd/conf/http.conf
Alias /cluster/ /var/www/html/cluster/
<Directory /var/www/html/cluster/>
    Require all granted
</Directory>

# Best Practice for a directory alias
Alias /ecommerce/ /var/www/html/ecommerce/
<Directory /var/www/html/ecommerce/>
    Require all granted
</Directory>


```

In the implementation above, we use the `Alias` to create a virtual folder that maps to a real directory(example */var/www/html/cluster/*.
This means that our URL will use the virtual folder specified by the `Alias` to server the files in that folder. We then use the `<Directory>` tag to specify the access permissions to the directory.


*What if we never used the `Alias` tag?*
In such a situation, the `DocumentRoot` would be our saviour.
```sh
# /etc/httpd/conf/httpd.conf

DocumentRoot "/var/html/www/html"

```
Although it looks short, this makes the URL long and hard to remember for clients. In this context, our URL would be `http://localhost/var/www/html/cluster/index.html` so as to server the `index.html file`.


4. **Test the endpoints**
We first need to reload the httpd service for the changes to take effect then test the endpoints

```sh
$ sudo  systemctl restart httpd

# Test the endpoint
$ curl localhost:8086/cluster/
$ curl localhost:8086/ecommerce/
```
The above should return the context of the index,html files in the specified folder.


## conusion
The above task is not just a excercise of installing and typing out configurations in the configuration files.
Its a depiction of one's understanding of how a systems administrator would use a webserver to serve files to a client.

