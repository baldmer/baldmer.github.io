---
layout: post
title: Running VirtualBox OSE
tags: [Debian, Linux]
---

Virtualization allows an unmodified operating system to run in a “virtual machine” unlike “paravirtualization” that the operating system needs to be modified. Typically the physical machine (where the virtualization software is running) is called “host” and the virtual machine is called guest.

I know a few virtualization software, like Qemu, VMware and VirtualBox, personally I prefer VirtualBox Open Source Edition (OSE). VirtualBox OSE is an open source software for virtualization provided by Sun Microsystems. For more information please, refer to the project’s official web page. http://www.virtualbox.org.

This is a simple way to get running VirtualBox on Debian Lenny Gnu/Linux.

```
# aptitude install virtualbox-ose virtualbox-ose-source \
virtualbox-ose-modules-$(uname -r) virtualbox-ose-guest-utils \
virtualbox-ose-guest-source
# aptitude install module-assistant
# m-a prepare
# m-a a-i virtualbox-ose
# modprobe vboxdrv
```

Add to vboxusers group unprivileged users in order to let them use VirtualBox.

```
# adduser mero vboxusers
Adding user `mero’ to group `vboxusers’ ...
Adding user mero to group vboxusers
Done.
```

To run virtuabox, as unprivileged user type.

```
$ VirtualBox
```

Creating a virtual machine is quite easy, just click on “New” and the “New Virtual Machine Wizard” will guide you through the process, then start the virtual machine and follow the typical install process for your favorite operating system.
