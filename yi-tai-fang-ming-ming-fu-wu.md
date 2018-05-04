# 以太坊命名服务

[以太坊命名服务(ENS)](https://ens.domains/)一个人类刻度的名字来指示以太坊上的地址。这个因特网的域名解析服务(DNS)相似，它将IP映射到人类可读的域名。

在ENS中，地址可以是签名也可以是智能合约地址。

你可以用web3j.eth来代替钱包地址`0x19e03255f667bdfd50a32722df860b1eeaf4d635`。

##在web3j使用

你可以web3j中交易的任何地方使用ENS名称。在实践中，在智能合约包装中，比如：

```java
YourSmartContract contract = YourSmartContract.load(
        "0x<address>|<ensName>", web3j, credentials, GAS_PRICE, GAS_LIMIT);
```

同时，当执行以太坊交易，比如使用命令行工具：

```bash
$ web3j wallet send <walletfile> 0x<address>|<ensName>
```


##web3j实现

无论何时使用web3j的交易管理，在这些场景中，EnsResolver用于去执行ENS查询。

分布步骤如下：

- 检查以太坊节点是否同步了
- 如果没有，失败
- 如果同步了，检查最近的区块上的时间戳
    
    - 如果时间多于三分钟，则失败
    - 否则执行查询

如果你想要去改变阈值参数到同步多于三分钟，这可以通过调用[ManagedTransaction](https://github.com/web3j/web3j/blob/master/core/src/main/java/org/web3j/tx/ManagedTransaction.java)类的方法实现。

##Unicode技术标准（UTS）#46

UTS#46被用于域名的过滤输入。web3j的ENS实现在试图解决之前对所有的输入执行了过滤。为了获得更多的实现细节，你可以参考[NameHash](https://github.com/web3j/web3j/blob/master/core/src/main/java/org/web3j/ens/NameHash.java)类。

##注册域名

当前，web3j值支持ENS域名的解析。它目前不支持注册。为了说明如何开始，你可以参考ENSde [quickstart](http://docs.ens.domains/en/latest/quickstart.html)。



