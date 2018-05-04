# 管理API

出了实现了标准的JSON-RPC的API，以太坊客户端，比如Geth和Parity通过JSON-RPC提供了一些额外的管理功能。

一个关键功能是他们提供了创建以太坊账号，并为在网络上的交易解锁账号。在Geth和Parity中，实现了他们的私有模块，详细信息如下：

* [Parity](https://github.com/paritytech/parity/wiki/JSONRPC-personal-module)
* [Geth](https://github.com/ethereum/go-ethereum/wiki/Management-APIs#personal)

web3j中支持了私有模块。那些同时在Geth和Parity中的方法在web3j的[Admin模块中](https://github.com/web3j/web3j/blob/master/core/src/main/java/org/web3j/protocol/admin/Admin.java)。

你可以用工厂方法初始化一个web3j连接：

```javascript
Admin web3j = Admin.build(new HttpService());  // defaults to http://localhost:8545/
PersonalUnlockAccount personalUnlockAccount = admin.personalUnlockAccount("0x000...", "a password").send();
if (personalUnlockAccount.accountUnlocked()) {
    // send a transaction
}
```

对于特定的Geth犯法，你可以使用Geth连接，对于Parity你可以使用关联的Parity连接。Parity连接提供了Parity的[Trace](https://github.com/paritytech/parity/wiki/JSONRPC-trace-module)模块。这些连接在web3j的geth以及parity模块中。

你可以通过[ParityIT](https://github.com/web3j/web3j/blob/master/integration-tests/src/test/java/org/web3j/protocol/parity/ParityIT.java)测试来进一步了解使用这些API。

