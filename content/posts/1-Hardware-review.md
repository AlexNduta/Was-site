+++
title = "1 Hardware Review"
date = "2024-12-03T12:42:47+03:00"
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
# DevOps Zero to Hero: # 1 Hardware Review

So, the hardware landed two weeks after the order. The specs were:

* `Lenovo` ThinkSystem SR650 V3 (2U Rack Server)
* `Processor`: Intel Xeon Silver 4410Y (12C 2.0GHz)
* `Memory`: 1 x 64GB DDR5 RDIMM
* `Disk Bay`: 8 SAS/SATA SFF
* `Storage`: 2 x 480GB Read Intensive SATA SSD, 4 x 1.2TB SAS HDD
* `NIC`: 4-port 1GbE RJ45, 2-port 10GbE SFP28
* `Power Supply`: 2 x 1100W Titanium
* `Fans`: 5 x Standard Fans

## Processor
Coming from the world of regular CPUs, you might not be familiar with `Intel Xeon` CPUs. Server CPUs are designed to balance performance and power consumption, striking a delicate balance. They are not as fast as regular CPUs in terms of clock speed, but they are optimized for sustained performance under heavy workloads.

Intel Xeon CPUs are designed with lower clock speeds compared to Intel i-series CPUs, which helps reduce heat generation and improve stability over long periods of time. Despite the lower clock speeds, they are designed for power efficiency, especially during intensive workload operations. The architecture allows them to achieve this.

The CPU in question is a 12-core server-grade processor ideal for enterprise data center environments. The 12 cores make it ideal for multitasking and parallel processing. The 30MB Layer 3 cache improves data retrieval speeds for frequently accessed data. Regular Intel i-series CPUs have a Layer 3 cache that ranges between 3-8MB for i3 to i7, respectively. This clearly shows that the Xeon CPU is not your average CPU.

The CPU architecture is based on `Sapphire Rapids` architecture, which enhances performance and efficiency compared to previous generations. Sapphire Rapids is the fourth generation of Intel Xeon Scalable processors and is based on the Golden Cove microarchitecture.

The CPU supports DDR5-4000 memory with a maximum of 4TB across 8 channels, facilitating high-speed data access and processing. With such killer features, the beast is optimized for workloads such as virtualization, data analytics, and general server functions like web hosting and file serving.

## Memory
With 64GB of memory, I have more than enough for my workloads. My plan is to run Windows Server as the main VM, a couple of Linux VMs for system hosting, and a ton of containers. This is more memory than I need.

## Storage
This was the hardest decision. With two 480GB SSDs as primary drives, I was stuck between using RAID 0 and RAID 1 for my setup. For the four 1.2TB SAS HDDs, I also had to do a lot of research to settle on the ideal drive setup. I'll write a comprehensive article on RAID and ZFS and how I arrived at the final decision.

## Network Interface Cards
My main intent is to create an aggregated link for redundancy, and having two 10GbE SFPs is the ideal solution. I still need to figure out how to pass multiple VLANs through the two aggregated links, as my Windows server will require multiple VLANs. My network architecture is subdivided into multiple VLANs, and Active Directory needs to serve each VLAN. Four 1GbE Ethernet ports are more than enough but I have future use cases in mind.

The setup was assembled at the freight station as an after-sales service, which really ruined the fun of figuring out where each component fits. This still saved me time and allowed me to focus on getting the system up and running. The following series of articles will build upon this one as I share my learning and re-learning in the process of getting the system fully operational.

