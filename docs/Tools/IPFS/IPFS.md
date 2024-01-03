## IPFS

IPFS官网: [https://ipfs.tech/](https://ipfs.tech/)

访问上传的资源地址:  https://ipfs.io/ipfs/XXXXX

for example:  [https://ipfs.io/ipfs/QmRUdwrd5wRpLdfeWUDaNKC8rTubSznPSqS3CRbiKCEaz7](https://ipfs.io/ipfs/QmRUdwrd5wRpLdfeWUDaNKC8rTubSznPSqS3CRbiKCEaz7)

### 什么是IPFS

常常用作Web3的数据存储，如NFTs, Defi

去中心化文件存储平台







### IPFS的优缺点





### FileCoin

FileCoin是IPFS系统中的代币

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

Pinata是一个IPFS数据固定服务，它提供了一个IPFS API和IPFS网关。Pinata可以帮助开发人员将内容上传到IPFS并使用专用网关以惊人的速度获取它。Pinata还提供了一个易于使用的Web界面，以便管理和分发您的IPFS内容。

### 通过Pinata在IPFS上传文件

直接网页端操作



代码操作

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

