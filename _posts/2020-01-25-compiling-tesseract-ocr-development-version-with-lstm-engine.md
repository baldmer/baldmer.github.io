---
layout: post
title: Compiling Tesseract OCR Development Version with LSTM engine
tags: [Linux, Google, C++, Data-Science]
---

I used `Tesseract OCR` in several projects under `Python`. This time I wanted to try it directly in `C++`. According to the authors [1, 2] the latest version of Tesseract uses a `LSTM` Neural Network. In this post we aim to compile the latest version for future experiments.

#### Install the required libraries

```
sudo apt-get install g++ make
sudo apt-get install autoconf automake libtool
sudo apt-get install pkg-config
sudo apt-get install libpng-dev
sudo apt-get install libjpeg8-dev
sudo apt-get install libtiff5-dev
sudo apt-get install zlib1g-dev
```

#### Compiling last version of leptonica

Download source code from [here](http://www.leptonica.org/) and extract its content.

```
$ gunzip leptonica-1.79.0.tar.gz
$ tar -xvf leptonica-1.79.0.tar
$ cd leptonica-1.79.0
```

We use the second option (2): "Building using autoconf method to build this library".

```
$ ./configure
$ make
$ sudo make install
```

To run the regression tests execute:

```
$ make check
...
...
===========================================================================
Testsuite summary for leptonica 1.79.0
===========================================================================
# TOTAL: 133
# PASS:  133
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
===========================================================================
```

In case of failed tests see `prog/test-suite.log`, in my case `pixa1_reg` failed because `gnuplot` was not installed.


#### Compiling Tesseract

```
$ git clone https://github.com/tesseract-ocr/tesseract.git
$ cd tesseract
$ ./autogen.sh
$ ./configure
$ make
$ sudo make install
$ sudo ldconfig
```

#### Data files

Download data files from [here](https://tesseract-ocr.github.io/tessdoc/Data-Files.html). Then move the language data to the folder `/usr/local/share/tessdata`. Finally, add the path of tessdata to `~/.bashrc` to make it permanent.

```
$ vim ~/.bashrc
export TESSDATA_PREFIX=/usr/local/share/tessdata
$ . ~/.bashrc
```

#### Test installation

We can execute the following command to test the installation.

```
$ tesseract -v
tesseract 5.0.0-alpha-628-g95be
 leptonica-1.79.0
  libjpeg 8d (libjpeg-turbo 1.4.2) : libpng 1.2.54 : libtiff 4.0.6 : zlib 1.2.8
 Found AVX
 Found SSE
 Found OpenMP 201307
```

"The master branch is using 5.0.0 versioning because of code modernization cause API compatibility issues with 4.x release", as sated in [Release Notes](https://tesseract-ocr.github.io/tessdoc/ReleaseNotes.html).


#### References

- [1][tessdoc](https://tesseract-ocr.github.io/tessdoc/Home.html)
- [2][tesseract](https://github.com/tesseract-ocr/tesseract)


