# 开发指引

## 编辑web3j

web3j包含了一些运行在在线以太坊客户端的测试用例。如果你没有一个运行的客户端，你可以参考如下说明排查执行情况：

运行一个全版本编译\(除去测试用例\):

```bash
$ ./gradlew check
```

运行测试用例：

```bash
$ ./gradlew  -Pintegration-tests=true :integration-tests:test
```

## 生成文档

web3j使用[Sphinx](http://www.sphinx-doc.org/en/stable/)文档生成器。

所有的文档\(除去项目的README.md\)之外都在/docs文件夹下。

为了编译文档的拷贝，先到项目的根目录：

```bash
$ cd docs
$ make clean html
```

然后你可以这样浏览生成的文档：

```bash
$ open build/html/index.html
```

