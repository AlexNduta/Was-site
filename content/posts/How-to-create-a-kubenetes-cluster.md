+++
title = "How  to create a 3-node kubernetes cluster using kubeadm"
date = "2025-09-03T12:42:47+03:00"
author = "Alex Kinyanjui"
authorTwitter = "" #do not include @
cover = ""
tags = ["k8s", "kubernetes", "kubeadm"]
keywords = ["k8s", "kubead", "pods", "Nodes"]
description = ""
showFullContent = false
readingTime = false
hideComments = false
+++

[setup script](https://github.com/AlexNduta/HomeLab/blob/main/setupscript.sh)
# How  to create a 3-node kubernetes cluster using kubeadm
- In kubernetes, a cluster is basicaly a collection of nodes. We majorly have a `Control plane` that does all the administrative operations and the `Nodes` whichs are responsible for housing the `pods`.



## Control plane
- This is the brains of the whole sytem, processes instrictions and sends them to the nodes via the `kubelet`
- The control plane contains the following:
	- Apiserver
	- Etcd
	- kubelet
	- kube-scheduler
	- kubectl
### Apiserver
- This is the tool that receives requests from users and redirects it to different components within the controlplane
- It acts as a gateway to the outside world and receives requests from the user using the `kubectl` client
- If a request is sent to create a pod `kubectl run nginx --image nginx`, the request is sent to the `kube-scheduler` which comunicates with the `kubelets` to know which node is going to process the workload
- The kubelets are found in every node and is the gateway between the nodes and the control plane
- It keeps regular watch for any instructions coming from the apiserver which is sent as a `podspec`
- The kubelet checks the podspec to see if the pods are in the described state, if not so, it communictaes with the `container runtime` to crate pods so as to meet the desired state.


## The setup
This is a basic setup running on a proxmox server. I have three VMs `control`, `Node0` and `Node1`
the main intention is to create a kubentes cluster


## The process
- There are four phases to this process:
	- **prerequisites**: preparing all of the three vms
	- **Control plane setup**: initialising the cluster brain on the `control` node
	- **worker node setup**: `nodde0` and `node1` joing the cluster
	- **Network Setup**: Installing a network pluging so that the pods in the cluster can communicate

## 1. Prerequisites: This is to be done on all three nodes
1. **Intsall a container runtime**
- k8s never runs containes by itself, it depends on a container runtie to do this. K8s communictates with a container runtime via the  Container Runtime Interface`CRI`. This gives us the freedom to work with whichever runtime we want as long as it is compatible with the CRI.
- examples of [container rutimes supported by kubentes include](https://landscapeapp.cncf.io/cncf/card-mode?category=container-runtime&grouping=category) include containerd, cri-o,  firecaker, kata
- Containerd is the industry standard container runtime that k8s uses to manage containers

```sh
$ sudo apt-get update
# install containerd
$ sudo apt-get install -y containerd

# Create the default configuration file
# sudo mkdir -p /etc/containerd
$ containerd config default | sudo tee /etc/containerd/config.toml

# configure containerd to use the system cgroup driver which helps in proper resource management
# this command basically just edits the file `config.toml` and converts the value to `true`
$ sudo set -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/contained/config.toml

# restart the containerd service
$ sudo systemctl restart containerd
```
2. **Disable swapp memory**
-  swap gives the system an illusion that there is more memory than the system has which could lead to underperfomance in context of kubernetes.
-  We are required to disable swapp as the `kubenetes scheduler` needs a predictable and stable measure of memory on every node.
-  By design, the `kubelet` fails if the swapp is turned on on a node

```sh
# disable swap for the current session
$ sudo swapoff -a

# permanetly disable swap by commentin the line swap in the fstab file
$ sudo set -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```

3. **configure critical kernel parameters**
-  we need to tell the linux kernel to allow IP traffic to be forwarded and let the `ip tables` to see the bridged network traffic.
-  k8s CNI plugins create a virtual network and the `bridge` is like a virtual swicth that every node gets to plugin to enable the communication between pods on diffrent nodes

*loading kernel modules*
```sh
# create a config file for these settings
cat << EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

# load these modules into the kernel
sudo modprobe overlay
sudo modeprobe br_netfilter

# confirm if the modules has been loaded succesfully
$ lsmod | grep overlay
$ lsmod | grep br_netfilter
```
- `overlay` module is an ideal docker overlay filesytem which kubernetes uses to manage container layer. without it docker cannot funtion properly and kubernetes cannot ochestrate  containers
- `br_netfilter module`: this module is crucial for networking in k8s. It enables briding and filtering capabilities allowing k8s to manage network traffic between pods and services effectively

  *configuring systcl parameters*
```sh
# These paramaters are required for CNI plugins to work
$ cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

# apply the parameters without having to reboot
$ sudo sysctl --system
```

**4. Install `kubeadm`, `kubelet`  and `kubectl`**
- `kubeadm` use to manage cluster operations with commands such as `init` and `join`
- `kubelet` This is the agent that runs on every node and listents to instructions from the control plane and manages the pods inside the nodes
- `kubectl` The commandline tool to interact with the cluster
```sh
# update the package index and install packages required to install kubernetes apt repository

$ sudo apt-get update
$ sudo apt-get install -y apt-transport-https ca-certificates curl gpg

# Download the public siging key for the k8s package repository
$ curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.34.0/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

# Add the approprite kubernets apt repository
$ echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

# update packages and install kubelet, kubead and kubectl
$ sudo apt-get update
$ sudo apt-get install -y kubeadm kubelet kubectl

# pin the package version to prevent accidental upgrade
$ sudo apt-mark hold kubelet kubeadm kubectl
```


## 2. Initialise the control plane > this is done on the control plane alone
- Considering that we have multiple nodes, w need to choose the node that will operate as the `control plane`
- Running `kubeadm init` bootstraps the control plane and setsup:
	- etcd -> the cluster db
	- ApI server
	- scheduler
	- and control manger

- we use the flag `--pod-network-cidr=192.168.0.0/16`   to prelocate IP address range for our pod network
- The ip address range should not conflict with the VM Ip address
```
$ sudo kubeadm init --pod-network-cidr=192.168.0.0/16
```
- The command might take a while, when done, it will print:
	- command to configure kubectl for your user
	- `kubeadm join` hash with a unique token and hash
- you are required to copy the kubeadm join command as you will use it in your worker nodes
```sh
# This is given as a response, use to to configure kubectl

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```


## 3. Join the worker nodes > run on the worker nodes only
- This is how we tell our worker nodes how and where to fine the control plane and authenticate with the control API server
- copy the `kubead join` command provided by the control plane


## 4. Install the pod network > run on the control node only
- At this point, the worker nodes have joined the cluster and running the command `kubectl get nodes` will return a list on woker nodes in `Ready` state.
- The nodes are will registerd bu there is no network fabric connecting them. This means that pods on `node0` cannot talk to pods on `node 1`
- We need to install a Container Network Interface to create this virtual network.
- We will use Callico

```sh
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/calico.yaml
```
- After this, running the command `kubectl get nodes` will return nodes in the ready state


**Reaources**
[creating a cluster with kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)
