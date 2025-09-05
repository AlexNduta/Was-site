+++
title = "What is Kubernetes ?"
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


# What is kubernetes?
This is a container ochestration tool that was initialy developed by Google for use as an internal tool. It was later on made open source and donated to the Cloud Native Foundation(CNCF).
This sounds like a lot of tech words to throw around in a single sentence. Lets go to the very basics. What is a container?     

A [container](https://alexnduta.netlify.app/posts/containerrisation/) is a package that contains an application and all the dependencies it needs to run. 
This makes it easy to share around applications and run them in different enviroment. This is all fun an games until you have to work with multiple containers. 
For multiple containerised applications, we use docker compose which is a IaaC used to define multiple container environments. 
This gets the job done but has limitations. We have to manualy manage and control the containers. This gets harder to scale or fix a falling container. 
A failing container could be a business critical service that could mean the ultimate death of the business due to frustrations faced by the customer accessing the service.

These challenges are solved with a container ochestration tool such as `kubernetes`, `Openshift`. But what really is a container ochestration tool?
This is a tool that abstracts all the container operations by letting us describe the infrasture we need, that is resources, number of container to run etc using a declarative yaml file called a `manifest`.
The platform does all the dirty work of managing, scaling, networking containerised application.
This comes in handy when working with alot of containers especially a micro-service enviroment.

Kubernetes does all this. we define our infrastructure in a decrarative way using yaml file called manifests. In the manifest, we can define a kubernetes object such as a `pod` how it should run, what image should run and how many replicas(instaces of the pod) should run.
I know I have just introduced two tech buzz-words without the courtesy of describing their meaning. So, what is a kubernetes object and what about a pod?

A Kubenetes object is a peristent entity that describe the desired state of our cluster. When you describe a kubenetes object, you are basically specifying what should be running and how it should be. examples include `Pods`, `Deployments`, `Services`, `Replicasets`, `Namespace`.

A `pod` is the smallest unit in context of kubernetes. It is used to encapsulate a containerised application.
It might cotain one or a few related containers. In kubernetes, we interact directly with pods. This means that all the resources we define are targeted to a pod which in turn transposes into the container running with.

kubernetes commonly refered to as K8s is enables us to describe an object like a  `Pod`, specify the number of replicas of the pod that should exist and how netowking should be done. Replicas are used to achieve high availibity(HA) especially when used with a Loadbalancer object. This mean that if we have have specified that we should have 3 replicas, kubernetes keeps checking if the desired number is always met, if at any point one of the pod is unreacheable, k8s tries to restart it. If this fails, another fresh pod is launched so as to achieve the desired number of replicas.
