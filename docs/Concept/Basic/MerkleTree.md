区块中存储很多交易，为了实现“可集”“可散”的功能，有了Merkel Tree



## **Merkle Tree**

任何区块都可以利用hash函数产生hash值（指纹），同时这个hash值也指定这个数据块。

Merkel Tree就是将hash function应用在树上。



Merkel Tree（Hash二叉树）由一个根节点，一组中间节点和一组叶子节点组成。

叶子节点存放原始数据，一个叶子节点存放一个交易。



![image-20240102121418018](assets\image-20240102121418018.png)



 ![image-20240102121430511](assets\image-20240102121430511.png)