```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

contract Ownable{
    address public onwer;
    constructor(){
        onwer=msg.sender;
    }
    modifier onlyOwner{
        require(msg.sender==onwer,"Only onwer can call this.");
        _;
    }
    function setOnwer(address _newOwner)external  onlyOwner{
        require(_newOwner!=address(0),"invalid address");
        onwer=_newOwner;
    }
    function onlyOwnerCanCall() external onlyOwner{

    }
    function anyOneCanCall() external {
        
    }
}
```

