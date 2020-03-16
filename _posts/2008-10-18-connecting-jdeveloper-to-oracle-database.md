---
layout: post
title: Connecting JDeveloper to Oracle Database
tags: [Java, Debian, Linux]
---

In this post I’ll show how to connect Oracle Developer 10.1.3 to an Oracle 10g XE human resources sample schema.

Prerequisites

- JDeveloper.

- Oracle database.

Oracle database schemas. By default Human Resources(hr) schema is installed in Oracle 10g XE. If you want to install more sample schemas refer to the documentation.

Connection

Start Oracle Database Server and unlock Human Resources(hr) user.

```
# /etc/init.d/oracle-xe start
Starting Oracle Net Listener.
Starting Oracle Database 10g Express Edition Instance.
```

If you are using HR sample schema for the first time you must unlock, grant CONNECT and RESOURCE roles and set a password to it. You can do this by using Oracle Enterprise Manager or through SQL * Plus commands.

Open your SQL * Plus terminal and type:

```
SQL * Plus
SQL * Plus: Release 10.2.0.1.0 – Production on Fri Oct 17 21:44:04 2008

Copyright© 1982, 2005, Oracle.  All rights reserved.

SQL> conn sys/my_secret_pass@XE as SYSDBA
Connected.
SQL> ALTER USER hr IDENTIFIED BY hr ACCOUNT UNLOCK;

User altered.

SQL> GRANT CONNECT, RESOURCE to hr;

Grant succeeded.

SQL> ALTER USER hr IDENTIFIED BY my_secret_pass;

User altered.

SQL> quit
```

Start JDeveloper and launch connection wizard.

```
$ cd jdeveloper/jdev/bin/
$ ./jdev

Oracle JDeveloper 10g 10.1.3.3
Copyright 1997, 2007 Oracle. All Rights Reserved
```

Once started go to connection navigator, right click on Database and select New Database Connection… to launch the Connection Wizard.

- Step 1: Specify a connection name and leave Oracle (JDBC) as connection type.
- Step 2: Specify user name and password for your HR schema and check Deploy Password box.
- Step 3: Specify connection information for the database machine. In my case the database server is hosted in the same machine I have JDeveloper installed. Type XE as System Identifier(SID).
- Step 4: Test connection.
