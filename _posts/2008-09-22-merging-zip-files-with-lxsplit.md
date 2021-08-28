---
layout: post
title: Merging zip files with lxsplit
tags: [Debian, Linux]
---

Recently I downloaded a multi-part zip file. I had a problem while extracting its content. A friend of mine told me that just simply extract the first file and automatically the parts will be joined. I have seen that working for rar files.

Options that I had.

- Using cat (Never mind, it doesn’t works)
- Using HJSplit a File splitting program for Windows XP, Vista, 2000, NT, 95, 98 and ME, also there is a version for Linux.
- Using xSplit a simple tool for splitting files and joining the splitted files on Unix-like platforms. Download this tool from [here](http://lxsplit.sourceforge.net/).

xSplit installation

```
$ make
# make install
```


Usage to fit my needs; if you want to have more information about this tool, please refer to the [project’s page](http://sourceforge.net/projects/lxsplit/).

 
```
$ lxsplit -j enus3.zip.001 
```

