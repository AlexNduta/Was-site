+++
title = "Intro: DevOps Zero to hero"
date = "2024-11-30T15:31:14+03:00"
#dateFormat = "2006-01-02" # This value can be configured for per-post date formatting
author = "Alex Kinyajui"
authorTwitter = "" #do not include @
cover = ""
tags = [""]
keywords = ["DevOps", "Netwoking", "Proxmox", "DevOps", "Automation"]
description = ""
showFullContent = false
readingTime = false
hideComments = false
+++

# DevOps Zero to hero

This will be a series of articles showcasing the skills I pick up along the way.

### Backstory
I've been a Network Support Technician for two years and recently got promoted to Network Security Engineer at the same organization. This role is more of a hybrid, as I'm responsible for the institution's IT infrastructure.

Our current setup is quite complex, and I'll draw an architectural diagram in future articles to illustrate the issues. Our infrastructure has many unnecessary connections and devices. For example, we host a Windows server as a VM on one PC and our Unifi controller on another.

My primary goal in this role is to simplify the setup and eliminate unnecessary connections, which pose security risks. The first step was to consolidate the server setup onto a single server for easier management and configuration. This led me to request a Lenovo SR650 V3 server.

While many would suggest migrating to the cloud and using EC2 instances, compliance restrictions require us to host and access all security devices within the organization's boundaries. I'm the only person authorized to access these systems remotely, and even that requires rigorous testing.

The main purpose of this blog is to document my learning journey as I set up the servers, make mistakes, and learn from them. I'm preparing for a DevOps role, so I'll document everything from hardware setup to using Terraform for VM and container deployment, as well as other DevOps tools. I'll also provide in-depth tutorials on concepts I learn along the way, which will serve as future reference points.

[Hardware Review](https://alexnduta.netlify.app/posts/1-hardware-review/)
