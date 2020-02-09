---
layout: post
title: Road-Warrior(Host to Net) configuration with OpenVPN
tags: [Debian, Linux]
---

The goal to accomplish is to share files stored in a Virtual Private Server(VPS) among clients connected to a Virtual Private Network(VPN), all this in a secure fashion over a public internet.

There are commercial and Open Source tools to set up Virtual Private Networks, the popular ones are IPSec and OpenVPN. For this post I use OpenVPN 2.0.

One of the highlights of OpenVPN is that it runs on user space, this means that if OpenVPN's security is compromised it won't affect the whole system or low level processes. Another highlight is that it is relatively easy to configure, actually you can have a basic VPN up and running in just a few minutes.

On the other hand is IPSec, honestly I have not work with it yet, but according to my search it is very difficult to configure and it has a very insecure design because it runs on kernel level, to be more specific at ring 0.

Take a look at what its creators said about it:

"We are of two minds about IPSec. On the one hand, IPSec is far better than any IP security protocol that has come before: Microsoft PPTP, L2TP, etc. On the other hand, we do not believe that it will ever result in a secure operational system. It is far too complex, and the complexity has lead to a large number of ambiguities, contradictions, inefficiencies, and weaknesses. We strongly discourage the use of IPSec in its current form for protection of any kind of valuable information, and hope that future iterations of the design will be improved. However, we even more strongly discourage any current alternatives, and recommend IPSec when the alternative is an insecure network. Such are the realities of the world".

— Ferguson and B. Schneier.

Well, let's get our hands dirty!

Basically the tools you need are:

* VPS with a GNU/Linux system (I'm using a XEN VPS with a GNU/Linux Debian OS)
* A set of tools to set up VPNs(OpenVPN 2.0)
* SAMBA server to share files
* Obvious stuff like client machines, internet, electricity,  etc.


Note: I assume you already have OpenVPN and dependencies installed on both server and client machines, please refer to the documentation of your favorite GNU/Linux distribution to install these packages.

Dependencies:

* openssl
* lzo
* pam


Setting up Public Key Infrastructure (PKI)

#### Step 1:

Create Master Certificate Authority (CA) and key

Copy the preconfigured examples to the /etc/openvpn directory

```
# cp -r /usr/share/doc/openvpn/examples/easy-rsa/ /etc/openvpn
```

Edit the vars file by setting your own parameters(KEY_COUNTRY, KEY_PROVINCE, KEY_CITY, KEY_ORG, KEY_EMAIL).

```
# cd /etc/openvpn/easy-rsa/2.0/
# vi vars
.
.
```

Then initialize the PKI.

```
# . ./vars
# ./clean-all
# ./build-ca
```

Enter your Common Name parameter(in my case "OpenVPN-mg-tech").

Note: you can leave most parameters as default.

#### Step 2:

Create Certificate and Key for Server

```
# ./build-key-server server
```

Enter "server" as Common Name parameter and answer "yes" to the last two queries.

#### Step 3:

Create Certificate and Key for client1

Note: Repeat these steps for each client you want to add to the VPN.

```
# ./build-key client1
```

Enter "client1" as Common Name parameter.

#### Step 4:

Diffie Hellman parameters

```
# ./build-dh
```

For more information on Diffie-Hellman refer to the RSA Laboratories.

Up to now you must have all keys and certificates in the "keys" subdirectory. In order to configure your clients you need to copy the generated files to the client machines, in my case these are:

* ca.crt
* client1.crt
* client1.key


Note: Ensure to copy these files over a secure channel like ssh.

Server and Client configuration.

For the configuration I want to accomplish I need a routed VPN. I use tun0 virtual interface to handle traffic among clients and server over UDP protocol.

#### Server configuration (/etc/openvpn/server.conf)

```
port 1194
proto udp
dev tun
persist-tun
ca /etc/openvpn/easy-rsa/2.0/keys/ca.crt
cert /etc/openvpn/easy-rsa/2.0/keys/server.crt
key /etc/openvpn/easy-rsa/2.0/keys/server.key
dh /etc/openvpn/easy-rsa/2.0/keys/dh1024.pem
server 10.8.0.0 255.255.255.0 # server ip will be 10.8.0.1
ifconfig-pool-persist ipp.txt
client-to-client # i want clients can reach each other.
keepalive 10 120
comp-lzo  
user nobody
group nogroup
persist-key
persist-tun
status openvpn-status.log
verb 4
```

#### Client configuration (/etc/openvpn/client.conf)

This configuration is for a GNU/Linux client and it is placed on the client machine.

```
client
proto udp
dev tun
persist-tun
ca ca.crt
cert client1.crt
key client1.key
remote 67.23.12.223 1194 # IP of the VPS server and OpenVPN port
resolv-retry infinite
nobind
comp-lzo 
user nobody
group nogroup
persist-key
verb 4
```

#### Test configuration

Once restart OpenVPN on both server and client test the configuration with a simple ping from client to server and server to client.

Ping from client(10.8.0.6) to server(10.8.0.1)

```
# ping 10.8.0.1
PING 10.8.0.1 (10.8.0.1) 56(84) bytes of data.
64 bytes from 10.8.0.1: icmp_seq=1 ttl=64 time=105 ms
64 bytes from 10.8.0.1: icmp_seq=2 ttl=64 time=102 ms
64 bytes from 10.8.0.1: icmp_seq=3 ttl=64 time=99.5 ms
.
.
```

Ping from server(10.8.0.1) to client(10.8.0.6)

```
# ping 10.8.0.6
PING 10.8.0.6 (10.8.0.6) 56(84) bytes of data.
64 bytes from 10.8.0.6: icmp_seq=1 ttl=64 time=112 ms
64 bytes from 10.8.0.6: icmp_seq=2 ttl=64 time=100 ms
64 bytes from 10.8.0.6: icmp_seq=3 ttl=64 time=100 ms
.
.
```

#### Configuring SAMBA server to share files among clients.

Note: I assume you have a SAMBA server already installed.

Configure by editing the smb.conf file.

```
# vi /etc/samba/smb.conf
```

When you add your windows clients to the VPN ensure to set the same "workgroup" for them.

```
workgroup = WORKGROUP
```

I want to access my whole home directory so I have to change the following options.

```
[homes]
   comment = Home Directories
   browseable = yes

   read only = no
```

Finally set the SMB password to an existing system user, in my case I already have a user added to the system called client1.

```
# smbpasswd -a client1
New SMB password:
Retype new SMB password:
Added user client1.
```

#### Firewall rules

In your firewall set of rules, ensure the OpenVPN and SAMBA ports are open.

```
-A INPUT -p udp -m udp --dport 1194 -j ACCEPT
-A INPUT -p tcp -m tcp --dport 445 -j ACCEPT 
```

Note: To load the firewall rules at startup add the following line to the "/etc/network/interfaces" file.

```
pre-up iptables-restore < /etc/iptables.up.rules

```

If it worked for you, Поздравляю!, if not try again and again until get it work. Here some links that could be helpful:

[openvpn](http://openvpn.org)

[blug-talk](http://openvpn.org/papers/BLUG-talk)

[howto](http://openvpn.org/index.php/documentation/howto.html)

