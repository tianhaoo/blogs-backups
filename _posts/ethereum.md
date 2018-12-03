---
title: ethereum
date: 2018-05-26 10:26:33
categories:
- 区块连
tags:
    - ethereum
---
# 在ubuntu18上面学习Ethereum

> 公钥是公开的，任何人都可以获取。私钥是保密的，只有拥有者才能使用。他人使用你的公钥加密信息，然后发送给你，你用私钥解密，取出信息。反过来，你也可以用私钥加密信息，别人用你的公钥解开，从而证明这个信息确实是你发出的，且未被篡改，这叫做数字签名，[有关数字签名](http://www.ruanyifeng.com/blog/2011/08/what_is_a_digital_signature.html)。10.10.

## 配置go语言环境

1. [下载](https://golang.org/dl/)

2. `tar zxvf go1.8.3.linux-amd64.tar.gz -C $GO_INSTALL_DIR`

3. `export PATH=$PATH:$GO_INSTALL_DIR/go/bin`

4. `go`

5. gvm(可选)`bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)`

<!--more-->

## 下载以太坊钱包：[ethereum/mist](https://github.com/ethereum/mist/releases)

1. 选择Rinkeby测试网络([区块链浏览器](https://rinkeby.etherscan.io/))

## geth客户端

1. [下载](https://ethereum.github.io/go-ethereum/)

2. `sudo tar zxvf geth-linux-amd64-1.8.8-2688dab4.tar.gz -C /opt`

3. `exprt PATH=$PATH:/opt/geth`

## 搭建一个私有链

### 初始化

```js
{
  "config": {
        "chainId": 12,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
  "alloc"      : {},
  "coinbase"   : "0x0000000000000000000000000000000000000000",
  "difficulty" : "0x20000",
  "extraData"  : "",
  "gasLimit"   : "0x2fefd8",
  "nonce"      : "0x0000000000000042",
  "mixhash"    : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "timestamp"  : "0x00"
}
```

```bash
geth init ./genesis.json --datadir "./chain"
```

### 启用私链

```bash
geth --networkid 12 --datadir "./chain"  console
```

上面命令的主体是geth console，表示启动节点并进入交互式控制台，--datadir选项指定使用./chain作为数据目录，--networkid选项后面跟一个数字，这里是12，表示指定这个私有链的网络id为12。网络id在连接到其他节点的时候会用到，以太坊公网的网络id是1，为了不与公有链网络冲突，运行私有链节点的时候要指定自己的网络id。

运行上面的命令后，就启动了区块链节点并进入了Javascript Console：

```bash
...
Welcome to the Geth JavaScript console!

instance: Geth/v1.5.6-stable/linux/go1.7.3
 modules: admin:1.0 debug:1.0 eth:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0 web3:1.0

>
```

* eth：包含一些跟操作区块链相关的方法
* net：包含以下查看p2p网络状态的方法
* admin：包含一些与管理节点相关的方法
* miner：包含启动&停止挖矿的一些方法
* personal：主要包含一些管理账户的方法
* txpool：包含一些查看交易内存池的方法
* web3：包含了以上对象，还包含一些单位换算的方法

### 创建账户

```js
eth.accounts
personal.newAccount()
```

### 查看账户余额

```js
eth.getBalance(eth.accounts[0])
```

### 启动&停止挖矿

```js
miner.start(1)
miner.stop()
```

### 发送交易

```js
amount = web3.toWei(5,'ether')
eth.sendTransaction({from:eth.accounts[0],to:eth.accounts[1],value:amount})
personal.unlockAccount(eth.accounts[0])
txpool.status
eth.blockNumber
```

## 在以太坊公链上利用CPU挖矿

### 新建一个目录

```bash
geth --networkid 1 --datadir "./chain"  console
```

### 创建公链账户

```js
eth.accounts
personal.newAccount()
```

### 开始挖矿

```js
miner.start(4:)
miner.stop()
```

以太坊会自动同步，下载整个链到本地，目前大概80多个G，所以最好准备大一点的磁盘

### 查看同步是否完成

```js
eth.synchronizing
```
