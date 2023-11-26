## web3.js和ether.js介绍

Web3.is 和ethers.js 都是 JavaScript 库，其作用是使开发者可以与以太坊区块链交互。这两个库都很实用，都能满足大多数以太坊开发者的需求。下面将重点围绕 Web3.js 和 Ethers,js 的相同点和不同点来对它们进行比较，以便你能更好地理解它们的细微区别。

### 什么是 web3.js?

Web3.js 是一个由以太坊基金会开发和维护的开源JavaScript库，使用HTTP或PC(lnter-Process Communication进程间通信)连接或 WebSocket 来和本地或远程以太坊节点进行交互的库。类比于 JavaScript库 axios 对 Web 服务器进行Ajax 调用，您可以使用Web3.js来读取和写入以太坊区块链。
web3.js。因此，有更广泛的支持，因为有更多的开发人员支持它，Web3.js 库由一系列模块的集合，服务于以太坊生态系统的各个功能，如:oweb3-eth 用来与以太坊区块链及合约的交互;

- web3-shh Whisper 协议相关，进行p2p通信和广播
- web3-bzz swarm 协议 (去中心化文件存储)相关
- web3-utils包含一些对 DApp 开发者有用的方法。

官网: https://web3js.org
GitHub: https://github.com/web3/web3.js

### 什么是ether.js

ethers.js库旨在为以太坊区块链及其生态系统提供一个小而完整的 JavaScript API 库，ethers,js 对比使用 web3.js 代码量更少，接口也更简洁
可以通过JSON-RPC、INFURA、Etherscan、Alchemy、Cloudflare或MetaMask连接到以太坊节点。
与 web3.js 相似，ethers.js 常用模块有:

- Ethers,provider 封装与以太坊区块链的连接。它可以用于签发查询和发送已签名的交易，这将改变区块链的状态
- Ethers.contract 部智能合约并与它交互。具体来说，该模块中的函数用于侦听从智能合约发射的事件、调用智能合约提供的函数、获取有关智能合约的信息，以及部署智能合约。
- Ethers.utils 提供用于格式化数据和处理用户输入的实用程序函数。Ethers.utils 的作用方式与 web3-utils 相似，能够简化去中心化应用的构建流程。
- Ethers.wallets 提供的功能与我们目前讨论过的其他模块截然不同。Ethers.wallet 的作用是使你可以与现有钱包(以太坊地址)建立连接、创建新钱包以及对交易签名。

官网: https://ethers.org
GitHub: https://github.com/ethers-io/ethers.js
web3.js 和ethers.js 该如何选择

作者对比
Web3.is 所有者是以太坊基金会
ethers.js 所有者是Richard Moore

