+++
title = " How to upgrade a kubernetes node in a cluster"
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

# context
we have 3 major nodes running
    - `control`
    - `node0`
    - `node1`
upon running the command `kubectl get nodes`, we realise that the two worker nodes are on kuberntes v1.30 and the controlplan is on kubentes v1.34.
Our main tasks is to upgrade the two nodes. 

When upgrading a node in a cluster, you need to work on one node at a time for high availability. This means that systems do not have to go down because of an upgrade.

**step 1: drain the target node** 

