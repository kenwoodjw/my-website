---
title: "Docker组件构成"
date: 2019-05-23T23:30:31+08:00
draft: false
author: "kenwood"
isCJKLanguage: true
tags: ["Docker"]
---

### 

### 历史

docker公司原名dotclound, Docker项目刚刚兴起时，Google也开源了一个在内部使用多年，经历过生产环境验证的Linux容器：lmctfy(Let Me Container That For You),面对Docker项目的强势崛起，google向docker公司表示了合作的愿望，关停lmctfy，和Docker公司共同推进一个中立的container runtime库，作为Docker项目的核心依赖。

不过,Docker 公司并没有认同这个明显会削弱自己地位的提议,还在不久后,自己发布了一个容器运行时库 Libcontainer。这次匆忙的、由一家主导的、并带有战略性考量的重构,成了Libcontainer 被社区长期诟病代码可读性差、可维护性不强的一个重要原因。至此,Docker 公司在容器运行时层面上的强硬态度,以及 Docker 项目在高速迭代中表现出来的不稳定和频繁变更的问题,开始让社区叫苦不迭。这种情绪在 2015 年达到了一个小高潮,容器领域的其他几位玩家开始商议“切割”Docker 项目的话语权。而“切割”的手段也非常经典,那就是成立一个中立的基金会。

2015 年 6 月 22 日,由 Docker 公司牵头,CoreOS、Google、RedHat 等公司共同宣布,Docker 公司将 Libcontainer 捐出,并改名为 RunC 项目,交由一个完全中立的基金会管理,然后以 RunC 为依据,大家共同制定一套容器和镜像的标准和规范。这套标准和规范,就是 OCI( Open Container Initiative )。OCI 的提出,意在将容器运行时和镜像的实现从 Docker 项目中完全剥离出来。这样做,一方面可以改善 Docker 公司在容器技术上一家独大的现状,另一方面也为其他玩家不依赖于 Docker 项目构建各自的平台层能力提供了可能。

所以这次,Google、RedHat 等开源基础设施领域玩家们,共同牵头发起了一个名为CNCF(Cloud Native Computing Foundation)的基金会。这个基金会的目的其实很容易理解:它希望,以 Kubernetes 项目为基础,建立一个由开源基础设施领域厂商主导的、按照独立基金会方式运营的平台级社区,来对抗以 Docker 公司为核心的容器商业生态。

面对 Kubernetes 社区的崛起和壮大,Docker 公司也不得不面对自己豪赌失败的现实。但在早前拒绝了微软的天价收购之后,Docker 公司实际上已经没有什么回旋余地,只能选择逐步放弃开源社区而专注于自己的商业化转型。所以,从 2017 年开始,Docker 公司先是将 Docker 项目的容器运行时部分 Containerd 捐赠给 CNCF 社区,标志着 Docker 项目已经全面升级成为一个 PaaS 平台;紧接着,Docker 公司布将 Docker 项目改名为 Moby,然后交给社区自行维护,而 Docker 公司的商业产品将占有Docker 这个注册商标。

### Docker核心组件

{{< figure src="/images/1558626500957.png" width="" height="" >}}

#### Docker Daemon

在docker1.8之前，docker守护进程启动命令为

```
docker -d
```

Docker1.8开始，启动命令变成了

```
docker daemon
```

Docker 1.11开始，守护进程启动命令变成了

```
dockerd

```

守护进程模块不停的在重构，其基本功能和定位没有变化。和一般的 CS 架构系统一样，守护进程负责和 Docker client 交互，并管理 Docker 镜像、容器

#### Containerd

[containerd](https://github.com/docker/containerd)是容器技术标准化之后的产物，为了能够兼容[OCI 标准](https://www.opencontainers.org/)，将容器运行时及其管理功能从 Docker Daemon 剥离。理论上，即使不运行 dockerd，也能够直接通过 containerd 来管理容器。（当然，containerd 本身也只是一个守护进程，容器的实际运行时由后面介绍的 runC 控制

{{< figure src="/images/1558626829831.png" width="" height="" >}}

containerd向上提供grpc接口，向下通过containerd-shim结合runc

Docker、containerd 和 containerd-shim 之间的关系，可以通过启动一个 Docker 容器，观察进程之间的关联。首先启动一个容器，

```
docker run -d busybox sleep 1000

然后通过`pstree`命令查看进程之间的父子关系（其中 20708 是`dockerd`的 PID）：
```

```
pstree -l -a -A 20708
```

```
dockerd -H fd://
  |-containerd --config /var/run/docker/containerd/containerd.toml --log-level info
  |   |-containerd-shim -namespace moby -workdir /var/lib/docker/containerd/daemon/io.containerd.runtime.v1.linux/moby/d092d81e7c9d73d5cd33944095ee78b17ffebeb17336f8537ce34e0f000a28eb -address /var/run/docker/containerd/containerd.sock -containerd-binary /usr/bin/containerd -runtime-root /var/run/docker/runtime-runc
  |   |   |-sshd -D
  |   |   `-10*[{containerd-shim}]
      |   -containerd-shim -namespace moby -workdir /var/lib/docker/containerd/daemon/io.containerd.runtime.v1.linux/moby/c3fb64e2a71e12ca2d641b0c252db0ac92067cd802227dc977e7af2910e6c3fe -address /var/run/docker/containerd/containerd.sock -containerd-binary /usr/bin/containerd -runtime-root /var/run/docker/runtime-runc
          |   |   |-sleep 1000


```

当 Docker daemon 启动之后，dockerd 和 docker-containerd 进程一直存在。当启动容器之后，docker-containerd 进程（也是这里介绍的 containerd 组件）会创建 docker-containerd-shim 进程，其中的参数 c3fb64e2a71e12ca2d641b0c252db0ac92067cd802227dc977e7af2910e6c3fe 就是要启动容器的 id。最后 docker-containerd-shim 子进程，已经是实际在容器中运行的进程（既 sleep 1000）

#### Runc

OCI 定义了容器运行时标准，runC 是 Docker 按照开放容器格式标准（OCF, Open Container Format）制定的一种具体实现。

runC 是从 Docker 的 libcontainer 中迁移而来的，实现了容器启停、资源隔离等功能。Docker 默认提供了 docker-runc 实现，事实上，通过 containerd 的封装，可以在 Docker Daemon 启动的时候指定 runc 的实现。

我们可以通过启动 Docker Daemon 时增加`--add-runtime`参数来选择其他的 runC 现。例如：

```
docker daemon --add-runtime "custom=/usr/local/bin/my-runc-replacement"
```

{{< figure src="/images/1558627532923.png" width="" height="" >}}

### 



参考链接：

https://stackoverflow.com/questions/46649592/dockerd-vs-docker-containerd-vs-docker-runc-vs-docker-containerd-ctr-vs-docker-c

https://segmentfault.com/a/1190000011294361

https://www.infoq.cn/article/2017/02/Docker-Containerd-RunC