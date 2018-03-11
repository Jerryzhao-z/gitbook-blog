# Docker快速上手
Docker可以视为虚拟化容器，即一种微型虚拟机。
## 基本概念

#### 镜像 image
镜像可以看作容器的一个快照，镜像不包含动态数据，仅保留一些参数配置，安装的程序和文件系统。
借助Union FS技术，Docker镜像被设计为层结构，前一层是后一层的基础，在后一层发生的改变只发生在本层，也就是说删除前一层的文件时，文件并不会真的删除，而仅仅被标注为被删除，事实上依然存在于创建层。
#### 容器 container
容器可以视为镜像的一个实例，启动后可以被认为是一个进程。容器进程运行于独立的命名空间，可以有自己的网络配置，用户，文件系统，和进程管理。容器可以通过挂载宿主机目录来保存一些运行在容器时产生的数据，比如log日志。
#### 仓库 repository
仓库就是一个存放docker镜像的远程服务器，可以用于拉取一些云端的镜像。
## 基本操作

从仓库获得镜像

```bash
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]

docker import [OPTIONS] file|URL|- [REPOSITORY[:TAG]] #从本地文件导入镜像

docker images # 列出镜像
docker images -a 
docker image rm [选项] <镜像1> [<镜像2> ...] # 删除镜像
```

获得镜像后我们可以基于镜像生成一个容器

```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

进入操作一个container

```bash
docker attach [OPTIONS] CONTAINER #因为attach使容器与local standard input, output, and error streams连接，所以我们不能使用Ctrl+C发送信号退出，而需要使用CTRL-p CTRL-q退出容器，并保留容器在后台

docker exec [OPTIONS] CONTAINER COMMAND [ARG...] #可以直接用Ctrl+C退出
```
