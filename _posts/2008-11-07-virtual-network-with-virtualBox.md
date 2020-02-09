---
layout: post
title: Virtual Network with VirtualBox
tags: [Debian, Linux]
---

In order to test some security tools, with different Operating Systems, I decided to set up a virtual LAN (Local Area Network) with VirtualBox OSE.

In this post I will focus only on Virtual LAN setup. There are many ways to accomplish this task, this is the way I take:

My Virtual LAN scenario

```
Wireless Network ID: 192.168.1.0/24
Wireless Network DHCP Range: 192.168.1.64 – 192.168.1.253
Wireless Network Default Gateway: 192.168.1.254
Host Machine Wireless Interface: wlan0
Host Machine IP: 192.168.1.68
Guest Machine Interface: tap0
Guest Machine IP: 192.168.1.10
Guest Machine DNS: 192.168.1.254
Guest Default Gateway: 192.168.1.254
Guest Machine Interface: tap1
Guest Machine IP: 192.168.1.11
Guest Machine DNS: 192.168.1.254
Guest Default Gateway: 192.168.1.254
```

Script to set up “n” Virtual Machines.

```
#! /bin/bash

VB_USER=baldo

if [ $USER != "root" ]; then
	echo "X - Sorry, you must be root to run this script"
else
   if [ $# -eq 0 ]; then
       echo "- Usage: sh n_vguests.sh [ip_guest1][ip_guest2]... "
   else
       sysctl net.ipv4.ip_forward=1
       sysctl net.ipv4.conf.wlan0.proxy_arp=1
       for ((i = 0; i < $#; i++)); do
           ips=("$@")
           echo "- Creating virtual interface tap$i for user $VB_USER"
           VBoxTunctl -b -u $VB_USER
           echo "- Enabling proxy ARP for tap$i interface"
           sysctl net.ipv4.conf.tap$i.proxy_arp=1
           echo "- Enabling tap$i interface"
           ifconfig tap$i up
           echo "- Adding static route for tap$i interface"
           route add -host ${ips[$i]} dev tap$i
       done
   fi
fi
```

Install required packages and run the above script providing an IP address for each Guest Machine.

```
# apt-get install uml-utilities
# sh n_vguests.sh 192.168.1.10 192.168.1.11
net.ipv4.ip_forward = 1
net.ipv4.conf.wlan0.proxy_arp = 1
- Creating virtual interface tap0 for user baldo
tap0
- Enabling proxy ARP for tap0 interface
net.ipv4.conf.tap0.proxy_arp = 1
- Enabling tap0 interface
- Adding static route for tap0 interface
- Creating virtual interface tap1 for user baldo
tap1
- Enabling proxy ARP for tap1 interface
net.ipv4.conf.tap1.proxy_arp = 1
- Enabling tap1 interface
- Adding static route for tap1 interface
```

Finally edit the Virtual Box guest settings. Choose “Attached to Host Interface” and specify a “tap” for the Interface Name. When the guest machine boots up, configure the network manually.
