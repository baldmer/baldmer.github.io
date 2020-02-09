---
layout: post
title: VirtualBox OSE, rebuilding module vboxdrv
tags: [Debian, Linux]
---

I just upgrade my kernel and when I try to start VirtualBox OSE, I get this message:

```
$ VBoxManage startvm  "Open Solaris" 
WARNING: The character device /dev/vboxdrv does not exist. 
	 Please install the virtualbox-ose-modules package for your kernel and 
	 load the module named vboxdrv into your system. 

	 You will not be able to start VMs until this problem is fixed. 
VirtualBox Command Line Management Interface Version 1.6.6_OSE 
(C) 2005-2008 Sun Microsystems, Inc. 
All rights reserved. 
.
.
```

Well, the WARNING is self explanatory, I need to rebuild the module “vboxdrv” for my new kernel version.

```
# aptitude install virtualbox-ose-modules-$(uname -r)  linux-headers-$(uname -r) 
# m-a prepare
# m-a a-i virtualbox-ose
# modprobe vboxdrv
```

To avoid modprobe every time you reboot or shut down the system, is good idea to load the module at startup. Just type:

```
# echo "vboxdrv" >> /etc/modules
```

Now, the problem is gone.

```
$ VBoxManage startvm  "Open Solaris" 
VirtualBox Command Line Management Interface Version 1.6.6_OSE 
(C) 2005-2008 Sun Microsystems, Inc. 
All rights reserved. 
```

Waiting for the remote session to open... 
Remote session has been successfully opened.

