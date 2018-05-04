# 故障排除

## 你是否有一个web3j的示例工程

是的，web3j的示例工程标识在[Quickstart](https://docs.web3j.io/quickstart.html)。

## 我正在提交一个交易，但是没有被挖到

在创建并发送一个交易之后，你会收到一个交易的hash，调用[eth\_getTransactionReceipt](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_gettransactionreceipt)总会返回一个控制，这标识这个交易还没有挖到：

```java
String transactionHash = sendTransaction(...);

// you loop through the following expecting to eventually get a receipt once the transaction
// is mined
EthGetTransactionReceipt.TransactionReceipt transactionReceipt =
        web3j.ethGetTransactionReceipt(transactionHash).sendAsync().get();

if (!transactionReceipt.isPresent()) {
    // try again, ad infinitum
}
```

然而，你从来没有收到一个交易回执。很不幸，在以太坊客户端中没有这个交易的错误信息。

```javascript
I1025 18:13:32.817691 eth/api.go:1185] Tx(0xeaac9aab7f9aeab189acd8714c5a60c7424f86820884b815c4448cfcd4d9fc79) to: 0x9c98e381edc5fe1ac514935f3cc3edaa764cf004
```

最简单的方式去看这个提交是否被挖到了是在Etherscan中使用[https://testnet.etherscan.io/address/0x查找这个交易。如果你的提交已经成功了，在你执行交易提交之后的几秒之后，你可以在Etherscan中看到这个交易。等待时间是挖矿的时间。](https://testnet.etherscan.io/address/0x查找这个交易。如果你的提交已经成功了，在你执行交易提交之后的几秒之后，你可以在Etherscan中看到这个交易。等待时间是挖矿的时间。)

![](https://docs.web3j.io/_images/pending_transaction.png)

如果你没有给这个交易签名，这将会消失在以太坊中。最可能的原因是交易随机数没有设置或者设置的太低了。请通过[交易随机数](https://docs.web3j.io/transactions.html#nonce)章节来获取更多信息。

## 我想要看JSON-RPC请求和回应的详细信息

web3j使用SLF4J，这是一个你很容易接入的首选日志框架。一个轻量级的方式是使用LOGBack,这已经在integration-test模块中集成了。

### 说明

如果你为你的应用配置了日志框架，你需要确保你配置了Logback的compile依赖，那个配置文件在`src/main/resources/logback.xml`中。

## 我想获得一些测试网络的以太币，但是我不想挖矿

你可以通过[Ehtereum testnets](https://docs.web3j.io/transactions.html#ethereum-testnets)来获取一些以太币。

## 我怎么去获取通过交易调用的智能合约方法的返回值？

你不能（获得返回值）.没有办法去获取通过交易调用的以太坊方法的返回值。如果你想要在交易中读取一个值，你必须使用事件。如果你想要查询一个在智能合约上的值，你必须使用调用，这个和交易是独立的。这些方法被标记为常量函数。[Smart smart contract wrappers](https://docs.web3j.io/smart_contracts.html#smart-contract-wrappers)替你处理了这些不同点。

下面的StackExchange的[post](http://ethereum.stackexchange.com/questions/765/what-is-the-difference-between-a-transaction-and-a-call)对于了解背后的原理是非常有用的。

## 有没有可能在交易中发送一个随意的文本

这是可以做到的。文本应该是ASCII编码的，并且需要转化成16进制的字符串被放在交易的data字段中。下面是这个的说明：

```java
RawTransaction.createTransaction(
        <nonce>, GAS_PRICE, GAS_LIMIT, "0x<address>", <amount>, "0x<hex encoded text>");

byte[] signedMessage = TransactionEncoder.signMessage(rawTransaction, ALICE);
String hexValue = Numeric.toHexString(signedMessage);

EthSendTransaction ethSendTransaction =
        web3j.ethSendRawTransaction(hexValue).send();
String transactionHash = ethSendTransaction.getTransactionHash();
...
```

## 提示

请确保你增加了交易的gas limit来保存这些文本。

下面的StackExchange的[post](http://ethereum.stackexchange.com/questions/2466/how-do-i-send-an-arbitary-message-to-an-ethereum-address)对于了解背景是非常有用的。

## 我生成了智能合约的包装，但是智能合约的二进制是空的？

如果你声明了一个Solidity的接口，但是你的一个方法实现和原始的接口定义不匹配，这将会导致二进制是空白的。

在下面这个例子中：

```text
contract Web3jToken is ERC20Basic, Ownable {
    ...
    function transfer(address _from, address _to, uint256 _value) onlyOwner returns (bool) {
    ...
}
```

我们忘了在继承的合约中声明`from`变量：

```text
contract ERC20Basic {
    ...
    function transfer(address to, uint256 value) returns (bool);
    ...
}
```

## 我的ENS查询失败了

你确定你是否连接了正确的网络去执行解析？

如果web3j告诉你这个节点还没有同步，你需要改变[ENS\_resolver](https://docs.web3j.io/ens.html#ens-implementation)中的`syncThreshold`。

## 你们是否有一个项目捐赠地址?

当然，你可以捐助Bitcoin或者Ether来自主我们开发web3j。

| Ehterenum | 0x2dfBf35bb7c3c0A466A6C48BEBf3eF7576d3C420 |
| --- | --- |
| Bitcoin | 1DfUeRWUy4VjekPmmZUNqCjcJBMwsyp61G |

