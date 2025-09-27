+++
title = "Troubleshooting a misconfigured kube-controller manager"
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

# Troubleshooting a misconfigured kube-controller manager 
[Lab](https://killercoda.com/killer-shell-cka/scenario/kube-controller-manager-misconfigured)
The main role of a `kube-controller manager` is to check the health of running containers. This means that in a k8s cluster, if the kube-controller is not operational, it would be impossible to detect failed applications to trigger the sel-healing mechanism which is basically the k8s system trying to restart the pods. This means that probes would also fail.

## context
- The problem here is that the kube-controller manager pod is not comming on. its is stuck on the `CrashLoopBack` status
- we can confirm this with the command `kubectl get pods -n kube-system`. This just lists all pods in the `kube-system` namespace
- considering that the `kube-controller manager` is a static pod, we can use the container runtime commands to check its logs. In context of `Docker`, we would use `docker ps` but our system uses `cri-o` and thus we can list all running containers using the command `watch crictl ps -a`. the `-a` flag just specifies that we want to see even the failed container
- The above commnad should list all running containers in the system even the failed and `kube-controller-manager` should be there
- Beaing a static pod, we can either use the `crictl logs container-id`, `journalctl` or `/var/log/syslog` file to check for errors

## fix
- Using the commad `crictl logs container-ID | grep -i error` we get an output telling us that there is an error in the manifest file
- The static pod's manifest files are located in `/etc/kubernetes/manifests/`
- a simple search on the patterns got from the error in the manifest file, we can get the rootcause of the error
- saving the manifest file will trigger the system to create another pod from the corrected manifest file. This might take awhile, alternatively, we can move the manifest file to another directory and put it back in the kubernetes manifests file. This will manualy trigger the creation of a new `kube-controller manager` pod
- running the command `kubectl get pods -n kube-system` will show the pod in **Running** state
