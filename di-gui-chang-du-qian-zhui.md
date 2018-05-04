# 递归长度前缀

递归长度编码方案是一种被用于以太坊的高效利用空间的对象序列化方案。

这个规范被定义在[以太坊黄皮书](http://gavwood.com/paper.pdf)中，同时也在[以太坊WIKI](https://github.com/ethereum/wiki/wiki/RLP)中。

## RLP类型

RLP编码器生命了两种支持的类型：

* string
* list

list类型可以嵌套任意次来支持复杂数据结构的编码。

web3j中的[RLP模块](https://github.com/web3j/web3j/tree/master/rlp)支持RLP编码能力，在[RlpEncoderTest](https://github.com/web3j/web3j/blob/master/rlp/src/test/java/org/web3j/rlp/RlpEncoderTest.java)中说明了编码不同的数值。

## 交易编码

在web3j中，RLP编码用于将以太坊交易对象编码成byte数组\(提交到网络之前，它将会被签名\)。交易类型以及签名逻辑在Crypto模块中，[TransactionEncoderTest](https://github.com/web3j/web3j/blob/master/crypto/src/test/java/org/web3j/crypto/TransactionEncoderTest.java)提供了交易签名以及编码的例子。

## 依赖

这是一个很轻量级的模块，它没有其他依赖。希望在JVM或者Android中使用以太坊的ABI的其他项目使用这个模块，而不是写他们自己的实现。

