## 1.哈希函数

​	[哈希函数](https://zh.wikipedia.org/wiki/%E6%95%A3%E5%88%97%E5%87%BD%E6%95%B8)(Hash function)又称散列函数，是一种从任意数据中创建数据“指纹”的方法。哈希函数可以将数据压缩成固定长度的数据串，得到的数据串称为哈希值。

​	良好的哈希函数具有如下性质：如果两个哈希值不同，那么这两个哈希函数的原始输入也不同，且hash是一个单向函数，我们可以输入数据得到其hash值，但无法通过hash值推断其原始输入。

​	比特币中使用的hash函数是sha256：

```
>>> import hashlib
>>> hashlib.sha256("hello world".encode()).hexdigest()
'b94d27b9934d3e08a52e52d7da7dabfac484efe37a5380ee9088f7ace2efcde9'
>>> hashlib.sha256("hello bithacks".encode()).hexdigest()
'c932a9c5c7b1df0d082a8a4c22df9faeb97912c1cae548ca0402c738b24e183d'
```

## 2.公私钥加密系统

​	[公私钥加密体系](https://zh.wikipedia.org/wiki/%E5%85%AC%E5%BC%80%E5%AF%86%E9%92%A5%E5%8A%A0%E5%AF%86)是非对称加密的一种，所谓非对称加密指的是加密与解密用的是不同的秘钥，公私钥加密系统中，公钥加密的数据可以使用私钥解密，私钥加密的数据可以用公钥解密。我们以Alice需要向Bob发送一段秘密消息的场景来简单介绍一下对称加密与非对称加密：

​	对称加密体系：

​		1.Alice和Bob共享一个秘钥

​		2.Alice将消息加密，发送给Bob

​		3.Bob收到消息，使用秘钥解密数据得到明文

​	非对称加密体系：

​		1.Alice有一个公钥public_A和一个私钥private_A，Bob有一个公钥public_B和一个私钥private_B，公钥对所有人可见，私钥只有自己可见

​		2.Alice使用Bob的公钥加密消息，发送给Bob

​		3.Bob收到密文，用自己的私钥解密数据

​	非对称密码学中还有一个概念叫做数字签名，用于向对方验证自己的身份。Alice用自己的私钥加密一段明文，并将密文发送给Bob，Bob可以使用Alice的公钥解密密文，如果可以正常解密出完整的明文，则可以证明此段消息来自Alice。

## 3.Merkle树

​	[Merkle树](https://zh.wikipedia.org/wiki/%E5%93%88%E5%B8%8C%E6%A0%91)是一种可以用于高效地验证数据完整性的树形数据结构，每个叶子节点是数据块，逐层hash向上组成一个树形结构，从结构上我们可以看到，最终树根hash由所有叶节点的数据所确定，如果叶节点上的数据被修改，那么树根hash值必然会发生变化。

![merkle](./pics/merkle.png)

​	比特币中区块数据的组织使用了Merkle树的结构，便于节点验证区块数据的完整性和正确性。