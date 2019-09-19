## BFC抵押文档

### 1.BFC挖矿coinbase交易
coinbase交易需要放2个vout地址,一个是基金会地址(一共10个地址)，另外一个是挖矿报块地址。基金会地址会抽成5%的挖矿收益(在满抵押情况)。


规则:

如果挖矿报块地址满足抵押额度,报块地址奖励95%，另外5%奖励给基金会地址。

如果挖矿报块地址不满足抵押额度，报块地址奖励30%，另外70%奖励给基金会地址。

其它转账手续费归挖矿报块地址所有，基金会不参与抽成。


### 2.矿池抵押30%收益
和传统矿池一样,不需要做任何操作,设置报块地址就可以获取30%区块奖励,另外70%自动进入到基金会地址.



### 3.矿池抵押100%收益
矿池如何想获取100%收益,需要在矿池的报块地址上抵押足够的币.

抵押标准为每K算力抵押300BFC

矿池报块地址算力为过去4032区块(约2周)的平均算力

```
取得某个报块地址的算力(默认4032块).
bitfree-cli m_gethashrate '{"address":"FBFWwW3aHCwZsUugxnyxu7uCHaatXiEpnP"}'
```

矿工可以通过bitfree-qt/bitfree-cli来发起一个到矿池报块地址的抵押操作。主网会计算每个矿池报块地址的抵押的币数量(由矿工抵押UTXO记录汇总)

由矿池来主动的解除抵押操作。

### 4.获取某个报块地址的平均算力
```
获取全网算力:
bitfree-cli m_gethashrate 
取得某个报块地址的算力(默认4032块).
bitfree-cli m_gethashrate '{"address":"FBFWwW3aHCwZsUugxnyxu7uCHaatXiEpnP"}'
```


### 5.获取某个地址的抵押UTXO

m_listunspent(需要钱包里面存放私钥，可以用来解除抵押)

```
m_listunspent ( minconf maxconf ["address",...] include_unsafe query_options )
```

例子:
```
bitfree-cli m_listunspent 6 999999 "[\"FBFWwW3aHCwZsUugxnyxu7uCHaatXiEpnP\"]"
```


### 6.获取全网抵押UTXO
```
m_listmortgage "address"
```

例子
```
取得全网所有的抵押
bitfree-cli m_listmortgage
取得某个地址的所有抵押
bitfree-cli m_listmortgage "FBFWwW3aHCwZsUugxnyxu7uCHaatXiEpnP"
```

### 7.发起抵押

抵押命令m_sendtoaddress。需要设置抵押地址和赎回地址。

抵押金额必须大于0.01BFC。

抵押手续费和交易手续费一样。

```
m_sendtoaddress "address" amount "mortgage_back_address" ( "comment" "comment_to" subtractfeefromamount replaceable conf_target "estimate_mode" )
```

例子
```
bitfree-cli m_sendtoaddress FBFo7v8ctLpiSK3gnvSvSZarFAkty9EbqA 0.02 FBFEytpMEQX7D8XXMvdjutVWboqpcssF2d

bitfree-cli m_sendtoaddress FBFo7v8ctLpiSK3gnvSvSZarFAkty9EbqA 0.02 FBFEytpMEQX7D8XXMvdjutVWboqpcssF2d "testmort" "testmorto" true

```


### 8.退/赎回抵押

只有抵押方才可以解除抵押

退抵押手续费固定0.01BFC，从赎回地址中自动扣除

```
m_sendbacktoaddress "txid" vout ( "comment" "comment-to" )
```

例子:
```
bitfree-cli m_sendbacktoaddress a5f122149151cd0b7ccdb1c937596a316b18dae8fa4fd7a14aa20c6ae1226c60 0
```
