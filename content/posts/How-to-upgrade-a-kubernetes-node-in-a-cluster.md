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

+++

##context
we have 3 major nodes running
    - `control`
    - `node0`
    - `node1`
upon running the command `kubectl get nodes`, we realise that the two worker nodes are on kuberntes v1.30 and the controlplan is on kubentes v1.34.
Our main tasks is to upgrade the two nodes. 
*NOTE:* This update procedure only applies to updating a patch version
When upgrading a node in a cluster, you need to work on one node at a time for high availability. This means that systems do not have to go down because of an upgrade.

**step 1: make the node unscheduable**
`kubectl cordon node01`
 This prevents any new nodes from being scheduled on this node as its under maintenance.

**step2: Drain the nodes**
`kubectl drain node01 --ignore-daemonsets`

This evicts all containers scheduled on this specified nodes

**step3: upgrade the kubelet and kubectl**
```sh
# query the database to findout which version is available for update
$ apt-cache show kubelet
$ apt install kubelet=<specified-version> - y

$ apt-cache show kubectl
$ sudo apt install kubectl=<specified-version> -y
```
 we should first upgrade the kubelet and kubectl before we upgrade the whole system

**step4: Upgrade the whole system**
```sh
$ kubeadm upgrade node

# restart the kubelet
$ systemctl restart kubelet
```

**step5: make the node scheduable**
After updating the node, we need to make the node scheduable or else all other pods will not be scheduled on it
``sh
# run this on the control plane
$ kubectl uncordon node01
```
we can test if the node is active by running the `get nodes` command on the control plane. we should see node01's state as `Ready`

