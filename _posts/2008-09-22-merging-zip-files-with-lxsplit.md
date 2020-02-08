---
layout: post
title: Merging zip files with lxsplit
tags: [Debian, Linux]
---

Recently I downloaded a multi-part zip file, I got in trouble while I was extracting. A friend of mine told me that just simply extract the first file and automatically the parts will be joined, that could works for rar files, but not for zip files.

While I was surfing the net I found this:

- Using cat (Never mind, it doesn’t works)
- Using HJSplit a File splitting program for Windows XP, Vista, 2000, NT, 95, 98 and ME, also there is a version for Linux.
- Using xSplit a simple tool for splitting files and joining the splitted files on Unix-like platforms. Download this tool from [here](http://lxsplit.sourceforge.net/).

Installation is so easy, just run:

```
$ make
# make install
```


Usage to fit my needs; if you want to have more information about this tool, please refer to the [project’s page](http://sourceforge.net/projects/lxsplit/).

 
```
$ lxsplit -j enus3.zip.001 

Creating merged file `enus3.zip’.
Complete size: 418328591 in 5 files.
Processing file `enus3.zip.001’ ...
Processing file `enus3.zip.002’ ...
Processing file `enus3.zip.003’ ...

Processing file `enus3.zip.004’ ...
Processing file `enus3.zip.005’ ...
Done!
```

