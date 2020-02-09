---
layout: post
title: Error Initializing OC4J
tags: [Java, Debian, Linux]
---

```
$ ./start_oc4j
08/10/10 13:15:34 Error initializing server: At least one valid code-source 
or import-shared-library element is required for shared-library 
“oracle.expression-evaluator” in 
/home/baldo/dev_tools/jdeveloper/j2ee/home/config/server.xml.
08/10/10 13:15:34 Fatal error: server exiting
```

This is my oracle.home in server.xml.

```
/home/baldo/dev_tools/java/jdevstudiobase10133/
```

And this is my current oracle.home.

```
/home/baldo/dev_tools/jdeveloper/
```

ERROR: DO NOT CHANGE ORACLE HOME AFTER SETUP SERVER CONFIGURATION.

To solve this problem replace your current server.xml to server.xml provided by oracle jdeveloper package. Or edit your current server.xml to add the new oracle.home.

```
$ mv /home/baldo/dev_tools/jdevstudiobase10133/j2ee/home/config/server.xml \
/home/baldo/dev_tools/jdeveloper/j2ee/home/config/
$ ./start_oc4j
08/10/10 14:01:55 Oracle Containers for J2EE 10g (10.1.3.3.0)  initialized
```