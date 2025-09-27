+++
title = "Troubleshooting a misconfigured kublete"
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

# Troubleshooting a misconfigured kublete
[Lab](https://killercoda.com/killer-shell-cka/scenario/kube-controller-manager-misconfigured)
- A `Kublete` is the agent that runs in the nodes that directly communicates with the `Apiserver` receiving instructions from the control node as a `podspec`.
- The kubelet receives a  podspec which is a YAML file from the apiserver that describes how the containers should be running. The kublet makes sure that the nodes are healthy and ready t

## context
- In this lab, the `kublete` in *node01* is not working and you can confirm that by getting to the node01 and checking the kubelet service status
```sh
$ ssh node01
$ systemctl status kubelet
# for older systems
$ service status kubelet
```
- With the kubelet down, this means that we are unable to know the state of our node. This makes it impossible to schedule any pod on it.
- Get the logs to see any issues raised
```sh
$ cat /var/log syslog | grep -i kubelet

# or

$ journalctl -xe | kubelet
```
This should give all logs related to the kubelet and why it failed
## fix

- In this lab, the error is in the flags. we need to make edits to the kubeadm configuration.
- The easiest way to know what configuration file to work with is to use the `find` command then filter using `grep`
```sh
$ find / | grep -i kubeadm
# make edits to the file containinig the `flag`
$ vi /var/lib/kubelet/kubeadm-flags.env

#reload the service
$ systemctl restart kubelet

# confirm the service is running
$ systemctl status kubelet
```
