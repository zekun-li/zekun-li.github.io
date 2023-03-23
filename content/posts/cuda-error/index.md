---
title: "CUDA Errors and Solutions"
date: 2020-06-08T08:06:25+06:00
description: CUDA Errors and Solutions
menu:
  sidebar:
    name: CUDA Errors and Solutions
    identifier: cuda-error
    weight: 2
tags: ["linux", "cuda"]
categories: ["linux"]
---


CUDA is a parallel computing model created by Nvidia to assist the usage of GPU. It is widely used in the training of deep-learning models. It greatly speed up the computation in model training compared with its CPU-based counterpart. However we might encouter problems when using it, and here is a summarization of some common problems and the solutions. Please note that this post is not emphasizing on the installation of CUDA. For the installation, please refer to [here](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).

#### Unexpected cublas Error

CUDA has been running smoothly for all the experiments until at one point, it throws error

```
 2018-10-23 16:48:45.860068: E tensorflow/stream_executor/cuda/cuda_blas.cc:366] failed to create cublas handle: CUBLAS_STATUS_NOT_INITIALIZED
 ```

 You check the GPU usage and definitely it is 0%. But there is no other process running on that GPU competing for resource. You have exactly the same environenment and the same code, but it just doesn’t work anymore. You scratch your head and don’t know what to do.

 The reason for that is, when a cuda job crashes, the cache get corrupted. It couldn’t re-collect the resource that it allocated in the begining. When the crash happens a lot of times, the resource would be used up and then it reports fail to create cublas error. A simple solution would be clean the cache.

 ```
  rm -r ~/.nv
 ```

 If this problem happens frequently, and have to run the above command very often, you might want to get out of the trouble by disabling the cache in the .bashrc file by adding this:

 ```
 CUDA_CACHE_DISABLE = 1
 ```

