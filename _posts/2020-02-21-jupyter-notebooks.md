---
layout: post
title: Setting up a Jupyter Notebook on a Local Environment
tags: [Linux, Python, C++]
---

Jupyter Notebooks are very useful for quick experimenting with code or in the case of Data Science for fast data analysis and prototyping. I have been using the `Python` Notebooks for a long time. Recently I had to experiment with some `C++` code while I was reviewing some standards. In this post, I will describe the procedure to work with Notebooks using `Python` and `C++`. A `Python` [environment](/blog/python-envs) is required. 

#### Python Jupyter Notebook

For a Notebook with `Python` support, we simply need to install the `notebook` package in our desired environment.

```
$ . myenv/bin/activate
$ pip install notebook
```

To start on `localhost` execute:

```
$ jupyter notebook
```

#### C++ Jupyter Notebook

The conda package `xeus-cling` will provide the Notebook with `C++` support. A conda [environment](/blog/python-envs#conda) named `cling` is assumed to exists.


```
$ conda activate cling
$ conda install xeus-cling -c conda-forge
```

We also need jupyter.

```
$ conda install notebook
```

To start the Notebook we follow the traditional procedure.

```
$ jupyter notebook
```

The Notebooks will be available at `http://localhost:8888/`, provided that the port `8888` is available, otherwise `8889` will be used. We can also use ready to use Notebooks with support for other languages [here](https://jupyter.org/try).







