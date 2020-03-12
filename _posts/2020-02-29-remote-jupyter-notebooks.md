---
layout: post
title: Remote Jupyter Notebooks
tags: [Linux, Python]
---

I started using Remote Jupyter Notebooks when I was working at GESIS. Sometimes I had to work with large datasets that my local computer was not able to load. The idea was to log in to the company server and use its resources, so I could execute the code on the server and see the results in my local computer. To accomplish this idea, I had to open a tunnel between the server and my local environment. In this post, I will use my server to describe this procedure. A `Python` environment with Jupyter Notebook and a `Linux` server with `ssh` are assumed to exist.


#### Step 1 - Log in to the remote server

```
$ ssh baldo@bm-pc
```

#### Step 2 - Start Jupyter Notebook on the server

Here, we activate the PIP environment and start the Notebook with no browser.

```
$ jupyter notebook --no-browser
```

#### Step 3 - Open tunnel

```
ssh -L 8000:localhost:8888 baldo@bm-pc
```

Here, all the traffic from the server will be redirected to the local computer on the port 8000. We can see the jupyter notebook in our local web browser by typing the address `http://localhost:8000`.









