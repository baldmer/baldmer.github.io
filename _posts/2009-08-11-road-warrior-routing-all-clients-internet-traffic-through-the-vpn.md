---
layout: post
title: Road-Warrior - Routing all client's internet traffic through the VPN
tags: [Debian, Linux]
---

#### Prerequisites

[Road-Warrior(Host to Net) configuration with OpenVPN](/2009-05-23-road-warrior-host-to-net-configuration-with-openvpn/)

#### IP forwarding

With IP forwarding you can set your Linux box to act as a router. To enable IP forwarding as root issue the following command.

```
# echo "1" > /proc/sys/net/ipv4/ip_forward
```

Note: To enable by default when your system boots up edit the "/etc/sysctl.conf" (on a Debian system).

```
# Uncomment the next line to enable packet forwarding for IPv4
#net.ipv4.ip_forward=1
```

#### Masquerading or packet mangling

Since Internet routers can not forward traffic from private IP addresses you need to invoke IP masquerading. Masquerading is when your Linux system rewrites the IP headers of network packets so the network packet appears to originate from a non-private IP address.

#### Iptables rules.

This is the set of iptables rules that I use for IP forwarding and packet mangling.

```
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
-A INPUT -i tun+ -j ACCEPT
-A FORWARD -i tun+ -j ACCEPT
-A FORWARD -o tun+ -j ACCEPT
.
.
*nat
:PREROUTING ACCEPT [244:17449]
:POSTROUTING ACCEPT [2:486]
:OUTPUT ACCEPT [2:486]
-A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE
.
.
.
```

Finally in your server configuration file, add the following line and restart the OpenVPN:

```
push "redirect-gateway def1"
```

Basically all traffic coming from the internal network(tun0) is forwarded to the Internet through the eth0 interface. Now all the Internet sites I visit record the IP of the OpenVPN server not the IP given by my ISP. One useful application for this configuration is that you can avoid the lack of security on wireless networks, because you connect to the Internet through the VPN.
