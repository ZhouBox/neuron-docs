# 安装

本节主要介绍了如何在 X86 / ARM Linux 系统上首次安装 Neuron 软件包。

## 下载

Neuron 软件包可从 Neuron 官网 [https://neugates.io/zh/downloads](https://neugates.io/zh/downloads) 上下载。

| 下载文件                      | 架构    |
| ---------------------------- | ------ |
| neuron-x.y.z-linux-amd64.deb | X86_64 |
| neuron-x.y.z-linux-armhf.deb | ARM_32 |
| neuron-x.y.z-linux-arm64.deb | ARM_64 |

版本号 x.y.z 说明：

* x 为主要版本号，如果整个系统结构得到增强，则可能会更改。
* y 是次要版本号，如果存在某些附加功能，则可能会更改。
* z 是Neuron软件中错误修复的补丁号。

## 安装条件

| Linux 发行版或设备 | 所需包          |
| :------------ | :---------------- |
| **Debian package system**</br>Ubuntu 20.xx </br>Ubuntu 18.xx | deb/tar.gz |
| **Redhat package system**</br>Contos 8</br>Centos 9 | rpm/tar.gz |

rpm/deb package中使用了 systemd 管理 neuron 进程，建议优先使用 rpm/deb package。

## 使用 deb 包安装

### 安装

根据不同版本及架构安装，例如：

```bash
$ sudo dpkg -i neuron-2.0.1-linux-armhf.deb
```

为避免 ubuntu 系统自动更新时替换 neuron 包，还需要执行以下命令使 neuron 软件包在 apt 升级中保留。

```bash
$ sudo apt-mark hold neuron
```

*注意* 成功安装 deb 包后，自动启动 Neuron

### 卸载

```bash
$ sudo dpkg -r neuron
```

## 使用 rpm 安装包

### 安装

根据不同版本及架构安装，例如：

```bash
$ sudo rpm -i neuron-2.0.1-linux-armhf.rpm --nodeps --force
```

*注意* 成功安装 rpm 包后，自启动 Neuron。

### 卸载

```bash
$ sudo rpm -e neuron
```

## 使用 .tar.gz 安装包

### 下载安装包

根据不同的版本及架构下载，例如：

```bash
$ wget https://www.emqx.com/en/downloads/neuron/2.0.1/neuron-2.0.1-linux-armhf.tar.gz
```

### 解压

```bash
$ sudo tar -zxvf neuron-2.0.1-linux-armhf.tar.gz
$ cd neuron-2.0.1-linux-armhf
```

#### 启动

执行如下命令可在当前终端启动：

```bash
$ ./neuron
```

若想以守护进程方式运行，则可执行如下命令：

```bash
$ ./neuron -d
```

执行如下命令可查看所有命令行可用参数：

```bash
$ ./neuron -h
```

## 使用 Docker 运行

### 获取镜像

docker 镜像请从 [docker hub](https://hub.docker.com) 网站下载。

```bash
$ docker pull neugates/neuron:2.0.1
```

### 启动

```bash
$ docker run -d --name neuron -p 7000:7000 -p 7001:7001 --privileged=true --restart=always neugates/neuron:2.0.1
```

* tcp 7000: 用于访问web。
* tcp 7001: http api端口。（api端口为web端口+1，例如，当web端口映射为8000时，api端口应映射为8001）
* --restart=always: docker进程重启时，自动重启neuron容器。
* --privileged=true：便于排查问题。
* --device /dev/ttyUSB0:/dev/ttyS0: 用于映射串口到docker。

## Neuron 操作

用 rpm 和 deb 方式安装的都可以通过以下指令查看/起停 Neuron：

### 查看 Neuron 状态

```bash
$ sudo systemctl status neuron
```

### 停止 Neuron

```bash
$ sudo systemctl stop neuron
```

#### 重启 Neuron

```bash
$ sudo systemctl restart neuron
```