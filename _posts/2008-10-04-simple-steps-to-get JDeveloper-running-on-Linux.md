---
layout: post
title: Simple steps to get JDeveloper running on Linux
tags: [Java, Debian, Linux]
---

1.- Download this wonderful feature from the official web page and unzip the file, there is not necessary to perform an installation.

http://www.oracle.com/technology/products/jdev/index.html

Note: `jdevstudio` includes the Windows version of JDK 5.0 Update 6 and the JDeveloper documentation. In my case i already have installed JDK 6.0. I downloaded `jdevstudiobase` which includes the JDeveloper documentation, but not JDK.

2 .- Set the variable `SetJavaHome` in the file <jdev_home>/jdev/bin/jdev.conf to the location of your Java installation. JavaHome is the location for your JDK directory, in my case /usr/lib/jvm/java-6-sun/.

3 .- Changing  Permissions

All JDeveloper files must have read permissions.

```
$ chmod -R g+r jdeveloper/
```

4 .- Backup and Replace old SDK cursors

```
# mv /usr/lib/jvm/java-6-sun/jre/lib/images/cursors/ \
/usr/lib/jvm/java-6-sun/jre/lib/images/cursors.bak
```

Extract and move the replacement cursors from: your/jdev_home/jdev/bin/clear_cursors.tar

```
$ tar xf clear_cursors.tar
# mv jre/lib/images/cursors/ /usr/lib/jvm/java-6-sun/jre/lib/images/
```

5 .- Starting JDeveloper

```
$ cd jdeveloper/jdev/bin/
$ ./jdev
Oracle JDeveloper 10g 10.1.3.3
...
```
