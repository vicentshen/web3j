# 命令行工具

每个发布的web3j的jar都提供了命令行工具。命令行允许你在终端中使用web3j的一些功能。

这些工具提供了：

- 钱包创建
- 钱包密码管理
- 从一个钱包转以太币到另一个钱包
- 生成Solidity智能合约的包装

你可以在项目的发布页面获取zipfile/tarball的命令行工具，OS X用户可以通过Homebrea，linux架构的用户可以通过AUR。

```bash
brew tap web3j/web3j
brew install web3j
```

通过zipfile运行，只需要解压zipfile并运行二进制：

```bash
$ unzip web3j-<version>.zip
   creating: web3j-3.0.0/lib/
  inflating: web3j-3.0.0/lib/core-1.0.2-all.jar
   creating: web3j-3.0.0/bin/
  inflating: web3j-3.0.0/bin/web3j
  inflating: web3j-3.0.0/bin/web3j.bat
$ ./web3j-<version>/bin/web3j

              _      _____ _     _
             | |    |____ (_)   (_)
__      _____| |__      / /_     _   ___
\ \ /\ / / _ \ '_ \     \ \ |   | | / _ \
 \ V  V /  __/ |_) |.___/ / | _ | || (_) |
  \_/\_/ \___|_.__/ \____/| |(_)|_| \___/
                         _/ |
                        |__/

Usage: web3j version|wallet|solidity ...
```

##钱包工具

生成一个新的以太坊钱包：

```bash
$ web3j wallet create
```

更新一个已存在钱包的密码

```bash
$ web3j wallet update <walletfile>
```

发送一笔交易到另一个地址

```bash
$ web3j wallet send <walletfile> 0x<address>|<ensName>
```

当向另一个地址发送以太币的时候，你需要在交易发送之前回答一些问题。看下面这个完整的例子。

下面的例子说明了使用web3j向另一个钱包发送以太币：

```bash
$ ./web3j-<version>/bin/web3j wallet send <walletfile> 0x<address>|<ensName>

              _      _____ _     _
             | |    |____ (_)   (_)
__      _____| |__      / /_     _   ___
\ \ /\ / / _ \ '_ \     \ \ |   | | / _ \
 \ V  V /  __/ |_) |.___/ / | _ | || (_) |
  \_/\_/ \___|_.__/ \____/| |(_)|_| \___/
                         _/ |
                        |__/

Please enter your existing wallet file password:
Wallet for address 0x19e03255f667bdfd50a32722df860b1eeaf4d635 loaded
Please confirm address of running Ethereum client you wish to send the transfer request to [http://localhost:8545/]:
Connected successfully to client: Geth/v1.4.18-stable-c72f5459/darwin/go1.7.3
What amound would you like to transfer (please enter a numeric value): 0.000001
Please specify the unit (ether, wei, ...) [ether]:
Please confim that you wish to transfer 0.000001 ether (1000000000000 wei) to address 0x9c98e381edc5fe1ac514935f3cc3edaa764cf004
Please type 'yes' to proceed: yes
Commencing transfer (this may take a few minutes)...................................................................................................................$

Funds have been successfully transferred from 0x19e03255f667bdfd50a32722df860b1eeaf4d635 to 0x9c98e381edc5fe1ac514935f3cc3edaa764cf004
Transaction hash: 0xb00afc5c2bb92a76d03e17bd3a0175b80609e877cb124c02d19000d529390530
Mined block number: 1849039
```

##Solidity智能合约包装生成器

请参考[Solidity smart contract wrappers](https://docs.web3j.io/smart_contracts.html#smart-contract-wrappers)。


