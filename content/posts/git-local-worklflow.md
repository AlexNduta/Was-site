+++
title = "Git Local workflow"
date = "2025-10-24T12:42:47+03:00"
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


When most people talk of `Git`, there is usualy that correlation between git and Github. What most do not understand is that git is a local version control system.
Github just extends it functionality by giving us a remote cloud-based source where we can store our version controlled files. This creates a resilient setup epecially in a situation where your computer mulfunctions, your files are safe because they are saved in the cloud. 
But did you know you can have a full local git workflow that does not involve Guthub? this was the main intention of git before cloud providers came into the picture. Let me put you upto speed on the setup

# Bare and non-bare reposository
For a directory to be tracked by git, it needs to be initialised, we can either initialise it as a `Bare`  or `Non-bare` repository.

**Non-Bare repository**
- This is a directory with a `.git` directory
- This is just a normal repository that the average user uses for their day-to-day workflow
- It is created by running the command `git init`

**Bare workflow**
- This is a priviledged directory
- It is created using the command `git init --bare`
- It does not have a working directory meaning that it does not have the `.git` folder, the contents(`objects`, `refs`, `HEAD`, `config`) of the directory git  are placed directly in the main folder that ends with `.git` like `myproject.git`
- You never edit files directly in a bare repository. Its main purpose is to receive pushes from developers and serve as a central source for others to pull from.
- cloud version control systems like Github store the bare repository and when clone the repository, we get the regular repository.

This basicly means that we have a server-client relationship between the two repository. The non-bare repository can be clonned multiple times and this is where all developers commit their work then commit all their work to the central server (bare repository)



