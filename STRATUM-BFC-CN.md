## Stratum BFC Protocol


### 1.miner->pool订阅服务

```
{"id":100,"method":"mining.subscribe","params":["ccminer/3.4.0"]}

```

矿池返回

```
{"id":100,"result":[null,"01000000"],"error":null}
```
这里返回result第二个参数"01000000" 是nonce1, nonce1是0~32字节长度的16hex数字

矿机应该计算32-sizeof(nonce1) = 个字节


### 2.miner->pool登陆服务

```
{"id":101,"method":"mining.authorize","params":["myminer.1","x"]}
{"id":101,"method":"mining.authorize","params":["FJhruFy3X3N8dHfGyRS5qbYXL9Wg7CKco2.1","x"]}

```
矿池返回
```
{"id":101,"result":true,"error":null}
```

### 3.pool->miner下放任务

```
{"id":null,"method":"mining.notify","params":["0","be0c2691ad202d38a669200e86b7f2f857a0ff39531844799db046263fc2300a","60858d61b171194c942eb3f31227e0d30c98767f3e4d828cfe020f811e234727","00000020","ffff0f20","3eb43a5d",true]}
{"id":null,"method":"mining.notify","params":["1","be0c2691ad202d38a669200e86b7f2f857a0ff39531844799db046263fc2300a","60858d61b171194c942eb3f31227e0d30c98767f3e4d828cfe020f811e234727","00000020","ffff0f20","52b43a5d",false]}
{"id":null,"method":"mining.notify","params":["2","be0c2691ad202d38a669200e86b7f2f857a0ff39531844799db046263fc2300a","60858d61b171194c942eb3f31227e0d30c98767f3e4d828cfe020f811e234727","00000020","ffff0f20","66b43a5d",false]}

```

params:参数
1. JOBID任务id  字符串
2. PREV_HASH上一个任务的hash 大端
3. MERKLE TREE交易的默克尔树 大端
4. VERSION 头版本大端
5. NBITS 头里面nbits字段大端
6. NTIMES 头时间戳大端
7. true|false 矿机接收到任务是否清除之前所有的任务

### 4.pool->miner下放难度

```
{"id":null,"method":"mining.set_target","params":["0fffff0000000000000000000000000000000000000000000000000000000000"]}
```


### 5.miner->pool提交任务


```
{"id":129,"method":"mining.submit","params":["myminer.1","40","5d2fe35c","ded041bca4a64e60e311d8bcd54a2b795515cda3bda14ffd30ab61cc","00000020","a8aba6818f160de66812f651a6acdfe997b14cd8709e242d8bd43c567ee3a506bf216345e0594e63e93a216da9d0440990b546b411c54a80cb72c90c954843d6ae3d60af325959ba58f1348e75cb362b5e9ed4cd42f743521adddce891fbefacd3c5444633de51ab2fb0b2df60e74f82a46b425293a9277bda03ed489c00e0a619f3e17340f7aad9dfd218afb42324351de74493a3e7b5e7352929f1d9c1409dcdcad2820fc713d748"]}

```
params:参数
1. 挖矿的账号/钱包.workerid
2. JOBID
3. 时间戳NTIMES, 应该和notify的保持一致(理论上是可以修改的)
4. nonce2, 32字节-nonce1的
5. VERSION 版本大端
6. solution a8(=168)表示后面solution长度(42x4字节),solution小端表示
