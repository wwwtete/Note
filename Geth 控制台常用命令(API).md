在[上一篇][1]文章中详细描述了如何搭建私有链，今天总结一下
Geth控制台常用的命令。
#### Geth命令基本用法
``` stylus
geth [选项] 命令 [命令选项] [参数…]
```
####  attach 节点
$geth attach ipc:/node/data/geth.ipc
$ geth attach http://ip:ipcport
attach节点有多种方式，不同的attach方式可用的`modules`也不相同
- 通过ipc文件路径attach到节点
``` stylus
geth attach enode2/data/geth.ipc 
Welcome to the Geth JavaScript console!

instance: Geth/enode2/v1.7.3-stable/darwin-amd64/go1.9.3
coinbase: 0x0cfd3ab46d552d30f8b1c4f1b08a90f40192de97
at block: 0 (Fri, 09 Feb 2018 14:41:48 CST)
 datadir: /Users/wangw/blockchain/eth/enode2/data
 modules: admin:1.0 debug:1.0 eth:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0 web3:1.0
```
- 通过IPC的Address ip地址attach到节点
``` stylus
geth attach http://192.168.1.128:8121
Welcome to the Geth JavaScript console!

instance: Geth/enode1/v1.7.3-stable/darwin-amd64/go1.9.3
coinbase: 0xc43881d20bb2a2b6d72801b4bc04cdcc35950a8b
at block: 0 (Fri, 09 Feb 2018 14:41:48 CST)
 modules: eth:1.0 net:1.0 personal:1.0 rpc:1.0 web3:1.0
```
#### 添加节点
在私有链中设置的节点，默认是不会互相发现并连接的，需要手动添加节点
首先attach节点1，获取节点地址
``` stylus
geth attach enode1/data/geth.ipc
Welcome to the Geth JavaScript console!

instance: Geth/enode1/v1.7.3-stable/darwin-amd64/go1.9.3
coinbase: 0xc43881d20bb2a2b6d72801b4bc04cdcc35950a8b
at block: 0 (Fri, 09 Feb 2018 14:41:48 CST)
 datadir: /Users/wangw/blockchain/eth/enode1/data
 modules: admin:1.0 debug:1.0 eth:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0 web3:1.0

> admin.nodeInfo.enode
"enode://5de4d18352a3a554b9487bf1453a7b6f0d1bada4fe90b0d819f72dcb6d06f29562fa5ebdc53fe4b63a63971dee365f56b01f3f5afefafdd4df3b6786062f8ce8@[::]:2001?discport=0"
> 
```
在返回的节点地址中，地址组成部分是: `enode://<节点id>@[IP地址]:端口`
将Ip地址部分替换成127.0.0.1(如果是在多台电脑上，则应替换成当前节点所在电脑的Ip地址)






  [1]: https://www.jianshu.com/p/2c2671ff1d5f