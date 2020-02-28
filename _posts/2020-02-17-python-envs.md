---
layout: post
title: Conda and PIP Python Environments
tags: [Linux, Python]
---

I heard about `Python` environments a long time ago while I was in a hackathon. At first the idea was a little bit complex, but over time it has become an easy routine. The idea is to have a technology to deal with the project's dependencies and ease its development and deployment. In this post, I describe the way to create a `Python` environment using `conda` and `PIP` package managers in a `Linux` system.

#### conda

To create a `conda` environment we use:

```
$ conda create -n myenv
```

We can have a look at the current `conda` environments existing in the system by typing the following command:

```
$ conda env list
# conda environments:
#
base                  *  /home/baldo/miniconda3
cling                    /home/baldo/miniconda3/envs/cling
myenv                    /home/baldo/miniconda3/envs/myenv
otherenv                 /home/baldo/miniconda3/envs/otherenv
```

The instructions to activate and deactivate the environments are displayed while creating the environment.

To activate we use:

```
$ conda activate myenv
```

To deactivate we use:

```
$ conda deactivate
```

#### PIP

The `PIP` environment management is more simple than `conda`, in this case, the environment is isolated in a specified folder (created).

```
$ python3.7 -m venv myenv
```

To activate we use:

```
$ source myenv/bin/activate
```

This was my favorite activation method. One day I logged into a server where I did not have `root` access and the command did not work. I had to use the following:

```
$ . myenv/bin/activate
```

To deactivate the environment we simply use:

```
$ deactivate
```
