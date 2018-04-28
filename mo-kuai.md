# 模块

为了满足开发者更加灵活的使用web3j的意愿，这个项目是由一些模块组成的。

依赖顺序如下：

* utils - 最小工具类的集合
* rlp - 递归长度前缀(RLP)解码器
* abi - 应用二级制接口解码器
* crypto - 用于在以太坊中交易以及密码/钱包管理的加密库
* tuples - 简单元祖库
* cor - 跟web3j核心组件高度相似，除了没有代码生成器
* codegen - 代码生成器
* console - 命令行工具

以下模块仅依赖核心模块。

* geth - Geth的JSON-RPC模块
* parity - Parity的JSON-RPC模块
* infura - Infura的HTTP头支持

对于大多数场景来说，核心模块包含了你所要用到的。对于核心模块的依赖是松耦合的，在你的工程中，你仅只需要关注跟以太坊网络的具体交易（比如 ABI/RLP编码，交易签名，etc）。

所有的模块都通过Maven以及二进制发布了，发布的名字列在了下面：

For Java:
    org.web3j:<模块名称>:<版本号>

For Android:
    org.web3j:<模块名称>:<版本号>-android

