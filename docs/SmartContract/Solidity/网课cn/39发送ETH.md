智能合约中有三种方法来发送主币

- 用transfer消耗2300 gas，如果失败则 reverts 
- send 消耗2300 gas，返回bool标示是否成功
- call  发送所有剩余all gas， return bool and data 

部署合约测试：
![image-20231127154735601](assets\image-20231127154735601.png)

![image-20231127154756180](assets\image-20231127154756180.png)



![image-20231129180601962](assets\image-20231129180601962.png)

触发了receive函数，回退，可以看到剩余gas数量

![image-20231129180712808](assets\image-20231129180712808.png)