# 校园跑腿小程序-服务器部署教程

### 1.准备工作

请在服务器直接安装宝塔用宝塔来安装环境
- 宝塔安装命令[https://www.bt.cn/new/download.html](https://www.bt.cn/new/download.html)
- 进入面板->【软件商店】
- 安装 nginx 选择 1.18 版本
- 安装 mysql 选择 8.x 或 5.7 以上版本
- 安装 PM2 管理器, 进入管理器 Node 版本选择 v14.18.0, 作者开发是 v14.18.0, 然后点击切换版本
- 安装 redis 最新即可


### 2.克隆源代码到服务器

```
# 执行克隆 或 将本地开发好的代码打包到服务器
git clone https://gitee.com/landalfyao/ddapp
```

结果：
获得一个"ddapp"目录

### 3.服务端

#### 3.1 配置文件

> 找到 /server/src/config/config.env.ts.bak 文件 复制并粘贴到同目录下。命名为 config.prod.ts 打开文件进行配置

```
// 数据库配置
...
orm: {
    /**
     * 单数据库实例
     */
    type: 'mysql',
    host: 'localhost',
    port: 3306,
    username: 'root', // 用户名
    password: 'root', // 密码
    database: 'ddrunv2-free', // 数据库
    synchronize: true, // 如果第一次使用，不存在表，有同步的需求可以写 true
    logging: false,
  }
  ...
  // 配置redis
  redis: {
    client: {
      port: 6379, // Redis port
      host: '127.0.0.1', // Redis host
      password: '',
      db: 0,
    },
  }
  ...
  // task 和 bull中的redis 都填一样的即可
```

#### 3.2 安装依赖

```
# 进入文件夹
cd ddrun

# 安装依赖
yarn
```

#### 3.3 构建

```
yarn build
```

#### 3.4 部署

```
// 若未安装pm2 请先安装
npm install -g pm2

// 开始部署
pm2 start ./ecosystem.config.js

// 查看是否部署成功
pm2 list

// 部署出错排查
pm2 logs
```

### 4.管理员端

#### 4.1 安装依赖

```
# 进入文件夹
cd ddrun

# 安装依赖
yarn
```

#### 4.2 构建

```
yarn build
```

构建成功后会在 admin/目录下生成 dist 目录

### 5.在本地打开小程序

#### 5.1 修改配置文件

```
// 打开/miniprogram/src/utils/constrants.ts

export const API = () => {
  return "http://xxx.xx.xxx/api/"; //修改为您的服务器域名
};
```

#### 5.2 安装依赖

```
yarn
```

#### 5.3 构建

```
yarn build-wx
```

#### 5.4 上传代码

- 打开微信开发者工具
- 导入项目，目录为 /miniprogram/dist/build/mp-weixin
- 提交代码

### 6.nginx 配置

- 注意只需查看管理员的即可
  [参考商用版 nginx 不熟教程](https://v0lxla43d1n.feishu.cn/wiki/wikcnu4pnYxBFoFwy2A7AYF6yBf)
