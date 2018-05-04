# 入门

将最新的web3j版本加到你的项目配置中。

## Maven

### Java 8:

```javascript
<dependency>
  <groupId>org.web3j</groupId>
  <artifactId>core</artifactId>
  <version>3.3.1</version>
</dependency>
```

### Android:

```javascript
<dependency>
  <groupId>org.web3j</groupId>
  <artifactId>core</artifactId>
  <version>3.3.1-android</version>
</dependency>
```

## Gradle

### Java 8:

```javascript
compile ('org.web3j:core:3.3.1')
```

### Android:

```javascript
compile ('org.web3j:core:3.3.1-android')
```

## 启动客户端

如果你还没有启动以太坊客户端，那么启动一个以太坊客户端，比如：[Geth](https://github.com/ethereum/go-ethereum/wiki/geth)为例：

```bash
$ geth --rpcapi personal,db,eth,net,web3 --rpc --rinkeby
```

或者[Parity](https://github.com/paritytech/parity)

```bash
$ parity --chain testnet
```

或者使用[Infura](https://infura.io/)，它提供了在云端运行的免费客户端：

```javascript
Web3j web3 = Web3j.build(new HttpService("https://morden.infura.io/your-token"));
```

如果想了解更多信息，可以参考[Useing Infura with web3j](https://docs.web3j.io/infura.html)。

获取用于在网络上交易的以太币的说明可以在这份文档的[测试网络](https://docs.web3j.io/transactions.html#ethereum-testnets)章节中找到。

## 开始发送请求

发送同步请求:

```javascript
Web3j web3 = Web3j.build(new HttpService());  // defaults to http://localhost:8545/
Web3ClientVersion web3ClientVersion = web3.web3ClientVersion().send();
String clientVersion = web3ClientVersion.getWeb3ClientVersion();
```

通过CompletableFuture发送一个异步请求\(将来会在Android上面实现\):

```javascript
Web3j web3 = Web3j.build(new HttpService());  // defaults to http://localhost:8545/
Web3ClientVersion web3ClientVersion = web3.web3ClientVersion().sendAsync().get();
String clientVersion = web3ClientVersion.getWeb3ClientVersion();
```

使用RxJava Observable:

```javascript
Web3j web3 = Web3j.build(new HttpService());  // defaults to http://localhost:8545/
web3.web3ClientVersion().observable().subscribe(x -> {
    String clientVersion = x.getWeb3ClientVersion();
    ...
});
```

### 提示：在Android中使用：

```javascript
Web3j web3 = Web3jFactory.build(new HttpService());  // defaults to http://localhost:8545/
...
```

## IPC

web3j同时支持IPC，对于运行在相同主机上的客户端通过文件sockets建立IPC连接。在创建服务的时候，用IpcService来相应的替换HttpService：

```javascript
// OS X/Linux/Unix:
Web3j web3 = Web3j.build(new UnixIpcService("/path/to/socketfile"));
...

// Windows
Web3j web3 = Web3j.build(new WindowsIpcService("/path/to/namedpipefile"));
...
```

### 提示：IPC对于web3j-android不可用。

## 通过Java的智能合约保来使用智能合约

web3j可以没有JVM的情况下，自动生成用于部署以及和智能合约交互的智能合约包代码。

编译你的只能合约来生成包装代码：

```javascript
$ solc <contract>.sol --bin --abi --optimize -o <output-dir>/
```

也可以通过web3j的命令行工具来生成包装代码

```javascript
web3j solidity generate /path/to/<smart-contract>.bin /path/to/<smart-contract>.abi -o /path/to/src/main/java -p com.your.organisation.name
```

现在你可以创建并部署你的智能合约了：

```javascript
Web3j web3 = Web3j.build(new HttpService());  // defaults to http://localhost:8545/
Credentials credentials = WalletUtils.loadCredentials("password", "/path/to/walletfile");

YourSmartContract contract = YourSmartContract.deploy(
        <web3j>, <credentials>,
        GAS_PRICE, GAS_LIMIT,
        <param1>, ..., <paramN>).send();  // constructor params
```

或者使用一个已经存在的合约：

```javascript
YourSmartContract contract = YourSmartContract.load(
        "0x<address>|<ensName>", <web3j>, <credentials>, GAS_PRICE, GAS_LIMIT);
```

和一个智能合约交易：

```javascript
TransactionReceipt transactionReceipt = contract.someMethod(
             <param1>,
             ...).send();
```

调用一个智能合约

```javascript
Type result = contract.someMethod(<param1>, ...).send();
```

如果想要获取更多的信息，请参数[Solidity智能合约包装](https://docs.web3j.io/smart_contracts.html#smart-contract-wrappers)

## 过滤器

web3j的函数式交互使得它更容易去设置一个监听区块链上事件的观察者。

为了接受所有区块链上区块增加的事件：

```javascript
Subscription subscription = web3j.blockObservable(false).subscribe(block -> {
    ...
});
```

接收所有区块链上新交易的事件：

```javascript
Subscription subscription = web3j.transactionObservable().subscribe(tx -> {
    ...
});
```

监听所有被提交到网上的等待的交易\(i.e. 在交易被打包进区块中之前\)：

```javascript
Subscription subscription = web3j.pendingTransactionObservable().subscribe(tx -> {
    ...
});
```

或者，你可以回放所有的区块到当前最新的区块，并且随后被创建的区块也会被通知出来：

```javascript
Subscription subscription = catchUpToLatestAndSubscribeToNewBlocksObservable(
        <startBlockNumber>, <fullTxObjects>)
        .subscribe(block -> {
            ...
});
```

在过滤器和事件中也描述了将会有一定量的交易和区块监听会被回放。

话题过滤器也是支持的：

```javascript
EthFilter filter = new EthFilter(DefaultBlockParameterName.EARLIEST,
        DefaultBlockParameterName.LATEST, <contract-address>)
             .addSingleTopic(...)|.addOptionalTopics(..., ...)|...;
web3j.ethLogObservable(filter).subscribe(log -> {
    ...
});
```

当监听不在需要的时候，需要取消监听：

```javascript
subscription.unsubscribe();
```

### 提示：过滤器在Infura中不支持。

如果想要进一步了解，可以查看[过滤器和事件](https://docs.web3j.io/filters.html)和[Web3jRx](https://github.com/web3j/web3j/blob/master/core/src/main/java/org/web3j/protocol/rx/Web3jRx.java)接口。

## 交易

web3j同时支持通过以太坊钱包文件\(推荐这种方式\)和以太坊客户端管理命令行来发送交易。

用以太坊钱包文件发送以太币：

```javascript
Web3j web3 = Web3j.build(new HttpService());  // defaults to http://localhost:8545/
Credentials credentials = WalletUtils.loadCredentials("password", "/path/to/walletfile");
TransactionReceipt transactionReceipt = Transfer.sendFunds(
        web3, credentials, "0x<address>|<ensName>",
        BigDecimal.valueOf(1.0), Convert.Unit.ETHER)
        .send();
```

或者如果你想要创建你自己的自定义交易：

```javascript
Web3j web3 = Web3j.build(new HttpService());  // defaults to http://localhost:8545/
Credentials credentials = WalletUtils.loadCredentials("password", "/path/to/walletfile");

// get the next available nonce
EthGetTransactionCount ethGetTransactionCount = web3j.ethGetTransactionCount(
             address, DefaultBlockParameterName.LATEST).send();
BigInteger nonce = ethGetTransactionCount.getTransactionCount();

// create our transaction
RawTransaction rawTransaction  = RawTransaction.createEtherTransaction(
             nonce, <gas price>, <gas limit>, <toAddress>, <value>);

// sign & send our transaction
byte[] signedMessage = TransactionEncoder.signMessage(rawTransaction, credentials);
String hexValue = Numeric.toHexString(signedMessage);
EthSendTransaction ethSendTransaction = web3j.ethSendRawTransaction(hexValue).send();
// ...
```

虽然用web3j的Transfer来交易以太币更加简单。

用以太坊客户端的管理命令行（确保你在你的客户端的keystore中有你的钱包）：

```javascript
Admin web3j = Admin.build(new HttpService());  // defaults to http://localhost:8545/
PersonalUnlockAccount personalUnlockAccount = web3j.personalUnlockAccount("0x000...", "a password").sendAsync().get();
if (personalUnlockAccount.accountUnlocked()) {
    // send a transaction
}
```

如果你想要用Parity的[Personal](https://github.com/paritytech/parity/wiki/JSONRPC-personal-module)或者[Trace](https://github.com/paritytech/parity/wiki/JSONRPC-trace-module),或者Geth的[Personal](https://github.com/ethereum/go-ethereum/wiki/Management-APIs#personal)客户端APIs,你可以分别使用org.web3j:parity和org.web3j:geth模块。

## 命令行工具

每个发布的web3j的jar包，都提供了命令行工具。命令行工具允许你在命令行中使用部分web3j的功能。

* 钱包常见
* 钱包密码管理
* 将资金从一个钱包转到另一个钱包
* 编译Solidity智能合约

如想了解更多请参数[文档](https://docs.web3j.io/command_line.html)。

## 更多细节

在Java 8中：

* web3j对于请求回应都是类型安全的。Java 8的可选类型包括了可选的或者空回应。
* 异步请求可以被包装进Java 8的[CompletableFutures](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html).web3j提供了所有异步请求的包装来确保任何的执行过程中的异常将会被捕获，而不是被丢弃了。这是由于CompletableFutures缺少对检查异常的支持，通常被重复抛出由于在检测中引起问题的异常。

在Java 8和Android中：

* Quantity二级制类型被当做BigIntegers。在一些简单的结果中，你可以通过Response.getResult\(\)获取到String类型的quantity。
* 在HttpService和IpcService类中，通过includeRawResponse可以在回应中获取原始JSON。

