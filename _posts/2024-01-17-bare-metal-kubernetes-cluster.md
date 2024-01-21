---
layout: default
comments: true
title:  "Bare Metal Kubernetes Cluster"
date:   2024-01-17 10:55:01 +0100
categories: Kubernetes
description: "I'll be detailing the construction of my 3-node Kubernetes cluster on older hardware within my home lab"
---
# [](#header-1) Bare Metal Kubernetes Cluster

In this blog post, I'll be detailing the construction of my 3-node Kubernetes cluster on older hardware within my home lab. Come along as I walk you through the process and share the lessons learned in my Kubernetes learning adventure! 

## [](#header-3) The Hardware

I've bought 3 old small form factor pc, they are all quad core i5 6500 with 8GB of RAM and 120 SSD. I bought them for 50 euro each, they are cheaper than raspberry pi 4, faster, and you don't have to deal with arm64 and sd cards, but of course, they take a bit more space :)

![hardware]({{ site.url }}/assets/kubernetes/hardware.jpeg)

From now on, for simpicity, I will call them server1, server2, server3. 
All 3 servers are connected to a switch which is connected to my home router

![topology]({{ site.url }}/assets/kubernetes/topology.png)

## [](#header-3) The OS

I've installed on all 3 servers debian 12 with xfce as desktop environment. I've also installed SSH server for ssh connection.

![debian12]({{ site.url }}/assets/kubernetes/debian12.png)

I've doubled checked that I could connect via ssh with the {% highlight console  %} ssh "maciti@192.168.1.13" {% endhighlight %} command from my dev laptop

And then, because I wanted to be able to connect to them via remote desktop as well, I installed and enabled XRDP

{% highlight console  %}

# sudo apt install xrdp -y

# sudo systemctl enable xrdp

{% endhighlight %} 













