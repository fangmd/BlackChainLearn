

代码实现记录

# 安装 各种

## 使用 Docker 部署

```
git clone https://github.com/yutianwu/ethereum-docker

cd ethereum-docker

```

**使用单机模式启动：**

```
# 用docker-compose使用docker-compose-standalone.yml在单机模式下启动，也就是启动一个docker容器。
docker-compose -f docker-compose-standalone.yml up -d

# 使用docker exec进入容器
docker exec -it ethereumdocker_geth_1 geth attach ipc://root/.ethereum/devchain/geth.ipc
```

**使用集群模式启动:**

```
docker-compose up -d

docker ps
```

容器启动之后，我们可以打开http://localhost:3000来查看各个go-eth的状态。

**手动进入容器启动节点开始挖矿**

```
docker exec -it ethereumdocker_eth_1 /bin/bash

miner.start()
```

再打开netstats的监控界面, 就可以看到矿工节点已经开始挖矿并不断生成新的区块

到这里，ethereum私链的集群已经搭建完毕。

## 不使用 Docker

### Go 环境

#### mac

```
brew install go
```

### 安装 ethereum

#### mac

### 安装 solc 编译器

#### mac

```

```

### 安装 Solidity 语言

#### mac

```
# 下面三个在 ethereum 安装的时候可能已经执行
brew update
brew upgrade
brew tap ethereum/ethereum

brew install solidity
```

### install go-ethereum （可选）

[https://github.com/ethereum/go-ethereum/wiki/Installation-Instructions-for-Mac](https://github.com/ethereum/go-ethereum/wiki/Installation-Instructions-for-Mac)

```
brew tap ethereum/ethereum
brew install ethereum

# /usr/local/Cellar/ethereum/1.8.1
```


# 遇到的问题



待看：

- [Ethereum入门：搭建私有链开发环境](https://zhuanlan.zhihu.com/p/26648454)




参考：

- [eth 开发环境部署](http://yutianx.info/2017/10/07/2017-10-07-docker-ethereum/)


