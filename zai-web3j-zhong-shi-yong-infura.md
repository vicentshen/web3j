# 在web3j中使用Infura

[ConsenSys](https://consensys.net/)提供了[Infura](https://infura.io/)服务,这使以太坊客户端运行在云商，那样你不需要自己搭建以太坊。

当你注册这个服务的时候，你会获取到一个token来连接到相应的以太坊网络:

以太坊主链：

[https://mainnet.infura.io/](https://mainnet.infura.io/)

以太坊测试网络\(Rinkeby\)

[https://rinkeby.infura.io/](https://rinkeby.infura.io/)

以太坊测试网络\(Kovan\)

[https://kovan.infura.io/](https://kovan.infura.io/)

以太坊测试网络\(Ropsten\)

[https://ropsten.infura.io/](https://ropsten.infura.io/)

为了获取一些在这些网络中使用的以太币，你可以参考[Ethereum testnets](https://docs.web3j.io/transactions.html#ethereum-testnets)。

## InfuraHttpClient

web3j的infura模块提供了一个Infura HTTP客户端，这支持了Infura特定的`Infura-Ethereum-Preferred-Client`头。这允许你去指定是否需要一个Geth或者Parity客户端去回应你的请求。你可以创建客户端就像常规的HTTPClient:

```java
Web3j web3 = Web3j.build(new HttpService("https://rinkeby.infura.io/<your-token>"));
Web3ClientVersion web3ClientVersion = web3.web3ClientVersion().send();
System.out.println(web3ClientVersion.getWeb3ClientVersion());
```

如果你想要去测试Infura的JSON-RPC调用，更新[CoreIT](https://github.com/web3j/web3j/blob/master/integration-tests/src/test/java/org/web3j/protocol/core/CoreIT.java)为你的URL并运行它。

你可以通过[Infura docs](https://github.com/INFURA/infura/blob/master/docs/source/index.html.md#choosing-a-client-to-handle-your-request)来获取更多信息。

## 交易

为了和Infura节点交易，你需要去创建并且在你发送之前进行离线签名交易，Infura节点无法看到你的加密的以太坊密钥文件，这需要通过你的私人Geth/Parity私有的管理命令来解锁。

你可以在[离线交易签名](https://docs.web3j.io/transactions.html#offline-signing)和[管理API](https://docs.web3j.io/transactions.html#offline-signing)章节中获取更多信息。

