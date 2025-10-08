+++
title = "Using Nginx as a Web server"
date = "2025-10-07T12:42:47+03:00"
#dateFormat = "2006-01-02" # This value can be configured for per-post date formatting
author = "Alex Kinyanjui"
authorTwitter = "" #do not include @
cover = ""
tags = ["Nginx", "Webserver", "SSL", "HTTP"]
keywords = ["Nginx", "Webserver", "SSL", "HTTP"]
description = ""
showFullContent = false
readingTime = false
hideComments = false
+++

A mentioned earlier, Nginx can be used  in a number of ways. One of them is as a webserver.
A webserver is a software that sends webpages stored in webservers to users on request. By default, the web uses HTTP protocol on port 80 which sends data in unencrypted format. Nginx supports both HTTP and HTTPS meaning that we can send data securely.
Nginx configuration files are found in the `/etc/nginx` directory. We majourly make changes to the `sites-available` directory which can be structured to house multiple sites each described as a configuration file with the `.conf` extension
After creating a site configuration file, we are supposed to publish the site to the `sites-enabled`. This is done by creating a symbolic link with the command
`ln -s ~/etc/nginx/sites-available/default ~/etc/nginx/sites-enabled/default`
The command above basically links the `site-enbaled/default` and for the changes to take place, we need to restart nginx using the command `sudo nginx -s reload`


# Connfigurations
By default, nginx uses one configuration file `/etc/nginx/nginx.conf` especially if nginx is installed with a package manager. We put our custom configurations in the `/etc/nginx/sites-available` and they can only be served if they are linked with the `sites-enables`directory

** example of an Nginx config file**
```sh

http {

    ##
    # Basic Settings
    ##

    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;
    # server_tokens off;

    # server_names_hash_bucket_size 64;
    # server_name_in_redirect off;

    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    ##
    # SSL Settings
    ##

    ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # Dropping SSLv3, ref: POODLE
    ssl_prefer_server_ciphers on;

    ##
    # Logging Settings
    ##

    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    ##
    # Gzip Settings
    ##

    gzip on;

    # gzip_vary on;
    # gzip_proxied any;
    # gzip_comp_level 6;
    # gzip_buffers 16 8k;
    # gzip_http_version 1.1;
    # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

    ##
    # Virtual Host Configs
    ##

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```
- The key-pairs above are called `directives` and directives always end with a `;`
- Each directive has a specific meaning and defines a feature in context of an application
- The ones ones the the braces `{}` are called `contexts`. Contexts also contains directives
- everything with a `$` is a varriable we can access in the config file
- the `include` statement is used to import another config file that build up on the config file

**Directive Blocks**
- They are enclosed in a module
- The root config is called the `main block`
- Blocks can be nested together to build up on a logic

```sh
http{
    server{
    listen 80;
    server_name example.com;
    access_log /var/log/nginx/example.com.log;
    location ^~/admin{
        index index.php;
    }
 	}
}
```
- I the example above, `http` is the main block that houses the `server` block
- The `server` block contains configurations for the *example.com* website
- The `acces_log` directive givses us a location where to store our logs


**`Nginx Configuration example`**
```sh
#/etc/nginx/conf.d/example.com
  1 server{
  2     listen 80;
  3     server_name example.com www.example.com;
  4     root /var/www/example.com/html;
  5. }
```

- The example above will is a configuaration of our site names `example.com` that will be served on port 80
- The `root` directive specifies where to file the files to serve

