vue3和web3.js交互

## vue安装

```
npm install -g @vue/cli
```

```
vue -V
@vue/cli 5.0.8
```

创建项目

```
vue create web3-wallet
```

![image-20231014153451460](assets\image-20231014153451460.png)

进入：http://localhost:8080/

![image-20231014154156073](assets\image-20231014154156073.png)

## Vue3以及Vant介绍使用

### web3相关第三方包

```shell
npm install web3 bip39 ethereumjs-tx@1.3.7 ethereumjs-util ethereumjs-wallet
```

> ethereumjs-tx使用1.3.7版本

### node-polyfill兼容文件配置

1.下载polyfill插件

```
npm install node-polyfill-webpack-plugin -D
```

在vue.config.js中配置

```js
const { defineConfig } = require('@vue/cli-service')
//引入插件
const NodePolyfillWebpackPlugin=require("node-polyfill-webpack-plugin")

module.exports = defineConfig({
  transpileDependencies: true,
  // 插件配置
  configureWebpack: {
    plugins: [
      new NodePolyfillWebpackPlugin()
    ],
  },
})

```



### [vant-ui UI组件库](https://vant-contrib.gitee.io/vant/#/zh-CN)

```
npm i vant
npm i unplugin-vue-components -D
```

在vue.config.js中配置插件

```js
const { defineConfig } = require('@vue/cli-service')
//引入插件
const NodePolyfillWebpackPlugin=require("node-polyfill-webpack-plugin")
// vant
const {VantResolver}=require("unplugin-vue-components/resolvers")
const ComponentsPlugin=require("unplugin-vue-components/webpack")

module.exports = defineConfig({
  transpileDependencies: true,
  // 插件配置
  configureWebpack: {
    plugins: [
      new NodePolyfillWebpackPlugin(),
      ComponentsPlugin({resolvers: [VantResolver()]}),
    ],
  },
})

```

测试app.vue中添加代码

```
<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
  <!-- <HelloWorld msg="Welcome to Your Vue.js App"/>
   -->
   <van-button type="primary">主要按钮</van-button>
<van-button type="success">成功按钮</van-button>
</template>

```

![image-20231014160452718](assets\image-20231014160452718.png)

![image-20231014160647512](assets\image-20231014160647512.png)

### 通过vm配置响应式

```
npm install postcss-px-to-viewport -D
```

## web3连接到以太坊网络（测试网、主网）

1. 什么是Web3

web3是以太坊官方开提供的一个连接以太坊区块链的模块，允许您使用HTTP或IPC与本地或远程以太坊节点进行交互，它包含以太坊生态系统的几乎所有功能。web3模块主要连接以太坊暴露出来的RPC层。开发者利用web3连接RPC层，可以连接任何暴露了RPC接口的节点，从而与区块链交互。web3是一个集合库，支持多种开发语言使用wbe3，其中的JavaScript API叫做web3.js、另外还有web3.py、web3j，web3.js将是我们钱包开发项目的重点。

 

> web3.js开发文档：web3.js - Ethereum JavaScript API — web3.js 1.0.0 documentation
>
> web3.js 中文文档 : web3.js - 以太坊 JavaScript API — web3.js 中文文档 — 登链社区
>
> github地址：https://github.com/web3/web3.js/tree/v1.0.0-beta.34 
>



2. 实例化web3对象

```js
var Web3=require('web3');
var web3=new Web3(Web3.givenProvider||'ws://some.local-or-remote.node:8546')
```

根据API可知需要指定节点地址，我们将ws://some.local-or-remote.node:8546

换成其它连接到以太坊网络的节点的地址，以此来确定连接的以太坊的网络。那么连接到以太坊网络的节点的地址是多少呢？这里我们需要使用到infura。

3. 获取连接到以太坊网络的节点地址

infura提供公开的 Ethereum主网和测试网络节点，到infura.io网站注册后即可获取各个网络的地址。请按照如下步骤获取地址。

**第一步**：打开 infura网站地址：https://infura.io/dashboard，使用邮箱注册后登陆如下所示：

![image-20231014161950496](assets\image-20231014161950496-1697271596917-1.png)

**第二步**：点击上图标记的“create new project”按钮创建一个新项目。然后弹出如下弹框，在输入框输入项目名，如”MyEtherWallet“，然后点击“create project”按钮创建。

**第三步**：然后会显示如下界面，点击下图中的选择框，可以看到提供主网、Kovan测试网络、Ropsten测试网络、Rinkeby测试网络的节点地址。



**第四步**：选择GoerLi测试网络，然后复制地址，将获取到类似这样的地址：

https://kovan.infura.io/v3/d93f......cd67，如下。

![image-20231014162459305](assets\image-20231014162459305.png)

4. 连接到以太坊测试网络

现在将复制的地址替换掉实例化web对象的地址，如下

```
<template>
  <van-button type="primary">主要按钮</van-button>
  <van-button type="success">成功按钮</van-button>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Web3 from 'web3';

var web3 = new Web3(Web3.givenProvider || 'wss://goerli.infura.io/ws/v3/XXXXX')
console.log(web3);

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style lang="less"></style>

```

![image-20231014163536981](assets\image-20231014163536981.png)

