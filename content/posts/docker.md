---
title: "Docker"
date: 2020-03-01T16:40:23+08:00
draft: false
author: "kenwood"
isCJKLanguage: true
tags: ["Docker"]
categories: ["Docker"]
---

### docker namespace

#### 概念

首先让我们看看docker 官方文档的解释

![docker官网](/images/docker01.jpg)

当你启动一个container，docker会为container创建 一系列的namespaces 

##### 小测试

```shell
# 在容器里运行
watch 'ps ax'
Every 2s: ps ax                                                                                                                                                                            2020-03-01 09:44:12

PID   USER     TIME  COMMAND
    1 root      0:03 /sbin/runsvdir -P /etc/service/enabled
   64 root      0:00 runsv felix
   65 root      0:00 runsv bird
   66 root      0:00 runsv bird6
   67 root      0:00 runsv confd
   69 root      1h21 calico-node -felix
   71 root      0:28 calico-node -confd
  176 root      1:38 bird -R -s /var/run/calico/bird.ctl -d -c /etc/calico/confd/config/bird.cfg
  177 root      1:26 bird6 -R -s /var/run/calico/bird6.ctl -d -c /etc/calico/confd/config/bird6.cfg
28692 root      0:00 sh
28720 root      0:00 watch ps ax
29670 root      0:00 ps ax
# 查看 宿主机pid
21609 pts/0    S+     0:00 watch ps ax
# 同样的进程不同的pid
```

```shell
# 查看进程树
pstree -p 
systemd(1)─┬─NetworkManager(974)─┬─{NetworkManager}(1034)
           │                     └─{NetworkManager}(1036)
           ├─agetty(6593)
           ├─aksusbd_x86_64(31834)─┬─{aksusbd_x86_64}(31835)
           │                       └─{aksusbd_x86_64}(31836)
           ├─auditd(934)───{auditd}(935)
           ├─chronyd(1003)
           ├─containerd-shim(1335)─┬─{containerd-shim}(1336)
           │                       ├─{containerd-shim}(1337)
           │                       ├─{containerd-shim}(1338)
           │                       ├─{containerd-shim}(1339)
           │                       ├─{containerd-shim}(1340)
           │                       ├─{containerd-shim}(1341)
           │                       ├─{containerd-shim}(1342)
           │                       ├─{containerd-shim}(1343)
           │                       └─{containerd-shim}(5137)
           ├─containerd-shim(1366)─┬─{containerd-shim}(1367)
           │                       ├─{containerd-shim}(1368)
           │                       ├─{containerd-shim}(1369)
           │                       ├─{containerd-shim}(1372)
           │                       ├─{containerd-shim}(1373)
           │                       ├─{containerd-shim}(1374)
           │                       ├─{containerd-shim}(1376)
           │                       ├─{containerd-shim}(1378)
           │                       └─{containerd-shim}(5139)
           ├─crond(1024)
           ├─dbus-daemon(967)
           ├─dockerd(1419)─┬─containerd(1433)─┬─containerd-shim(2424)─┬─pause(2498)
           │               │                  │                       ├─{containerd-shim}(2426)
           │               │                  │                       ├─{containerd-shim}(2427)
           │               │                  │                       ├─{containerd-shim}(2428)
           │               │                  │                       ├─{containerd-shim}(2430)
           │               │                  │                       ├─{containerd-shim}(2431)
           │               │                  │                       ├─{containerd-shim}(2432)
           │               │                  │                       ├─{containerd-shim}(2435)
           │               │                  │                       ├─{containerd-shim}(2438)
           │               │                  │                       ├─{containerd-shim}(2616)
           │               │                  │                       └─{containerd-shim}(5154)

```

docker 启动流程

dockerd-> containerd->containerd-shim-puase

#### containerd

**containerd** is available as a daemon for Linux and Windows. It manages the complete container lifecycle of its host system, from image transfer and storage to container execution and supervision to low-level storage to network attachments and beyond.

[containerd 官网]: https://containerd.io/
[containerd github]: https://github.com/containerd/containerd

容器化的运行时要求非常低。 与Linux和Windows容器功能集的大多数交互都是通过runc和/或特定于操作系统的库（例如，适用于Microsoft的hcsshim）处理的。 RUNC.md中始终列出了当前所需的runc版本。

#### runc

什么是runc

`runc` is a CLI tool for spawning and running containers according to the OCI specification.

[runc github]: https://github.com/opencontainers/runc

#### 跟踪

```
strace -f -p `pidof containerd` -o strace_log
# 查找关键词unshare
 unshare(CLONE_NEWNS|CLONE_NEWUTS|CLONE_NEWIPC|CLONE_NEWPID|CLONE_NEWNET <unfinished ...>

```

什么是unshare

取消共享-运行具有父级未共享的某些名称空间的程序

**http://man7.org/linux/man-pages/man1/unshare.1.html**

unshare/clone/syscalls: please fake pid for child"