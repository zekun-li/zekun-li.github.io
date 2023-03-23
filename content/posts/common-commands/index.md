---
title: "Common Commands"
date: 2021-01-05
description: Common Commands
menu:
  sidebar:
    name: Common Commands
    identifier: commands
    weight: 7
tags: ["linux", "command"]
categories: ["linux"]
---


##### Add python2 kernels to jupyter notebook:

(source: https://ipython.readthedocs.io/en/stable/install/kernel_install.html)

```
python2 -m pip install ipykernel
python2 -m ipykernel install --user
```

##### Add python3 kernels to jupyter notebook:

```
python3 -m pip install ipykernel
python3 -m ipykernel install --user
```

##### AttributeError: module 'matplotlib.colors' has no attribute 'to_rgba'


Update matplotlib 
```
python3 -m pip install --upgrade matplotlib
```


##### Customized Datagenerator having AttributeError: ‘ProgbarLogger’ object has no attribute ‘log_values’ 


* Check if the data generator could not load data correctly (0 sample)

	* If have __len__() function but returned 0.

	* __len__() is not required for **object** class, but is required for **keras.utils.Sequence** class.