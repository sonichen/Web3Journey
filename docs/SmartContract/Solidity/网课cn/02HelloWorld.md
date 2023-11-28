solidity程序结构

1. 写版权声明

```
// SPDX-License-Identifier: MIT
```

2. 写solidity版本号

```
pragma solidity 0.8.18;// 编译器必须是0.8.18
pragma solidity ^0.8.18;// 可以支持0.8版本内大于0.8.18的编译器
```

3. 写合约

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

contract HelloWorld{
    string public myString="Hello, World";
}

```

note：公开的变量自带get方法

![image-20231126110006660](assets\image-20231126110006660.png)