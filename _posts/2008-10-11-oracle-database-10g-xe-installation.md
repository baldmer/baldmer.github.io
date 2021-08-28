---
layout: post
title: Oracle Database 10g XE Installation
tags: [Java, Debian, Linux]
---

To perform this installation download the package form oracle website according to your OS, Linux Distribution or System Architecture..

http://www.oracle.com/technology/software/products/database/index.html

Installation on Debian Lenny. As root run the following commands:

``` 
root@debian:/home/baldo# dpkg -i oracle-xe_10.2.0.1-1.0_i386.deb 
```

Install Linux kernel AIO access library development files.

```
root@debian:/home/baldo# aptitude install libaio-dev
root@debian:/home/baldo# dpkg -i oracle-xe_10.2.0.1-1.0_i386.deb
```

Configuration.

``` 
# /etc/init.d/oracle-xe configure
```

Follow the instructions.


Start, stop and restart the Database Server using the following.

```
root@debian:/home/baldo# /etc/init.d/oracle-xe start

root@debian:/home/baldo# /etc/init.d/oracle-xe stop

root@debian:/home/baldo# /etc/init.d/oracle-xe restart

```

Note: This installation is only for testing purposes.
