

# Install




# 

```
geth --testnet --syncmode "fast" --rpc --rpcapi db,eth,net,web3,personal --cache=1024  --rpcport 8545 --rpcaddr 127.0.0.1 --rpccorsdomain "*" --bootnodes "enode://20c9ad97c081d63397d7b685a412227a40e23c8bdc6688c6f37e97cfbc22d2b4d1db1510d8f61e6a8866ad7f0e17c02b14182d37ea7c3c8b9c2683aeb6b733a1@52.169.14.227:30303,enode://6ce05930c72abc632c58e2e4324f7c7ea478cec0ed4fa2528982cf34483094e9cbc9216e7aa349691242576d552a2a56aaeae426c5303ded677ce455ba1acd9d@13.84.180.240:30303"
```


var greeterCompiled = web3.eth.compile.solidity(greeterSource)


# 转账

eth.sendTransaction({from: '0x608f1e00d17444dd6ea61490c5c9130e2eee5455', to: '0x36607d6a5fd3243ca7ad1f4c74031b3c9c535f47', value: web3.toWei(1, "ether")})


# 创建账号

```
personal.newAccount("Account Password")
```

# 解锁账号

在部署合约前需要先解锁账户

```
personal.unlockAccount(eth.accounts[1],"Account Password");
```


# 挖矿

开始挖矿

```
miner.start(1)
```

停止挖矿：

```
miner.stop(1)
```


参考：

- [https://learnblockchain.cn/2017/11/29/geth_cmd_options/](https://learnblockchain.cn/2017/11/29/geth_cmd_options/)