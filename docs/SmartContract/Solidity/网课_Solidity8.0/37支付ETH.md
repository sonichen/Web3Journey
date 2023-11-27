标注payable关键词，可以接受以太坊token

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

contract Payable{
    address payable public owner;// 标志该地址可以转账

    constructor(){
        owner=payable (msg.sender);
    }

    function deposit() external payable {}

    function getBalance() external view returns(uint){
        return address(this).balance;
    }
}
```

![image-20231127151852262](assets\image-20231127151852262.png)