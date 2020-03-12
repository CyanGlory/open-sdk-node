YouzanYun SDK
=======

[![NPM Version](https://img.shields.io/npm/v/youzanyun-sdk.svg?style=flat)](https://www.npmjs.com/package/youzanyun-sdk)
[![License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)
[![Downloads](https://img.shields.io/npm/dt/youzanyun-sdk.svg)]()
[![Build Status](https://travis-ci.org/youzan/open-sdk-node.png)](https://travis-ci.org/youzan/open-sdk-node)
[![Coverage Status](https://coveralls.io/repos/github/youzan/open-sdk-node/badge.svg?branch=master)](https://coveralls.io/github/youzan/open-sdk-node?branch=master)

## 介绍

使用 javaScript 调用 [有赞云](https://doc.youzanyun.com/doc#/content/27027/27193) 接口。

## 安装

使用 npm 进行安装。

```bash
npm i youzanyun-sdk --save
```

## 使用

示例代码可参考 [examples](examples)  

### 1. 获取及刷新 access_token

#### 1.1 工具型应用 获取 access_token

```node
const youzanyun = require('youzanyun-sdk');

// 获取token
const resp = youzanyun.token.get({
  authorize_type: 'authorization_code',
  client_id: 'YOUR_CLIENT_ID',
  client_secret: 'YOUR_CLIENT_SECRET',
  code: 'YOUR_CODE',
  redirect_uri: 'YOUR_REDIRECT_URI',
});
```

#### 1.2 自用型应用 获取 access_token

```node
const youzanyun = require('youzanyun-sdk');

const resp = youzanyun.token.get({
  authorize_type: 'silent',
  client_id: 'YOUR_CLIENT_ID',
  client_secret: 'YOUR_CLIENT_SECRET',
  grant_id: 110,
  refresh: true, // 是否获取refresh_token(可通过refresh_token刷新token)
});
```

#### 1.3 工具型应用及自用型应用 刷新access_token

```node
const youzanyun = require('youzanyun-sdk');

// 刷新token
const resp = youzanyun.token.get({
  authorize_type: 'refresh_token',
  client_id: 'YOUR_CLIENT_ID',
  client_secret: 'YOUR_CLIENT_SECRET',
  refresh_token: 'YOUR_REFRESH_TOKEN',
});
```

#### 1.4 符合 ES6 规范的写法

以自用型无容器应用获取 token 为例

```node
const youzanyun = require('youzanyun-sdk');

async function getTokenAndPrint() {
    try {
        let data = await youzanyun.token.get({
            authorize_type: 'silent',
            client_id: 'YOUR_CLIENT_ID', // 客户端编号, 可以在有赞云-控制台-应用-应用概况-应用信息, 位置获取
            client_secret: 'YOUR_CLIENT_SECRET', // // 客户端密钥, 可以在有赞云-控制台-应用-应用概况-应用信息, 位置获取
            grant_id: 110, // 店铺ID, 可以在有赞云-控制台-应用-应用概况-授权信息, 位置获取
            refresh: true, // 是否获取refresh_token(可通过refresh_token刷新token)
          }).data;
        const token = data.data.access_token;
        console.log(token);
        
    } catch (error) {
        console.log('error: ', error);
        
    }
}

getTokenAndPrint();
```

### 2. 接口调用

#### 2.1 Token方式

```node
const youzanyun = require('youzanyun-sdk');

const token = 'f59b1a6bb04f4eqweqd1c6af315d';
const params = {tid: 'E20190509110527067500013'};

const resp = youzanyun.client.call({
  api: 'youzan.trade.get',
  version: '4.0.0',
  token,
  params,
});
```

#### 2.2 文件上传

```node
const youzanyun = require('youzanyun-sdk');

const token = 'f59b1a6bb0asdasq613d1c6af315d';
const files = {'image': path.resolve(__dirname, './pic.png')};

const resp = youzanyun.client.call({
  api: 'youzan.materials.storage.platform.img.upload',
  version: '3.0.0',
  token,
  params: {},
  files,
});
```

### 3. 消息解密

```node
const youzanyun = require('youzanyun-sdk');

const messages = 'YOUR_RECEIVED_MESSAGES';
const clientSecret = 'YOUR_CLIENT_SECRET';

const resp = youzanyun.crypto.decrypt(messages, clientSecret);
```

## License

[MIT](LICENSE)
