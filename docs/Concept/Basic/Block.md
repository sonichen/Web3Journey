## 区块链如何组成

区块链是区块组成的链。区块链像每个节点手中记录的一模一样的大账本，每个区块是账本中的一页。



## 区块是如何连接成区块链的？

区块是一种数据结构。

区块按照顺序相连形成的链状结构，也就是区块链大账本。



## 创世块

创世块（Genesis Block）是区块链中的第一块区块



## 区块

区块（Block）主要分为区块头（Block Header）和交易（transaction）两个部分。

![image-20240102120148493](assets\image-20240102120148493.png)

> 基于不同的实现机制，不同区块链还有不同附加信息。比如比特币中，区块头还包括随机数和难度目标等信息。

### 区块头

区块链头Block Header

- PrevHash: 前一个区块头的Hash值
- Merkle Root：由账本所有的交易信息层层哈希所得，易于校验。
- Timestamp：记录的时间戳
- Nonce：只用一次的随机数(nonce)是指在区块链中**被随机使用的一串仅用一次的数字,**它以特定的格式附加到区块中,**并用于验证区块的完整性和有效性**。
- Subsidy：区块链奖励
- ...



### **交易**

通常以Merkle Tree的方式记录，长度可变，记录着当前区块的交易细节。

> 区块中还包含交易计数器，表示每个区块中包含交易的数量。






