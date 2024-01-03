## IPFS

官网：https://www.ipfs.io

IPFS（InterPlanetary File System），星际文件系统。

可以把IPFS视为网盘，将数据上传到IPFS之后会得到一个hash值（也就是这个数据的地址），无需知道文件存储在哪里，通过哈希值就能够找到这个文件，IPFS的网络里是根据**内容寻址**的。



IPFS 技术就是把文件打碎，分散地存储在不同的硬盘里，下载的时候，再从这些散落在全球各地的硬盘里读取。



IPFS是一个协议，也是一个P2P网络，类似于BT网络，但是拥有更加强大的功能，使得IPFS拥有可以取代HTTP的潜力。



IPFS有一个激励层——Filecoin，是一个基于区块链的分布式存储网络，他把云存储变为一个算法市场，通过代币来做资源（存储和检索）使用者（IPFS用户）和资源的提供者（矿工）之间的桥梁，在这里，交易双方可以提出自己的需要达成交易。



## 和传统文件系统区别

> IPFS和传统文件系统的一个重要区别就是——内容寻址。顾名思义，就是文件的内容定了，其地址（访问路径）也就确定了。这和我们平时存放文件不一样。通常，我可以给一张图片随意更换文件名，把它拷贝到不同的路径。这样，一模一样的文件，其访问方式却随时变化，不可能根据文件的内容确定其访问路径。
>
> 相比较而言，内容寻址的IPFS就具有一个天然的优势——防篡改。数据只要修改了一个bit，其地址就彻底变化。

## windows本地构建节点

下载：https://dist.ipfs.tech/#go-ipfs

创建节点

```
.\ipfs.exe init
```

查看节点ID

```
 .\ipfs.exe id
```

启动节点服务器

```
.\ipfs.exe daemon
```

向节点新增文件

```
 .\ipfs.exe add hello.txt
```

新增文件夹

```
.\ipfs.exe add -r hello
```

浏览器打开http://localhost:5001/webui

![image-20240103182208577](assets\image-20240103182208577.png)









## Pinata

官网: [https://www.pinata.cloud/](https://www.pinata.cloud/)

提供了一个可视化的平台对IPFS进行操作

Pinata is an IPFS Pinning Service that provides user with, you guessed it, IPFS services!

Pinata是一个IPFS数据固定服务，它提供了一个IPFS API和IPFS网关。Pinata可以帮助开发人员将内容上传到IPFS并使用专用网关以惊人的速度获取它。Pinata还提供了一个易于使用的Web界面，以便管理和分发IPFS内容。



### 通过Pinata在IPFS上传文件



#### 网页操作

根据网页提示上传一个图片，得到hash值QmR88UDpzfq6Uaf2e6ouHDgaoEsMK2W1fqzSNGuCbAKdEQ。

查询[https://ipfs.io/ipfs/QmR88UDpzfq6Uaf2e6ouHDgaoEsMK2W1fqzSNGuCbAKdEQ/](https://ipfs.io/ipfs/QmR88UDpzfq6Uaf2e6ouHDgaoEsMK2W1fqzSNGuCbAKdEQ/ ) 看到上传内容。



#### 代码

```js
const axios = require('axios')
const FormData = require('form-data')
const fs = require('fs')
const JWT = 'Bearer PASTE_YOUR_PINATA_JWT'

const pinFileToIPFS = async () => {
    const formData = new FormData();
    const src = "path/to/file.png";
    
    const file = fs.createReadStream(src)
    formData.append('file', file)
    
    const pinataMetadata = JSON.stringify({
      name: 'File name',
    });
    formData.append('pinataMetadata', pinataMetadata);
    
    const pinataOptions = JSON.stringify({
      cidVersion: 0,
    })
    formData.append('pinataOptions', pinataOptions);

    try{
      const res = await axios.post("https://api.pinata.cloud/pinning/pinFileToIPFS", formData, {
        maxBodyLength: "Infinity",
        headers: {
          'Content-Type': `multipart/form-data; boundary=${formData._boundary}`,
          Authorization: JWT
        }
      });
      console.log(res.data);
    } catch (error) {
      console.log(error);
    }
}

```



```shell
{
  "IpfsHash": "bafkreih5aznjvttude6c3wbvqeebb6rlx5wkbzyppv7garjiubll2ceym4",
  "PinSize": 32942,
  "Timestamp": "2023-06-16T17:24:37.998Z"
}
```

