+++
title = "How to use Nginx as a Loadbalancer"
date = "2025-10-07T12:42:47+03:00"
#dateFormat = "2006-01-02" # This value can be configured for per-post date formatting
author = "Alex Kinyanjui"
authorTwitter = "" #do not include @
cover = ""
tags = ["Nginx", "Webserver", "SSL", "HTTP"]
keywords = ["Nginx", "Webserver", "SSL", "HTTP", "Loadbalancer"]
description = ""
showFullContent = false
readingTime = false
hideComments = false
+++

# What is a Loadbalancer?
In situations where we have a single server and multiple clients, our server can be overwhelmed making it unable to serve webpages as expected. In a technical fashion, its is recommended to have multiple servers to distribute the request load. Having multiple severs is good but its all useless unless there's a tool that handles how the logic of how the load is balanced through the servers.
A load balancer basically accepts request form users on behalf of severs and then routes traffic to the servers depending on the algorithm used. The algorithm could require the loadbalancer to either forward traffic to a server that's alive or just any sever. The main goal is to recduce the load on the severs. This reduces cases such as DDOS attacks as no single sever will have to handle all traffic.
As discussed earlie, nginx can also be used as a Loadbalancer

## What are the benefits of a load balancer
1. Improved perfomance due to traffic being evenly distributed across the specified server
2. Scalabioity: Allows you to scale horizontaly where instead of getting more powerful servers, you can get simple servers
3. High Availability is achieved as traffic is well distributed, if one of the cluster servers is out, it does not affect  the main application
4. i


## Scenario
I have 3 app servers with the hostnames as follows:
    - server stapp01.stratos.xfusioncorp.com
    - server stapp02.stratos.xfusioncorp.com
    - server stapp03.stratos.xfusioncorp.com

I also have another server that I intend on using it as a loadbalancer. The end goal is enable by loadbalancer to takeup traffic from users and distribute it across my 3 servers.

## Solution
1. install nginx in the server we intend to use as the load balanacer
`sudo dnf install nginx`
2. Access the configuration file in the path `/et/nginx/nginx.conf` and configure it to be used as a loadbalancer
3. edit the config file to this
```sh

http {
upstream app_server {
    server stapp01.stratos.xfusioncorp.com:8087;
    server stapp02.stratos.xfusioncorp.com:8087;
    server stapp03.stratos.xfusioncorp.com:8087;
}

server {
    listen 80;

    server_name _;

    access_log /var/log/nginx/access.log;

    location / {
        proxy_pass http://app_server;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
**breadkdown**
`http{}` is the main container for the nginx webserver configuration
    - everything related to serving HTTP trafic goes here
`upstream app_server{}` defines a server group called *app_server*. This is basically our cluster 
> the `server` defines every speciifc server name and their serving port

`server{..}` Defines the virtual host or server instance
`access_log` specifies where the all successful logins will be logged
`location \` This catches all trafic on the url *\*
`proxy_pass` specifies where to send the data when its received and in this context, its sent over to the upstream group `app_server`

Now, with the above setup and a simple webserver serving the default 'welcome' webpage running on our servers, anytime we hit the Loadbalancer, it will direct traffic to either of the three servers
