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

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

contract SendEther{
    constructor() payable{}

    receive() external payable { }

    //transfer 2300 gas reverts 
    // 使用 transfer()：
    // 场景：通常，transfer() 是最常用的发送以太币方法，因为它是最安全的。
    // 用途：用于将以太币发送到地址，特别是在向外部地址发送以太币时。它会自动抛出异常，如果接收方合约执行失败或者 gas 不足。
    function sendViaTransfer(address payable _to) external payable {
        _to.transfer(123);
    }

    //send 2300 gas return bool
    // 使用 send()：
    // 场景：send() 是一种在发送以太币时返回布尔值的方法。适用于需要检查发送是否成功的情况。
    // 用途：可以在发送以太币时检查操作是否成功，并根据结果采取适当的操作
    function sendViaSend(address payable _to) external payable{
        bool sent=_to.send(123);
        require(sent,"send failed");
    }

    //call  all gas return bool and data 会把剩余GAS消耗
    // 使用 call：
    // 场景：call 具有更高级的功能，它可以调用合约函数并传递数据。适用于更复杂的发送以太币操作。
    // 用途：可以使用 call 来调用合约函数，包括传递以太币，但需要小心处理返回值和异常。
    function sendViaCall(address payable _to) external payable{
        (bool success,)=_to.call{value:123}("");
        require(success,"call failed");
    }
}

contract EthReceiver{
    event Log(uint amount,uint gas);
    receive() external payable { 
        emit Log(msg.value, gasleft());
    }
}
```

