---
layout: post
title: Solution to protect your servers against DDoS attacks
tags: [Debian, Linux]
---

Recently a friend found a script to mitigate a DDoS attack and asked to set up on our Server. Basically this script identifies IPs with large amount of connections and block them for a certain period of time.

Installation is quite easy, just type:

```
$ wget http://www.inetbase.com/scripts/ddos/install.sh
# chmod 0700 install.sh
# ./install.sh
```

It creates a subdirectory under `/usr/local` called ddos, download source files and create a cron task to run every minute.

If you want customize its configuration edit the file `/usr/local/ddos/ddos.conf`.

Now, we just have to wait for someone to test it.

Note: This is not an overall solution to prevent this kind of attacks, it just deflate them.

For more information on this project, visit [here](http://deflate.medialayer.com).
