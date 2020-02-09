---
layout: post
title: Configuring a Secure Shell Server under Linux
tags: [Debian, Linux]
---

This is a basic configuration for a Secure Shell Server under Linux. I assume you already have installed the SSH Server, if not, please refer to the documentation of your favorite Linux Distribution, you will find that it’s quite easy to install. I will be using FreeBSD as a client and Linux as SSH Server.

#### SSH public/private key

Public/private key is one effective method to secure access to our server. A public key is placed on the server, whereas the private key is placed on the local computer. If someone login onto our server, they must have to provide the private key, instead of just a simple password.

#### Generating public/private rsa key pair on the local computer(client)

```
$ ssh-keygen -t rsa
Generating public/private rsa key pair.		
Enter file in wich to save the key (/home/baldo/.ssh/id_rsa):		
Created directory ’/home/baldo/.ssh’.
Enter passphrase (empty for no passphrase):
Enter the same passphrase again:
Your indetification has been saved in /home/baldo/.ssh/id_rsa.
Your indetification has been saved in /home/baldo/.ssh/id_rsa.pub.
The key fingerprint is:
c0:a7:25:c0:4a:aa:32:58:88:16:f9:56:97:f2:05:1f baldo@baldiyo.com
```

Once the rsa key pair is generated copy the id_rsa.pub(public key) to the server.

Note: Since FreeBSD does not have a ssh-copy-id as a part of OpenSSH package, copy the id_rsa.pub to the server and setup it manually.

```
$ scp  ~ /.ssh/id_rsa.pub baldo@192.168.1.68:/home/baldo/.ssh/id_rsa.pub.tmp
baldo@192.168.1.68’s password:
id_rsa.pub         100% 399    0.4KB/s 00:00   
```

I already have created a user called baldo on the server.

```
$ ssh baldo@192.168.1.68
baldo@192.168.1.68’s password:

The programs included with the Debian GNU/Linux system are free software;	
the exact distribution terms for each program are described in the	
individual files in /usr/share/doc/ * /copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRATY, to the extent
permitted by applicable law.
You have mail.
Last login: Tue Nov 11 02:45:51 2008 from 192.168.1.12
baldo@baldiyo: ~ $ cat .ssh/id_rsa.pub.tmp >> authorized_keys
baldo@baldiyo: ~ $ rm .ssh/id_rsa.pub.tmp
```

#### Configuring the ssh server

I highly recommend do not use the default configuration. As root modify the ”/etc/ssh/sshd_config” file.

This is my configuration.

```
# Package generated configuration file
# See the sshd(8) manpage for details

# What ports, IPs and protocols we listen for
Port 70000
# Use these options to restrict which interfaces/protocols sshd will bind to
#ListenAddress ::
#ListenAddress 0.0.0.0
Protocol 2
# HostKeys for protocol version 2
HostKey /etc/ssh/ssh_host_rsa_key
HostKey /etc/ssh/ssh_host_dsa_key
#Privilege Separation is turned on for security
UsePrivilegeSeparation yes

# Lifetime and size of ephemeral version 1 server key
KeyRegenerationInterval 3600
ServerKeyBits 768

# Logging
SyslogFacility AUTH
LogLevel INFO

# Authentication:
LoginGraceTime 120
PermitRootLogin no
StrictModes yes

RSAAuthentication yes
PubkeyAuthentication yes
#AuthorizedKeysFile    %h/.ssh/authorized_keys

# Don’t read the user’s ~ /.rhosts and ~ /.shosts files
IgnoreRhosts yes
# For this to work you will also need host keys in /etc/ssh_known_hosts
RhostsRSAAuthentication no
# similar for protocol version 2
HostbasedAuthentication no
# Uncomment if you don’t trust ~ /.ssh/known_hosts for RhostsRSAAuthentication
#IgnoreUserKnownHosts yes

# To enable empty passwords, change to yes (NOT RECOMMENDED)
PermitEmptyPasswords no

# Change to yes to enable challenge-response passwords (beware issues with
# some PAM modules and threads)
ChallengeResponseAuthentication no

# Change to no to disable tunnelled clear text passwords
PasswordAuthentication no

# Kerberos options
#KerberosAuthentication no
#KerberosGetAFSToken no
#KerberosOrLocalPasswd yes
#KerberosTicketCleanup yes

# GSSAPI options
GSSAPIAuthentication no
#GSSAPICleanupCredentials yes

X11Forwarding no
X11DisplayOffset 10
PrintMotd no
PrintLastLog yes
KeepAlive yes
#UseLogin no

#MaxStartups 10:30:60
#Banner /etc/issue.net

# Allow client to pass locale environment variables
AcceptEnv LANG LC_*

Subsystem sftp /usr/lib/openssh/sftp-server

UsePAM no

UseDNS no

AllowUsers baldo
```


Finally restart the SSH Server and logout to test the configuration.

```
# /etc/init.d/ssh restart
Restarting OpenBSD Secure Shell server: sshd.

$ ssh -p 7000 baldo@192.168.1.68
Enter passphrase for key ’/home/baldo/.ssh/id_rsa’:
Last login: Wed Nov 12 01:41:45 from 192.168.1.12
baldo@baldiyo.com:~$
```
