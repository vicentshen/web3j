# 交易

宽泛地说，以太坊支持三中类型的交易：

1. 从一个账号转移以太币到另一个账号
2. 创建一个智能合约
3. 和智能合约交互

为了使用这些交易，发起交易的以太坊账号中需要有以太币（以太坊区块链上的燃料）。这被用于支付Gas消耗，这是以太坊客户端代表你去执行交易或者向以太坊区块链中提交数据的消耗。下面这个链接[获取以太币](https://docs.web3j.io/transactions.html#obtaining-ether)描述了如何获取以太币。

另外，可以查询一个智能合约的状态，这个描述了[查询智能合约的状态](https://docs.web3j.io/transactions.html#querying-state)。

![web3j](https://docs.web3j.io/_images/web3j_transaction.png)

## 获取以太币

你有两个选项去获取以太币：

1. 自己挖矿
2. 从另一个账号中后去以太币

在私有环境或公共测试环境自己挖矿是相当直截了当的。然而，在主线环境这需要巨大的专用GPU时间，一般来说\(译者注：挖矿\)不太可行，除非你有一台有多个专用GPU的游戏电脑。如果你想要使用私有环境，下面是一些指引[Homestead documentation](https://ethereum-homestead.readthedocs.io/en/latest/network/test-networks.html#id3)。

你需要通过一个交换才能购置一些以太币。不同的地方有不同的汇率，你需要去研究哪个地方对你来说是最好的。[Homestead documentation](https://ethereum-homestead.readthedocs.io/en/latest/ether.html#list-of-centralised-exchange-marketplaces)文档中包含了一些比较容易上手的交易所。

## 以太坊测试网络

在以太坊中有一些专用的测试网络，你可以通过不同的客户端使用。

* Rinkeby\(仅限Geth\)
* Kovan\(仅限Parity\)
* Ropsten\(Geth以及Parity\)

在开发阶段，建议你使用Rinkeby或者Kovan测试网络。因为他们使用了PoA\(Proof of Authority\)共识机制来确保交易和区块以一致，及时的方式来生成。Ropsten测试网络是最接近主链的，它也使用了PoW（Proof of Work），它在以前一直遭受着攻击，对于开发者来说会有更多的问题。

你可以通过[https://www.rinkeby.io/](https://www.rinkeby.io/)向Rinkeby Crypto Faucet申请在Rinkeby测试网络中的以太币。

获取Kovan上的虚拟币的细节在[这里](https://github.com/kovan-testnet/faucet)。

如果你需要在Ropstern测试网络中获取一些以太币在最开始的时候，请向[web3j Gitter channel](https://gitter.im/web3j/web3j)发送你的钱包地址，然后你会收到一些比特币。

## 在测试网络/私有网络中挖矿

在以太坊测试环境中，挖矿难度被设置低于主链。这意味着你可以通过常规CPU比方说你的笔记本来挖新的以太币。你需要做的是运行一个以太坊客户端比方说Geth或者Parity来收集以太币。更近一步的说明在各自的网站中。

Geth:[https://github.com/ethereum/go-ethereum/wiki/Mining](https://github.com/ethereum/go-ethereum/wiki/Mining)

Parity:[https://github.com/paritytech/parity/wiki/Mining](https://github.com/paritytech/parity/wiki/Mining)

一旦你挖到了一定的比特币，你就可以在区块链上交易了。

然后，如上所说，使用Kovan或者Rinkeby测试网络更加容易。

## Gas

当一笔交易发生在以太坊上，你需要向代表你执行交易或者向以太坊区块链提交交易输出数据的客户端支付交易消耗。

这个消耗是用gas来衡量的，gas是在以太坊虚拟机中执行一笔交易消耗的指令的数量。请参考[Homestead documentation](http://ethdocs.org/en/latest/contracts-and-transactions/account-types-gas-and-transactions.html?highlight=gas#what-is-gas)获取更多信息。

这意味着对你来说，以太坊客户端完成一笔交易的消耗决定于两个变量：

### Gas price

这个数量以每个gas单位的以太币价格。web3j使用了一个默认的价格：22,000,000,000 Wei \(22 x 10-8 以太币\).这定义在[ManagedTransaction](https://github.com/web3j/web3j/blob/master/core/src/main/java/org/web3j/tx/ManagedTransaction.java)中。

### Gas limit

这是你希望在交易执行中消耗的总gas数量。这是一个单独交易的最大上上限，在以太坊区块中，这个值最小是6,700,000。当前的gas limit可以在[https://ethstats.net/](https://ethstats.net/)看到。

这些参数共同决定了你希望的最大的消耗的比特币数量，你的消耗不会多于 gas price \* gas limit。

gas的价格可以影响交易的生效速度，这取决于给矿工们的其他其他交易的gas价格。

你需要去调整这些参数以确保你的交易将会及时成交。

## 交易机制

当你有一个有一些以太币的有效账号，你可以用以下两种机制在以太坊中交易：

1. [通过以太坊客户端签名交易](https://docs.web3j.io/transactions.html#signing-via-client)
2. [离线交易签名](https://docs.web3j.io/transactions.html#offline-signing)

web3j支持两种机制。

## 通过以太坊客户端做交易签名

为了通过以太坊客户端交易，首先，你需要确保知道你的钱包地址。你最好在你自己的Geth/Parity客户端中运行。一旦你的客户端运行起来了，你可以创建一个钱包：

* [Geth Wiki](https://github.com/ethereum/go-ethereum/wiki/Managing-your-accounts)包含了不同的机制。Geth支持比如导入私钥文件，在控制台创建新账号。
* 你也可以通过JSON-RPC的admin命令，比方说`personal_newAccmount`。

创建了钱包文件之后，你可以通过web3j解锁你的账号，首先你需要创建一个支持Partiy以及Geth的admin命令行的web3j对象：

```java
Admin web3j = Admin.build(new HttpService());
```

然后你可以解锁账号，然后发送一个交易：

```java
PersonalUnlockAccount personalUnlockAccount = web3j.personalUnlockAccount("0x000...", "a password").send();
if (personalUnlockAccount.accountUnlocked()) {
    // send a transaction
}
```

你可以用[EthSendTransaction](https://github.com/web3j/web3j/blob/master/core/src/main/java/org/web3j/protocol/core/methods/response/EthSendTransaction.java)创建的[Transaction](https://github.com/web3j/web3j/blob/master/core/src/main/java/org/web3j/protocol/core/methods/request/Transaction.java)类型来发送交易:

```java
Transaction transaction = Transaction.createContractTransaction(
              <from address>,
              <nonce>,
              BigInteger.valueOf(<gas price>),  // we use default gas limit
              "0x...<smart contract code to execute>"
      );

      org.web3j.protocol.core.methods.response.EthSendTransaction
              transactionResponse = parity.ethSendTransaction(ethSendTransaction)
              .send();

      String transactionHash = transactionResponse.getTransactionHash();

      // poll for transaction response via org.web3j.protocol.Web3j.ethGetTransactionReceipt(<txHash>)
```

怎么获取数值将在下面讲解。

你可以用[DeployContactIT](https://github.com/web3j/web3j/blob/master/integration-tests/src/test/java/org/web3j/protocol/scenarios/DeployContractIT.java)做单元测试，你还可以在它的父类[Scenario](https://github.com/web3j/web3j/blob/master/integration-tests/src/test/java/org/web3j/protocol/scenarios/Scenario.java)中获取更多交易流程的详情。

你可以在[管理APIS](https://docs.web3j.io/management_apis.html)章节中获得在web3j中支持的不同的admin命令更多细节。

## 离线交易签名

如果你不想管理你自己的以太坊客户度an，或者你不想给以太坊客户端提供钱包详细信息比如密码，那么离线交易签名正好可以满足。

离线交易签名允许你在web3j中用以太坊钱包签名一笔交易，这允许你通过私钥证书完全控制交易。一笔离线创建的交易可以发送给网络上的任意以太坊客户端，它会把这笔交易广播给其他节点，倘若这是一笔有效的交易。

你也可以在需要的时候来给交易签名。这可以通过重写[ECKeyPair](https://github.com/web3j/web3j/blob/master/crypto/src/main/java/org/web3j/crypto/ECKeyPair.java#L41)中的sign方法。

## 创建并使用钱包文件

为了签名利息按交易，你需要钱包文件或者跟一个以太坊钱包/账号关联的公私钥。

web3j同时可以为你生成一个安全的以太坊钱包文件，也可以使用一个已经存在的钱包文件。

创建一个新的钱包文件：

```java
String fileName = WalletUtils.generateNewWalletFile(
        "your password",
        new File("/path/to/destination"));
```

通过钱包文件加载credentials：

```java
Credentials credentials = WalletUtils.loadCredentials(
        "your password",
        "/path/to/walletfile");
```

这些credentials可以签名交易。

你可以通过[Web3 Secret Storage Definition](https://github.com/ethereum/wiki/wiki/Web3-Secret-Storage-Definition)来获得一个完整的钱包文件说明。

## 签名交易

应该使用[RawTransaction](https://github.com/web3j/web3j/blob/master/crypto/src/main/java/org/web3j/crypto/RawTransaction.java)来容纳离线签名。 RawTransaction跟之前提到的Transaction类型是相似的，除了它不需要一个原地址，这可以通过签名推断出来。

为了创建并签名一个原始交易，应该按照如下的顺序进行：

1. 获取发送账号的下一个有效的[随机数](https://docs.web3j.io/transactions.html#nonce)
2. 创建RawTransaction对象
3. 用[递归长度前缀](https://docs.web3j.io/rlp.html)编码RawTransaction对象
4. 签名RawTransaction对象
5. 向一个节点发送RawTransaction对象

随机数是一个自增的用于唯一标识交易的数字。一个随机数只能被用一次，在那笔交易被挖到之前可能有多个相同随机数的多个版本的交易，但是，一旦被挖到了，任何之后的\(相同随机数的交易\)提交将会被拒绝。

一旦你获得下一个可以用的[随机数](https://docs.web3j.io/transactions.html#nonce)，这个数值就可以用于创建你自己的交易对象：

```java
RawTransaction rawTransaction  = RawTransaction.createEtherTransaction(
             nonce, <gas price>, <gas limit>, <toAddress>, <value>);
```

交易之后可以被签名和编码：

```java
byte[] signedMessage = TransactionEncoder.signMessage(rawTransaction, <credentials>);
String hexValue = Numeric.toHexString(signedMessage);
```

credentials通过[创建并使用钱包文件](https://docs.web3j.io/transactions.html#wallet-files)创建。

然后交易使用[eth\_sendRawTransaction](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_sendrawtransaction)被发送出去：

```java
EthSendTransaction ethSendTransaction = web3j.ethSendRawTransaction(hexValue).sendAsync().get();
String transactionHash = ethSendTransaction.getTransactionHash();
// poll for transaction response via org.web3j.protocol.Web3j.ethGetTransactionReceipt(<txHash>)
```

你可以通过[CreateRawTransactionIT](https://github.com/web3j/web3j/blob/master/integration-tests/src/test/java/org/web3j/protocol/scenarios/CreateRawTransactionIT.java)测试用例来获得一个创建和发送原交易的完整例子。

## 交易随机数

随机数是一个自增的用于唯一标识交易的数字。一个随机数只能被用一次，在那笔交易被挖到之前可能有多个相同随机数的多个版本的交易，但是，一旦被挖到了，任何之后的\(相同随机数的交易\)提交将会被拒绝。

你可以通过[eth\_getTransactionCount](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_gettransactioncount)方法来后去下一个可用的随机数：

```java
EthGetTransactionCount ethGetTransactionCount = web3j.ethGetTransactionCount(
             address, DefaultBlockParameterName.LATEST).sendAsync().get();

     BigInteger nonce = ethGetTransactionCount.getTransactionCount();
```

然后你可以使用这个随机数来创建你的交易对象：

```java
RawTransaction rawTransaction  = RawTransaction.createEtherTransaction(
             nonce, <gas price>, <gas limit>, <toAddress>, <value>);
```

## 交易类型

在web3j中不同的交易类型都是通过Transaction和RawTransaction对象工作的。最大的一个区别是Transaction对象必须有一个源地址，那样通过[eth\_sendTransaction](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_sendtransaction)处理发送交易请求的以太坊客户端才能知道用哪一个钱包来签名和发送交易。如上所说，这\(译者注：代指源地址\)在离线签名的原始交易中是非必要的。

下面的章节列出了对于不同的交易类型的关键的交易属性。下面的属性是所有类型都需要的：

* Gas price
* Gas limit
* Nonce
* From

在下面所有的例子中，Transaction和RawTransaction对象是可以互换的。

## 从一个账号转移比特币到另一个

在两个账号之间发送以太币的最小的交易对象的细节：

`to`

目标地址

`value`

你需要向目标地址发送的以太币数量

```java
BigInteger value = Convert.toWei("1.0", Convert.Unit.ETHER).toBigInteger();
RawTransaction rawTransaction  = RawTransaction.createEtherTransaction(
             <nonce>, <gas price>, <gas limit>, <toAddress>, value);
// send...
```

然而，在这里更加推荐你使用[Transaction class](https://github.com/web3j/web3j/blob/master/core/src/main/java/org/web3j/tx/Transfer.java)发送以太币，它会替你管理随机数并轮询响应。

```java
Web3j web3 = Web3j.build(new HttpService());  // defaults to http://localhost:8545/
Credentials credentials = WalletUtils.loadCredentials("password", "/path/to/walletfile");
TransactionReceipt transactionReceipt = Transfer.sendFunds(
        web3, credentials, "0x<address>|<ensName>",
        BigDecimal.valueOf(1.0), Convert.Unit.ETHER).send();
```

## 使用智能合约的推荐方式

当使用下面列出的智能合约包装，你必须要手工将Solidity转换成原生java类型。使用web3j的[Solidity smart contract wrappers](https://docs.web3j.io/smart_contracts.html#smart-contract-wrappers)将大大提高效率，它处理了所有的代码生成和类型转换。

## 创建一个智能合约

为了部署一个智能合约，下面的一些属性是必须的：

`value`

你想要保存在智能合约中的比特币。

`data`

16进制的，编译过的智能合约创建代码

```java
// using a raw transaction
RawTransaction rawTransaction = RawTransaction.createContractTransaction(
        <nonce>,
        <gasPrice>,
        <gasLimit>,
        <value>,
        "0x <compiled smart contract code>");
// send...

// get contract address
EthGetTransactionReceipt transactionReceipt =
             web3j.ethGetTransactionReceipt(transactionHash).send();

if (transactionReceipt.getTransactionReceipt.isPresent()) {
    String contractAddress = transactionReceipt.get().getContractAddress();
} else {
    // try again
}
```

如果只能合约包含一个构造器，关联的构造字段值必须被编码并被打包到编译过的智能合约代码：

```java
String encodedConstructor =
             FunctionEncoder.encodeConstructor(Arrays.asList(new Type(value), ...));

// using a regular transaction
Transaction transaction = Transaction.createContractTransaction(
        <fromAddress>,
        <nonce>,
        <gasPrice>,
        <gasLimit>,
        <value>,
        "0x <compiled smart contract code>" + encodedConstructor);

// send...
```

## 和智能合约交易

和一个已经存在的智能合约交易，下面的一些属性是必须的：

`to`

智能合约地址

`value`

你希望保存在智能合约中保存的以太币

`data`

编码的函数选择器和参数

web3j替你完成函数编码，你可以通过[Application Binary Interface](https://docs.web3j.io/abi.html)章节获取更多实现细节。

```java
Function function = new Function<>(
             "functionName",  // function we're calling
             Arrays.asList(new Type(value), ...),  // Parameters to pass as Solidity Types
             Arrays.asList(new TypeReference<Type>() {}, ...));

String encodedFunction = FunctionEncoder.encode(function)
Transaction transaction = Transaction.createFunctionCallTransaction(
             <from>, <gasPrice>, <gasLimit>, contractAddress, <funds>, encodedFunction);

org.web3j.protocol.core.methods.response.EthSendTransaction transactionResponse =
             web3j.ethSendTransaction(transaction).sendAsync().get();

String transactionHash = transactionResponse.getTransactionHash();

// wait for response using EthGetTransactionReceipt...
```

你无法获取交易函数的调用的返回值，请忽略消息签名的返回值。但是，你可以通过filter来捕捉函数的返回值。请参考[Filters and Events](https://docs.web3j.io/filters.html)获取更多细节。

## 查询智能合约的状态

这个功能可以通过[eth\_call](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_call)的JSON-RPC调用实现。

eth\_call允许你调用智能合约上的方法来查询一个值。这个函数没有交易消耗，因为它没有改变任何智能合约的函数状态，它只是返回了一些值：

```java
Function function = new Function<>(
             "functionName",
             Arrays.asList(new Type(value)),  // Solidity Types in smart contract functions
             Arrays.asList(new TypeReference<Type>() {}, ...));

String encodedFunction = FunctionEncoder.encode(function)
org.web3j.protocol.core.methods.response.EthCall response = web3j.ethCall(
             Transaction.createEthCallTransaction(<from>, contractAddress, encodedFunction),
             DefaultBlockParameterName.LATEST)
             .sendAsync().get();

List<Type> someTypes = FunctionReturnDecoder.decode(
             response.getValue(), function.getOutputParameters());
```

## 说明

如果调用了一个无效的函数，或者返回了一个null的结果，它将会返回一个[Collections.emptyList\(\)](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#emptyList--)实例。

