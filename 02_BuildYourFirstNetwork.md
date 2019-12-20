# 02 构建你的第一个Fabric网络



文档： https://hyperledger-fabric.readthedocs.io/en/release-1.4/build_network.html

构建你的第一个网络(BYFN) 的场景提供了由2个组织，每个组织维护2个peer对等节点的Hyperledger Fabric网络。这个网络默认部署了一个独立的排序服务，当然其他的排序服务的实现也是可以的。

4 peer 

1 orderer

1 cli

估计需要6个docker

## 环境准备
---


禁用 vbguest 的自动更新，加速vagrant启动速度


更新系统:
```
yum update
yum upgrade
```


安装 curl, wget, git, vim/emacs/nano, golang


## 安装prerequest
---


https://docs.docker.com/install/linux/docker-ce/centos/

containerd.io 的版本 1.2.0 而最新的 docker-ce 19.03.5 要求 containerd.io 必须 >= 1.2.2

```
$sudo yum install docker-ce docker-ce-cli containerd.io

Last metadata expiration check: 0:53:58 ago on Wed 18 Dec 2019 01:49:26 AM UTC.
Error:
 Problem: package docker-ce-3:19.03.5-3.el7.x86_64 requires containerd.io >= 1.2.2-3, but none of the providers can be installed
  - cannot install the best candidate for the job
  - package containerd.io-1.2.10-3.2.el7.x86_64 is excluded
  - package containerd.io-1.2.2-3.3.el7.x86_64 is excluded
  - package containerd.io-1.2.2-3.el7.x86_64 is excluded
  - package containerd.io-1.2.4-3.1.el7.x86_64 is excluded
  - package containerd.io-1.2.5-3.1.el7.x86_64 is excluded
  - package containerd.io-1.2.6-3.3.el7.x86_64 is excluded
(try to add '--skip-broken' to skip uninstallable packages or '--nobest' to use not only best candidate packages)

```


### 安装符合最新版docker-ce要求的containerd.io
---

有以下3中安装形式：

1.  软件源直接安装

```
sudo yum install --nobest docker-ce docker-ce-cli containerd.io
```

2. 下载 rpm 包

https://download.docker.com/linux/centos/7/x86_64/stable/Packages/

rpm 包解压后的目录结构

```
.
├── etc
│   └── containerd
│       └── config.toml
└── usr
    ├── bin
    │   ├── containerd
    │   ├── containerd-shim
    │   ├── ctr
    │   └── runc
    ├── lib
    │   └── systemd
    │       └── system
    │           └── containerd.service
    └── share
        ├── doc
        │   └── containerd.io-1.2.10
        │       └── README.md
        ├── licenses
        │   └── containerd.io-1.2.10
        │       └── LICENSE
        └── man
            ├── man1
            │   ├── containerd.1
            │   ├── containerd-config.1
            │   └── ctr.1
            └── man5
                └── containerd-config.toml.5

15 directories, 12 files
```

```
 sudo yum install -y ./containerd.io-1.2.10-3.2.el7.x86_64.rpm
```


3. 手动编译


编译containerd
<hr/>

```
# v1.2.10

./bin
├── containerd
├── containerd-shim
├── containerd-shim-runc-v1
#├── containerd-shim-runc-v2 // v1.3.0 中生成的
├── containerd-stress
└── ctr
```

~~与rpm包执行程序不匹配，需要手动调整，将 containerd-shim-runc-v1 改为 runc 或者 做软连接~~

编译 runc
<hr/>

生成的二进制执行文件中没有runc，而runc 需要从 https://github.com/opencontainers/runc 编译， 对应的源码版本：

```
runc version 1.0.0-rc8+dev
commit: 3e425f80a8c931f88e6d94a8c831b9d5aa481657
spec: 1.0.1-dev
```

手动创建 systemd 和 配置文件
<hr/>


containerd.io安装完后，重新执行下面的命令来安装docker-ce

```
 sudo yum install docker-ce docker-ce-cli
```


### 安装docker-compose
---

https://docs.docker.com/compose/install/


```
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

```


### 安装 node.js

https://github.com/nodesource/distributions/blob/master/README.md



```
curl -sL https://rpm.nodesource.com/setup_10.x | bash -

## Run `sudo yum install -y nodejs` to install Node.js 10.x and npm.
## You may also need development tools to build native addons:
     sudo yum install gcc-c++ make
## To install the Yarn package manager, run:
     curl -sL https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
     sudo yum install yarn

```

python 默认没有安装，后续需要时再安装



## 安装示例、二进制文件和Docker镜像
---


https://hyperledger-fabric.readthedocs.io/en/release-1.4/install.html


随机启动Docker
```
sudo systemctl enable docker.service
```

### 下载示例仓库

https://github.com/hyperledger/fabric-samples.git

