---
layout: post
title: Upgrading to Ubuntu Bionic on my Home Server
tags: [Linux]
---


Originally in my headless home Server I installed `Ubuntu Bionic Beaver`, this was one of the newest releases at that time. After a while, I decided to stick for a few more months to the stable `Ubuntu Xenial Xerus` version. By now I believe that is a good time to start working with `Bionic`, even though there is still more time until `Xenial` reaches its end of life. At the time of writing this post, the end of life for `Xenial` is on April 2024 and the end of standard support on April 2021.

This time I did not want to go through the whole process of installing everything from scratch. Fortunately, I found an alternative to performing the upgrade from the command shell. The magic is in the following directory:

```bash
$ ls /usr/lib/ubuntu-release-upgrader
check-new-release  check-new-release-gtk  do-partial-upgrade  release-upgrade-motd

```

Let's go there.

```bash
$ cd /usr/lib/ubuntu-release-upgrader/
```

Check for a new release.

```bash
$ ./check-new-release
Checking for a new Ubuntu release
New release '18.04.4 LTS' available.
Run 'do-release-upgrade' to upgrade to it.
```

We proceed with the upgrade.

```bash
$ sudo do-release-upgrade
[sudo] password for baldo: 
Checking for a new Ubuntu release
Get:1 Upgrade tool signature [819 B]                                                                                                                 
Get:2 Upgrade tool [1.242 kB]                                                                                                                        
Fetched 1.243 kB in 0s (0 B/s)                                                                                                                       
authenticate 'bionic.tar.gz' against 'bionic.tar.gz.gpg' 
extracting 'bionic.tar.gz'


Reading cache

Checking package manager

Continue running under SSH? 

This session appears to be running under ssh. It is not recommended 
to perform a upgrade over ssh currently because in case of failure it 
is harder to recover. 

If you continue, an additional ssh daemon will be started at port 
'1022'. 
Do you want to continue? 

Continue [yN] y

Starting additional sshd

To make recovery in case of failure easier, an additional sshd will
be started on port '1022'. If anything goes wrong with the running
ssh you can still connect to the additional one.
If you run a firewall, you may need to temporarily open this port. As
this is potentially dangerous it's not done automatically. You can
open the port with e.g.:
'iptables -I INPUT -p tcp --dport 1022 -j ACCEPT'

To continue please press [ENTER]

```

In my case, if something goes wrong I could manage to recover my Server. But in most cases, the Server is not at home, so it is advised to follow the above recommendation.

```bash
...
...
...
Updating repository information

Third party sources disabled 

Some third party entries in your sources.list were disabled. You can 
re-enable them after the upgrade with the 'software-properties' tool 
or your package manager. 

To continue please press [ENTER]


...
...
...


Do you want to start the upgrade? 


54 installed packages are no longer supported by Canonical. You can 
still get support from the community. 

47 packages are going to be removed. 459 new packages are going to be 
installed. 1658 packages are going to be upgraded. 

You have to download a total of 1.447 M. This download will take 
about 4 minutes with your connection. 

Installing the upgrade can take several hours. Once the download has 
finished, the process cannot be canceled. 

 Continue [yN]  Details [d]y
 
```
 
 
Configuring docker.io, this is because I have Docker running on my Server. 
 
```bash
If Docker is upgraded without restarting the Docker daemon, Docker will often have trouble starting new containers, and in some cases even maintaining the containers it is currently running. See https://launchpad.net/bugs/1658691 for an example of this breakage.                    

Normally, upgrading the package would simply restart the associated daemon(s). In the case of the Docker daemon, that would also imply stopping all running containers (which will only be restarted if they are part of a "service", have an appropriate restart policy configured, or have some other means of being restarted such as an external systemd unit).                                                                 

  │ Automatically restart Docker daemon? 
  
  yes

```

The script automatically continues with the upgrade.


```bash
...
...
...

done
Processing triggers for linux-image-4.15.0-91-generic (4.15.0-91.92) ...
/etc/kernel/postinst.d/initramfs-tools:
update-initramfs: Generating /boot/initrd.img-4.15.0-91-generic
/sbin/ldconfig.real: Warning: ignoring configuration file that cannot be opened: /etc/ld.so.conf.d/x86_64-linux-gnu_EGL.conf: No such file or directory
/etc/kernel/postinst.d/zz-update-grub:
Sourcing file /etc/default/grub
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-4.15.0-106-generic
Found initrd image: /boot/initrd.img-4.15.0-106-generic
Found linux image: /boot/vmlinuz-4.15.0-101-generic
Found initrd image: /boot/initrd.img-4.15.0-101-generic
Found linux image: /boot/vmlinuz-4.15.0-91-generic
Found initrd image: /boot/initrd.img-4.15.0-91-generic
Found memtest86+ image: /boot/memtest86+.elf
Found memtest86+ image: /boot/memtest86+.bin
done
Processing triggers for resolvconf (1.79ubuntu10.18.04.3) ...
Processing triggers for bamfdaemon (0.5.3+18.04.20180207.2-0ubuntu1) ...
Rebuilding /usr/share/applications/bamf-2.index...
Reading package lists... Done    
Building dependency tree          
Reading state information... Done

Checking for installed snaps
No snaps are installed yet. Try 'snap install hello-world'.

Installing snap gnome-3-26-1604
packet_write_wait: Connection to 2a11:d24:6811:1880:7124:a07:7dba:1c1c port 22: Broken pipe

```

At this point, the boot loader should be configured with the new system. Finally, we connect via ssh to test the upgrade.


~~~
~$ ssh baldo@bm-server
Warning: Permanently added the ECDSA host key for IP address '1b10:c31:c010:b911:10f4:6828:d212:71d1' to the list of known hosts.
baldo@bm-server's password: 
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-106-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

 * "If you've been waiting for the perfect Kubernetes dev solution for
   macOS, the wait is over. Learn how to install Microk8s on macOS."

   https://www.techrepublic.com/article/how-to-install-microk8s-on-macos/

0 packages can be updated.
0 updates are security updates.

Last login: Tue Jun 23 20:16:53 2020 from 1b10:c31:c010:b911:d117:229b:c5cf:73d6

~~~


# References

[BionicBeaver](https://wiki.ubuntu.com/BionicBeaver/ReleaseNotes#:~:text=Press%20Alt%2BF2%20and%20type,follow%20the%20on%2Dscreen%20instructions.)
 








