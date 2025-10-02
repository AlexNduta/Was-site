+++
title = "What are iptables?"
date = "2024-10-01T12:42:47+03:00"
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

- This is the linux packet filtering tool baked-into linux systems
- This is the userspace module for managing `Netfilter`
- Acts as a firewall filtering packets at the kernel level by using rules to filter packets
- Rules defined determine how network traffic is handled by the system


  ## Structure
- `Tables`-
  	- These are collection of chains that perform a specific function.
  	- Most commontables include `filter` , `nat`, `mangle`
- `chains`
	- These are a list of rules that packets are checked against
	- The filter table has three default chains
		- `INPUT`: For packets destined for local packets. packets coming into the server
		- `OUTPUT`:  For localy generated packtes. packets leaving the server
		- `FORWARD`: For packets being routed through the server to another destination
- `Rules`
	- Each rule withing a chain specifies a set of criteria for matching a packet
	- The criteria could be the source and destination IP address, protocol or port
- `Targets`
	- if a packet matches a rule, the rule target determines the action to be taken
	- they can either be:
		- `ACCEPT`: Allow packets to proceed
		- `REJECT`: Discard a packet and send error messages back to the sender
		- `DROP`: Silently discard the packets
	 
## How Ip tables work
- when a packet enters or leaves a Linux system, the Netfilter framework in the kernel processes it by following the steps below:
  > The packet is run through the rules in the rlevant table chain
  > Each rule is checked in the order it appears in the chain
  > if apacket matches a rule, the corresponding target action is executed, this could be accept, drop or reject
  > If the packet makes it to the end of a chain without matching any rule, the default rule is applied
  
# unblock a specific port
```sh
$ sudo iptables -I INPUT 2 -p tcp --dport <specific-port> -j ACCEPT
```
- The comand above adds an entry that allows all incoming traffic from the `specified-pot` and adds the rule in the `INPUT` chain on line number 2
- Being at the very top means that its treated with priority making it override all other rules below it


# unblock a specif port for a specified target IP address
```sh
dnf install iptables-services -y

systemctl enable iptables.service
systemctl start iptables.service

iptables -t filter -I INPUT 1 -p tcp --dport 3003 -s 172.16.238.14 -j ACCEPT 
service iptables save
```
- from the above example, we allow traffic from the specified source address on the specif port `3003`
- From the specified sever, we can test the connection using `telnet IP-address port`
