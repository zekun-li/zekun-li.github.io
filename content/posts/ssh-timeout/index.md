---
title: "Stop SSH connection from timeout"
date: 2021-01-05
description: Stop SSH connection from timeout
menu:
  sidebar:
    name: Stop SSH connection from timeout
    identifier: ssh-timeout
    weight: 9
tags: ["ssh", "connection"]
categories: ["linux"]
---

When you're connecting to the server, sometimes you got "Broken Pipe" prompt that kicks you off from the server if you are not active for a while. This can be quite annoying if you have unsaved changes in the code and was just away for a bit searching for some solution. This can be stopped from happening if you could change the ssh configuration on the server.


Open the file /etc/ssh/sshd_config on server and add the following lines in the end.

```
ClientAliveInterval 100

ClientAliveCountMax 3
```


The first line lets the server send out a package to client every 100 seconds and the second line tells the server do not stop connection until the client hasn't been active for 3 queries. So you will have 300 seconds in total before the connection breaks due to inactivity. 


Most importantly, restart the sshd to make the changes effective.

```
sudo service sshd restart
```

or 

```
sudo /etc/init.d/sshd restart
```

Enjoy it!
