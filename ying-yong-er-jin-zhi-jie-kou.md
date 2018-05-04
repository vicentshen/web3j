# 应用二进制接口

应用二进制接口是一个用于在以太坊网络中使用智能合约的数据编码方案。在ABI中定义的类型和你用Solidity中写的[Smart Contacts](https://docs.web3j.io/smart_contracts.html)是一样的，如：uint8,...uint256,int8,...int256,bool,string。

在web3j中的[ABI module](https://github.com/web3j/web3j/tree/master/abi)中提供了ABI规范的支持，并且包含了：

- Java实现了所有的API类型，包括和原生Java类型的转换
- 函数和事件支持
- 大量的单元测试

##类型映射

原生Java到API类型映射如下：

- boolean > bool
- BigInteger > uint/int
- byte[] > bytes
- String > string and address types
- List<> > dynamic/static array

BigInteger类型必须用户数字类型，在以太坊中256bit的整数值被认为是数字类型。

[Fixed point types](http://solidity.readthedocs.io/en/develop/abi-spec.html#types)已经被定义在以太坊中，但是并没有在Solidity中实现，这阻碍了web3j也没有去支持它。一旦在Solidity中可用，我们会引入到web3j的ABI模块中。

你可以通过[Solidity smart contact wrappers](https://docs.web3j.io/smart_contracts.html#smart-contract-wrappers)了解更多在Java中使用ABI类型的信息。

##更多细节

你可以通过[ABI单元测试](https://github.com/web3j/web3j/tree/master/abi/src/test/java/org/web3j/abi)来获取编解码的例子。

##依赖

这是非常清凉的模块，它只依赖了用于加密Hash的第三方依赖[Bouncy Castle](https://www.bouncycastle.org/)(android中是[Spongy Castle](https://rtyley.github.io/spongycastle/))。希望在JVM或者Android中使用以太坊的ABI的其他项目使用这个模块，而不是写他们自己的实现。