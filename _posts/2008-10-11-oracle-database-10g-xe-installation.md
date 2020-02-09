---
layout: post
title: Oracle Database 10g XE Installation
tags: [Java, Debian, Linux]
---

To perform this installation download the package form oracle website according to your OS, Linux Distribution or System Architecture..

http://www.oracle.com/technology/software/products/database/index.html

Installation on Debian Lenny. As root run the following commands:

``` 
# dpkg -i oracle-xe_10.2.0.1-1.0_i386.deb
Selecting previously deselected package oracle-xe.
(Reading database … 130936 files and directories currently installed.)
Unpacking oracle-xe (from oracle-xe_10.2.0.1-1.0_i386.deb) ...
dpkg: dependency problems prevent configuration of oracle-xe:
oracle-xe depends on libaio (>= 0.3.96)  libaio1 (>= 0.3.96); however:
Package libaio is not installed.
Package libaio1 is not installed.
dpkg: error processing oracle-xe (—install):
dependency problems – leaving unconfigured
Processing triggers for man-db …
Errors were encountered while processing:
oracle-xe*
```

Install Linux kernel AIO access library development files.

```
# aptitude install libaio-dev
# dpkg -i oracle-xe_10.2.0.1-1.0_i386.deb
Selecting previously deselected package oracle-xe.
(Reading database … 130991 files and directories currently installed.)
Unpacking oracle-xe (from oracle-xe_10.2.0.1-1.0_i386.deb) ...
Setting up oracle-xe (10.2.0.1-1.0) ...
update-rc.d: warning: /etc/init.d/oracle-xe missing LSB information
update-rc.d: see http://wiki.debian.org/LSBInitScripts

Executing Post-install steps…
You must run ’/etc/init.d/oracle-xe configure’ \
as the root user to configure the database.

Processing triggers for man-db …
```

Configuration.

``` 
# /etc/init.d/oracle-xe configure
```

Oracle Database 10g Express Edition Configuration

This will configure on-boot properties of Oracle Database 10g Express
Edition.  The following questions will determine whether the database should
be starting upon system boot, the ports it will use, and the passwords that
will be used for database accounts.  Press <Enter> to accept the defaults.
Ctrl-C will abort.

Specify the HTTP port that will be used for Oracle Application Express [8080]:

Specify a port that will be used for the database listener [1521]:

Specify a password to be used for database accounts.  Note that the same
password will be used for SYS and SYSTEM.  Oracle recommends the use of
different passwords for each database account.  This can be done after
initial configuration:
Confirm the password:

Do you want Oracle Database 10g Express Edition to be started on boot (y/n) [y]:n

Starting Oracle Net Listener…Done
Configuring Database…

Starting Oracle Database 10g Express Edition Instance…Done
Installation Completed Successfully.
To access the Database Home Page go to `http://127.0.0.1:8080/apex`


To start, stop and restart the Database Server from command line. As root type:

```
# /etc/init.d/oracle-xe start
Starting Oracle Net Listener.
Starting Oracle Database 10g Express Edition Instance.

# /etc/init.d/oracle-xe stop
Shutting down Oracle Database 10g Express Edition Instance.
Stopping Oracle Net Listener.

# /etc/init.d/oracle-xe restart
Shutting down Oracle Database 10g Express Edition Instance.
Stopping Oracle Net Listener.

Starting Oracle Net Listener.
Starting Oracle Database 10g Express Edition Instance.
```

Note: This installation is only for testing purposes.
