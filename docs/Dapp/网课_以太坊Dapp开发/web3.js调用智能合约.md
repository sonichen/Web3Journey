## web3与智能合约

获取合约部署后的配置文件 在 artifacts 下的的 合约名json 文件

![image-20231025143331575](assets\image-20231025143331575.png)

### window.ethereum API

MetaMask会向网页注入一个全局的API变量`window.ethereum`，出于历史遗留原因，这个全局API变量也可以使用window.web3.currentProvider来访问。该AP1允许 网站请求用户登录，可以从用户接入的区块链读取数据，并切能够提示用户签名要提交的交易。
你可以使用这个API来检测一个浏览器是否注入了`window.ethereum`:

```js
if(typeof window.ethereum !="undefined"){
    console.log("MetaMask is installed")
}
```

ethereum API本身很简单，它同时也封装了以大坊JSON-RPC消息，就像那些流行的库例如web3、truffle、ethjs,Embark等等一样。

### ethereum.isConnected()

如果提供者连接到当前链返回true，否则返回false。
如果提供商未连接，则必须重新加载页面才能重新建立连接

### eth_requestAccounts- 请求用户授权

### ethereum.selectedAddress- 获取当前用户账号

ethereum.selectedAddress 属性返回表示用户当前选择的以大坊账号，16进制字符串表示

### ethereum.isMetaMask-检测是否使用MetaMask

ethereumisMetaMask返回true或false，表示当前用户是否安装了MetaMask.

## 调用智能合约(MetaMask)

项目完整代码： https://github.com/sonichen/Web3.0_Project_Demo/tree/main/%E4%BB%A3%E5%B8%81%E7%BC%96%E5%86%99_ERC20

安装web3

```shell
npm i web3
```



### 实例web3

```js
import { ref } from "vue"
import Web3 from "web3";
import mtcJson from "/contracts/MyToken.json"

// 实例web3
const geerliWS="wss://goerli.infura.io/ws/v3/9b2967006bbc48d789b3afe8d4053643";
const web3=new Web3(Web3.givenProvider||geerliWS);
```



### 账户链接

```js
// 账户连接
// 立即执行(()=>{})()
(async()=>{
  const account=await web3.eth.requestAccounts();
  console.log(account);
})();
```



### 实例合约

```js
// 实例合约
const mtcContract=new web3.eth.Contract(
  mtcJson.abi,
  "0x505e69197375A10E9d55781Fff222D2d7cC8Fa87"
);
```



### 方法

![image-20231025154437914](assets\image-20231025154437914.png)

![image-20231025154449114](assets\image-20231025154449114.png)

### 事件

![image-20231025154500572](assets\image-20231025154500572.png)

### 完整代码

```solidity

<template>
  <h1>sc基本信息</h1>
  <hr>
  <p>当前账号: {{ currentAccount[0] }}</p>
  <p>代币名称: {{ name }}</p>
  <p>代币标识: {{ symbol }}</p>
  <p>发行量: {{ ts }}</p>
  <p>持有数量: {{ bo }}</p>
  <h1>转账操作</h1>
  <hr>
  <p>转入地址
    <input type="text" v-model="toAddress">
  </p>
  <p>抓出金额
    <input type="text" v-model="amount">sc
  </p>
  <button @click="send">开始转账</button>
</template>

<script setup>
import { ref, onMounted, computed } from "vue"
import Web3 from "web3";
import mtcJson from "/contracts/MyToken.json"

// 实例web3
const geerliWS = "wss://goerli.infura.io/ws/v3/9b2967006bbc48d789b3afe8d4053643";
const web3 = new Web3(Web3.givenProvider || geerliWS);

// 实例合约
const mtcContract = new web3.eth.Contract(
  mtcJson.abi,
  "0x596dB4E8f471B421D4eE80ed03Fe63c24f5f1e00" // 部署后得到的交易ID
);
console.log(mtcContract)
// 定义代币信息
const name = ref("");
const symbol = ref("");
const totalSupply = ref("");
const balanceOf = ref(0);
const currentAccount = ref("");
// 定义转账信息
const toAddress = ref("");
const amount = ref("");
// 获取代币信息
const getCoinInfo = async () => {
  const account = await web3.eth.requestAccounts();
  currentAccount.value = account;
  name.value = await mtcContract.methods.name().call();
  symbol.value = await mtcContract.methods.symbol().call();
  totalSupply.value = await mtcContract.methods.totalSupply().call();
  balanceOf.value = await mtcContract.methods.balanceOf(account[0]).call();
};
const ts = computed(() => {
  return web3.utils.fromWei(totalSupply.value, "ether");
});
const bo = computed(() => {
  return web3.utils.fromWei(String(balanceOf.value), "ether");
});
// 转账
const send = () => {
  const weiAmount = web3.utils.toWei(amount.value, "ether");
  mtcContract.methods
    .transfer(toAddress.value, weiAmount)
    .send({
      from: currentAccount.value[0],
    })
    .on("receipt", (res) => {
      console.log("交易成功");
      console.log(res);
    })
};
onMounted(() => {
  getCoinInfo();
});
// 方法
</script>

<style lang="less">
body {
  padding: 10px
}
</style>

```

![image-20231025165904642](assets\image-20231025165904642.png)