连接到以太坊主网与GoerLi测试网络一样的，只需复制主网节点的地址去实例化web3即可。由于在主网上交易需要花费gas，因此我们基于GoerLi测试网络进行开发，后续开发完成后可再切换到主网。在我们开发的项目源码中，我将获取web3实例的代码封装到了myUtils.js文件的getweb3()方法中，用于整个项目统一调用。

## web3高频API

### 账号创建

创建账号需要使用web3.js

```
web3.eth.accounts.create([entropy]);
```

entropy - string可选：是一个随机字符串，作为解锁账号的密码。如果没有传递字符串，就是用random生成随机字符串。

![image-20231014170018910](assets\image-20231014170018910.png)

![image-20231014163934415](assets\image-20231014163934415.png)



```vue
<template>
  <div >api</div>
</template>

<script setup>
 import Web3 from 'web3';
// 实例化web3
var web3 = new Web3(Web3.givenProvider || 'wss://goerli.infura.io/ws/v3/9b2967006bbc48d789b3afe8d4053643')
// 创建账户
const account=web3.eth.accounts.create("123");
console.log(account);
</script>
<style scoped lang='less'>
</style>
```



### 余额获取

```vue
<template>
    <h1>账户信息：</h1>
    <van-divider />
    <p>地址: 0xA55b1C8De4d960D909457F4420bB69fEf1349187</p>
    <p>私钥: 0x1918fd2a7e94ef4ec4280562de49f4aa54b72afa6e80d6ecb100d5e03d492e37</p>
</template>

<script setup>
import Web3 from 'web3';
// 实例化web3
var web3 = new Web3(Web3.givenProvider || 'wss://goerli.infura.io/ws/v3/9b2967006bbc48d789b3afe8d4053643')
// 创建账户
// 每次执行都生成新账号
// const account=web3.eth.accounts.create("123");
// console.log(account.address);
// console.log(account.privateKey);

const address = "0xA55b1C8De4d960D909457F4420bB69fEf1349187";
// 获取余额
// const mount=web3.eth.getBalance(address)
// console.log(mount)
web3.eth.getBalance(address).then((res) => {
    console.log(res)
});
</script>
<style scoped lang='less'></style>
```



### 单位转化

Eth转为wei



wei转为Eth

### Eth转账

```vue
<template>
    <h1>账户信息：</h1>
    <van-divider />
    <!-- <p>地址: 0xA55b1C8De4d960D909457F4420bB69fEf1349187</p>
    <p>私钥: 0x1918fd2a7e94ef4ec4280562de49f4aa54b72afa6e80d6ecb100d5e03d492e37</p> -->
    <p>地址: 0xA55b1C8De4d960D909457F4420bB69fEf1349187</p>
    <p>私钥: 0x1918fd2a7e94ef4ec4280562de49f4aa54b72afa6e80d6ecb100d5e03d492e37</p>
    <h1>转账</h1>
    <van-divider/>
    <van-button type="primary" @click="send">开始转账</van-button>
</template>

<script setup>
import Web3 from 'web3';
import TX from "ethereumjs-tx"

// 实例化web3
var web3 = new Web3(Web3.givenProvider || 'wss://goerli.infura.io/ws/v3/9b2967006bbc48d789b3afe8d4053643')

// 转账
// 1.构建转发数据体
const send= async()=>{
    var address = "0xA55b1C8De4d960D909457F4420bB69fEf1349187";
    var privateKey ="0x1918fd2a7e94ef4ec4280562de49f4aa54b72afa6e80d6ecb100d5e03d492e37";

    // 构建转账参数
    // 获取账户交易次数
    const nounce=await web3.eth.getTransactionCount(address);
    console.log(nounce);
    // 获取预计转账的gas费用
    const gasPrice=await web3.eth.getGasPrice();
    // 转账金额：以wei作为单位
    const value=web3.utils.toWei("0.01", 'ether');
    const rawTx={
        from:address,
        to:"0xb174Eff97638788505B3D7c9532B945086d717f5",
        nounce,
        gasPrice,
        value,
        data:"0x0000"
    };
   
    // //  //////////////////////////////////////
    // //  2. 生成serializedTx
     //通过转账参数计算最终gas费用，并且通过私钥将转账参数进行编码加密
    // 将私钥去除"ox"后进行hex转换
    var privateKey=new Buffer(privateKey.slice(2),"hex");
    // 需要将交易的数据进行预估gas计算，如何将gas值设置到数据参数中
    let gas=await web3.eth.estimateGas(rawTx);
    console.log(gas);
    rawTx.gas=gas;
    // 通过ethereumjs-tx实现私钥加密
    var tx=new TX(rawTx);
    tx.sign(privateKey);
    var serializedTx=tx.serialize();
    //////////////////////////////////////
    // 通过sendSignedTransaction api发送转账交易，并且获取id
    web3.eth
        .sendSignedTransaction("0x"+serializedTx.toString("hex"))
        .on("transactionHash",(txid)=>{
            console.log("交易成功，请在区块链浏览器查看");
            console.log("交易id", txid);
            console.log("https://goerli.etherscan.io/tx/&{txid}");
        })
        .on("receipt",(ret)=>{console.log("receipt")})
        .on("confirmation",(ret=>{console.log("confirmation")}))
        .on("error",(err=>{
            console.log("error"+err);
        }));


    /////////////////////////////////////
}
</script>
<style scoped lang='less'></style>
```
