---
layout: post
title: Connecting JDeveloper to Oracle Database
tags: [Java, Debian, Linux]
---

For connecting Oracle Developer 10.1.3 to the Oracle 10g XE human resources sample schema we need:

- JDeveloper.

- Oracle database.

Connection

Start Oracle Database Server and unlock Human Resources(hr) user.

```
# /etc/init.d/oracle-xe start
```

If you are using HR sample schema for the first time you must unlock, grant CONNECT and RESOURCE roles and set a password to it. You can do this by using Oracle Enterprise Manager or through SQL * Plus commands.

Open your SQL * Plus terminal and type:

```
SQL> conn sys/baldo_secret_pass@XE as SYSDBA
SQL> ALTER USER hr_user IDENTIFIED BY hr_user ACCOUNT UNLOCK;
SQL> GRANT CONNECT, RESOURCE to hr_user;
SQL> ALTER USER hr_user IDENTIFIED BY baldo_secret_pass;
SQL> quit
```

Start JDeveloper and launch connection wizard.

```
$ cd jdeveloper/jdev/bin/
$ ./jdev
```

Once started go to connection navigator, right click on Database and select New Database Connection… to launch the Connection Wizard.

- Step 1: Specify a connection name and leave Oracle (JDBC) as connection type.
- Step 2: Specify user name and password for your HR schema and check Deploy Password box.
- Step 3: Specify connection information for the database machine. In my case the database server is hosted in the same machine I have JDeveloper installed. Type XE as System Identifier(SID).
- Step 4: Test connection.
