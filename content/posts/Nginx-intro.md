+++
title = "What is Nginx?"
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

# What is Nginx?

This is a software used to 'serve' web content to our browser. It is an intermediary between a client and a server. In this instance, it acts as a reverse proxy where it receives client's requests on behalf of a server.
Nginx can either be used as:
    - HTTP Server
    - Revers proxy
    - Mail Proxy
    - Generic TCP/UDP proxy server
Feautures in Nginx are activated using modules. This feature allows admins to only use the features they need. This means that you could install nginx and only use it as a HTTP server only

## What are the advantages of using Nginx?
1. Speed
2. Loadbalancing -> can be used to distribute traffic to backend service. This comes as an advantage to prevent DDOS attacks or overwhelming one server
3. Nginx has the ability to run on very cheap hardware
4. nginx has the ability to update without interrupting running workloads
5. Very easy to maintain and install

