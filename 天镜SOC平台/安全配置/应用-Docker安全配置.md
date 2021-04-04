# Docker 安全配置

Docker是一个开源的引擎，可以轻松地为任何应用创建一个轻量级的、可移植的、自给自足的容器。本文介绍了使用Docker服务的安全加固方案，帮助您搭建一个安全可靠的容器集成环境。

### 加固主机操作系统

在部署前需要对服务器操作系统进行安全加固，例如，更新所有软件补丁、配置强密码、关闭不必要的服务端口等。具体请参考以下内容：

- [Windows操作系统安全加固](https://www.alibabacloud.com/help/zh/doc-detail/49781.htm)
- [Linux操作系统加固](https://www.alibabacloud.com/help/zh/doc-detail/49809.htm)

### 使用强制访问控制策略

启用强制访问控制（Mandatory Access Control (MAC)），根据业务场景的具体分析，对Docker中使用的各种资源设置访问控制。

启用AppAamor功能：



```
docker run --interactive --tty --security-opt="apparmor:PROFILENAME" centos/bin/bash
```

启用SElinux功能：



```
docker daemon --selinux-enabled
```

### 配置严格的网络访问控制策略

根据实际应用，对需要外网访问的端口（例如**管理界面、API 2375端口**等重要端口）、需要与外网交互的网络地址、端口、协议等进行梳理，使用iptables或[ECS安全组策略](https://www.alibabacloud.com/help/zh/doc-detail/25475.htm)对网络的出入设置严格的访问控制。

### 不要使用root用户运行docker应用程序

在软件使用中，有一些必须由root用户才能够进行的操作。但从安全角度，您需要将这一部分操作与仅使用普通用户权限即可执行的操作分离解耦。

在编写dockerfile时，您可以使用类似如下的命令创建一个普通权限用户，并设置创建的UID为以后运行程序的用户：



```
RUN useradd noroot -u 1000 -s /bin/bash --no-create-homeUSER norootRUN Application_name
```

Docker命令参考：

- https://docs.docker.com/reference/builder/#user
- https://docs.docker.com/reference/builder/#run

### 禁止使用特权

默认情况下，Docker容器是没有特权的，一个容器不允许访问任何设备；但当使用`--privileged`选项时，则该容器将能访问所有设备。

例如，当打开`--privileged`选项后，您就可以对Host中`/dev/`下的所有设备进行操作。但如果不是必须对host上的所有设备进行访问的话，您可以使用`--device`仅添加需要操作的设备。

### 控制Docker容器资源配额

#### 控制CPU资源配额

**控制CPU份额**

- Docker提供`–cpu-shares`参数，用于在创建容器时指定容器所使用的CPU份额值。

  使用示例： 当使用命令`docker run -tid –cpu-shares 100 ubuntu:stress`创建容器时，则最终生成的cgroup的CPU份额配置可以下面的文件中找到。

  

  ```
  root@ubuntu:~# cat /sys/fs/cgroup/cpu/docker/<容器的完整长ID>/cpu.shares100
  ```

  `cpu-shares`的值不能保证可以获得1个vcpu或者多少GHz的CPU资源，仅仅只是一个弹性的加权值。

- Docker提供了`–cpu-period`、`–cpu-quota`两个参数，用来控制容器可以分配到的CPU时钟周期。

  `–cpu-period`用来指定容器对CPU的使用要在多长时间内做一次重新分配，而`–cpu-quota`是用来指定在这个周期内，最多可以有多少时间用来运行这个容器。与`–cpu-shares`不同的是，这种配置是指定一个绝对值，而且没有弹性在里面，容器对CPU资源的使用绝对不会超过配置的值。

  `cpu-period`和`cpu-quota`的单位为微秒（μs）。`cpu-period`的最小值为1000微秒，最大值为1秒（10^6 μs），默认值为0.1秒（100000 μs）。`cpu-quota`的值默认为-1，表示不做控制。

  举例说明，如果容器进程需要每1秒使用单个CPU的0.2秒时间，可以将`cpu-period`设置为1000000（即1秒），cpu-quota设置为200000（0.2秒）。当然，在多核情况下，如果允许容器进程需要完全占用两个CPU，则可以将`cpu-period`设置为100000（即0.1秒），`cpu-quota`设置为200000（0.2秒）。

  使用示例：使用如下命令来创建容器`docker run -tid –cpu-period 100000 –cpu-quota 200000 ubuntu`。

**控制CPU内核**

对于多核CPU的服务器，使用`–cpuset-cpus`和`–cpuset-mems`参数，可以限定容器运行使用哪些CPU内核和内存节点。

该功能可以对需要高性能计算的容器进行性能最优的配置，对具有NUMA拓扑（具有多CPU、多内存节点）的服务器尤其有用。而如果服务器只有一个内存节点，则`–cpuset-mems`的配置基本上不会有明显效果。

使用示例：使用命令`docker run -tid –name cpu1 –cpuset-cpus 0-2 ubuntu`，表示创建的容器只能用0、1、2这三个内核。

**混合使用CPU配额控制参数**

在上述参数中，`cpu-shares`控制只用在容器竞争同一个内核的时间片时。如果通过`cpuset-cpus`指定容器A使用内核0，容器B只使用内核1，则在主机上只有这两个容器使用对应内核的情况，它们各自占用全部的内核资源，`cpu-shares`没有明显效果。

`cpu-period`、`cpu-quota`这两个参数一般联合使用。在单核或者通过`cpuset-cpus`强制容器使用一个CPU内核的情况下，即使`cpu-quota`超过`cpu-period`，也不会使容器使用更多的CPU资源。

`cpuset-cpus`、`cpuset-mems`只对多核、多内存节点上的服务器有效，并且必须与实际的物理配置匹配，否则也无法达到资源控制的目的。

#### 控制内存配额

和CPU控制一样，Docker也提供了若干参数来控制容器的内存使用配额，可以控制容器的swap大小、可用内存大小等。主要有以下参数：

- `memory-swappiness`：控制进程将物理内存交换到swap分区的倾向，默认系数为60。系数越小，就越倾向于使用物理内存。取值范围为0-100。当值为100时，表示尽量使用swap分区；当值为0时，表示禁用容器swap功能。这点不同于宿主机，宿主机swappiness设置为0时，也不会禁用swap。
- `–kernel-memory`：内核内存，不会被交换到swap上。一般情况下，不建议修改，可以直接参考Docker的官方文档。
- `–memory`：设置容器使用的最大内存上限。默认单位为byte，可以使用K、G、M等带单位的字符串。
- `–memory-reservation`：启用弹性的内存共享。当宿主机资源充足时，允许容器尽量多地使用内存，当检测到内存竞争或者低内存时，强制将容器的内存降低到`memory-reservation`所指定的内存大小。不设置此选项时，有可能出现某些容器长时间占用大量内存，带来性能上的损失。
- `–memory-swap`：等于内存和swap分区大小的总和。设置为-1时，表示swap分区的大小是无限的。默认单位为byte，可以使用K、G、M等带单位的字符串。如果`–memory-swap`的设置值小于`–memory`的值，则使用默认值，为`–memory-swap`值的两倍。

### 不要运行不可信的Docker镜像

不要运行不可信的Docker镜像作为互联网服务器，避免运行不完全理解的Docker镜像作为互联网服务器。

### 开启日志记录功能

Docker的日志可以分成两类，一类是stdout标准输出，另一类是文件日志。Dockerd支持的日志级别有debug、info、warn、error、fatal，默认的日志级别为info。

必要的情况下，您需要设置日志级别，这可以通过配置文件，或者启动参数`-l`或`--log-level`来完成。

**方法一**：修改配置文件`/etc/docker/daemon.json`。



```
{  "log-level": "debug"}
```

**方法二**：使用`docker run`的时候指定`--log-driver=syslog --log-opt syslog-facility=daemon`。

### 定期安全扫描和更新补丁

在生产环境中使用漏洞扫描工具可以检测镜像中的已知漏洞。

- 容器通常都不是从头开始构建的，所以一定要进行安全扫描，以便及时发现基础镜像中任何可能存在的漏洞，并及时更新补丁。
- 在应用程序交付生命周期中加入漏洞扫描的安全质量控制，防止部署易受攻击的容器。

通过采用以上积极的防范措施，即在整个容器的生命周期中建立和实施安全策略，可以有效地保证一个集成容器环境的安全性。

### 参考文档

[1]. https://benchmarks.cisecurity.org/downloads/show-single/index.cfm?file=docker16.100
[2]. https://linux-audit.com/docker-security-best-practices-for-your-vessel-and-containers/-
[3]. [https://d3oypxn00j2a10.cloudfront.net/assets/img/Docker%20Security/WP_Intro_to_container_security_03.20.2015.pdf](https://d3oypxn00j2a10.cloudfront.net/assets/img/Docker Security/WP_Intro_to_container_security_03.20.2015.pdf)
[4]. https://docs.docker.com/edge/engine/reference/commandline/dockerd/