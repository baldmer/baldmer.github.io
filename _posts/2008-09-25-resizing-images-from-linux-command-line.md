---
layout: post
title: Resizing images from Linux command line
tags: [Debian, Linux]
---

`Gallery.py` script does the work.

Just type:

``` 
$ python gallery.py—resize 800 600 ../photos/tm 005.JPG
Unable to import PIL.Image; is the Python Imaging Library installed?
```

Wooops !!, it doesn’t works. I have to install Imaging Library.

Prerequisites required.

Feature library

JPEG support libjpeg (6a or 6b)

http://www.ijg.org

http://www.ijg.org/files/jpegsrc.v6b.tar.gz

ftp://ftp.uu.net/graphics/jpeg/

PNG support zlib (1.2.3 or later is recommended)

http://www.gzip.org/zlib/

OpenType/TrueType freetype2 (2.1.3 or later is recommended)

support http://www.freetype.org

http://freetype.sourceforge.net

```
$ wget http://effbot.org/downloads/Imaging-1.1.6.tar.gz
$ tar -zxvf Imaging-1.1.6.tar.gz
$ cd Imaging-1.1.6
$ python setup.py build_ext -i

imaging.c:3041: error: expected ‘=’, ‘,’, ‘;’, ‘asm’ or ‘attribute’ \
before ‘functions’
imaging.c:3138: warning: return type defaults to ‘int’
imaging.c: In function ‘DL_EXPORT’:
imaging.c:3138: error: expected declaration specifiers before ‘init_imaging’
imaging.c:3149: error: expected ‘{’ at end of input
error: command ‘gcc’ failed with exit status 1
```

To solve this problem install python development libraries.

```
# apt-get install python2.5-dev
$ python setup.py build_ext -i

PIL 1.1.6 BUILD SUMMARY

version       1.1.6
platform      linux2 2.5.2 (r252:60911, Sep  24 2008, 09:22:44)
[GCC 4.3.1]

**TKINTER support not available
- JPEG support ok
- ZLIB (PNG/ZIP) support ok
- FREETYPE2 support ok
```

To check build, run the selftest.py script. And then install.

``` 
$ python selftest.py
57 tests passed.

# python setup.py install
```

Now, everything works like a charm.

```
$ python gallery.py—resize 800 600 ../photos/tm 005.JPG
./photos/tm 005.JPG: was 2848,2136; resizing to 800,600
```

Script to resize more than one image.

```
#! /bin/bash

for i in $( ls $1 ); do
	python gallery.py—resize 800 600 $1$i
done

 
$ sh resize_all.sh ../photos/
./photos/tm005.JPG: was 800,600; resizing to 800,600
./photos/tm006.JPG: was 2136,2848; resizing to 800,600
./photos/tm007.JPG: was 2848,2136; resizing to 800,600
./photos/tm008.JPG: was 2848,2136; resizing to 800,600
./photos/tm009.JPG: was 2848,2136; resizing to 800,600
./photos/tm010.JPG: was 2848,2136; resizing to 800,600
./photos/tm011.JPG: was 2848,2136; resizing to 800,600
./photos/tm012.JPG: was 2848,2136; resizing to 800,600
```

