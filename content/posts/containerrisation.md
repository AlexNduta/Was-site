+++
title = "Docker: Introduction to containerization"
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

# What is containerization
A `container` is a package that simulates the underlying operating system and is packaged with all the dependecies that your application need to run. Unlike a `Virtual Machine` that simulates and  virtualise the underlying hardware, a container only virtualise the OS. They provide a way to package applications with required dependencies that is seperated from the host OS. This enables the application to run out-of-the-box despite running on a different underlying enviroment other than the one its was compiled on. Containers basically act as lightweight VMs and they share the underlying OS.

Let me explain this in the simplest way possible. A developer builds an application that is supposed to run in an ubuntu 22.04. This basically means that if the developer wants to run the appplication in on another machine, they need to make sure that the target machine has the same operating system and dependencies installed. With the cloud, this issue is easy to reslove because you can get a VM on demand for a specified environment. 
Things get sticky when the application is being built collaboratively and the other developers have a different enviroment(windows is a good example) than the target enviroment. This makes it practically impossible run or test the application as Linux and windows dependencies are not compatible.

With  containerization, the developer will just package the application in a container with all the dependencies required for the app to run alongside the runtime of choice(OS). If they were to share the application/code with another developer, all he/she needs is a container runtime enviroment(`daemon`) and the application will run out-of-the-box.

# Origin of containers
When we mention containers, what comes to mind to most is `Docker`. There are so many types of containers and the best way to learn either of the exhaustive list of types of containers is to understand the underlying technlogy behind. The concept of containerization dates back to `chroot` systemcall in Unix. This syscall allowed processes to change their root directory leading to isolation from the filesystem. 
### chroot
[Chroot](https://en.wikipedia.org/wiki/Chroot) is the first concept that led to the conception of container technology. It is used to change the root directory(/) of the current running process and its child processes.
- Your current working directory could be `/home/myfiles` and using chroot we can restrict the process we are running to that directory.
- The change creates a restricted enviroment referred to as a `chroot jail` or `jailed directory` where the processes are limited to accessing only files within the new directory.
- The tools is useful for:
	- `Creating a test enviroment`: Developers can create an isolated test environment  for softwares without damaging the main system.
	- `System Recovery` Chroot can be used to repair a broken system such a repairing file systems or password recovery.
	- `Reinstalling the Bootloader`: If the bootloader is corrupted, chroot allows you to access the system enviroment to reinstall or repair it without affecting the main root system.
```sh
# Chroot syntax
$ chroot /path-to-new-root-dir  command-to-jail

# alternative
$ chroot /path-to-new-root-dir /path-to-server-process
```
The syntax above shows the basic usage of the chroot command in any Unix or Unix-like systems. We can also use comand options or flags.
The concept of process isolation is heavily used in containerization. In 2000 FreeBSD introduced `jails` that was a build up of chroot and allowed for advanced isolation by allowing multiple indipendent enviroments on a single system complete with  their own user and network configurations.
These concepts brought about the idea of process isolation but was not the full blown idea of containerization

### Cgroups
A [Control group](https://en.wikipedia.org/wiki/Cgroups) is a Linux feature that limits, accounts for and isolates resource usage of a single or collection of processes. Some of the resouces include, CPU, memory, network, disk I/O etc..
Control groups are used to control how much a given resouce can be used by a process or set of processes.They are the key componet in a container because there are multiple processes running in a container and the usage of each has to be curbed to avoid resource over usage by a process. 
They enable fine-grained control over system reource allocation for groups and processes

### NameSpaces
[Namespaces](https://en.wikipedia.org/wiki/Namespace) are a feature found in Linux that offer isolation of system resources to different specified processes. They give an illussion that a process has its private instance of a resource. This could be resources such as filesystems, processID and network intefaces. 

There are multiple types of Namespaces:
Note: for better understanding, in the explanations below, we will use room as a representative of an isolated enviroment with dedicated resources.
1.`Process ID namespace` 
sets a process ID to processes for identification and makes sure that a process in one 'room' does not know about another.
2.`Unix Timesharing system(UTS) Namespace`
Isolates hostnames and domain names. allow each 'room' to have its own hostname and domain name and the hostname can be renamed without affecting the host OS
3.`Inter-process Communication (IPC) Namespace`
Seperates shared resources such asshared memory and message queues and ensure that such resources are not accessible to other rooms.
4.`Network Namespace`
Isolates network interphases, Internet protocol address(IP) and ensures that each room has its own network configurations.
5.`User namepaces`
Isolates users and group ID's. A process can have root priviledges in one room but not in the main host OS
6. `Cgroup Namespace`
Makes a 'room' 'think' it has its own set of resources by isolating its own set of reources.


Together, namespaces and Cgroups form the backbone of container technology, enabling secure and efficient application deployment in modern cloud environments.
Some examples of container technology is Docker, Kubernetes, Podman, and LXD.
