# BigData Docker

为大数据课程制作的 Docker 环境，模拟多机环境，覆盖课程所需的各种软件环境

项目初衷是帮助大家不受环境配置的困扰，提高学习效率；至于写代码和命令行肯定有一些变化，遇到问题可以直接提 issue

> 本项目借鉴 [docker-hadoop-workbench](https://github.com/bambrow/docker-hadoop-workbench) 项目和 [big-data-europe](https://github.com/big-data-europe) 组织下的多个项目，最终对课程进行一些适应性修改实现工作

**注意**：由于 README 写的比较急，如果有问题请尽量使用仓库的 issue 提交问题，方便我快速更新为更多人解决问题

## 简介

项目使用 [Docker Compose](https://docs.docker.com/compose/) 功能构建了一个多机环境，Compose 包括如下 container ：

- Hadoop
- Hive (with PostgreSQL)
- HBase
- R
- Spark

## 使用说明

### 使用前环境准备

1. 配置一个可以使用 Docker 的环境

   - 使用 VMware 中的 Linux 环境
   - 使用 WSL2
     - 关于 WSL2，我的一篇博客略有介绍：[WSL2](https://blog.holakk.cf/2021/03/20/WSL2%E7%9A%84%E8%AF%A6%E7%BB%86%E9%85%8D%E7%BD%AE%E6%96%B9%E6%A1%88-%E5%8C%85%E6%8B%AC%E4%BB%A3%E7%90%86%E5%92%8Cdocker%E4%BB%A5%E5%8F%8A%E7%BB%88%E7%AB%AF%E9%85%8D%E7%BD%AE/)

2. 安装 Docker 及 Docker Compose

   - 安装 Docker：[Docker](https://docs.docker.com/get-docker/)
     - Docker 没有原生的 Windows 支持，请使用 WSL2 配合 Docker Desktop
     - Docker Desktop 也在 Linux 上有了 Beta 版本，当然并不建议，Linux 请按照文档逐步安装 Docker
   - 安装 Docker Compose：[Docker Compose](https://docs.docker.com/compose/)
     - WSL2 配置方案中的 Docker Desktop 自带了 Docker Compose
     - 普通 Linux 方案请按照文档安装

3. 为 Docker 更换一个合适的源

   - [中科大镜像源](https://mirrors.ustc.edu.cn/help/dockerhub.html)

4. 建立适合版本的本地 HBase 镜像
   - 使用我提供的 DockerFile 建立本地 HBase 镜像，执行命令：
     1. `cd images/HBase-standalone`
     2. `docker build -t bde2020/hbase-standalone:2.0.0-hbase2.2.2 .`

### 启动 Compose

我封装了一个简单的启动脚本，当然大家可以根据自己的情况自行修改

```shell
chmod +x ./start.sh
./start.sh
```

同样的有关关闭也做了一个简单的脚本

```shell
chmod +x ./stop.sh
./stop.sh
```

### 注意事项

1. 请务必保证已经建立了本地 HBase 镜像，由于这部分基本工作参考了 big-data-europe 组织的项目，不打算自行推送镜像到 Docker Hub（或许之后给他们提个 PR 就好了）
2. Compose 的实现哲学不希望用户大量修改其中配置，请在确保完全完成任务后再 stop Compose，否则你的代码、手动安装的东西就全丢失了
3. 由于模拟了多机环境，需要大家自行考虑更改代码或者命令行指令，项目仅仅帮助大家减少配环境的痛苦
4. 项目使用 PostgreSQL 替代课程中的 MySQL，虽然他们操作基本相同，但还是要留意一些，有问题欢迎提问
5. 本项目不提供 Eclipse 之类的 IDE 环境，这个问题可以通过 VScode 的 Remote 功能解决，当然如果不适应那就只能承受配环境的痛苦咯（〒 ▽ 〒）

## 启动后的一些使用

- hadoop 相关的操作请进入 namenode 实现
- hive 相关的操作请进入 hive-server 实现
- hive 中 PostgreSQL 相关的操作请进入 hive-metastore-postgresql 实现
- HBase 相关的操作 请进入 hbase 实现
- R 语言相关的数据分析操作请进入 R-data-analysis 实现

进入的指令统一为 `docker exec -it <container_name> /bin/bash`

有兴趣的话可以看 Compose yaml 文件来了解更多实现细节

下面列出一些 Compose 启动后或许有用的端口

- Namenode: <http://localhost:9870/dfshealth.html#tab-overview>
- Datanode: <http://localhost:9864/>
- ResourceManager: <http://localhost:8088/cluster>
- NodeManager: <http://localhost:8042/node>
- HistoryServer: <http://localhost:8188/applicationhistory>
- HiveServer2: <http://localhost:10002/>
- Spark Master: <http://localhost:8080/>
- Spark Worker: <http://localhost:8081/>
- Spark Job WebUI: <http://localhost:4040/> (only works when Spark job is running on spark-master)
- Presto WebUI: <http://localhost:8090/>
- Spark History Server：<http://localhost:18080/>

## PR

项目不能完全保证覆盖使用需求，欢迎有能力的同学帮助解答疑问和提交 PR 完善项目

## 联系我

由于项目用于课程使用，请尽量在 issue 中提问
