---
title: "Run jupyter notebook in docker container on remote server"
date: 2021-08-08
description: Tutorial of jupyter notebook with docker container
menu:
  sidebar:
    name: Run jupyter notebook in docker container on remote server
    identifier: jupyter-docker
    weight: 1
tags: ["docker", "command"]
categories: ["linux"]
---

**Jupyter notebook** is an extremely convieninent tool for debugging a piece of code. It runs on browser and allows you to run code piece by piece instead of fresh from the begining. With docker, jupyter notebook is slightly more confusing than it is on local machine. This short article talks about how to correctly forward the port to run jupyter notebook in docker container on remote server.

We will have three locations to deal with; local machine, remote server, docker container. The port forwarding will build a link docker container -> remote server -> local machine.

Same as usual, you should first log onto the server through ssh (see sample code below). You should replace `zekunl` with your user name and `arya.usc.edu` with your server ip address.

```
ssh zekunl@arya.usc.edu
```
On the remote server, you can create a docker container by
```
 sudo nvidia-docker run --rm -ti  -p 9999:8888  -v /home/zekunl/detectNet:/home/zekunl/detectNet/ spatialcomputing/deep-learning-env-gpu
```
&nbsp;

the `-p` parameter maps the port inside container to remote server. `9999` is the <mark>remote server port</mark>, and `8888` is the <mark> container port</mark>. Remote server port can be any un-used port but the container port **MUST** be 8888 since its the port allocated for jupyter notebook.

If you have multiple docker instances and you’ve run the above command in the first instance, then in the other instance, you can still map the `8888` port in the container, but the second instance can not map to the `9999` port on remote server anymore, because it’s alreay occupied by the first docker instance.

Then we put up jupyter notebook inside the container:
```
jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser
```

The last thing is to forward the remote server port to local machine:

```
ssh -N -L 9999:localhost:9999 zekunl@arya.usc.edu
```

If you open your browser and type [http://localhost:9999](http://localhost:9999), you will be prompted to enter the token for verification and the content inside docker will show up in the browser.

