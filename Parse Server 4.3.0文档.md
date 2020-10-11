<p align="center">
    <img alt="Parse Server" src="https://github.com/parse-community/parse-server/raw/master/.github/parse-server-logo.png" width="500">
  </a>
</p>

<p align="center">
  Parse Server是一款开源后端服务框架，可以部署到任何能运行Node.js的基础架构中。
</p>


<p align="center">
    <a href="https://twitter.com/intent/follow?screen_name=parseplatform"><img alt="Follow on Twitter" src="https://img.shields.io/twitter/follow/parseplatform?style=social&label=Follow"></a>
  <a href="https://travis-ci.org/parse-community/parse-server"><img alt="Build status" src="https://img.shields.io/travis/parse-community/parse-server/master.svg?style=flat"></a>
    <a href="https://codecov.io/github/parse-community/parse-server?branch=master"><img alt="Coverage status" src="https://img.shields.io/codecov/c/github/parse-community/parse-server/master.svg"></a>
    <a href="https://www.npmjs.com/package/parse-server"><img alt="npm version" src="https://img.shields.io/npm/v/parse-server.svg?style=flat"></a>
    <a href="https://community.parseplatform.org/"><img alt="Join the conversation" src="https://img.shields.io/discourse/https/community.parseplatform.org/topics.svg"></a>
    <a href="https://greenkeeper.io/"><img alt="Greenkeeper badge" src="https://badges.greenkeeper.io/parse-community/parse-server.svg"></a>
</p>

<p align="center">
    <img alt="MongoDB 3.6" src="https://img.shields.io/badge/mongodb-3.6-green.svg?logo=mongodb&style=flat">
    <img alt="MongoDB 4.0" src="https://img.shields.io/badge/mongodb-4.0-green.svg?logo=mongodb&style=flat">
</p>

<h2 align="center">我们的赞助商</h2>
<p align="center">
    <p align="center">我们的支持者和赞助者帮助确保Parse Platform的质量和及时开发。</p>
  <details align="center">
  <summary align="center"><b>🥉 Bronze Sponsors</b></summary>
  <a href="https://opencollective.com/parse-server/sponsor/0/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/0/avatar.svg"></a>
  </details>

</p>
<p align="center">
  <a href="#backers"><img alt="Backers on Open Collective" src="https://opencollective.com/parse-server/tiers/backers/badge.svg" /></a>
  <a href="#sponsors"><img alt="Sponsors on Open Collective" src="https://opencollective.com/parse-server/tiers/sponsors/badge.svg" /></a>
</p>
<br>

Parse Server 与Express Web应用程序框架一起使用。可以将其添加到现有的Web应用程序中，也可以单独运行。

可以在[wiki](https://github.com/parse-community/parse-server/wiki)中访问Parse Server的完整文档。 The [Parse Server 指南](http://docs.parseplatform.org/parse-server/guide/) 是很好的入门起点。 [API reference](http://parseplatform.org/parse-server/api/) 和 [Cloud Code guide](https://docs.parseplatform.org/cloudcode/guide/) 也可供参考。 如果您有兴趣为Parse Server开源做贡献，[Development guide](http://docs.parseplatform.org/parse-server/guide/#development-guide) 这本开发指南将帮助您开始。

- [入门](#getting-started)
    - [运行 Parse Server](#running-parse-server)
        - [本地](#locally)
        - [Docker](#inside-a-docker-container)
        - [保存您的第一个对象](#saving-your-first-object)
        - [链接 SDK](#connect-your-app-to-parse-server)
    - [在其他地方运行](#running-parse-server-elsewhere)
        - [示例应用](#parse-server-sample-application)
        - [Parse Server + Express](#parse-server--express)
    - [配置](#configuration)
        - [基本选项](#basic-options)
        - [客户端可选密钥](#client-key-options)
        - [邮件验证 & 密码重置](#email-verification-and-password-reset)
        - [自定义页面](#custom-pages)
        - [使用环境变量](#using-environment-variables-to-configure-parse-server)
        - [可用适配器 Adapters](#available-adapters)
        - [配置文件适配器 File Adapters](#configuring-file-adapters)
        - [日志](#logging)
- [实时查询](#live-queries)
- [GraphQL](#graphql)
- [升级到 3.0.0](#upgrading-to-300)
- [支持](#support)
- [驾驭技术前沿](#want-to-ride-the-bleeding-edge)
- [贡献](#contributing)
- [贡献者](#contributors)
- [赞助商](#sponsors)
- [资助者](#backers)


# 入门

最快，最简单的入门方法是在本地运行MongoDB和Parse Server。

## 运行 Parse Server

在开始之前，请确保已安装下列依赖：

- 包含`npm`的[NodeJS](https://www.npmjs.com/) 
- [MongoDB](https://www.mongodb.com/) 或 [PostgreSQL](https://www.postgresql.org/)(包含 [PostGIS](https://postgis.net) 2.2.0 以上)
- [Docker](https://www.docker.com/) 可选安装

### 本地
```bash
$ npm install -g parse-server mongodb-runner
$ mongodb-runner start
$ parse-server --appId APPLICATION_ID --masterKey MASTER_KEY --databaseURI mongodb://localhost/test
```
***注意:*** *如果 * `-g` *由于权限问题* (`npm ERR! code 'EACCES'`)而导致安装失败，*请参考 [这个链接](https://docs.npmjs.com/getting-started/fixing-npm-permissions)。*


### 安装到Docker容器中
```bash
$ git clone https://github.com/parse-community/parse-server
$ cd parse-server
$ docker build --tag parse-server .
$ docker run --name my-mongo -d mongo
$ docker run --name my-parse-server -v cloud-code-vol:/parse-server/cloud -v config-vol:/parse-server/config -p 1337:1337 --link my-mongo:mongo -d parse-server --appId APPLICATION_ID --masterKey MASTER_KEY --databaseURI mongodb://mongo/test
```

您可以使用任意字符串作为应用程序ID和主密钥。这些将由您的客户端用于通过Parse Server进行身份验证。

就是这样！您现在正在计算机上运行单独的Parse Server。

**想使用远程MongoDB？** 启动`parse-server`时请传递  `--databaseURI DATABASE_URI` 参数。 [点这里](#configuration)了解更多Parse Server的配置参数。想了解完整的可配置项列表，可运行`parse-server --help`获得。

### 保存您的第一个对象

现在Parse Server运行起来了，是时候保存您的第一个对象了。 我们会用 [REST API](http://docs.parseplatform.org/rest/guide)进行操作，您也可以使用任何 [Parse SDKs](http://parseplatform.org/#sdks)进行同样的操作。 运行下面的命令：

```bash
$ curl -X POST \
-H "X-Parse-Application-Id: APPLICATION_ID" \
-H "Content-Type: application/json" \
-d '{"score":1337,"playerName":"Sean Plott","cheatMode":false}' \
http://localhost:1337/parse/classes/GameScore
```

您将会得到类似如下响应内容：

```js
{
  "objectId": "2ntvSpRGIK",
  "createdAt": "2016-03-11T23:51:48.050Z"
}
```

现在您可以直接检索此对象 (请确保将 `2ntvSpRGIK` 替换为您创建对象时收到的实际  `objectId` )：

```bash
$ curl -X GET \
  -H "X-Parse-Application-Id: APPLICATION_ID" \
  http://localhost:1337/parse/classes/GameScore/2ntvSpRGIK
```
```json
// Response
{
  "objectId": "2ntvSpRGIK",
  "score": 1337,
  "playerName": "Sean Plott",
  "cheatMode": false,
  "updatedAt": "2016-03-11T23:51:48.050Z",
  "createdAt": "2016-03-11T23:51:48.050Z"
}
```

但是，通过对象id检索的方式并不切实际。 大多数情况下您需要对一个集合进行检索，像这样：

```bash
$ curl -X GET \
  -H "X-Parse-Application-Id: APPLICATION_ID" \
  http://localhost:1337/parse/classes/GameScore
```
```json
// `results`数组中包含所有匹配对象
{
  "results": [
    {
      "objectId": "2ntvSpRGIK",
      "score": 1337,
      "playerName": "Sean Plott",
      "cheatMode": false,
      "updatedAt": "2016-03-11T23:51:48.050Z",
      "createdAt": "2016-03-11T23:51:48.050Z"
    }
  ]
}

```

要了解更多有关在Parse Server上使用保存和查询对象的信息，请查阅[Parse 文档](http://docs.parseplatform.org).

### 将您的应用程序接入Parse Server

Parse 为所有主流平台提供了 SDK 。 请参考[如何将您的应用程序接入Parse Server](https://docs.parseplatform.org/parse-server/guide/#using-parse-sdks-with-parse-server).

## 在其他地方运行 Parse Server

一旦您对工程的工作方式有了更好的了解，请参考 [Parse Server wiki](https://github.com/parse-community/parse-server/wiki) 的深入指南，了解如何将Parse Server部署到主流基础架构提供商上。继续阅读以了解更多有关运行Parse Server的其他方式。

### Parse Server 示例程序

我们提供了一个基本的 [Node.js 应用程序](https://github.com/parse-community/parse-server-example)， 在Express上使用Parse Server模块，使其可以轻易的部署到各种基础架构提供商上：

* [Heroku and mLab](https://devcenter.heroku.com/articles/deploying-a-parse-server-to-heroku)
* [AWS and Elastic Beanstalk](http://mobile.awsblog.com/post/TxCD57GZLM2JR/How-to-set-up-Parse-Server-on-AWS-using-AWS-Elastic-Beanstalk)
* [Google App Engine](https://medium.com/@justinbeckwith/deploying-parse-server-to-google-app-engine-6bc0b7451d50)
* [Microsoft Azure](https://azure.microsoft.com/en-us/blog/azure-welcomes-parse-developers/)
* [SashiDo](https://blog.sashido.io/tag/migration/)
* [Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-run-parse-server-on-ubuntu-14-04)
* [Pivotal Web Services](https://github.com/cf-platform-eng/pws-parse-server)
* [Back4app](http://blog.back4app.com/2016/03/01/quick-wizard-migration/)
* [Glitch](https://glitch.com/edit/#!/parse-server)
* [Flynn](https://flynn.io/blog/parse-apps-on-flynn)

### Parse Server + Express

您也可以创建一个Parse Server实例，挂载到一个新的或现有的Express网站上：

```js
var express = require('express');
var ParseServer = require('parse-server').ParseServer;
var app = express();

var api = new ParseServer({
  databaseURI: 'mongodb://localhost:27017/dev', // 您的MongoDb连接
  cloud: '/home/myApp/cloud/main.js', // 云函数代码的地址
  appId: 'myAppId',
  masterKey: 'myMasterKey', // 将此密钥保密！
  fileKey: 'optionalFileKey',
  serverURL: 'http://localhost:1337/parse' // 如果需要https，别忘了改这里
});

// 带有/parse前缀的URL将会通过Parse API处理
app.use('/parse', api);

app.listen(1337, function() {
  console.log('parse-server-example running on port 1337.');
});
```

有关所有可用选项列表通过运行 `parse-server --help`获得，或者可以查看[Parse Server 配置](http://parseplatform.org/parse-server/api/master/ParseServerOptions.html).

## 配置

Parse Server 可通过下列参数进行配置。您需要传递这些参数来运行独立的`parse-server`，或者加载一个JSON格式的配置文件 `parse-server path/to/configuration.json`。 如果您在Express上运行Parse Server，您也可以将它们作为参数传递给`ParseServer`。

有关所有可用选项列表通过运行 `parse-server --help`获得，或者可以查看[Parse Server 配置](http://parseplatform.org/parse-server/api/master/ParseServerOptions.html).

### 基本参数

* `appId` **(必需)** - 与此服务器实例一起托管的应用程序ID。您可以使用任意字符串。对于已迁移的应用程序，这应与您托管的Parse应用程序匹配。
* `masterKey` **(必需)** - 用于覆盖ACL安全性的主密钥。您可以使用任意字符串。保密！对于已迁移的应用程序，这应与您托管的Parse应用程序匹配。
* `databaseURI` **(必需)** - 数据库的连接字符串，例如 `mongodb://user:pass@host.com/dbname`。如果您的密码包含特殊字符，请确保对密码进行[URL编码](https://app.zencoder.com/docs/guides/getting-started/special-characters-in-usernames-and-passwords) 。
* `port` - 默认端口为1337，指定此参数以使用其他端口。
* `serverURL` - 您的Parse Server的URL（不要忘记指定http：//或https：//）。从云函数向Parse Server发出请求时，将使用此URL。
* `cloud` - 云函数入口文件`main.js`的绝对路径。
* `push` - APNS和GCM推送的配置选项。请参阅 [推送通知快速入门](http://docs.parseplatform.org/parse-server/guide/#push-notifications_push-notifications-quick-start).

### 客户端密钥参数

Parse Server不再需要与Parse一起使用的客户端密钥。如果您仍然需要它们，也许能够拒绝访问较旧的客户端，则可以在初始化时设置密钥。设置这些key中的任何一个，则会要求所有请求携带一个已配置的key。

* `clientKey`
* `javascriptKey`
* `restAPIKey`
* `dotNetKey`

### 邮件验证和密码重置

验证用户电子邮件地址并通过电子邮件启用密码重置需要email adapter。作为`parse-server`的一部分，我们提供了一个用于通过Mailgun发送电子邮件的adapter。要使用它，请注册Mailgun，并将其添加到您的初始化代码中：

```js
var server = ParseServer({
  ...otherOptions,
  // 开启邮件验证
  verifyUserEmails: true,

  // if `verifyUserEmails` is `true` and
  //     if `emailVerifyTokenValidityDuration` is `undefined` then
  //        email verify token never expires 永不过期
  //     else
  //        email verify token expires after `emailVerifyTokenValidityDuration` 会在这个时间后过期
  //
  // `emailVerifyTokenValidityDuration` defaults to `undefined`
  //
  // email verify token below expires in 2 hours (= 2 * 60 * 60 == 7200 seconds)
  emailVerifyTokenValidityDuration: 2 * 60 * 60, // in seconds (2 hours = 7200 seconds)

  // set preventLoginWithUnverifiedEmail to false to allow user to login without verifying their email
  // set preventLoginWithUnverifiedEmail to true to prevent user from login if their email is not verified 是否阻止用户在其邮件验证前登录
  preventLoginWithUnverifiedEmail: false, // defaults to false

  // The public URL of your app.
  // This will appear in the link that is used to verify email addresses and reset passwords.
  // Set the mount path as it is in serverURL
  publicServerURL: 'https://example.com/parse',
  // Your apps name. This will appear in the subject and body of the emails that are sent.
  appName: 'Parse App',
  // The email adapter
  emailAdapter: {
    module: '@parse/simple-mailgun-adapter',
    options: {
      // The address that your emails come from
      fromAddress: 'parse@example.com',
      // Your domain from mailgun.com
      domain: 'example.com',
      // Your API key from mailgun.com
      apiKey: 'key-mykey',
    }
  },

  // 帐户锁定设置 (可选) - 默认为 undefined
  // 如果设置了帐户锁定策略，并且失败登录尝试的阈值数量超过“阈值”，则“ login” api调用将返回错误代码“ Parse.Error”。错误消息“ OBJECT_NOT_FOUND”，由于多次登录尝试失败，您的帐户已被锁定。请在<duration>分钟后重试。在“ duration”分钟未尝试登录后，应用程序将允许用户再次尝试登录。
  accountLockout: {
    duration: 5, // 锁定持续时间设置确定锁定的帐户在自动解锁之前保持锁定状态的分钟数。将其设置为大于0且小于100000的值。
    threshold: 3, // 阈值策略设置确定将导致用户帐户被锁定的失败登录尝试次数。整数值大于0和小于1000。
  },
  // 密码规范设置，可选
  passwordPolicy: {
    // 可设置其中一个或都设置，以保证强制使用强密码
    // 如果两项都被设置，则必须都通过才可设置密码
    // 1. 正则对象或者正则字符串
    validatorPattern: /^(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.{8,})/, // 至少8个字符，至少1个小写字母，1个大写字母和1位数字
    // 2. 回调函数
    validatorCallback: (password) => { return validatePassword(password) },
    validationError: 'Password must contain at least 1 digit.' // 可选的错误信息设置，替代默认的 "Password does not meet the Password Policy requirements."。
    doNotAllowUsername: true, // 禁止在密码中使用用户名
    maxPasswordAge: 90, // 密码到期天数的设置。 如果用户在注册/上次重置后的这段时间内未重置密码，则登录失败。
    maxPasswordHistory: 5, // 可选设置，以防止重复使用前n个密码。可以指定的最大值为20。未指定最大值或指定0将不会强制执行对比历史记录。
    //可选设置，设置密码重置链接的有效期限（秒）
    resetTokenValidityDuration: 24*60*60, // 24小时后过期
  }
});
```

您还可以使用社区提供的其他电子邮件适配器，例如：
- [parse-smtp-template (Multi Language and Multi Template)](https://www.npmjs.com/package/parse-smtp-template)
- [parse-server-postmark-adapter](https://www.npmjs.com/package/parse-server-postmark-adapter)
- [parse-server-sendgrid-adapter](https://www.npmjs.com/package/parse-server-sendgrid-adapter)
- [parse-server-mandrill-adapter](https://www.npmjs.com/package/parse-server-mandrill-adapter)
- [parse-server-simple-ses-adapter](https://www.npmjs.com/package/parse-server-simple-ses-adapter)
- [parse-server-mailgun-adapter-template](https://www.npmjs.com/package/parse-server-mailgun-adapter-template)
- [parse-server-sendinblue-adapter](https://www.npmjs.com/package/parse-server-sendinblue-adapter)
- [parse-server-mailjet-adapter](https://www.npmjs.com/package/parse-server-mailjet-adapter)
- [simple-parse-smtp-adapter](https://www.npmjs.com/package/simple-parse-smtp-adapter)
- [parse-server-generic-email-adapter](https://www.npmjs.com/package/parse-server-generic-email-adapter)

### 自定义页面

可以更改应用程序的默认页面，并将用户重定向到其他路径或域名。

```js
var server = ParseServer({
  ...otherOptions,

  customPages: {
    passwordResetSuccess: "http://yourapp.com/passwordResetSuccess",
    verifyEmailSuccess: "http://yourapp.com/verifyEmailSuccess",
    parseFrameURL: "http://yourapp.com/parseFrameURL",
    linkSendSuccess: "http://yourapp.com/linkSendSuccess",
    linkSendFail: "http://yourapp.com/linkSendFail",
    invalidLink: "http://yourapp.com/invalidLink",
    invalidVerificationLink: "http://yourapp.com/invalidVerificationLink",
    choosePassword: "http://yourapp.com/choosePassword"
  }
})
```

### 使用环境变量配置Parse Server

您可以使用环境变量配置Parse Server：

```bash
PORT
PARSE_SERVER_APPLICATION_ID
PARSE_SERVER_MASTER_KEY
PARSE_SERVER_DATABASE_URI
PARSE_SERVER_URL
PARSE_SERVER_CLOUD
```

默认端口为1337，要使用其他端口，请设置PORT变量：

```bash
$ PORT=8080 parse-server --appId APPLICATION_ID --masterKey MASTER_KEY
```

有关所有可用的环境变量选项列表通过运行 `parse-server --help`获得，或者可以查看[Parse Server 配置](https://github.com/parse-community/parse-server/blob/master/src/Options/Definitions.js)。

### 可用的 Adapter

所有可用的官方Adapter都在[npm (@parse)](https://www.npmjs.com/search?q=scope%3Aparse)。

这个组织[Parse Server Modules](https://github.com/parse-server-modules) 提供了一些维护良好的Adapter。

您还可以通过搜索[npm](https://www.npmjs.com/search?q=parse-server%20adapter&page=1&ranking=optimal)找到更多社区维护的Adapter。

### 配置文件适配器 File Adapter

Parse Server 允许开发者选择不同的Adapter托管文件：

* `GridFSBucketAdapter`，MongoDB支持;
* `S3Adapter`，[Amazon S3](https://aws.amazon.com/s3/)支持;
* `GCSAdapter`，[Google Cloud Storage](https://cloud.google.com/storage/)支持；

`GridFSBucketAdapter`默认提供且无需配置，如果您对其他的感兴趣，请参考[Parse Server 指南](http://docs.parseplatform.org/parse-server/guide/#configuring-file-adapters).

### 强制幂等
 
**注意，这是实验性功能，可能不适用于生产环境。**

通常由于网络问题或移动端的网络适配器访问限制，此功能对Parse Server多次收到的相同请求进行重复数据删除。

相同的请求由其 `X-Parse-Request-Id`请求标头标识。因此，客户端请求必须包括此标头才能应用重复数据删除。不包含此标头的请求不能删除重复数据，并且由Parse Server正常处理。这意味着向客户端推出此功能是无缝的，因为启用此功能后Parse Server仍会处理不带此标头的请求。

> 需要在客户端启用此功能以发送标头，并在服务器上启用以处理标头。请参阅特定的Parse SDK文档，以了解是否支持该功能。

重复数据删除仅用于对象创建和更新（`POST`以及`PUT`请求）。对于对象查找和删除（`GET`以及`DELETE`请求），不执行重复数据删除，因为根据定义，这些操作已经是幂等的。

#### 配置示例
```
let api = new ParseServer({
    idempotencyOptions: {
        paths: [".*"],       // 强制应用所有请求
        ttl: 120             // 保存请求id 120s
    }
}
```
#### 参数

| 参数 | 可选 | 类型  | 默认值 | 示例 | 环境变量 | 描述 |
|-----------|----------|--------|---------------|-----------|-----------|-------------|
| `idempotencyOptions` | yes | `Object` | `undefined` |   | PARSE_SERVER_EXPERIMENTAL_IDEMPOTENCY_OPTIONS | 配置使用强制幂等的路由 |
| `idempotencyOptions.paths`| yes | `Array<String>`  | `[]` |  `.*` (all paths, includes the examples below), <br>`functions/.*` (all functions), <br>`jobs/.*` (all jobs), <br>`classes/.*` (all classes), <br>`functions/.*` (all functions), <br>`users` (user creation / update), <br>`installations` (installation creation / update) | PARSE_SERVER_EXPERIMENTAL_IDEMPOTENCY_PATHS | An array of path patterns that have to match the request path for request deduplication to be enabled. The mount path must not be included, for example to match the request path `/parse/functions/myFunction` specifiy the path pattern `functions/myFunction`. A trailing slash of the request path is ignored, for example the path pattern `functions/myFunction` matches both `/parse/functions/myFunction` and `/parse/functions/myFunction/`. |
| `idempotencyOptions.ttl` | yes | `Integer` | `300` | `60` (60 seconds) | PARSE_SERVER_EXPERIMENTAL_IDEMPOTENCY_TTL | The duration in seconds after which a 从数据库中丢弃请求记录之后的持续时间（秒）。由于网络问题而产生的重复请求可能会在几毫秒内（最多几秒钟）到达。此值必须大于0。|

#### 注意

- 该功能目前仅适用于MongoDB，不适用于Postgres。

### 日志

Parse Server将日志默认记录到：
* 控制台
* JSON文件中，每天换行

在Parse Dashboard中也可查看日志。

**想要记录每个请求和响应？** 启动`parse-server`时设置 `VERBOSE` 环境变量。用法：-  `VERBOSE='1' parse-server --appId APPLICATION_ID --masterKey MASTER_KEY`

**想将日志文件放到其他文件夹中？** 启动`parse-server`时设置 `PARSE_SERVER_LOGS_FOLDER` 环境变量。 用法：-  `PARSE_SERVER_LOGS_FOLDER='<path-to-logs-folder>' parse-server --appId APPLICATION_ID --masterKey MASTER_KEY`

**想要记录特定级别？** 启动`parse-server`时设置 `logLevel` 环境变量。用法：-  `parse-server --appId APPLICATION_ID --masterKey MASTER_KEY --logLevel LOG_LEVEL`

**想要新行分隔的JSON错误日志（供CloudWatch，Google Cloud Logging等使用）吗？** 启动`parse-server`时设置 `JSON_LOGS` 环境变量。用法：-  `JSON_LOGS='1' parse-server --appId APPLICATION_ID --masterKey MASTER_KEY`

# 实时查询

实时查询旨在用于实时响应式应用程序中，在这些应用程序中，仅使用传统查询范例可能会导致一些问题，例如响应时间增加以及网络和服务器使用率较高。如果您需要用来自数据库的新鲜数据不断更新页面，则经常使用实时查询，这种情况通常发生在（但不限于）在线游戏，消息传递客户端和共享任务列表中。

看看[Live Query指南](https://docs.parseplatform.org/parse-server/guide/#live-queries)，[Live Query Server设置指南](https://docs.parseplatform.org/parse-server/guide/#scalability)和[Live Query协议规范](https://github.com/parse-community/parse-server/wiki/Parse-LiveQuery-Protocol-Specification)。您可以设置独立服务器或多个实例以实现可伸缩性（推荐）。

# GraphQL

由Facebook开发的[GraphQL](https://graphql.org/)是一种用于API的开源数据查询和处理语言。除了传统的REST API，Parse Server还会根据您当前的应用程序架构自动生成GraphQL API。Parse Server还允许您定义自定义GraphQL查询和改变，其解析器可以绑定到您的云函数。

## 运行

### 使用命令行方式运行

运行Parse GraphQL API的最简单方法是通过CLI：

```bash
$ npm install -g parse-server mongodb-runner
$ mongodb-runner start
$ parse-server --appId APPLICATION_ID --masterKey MASTER_KEY --databaseURI mongodb://localhost/test --publicServerURL http://localhost:1337/parse --mountGraphQL --mountPlayground
```

启动服务器后，您可以在浏览器中访问http:// localhost:1337/playground来开始使用GraphQL API。

***注意:*** ***不要*** 在生产环境中使用 --mountPlayground 选项 [Parse Dashboard](https://github.com/parse-community/parse-dashboard) 具有内置的GraphQL Playground，生产应用程序推荐使用它。

### 使用 Docker

您还可以在Docker容器中运行Parse GraphQL API：

```bash
$ git clone https://github.com/parse-community/parse-server
$ cd parse-server
$ docker build --tag parse-server .
$ docker run --name my-mongo -d mongo
$ docker run --name my-parse-server --link my-mongo:mongo -v cloud-code-vol:/parse-server/cloud -v config-vol:/parse-server/config -p 1337:1337 -d parse-server --appId APPLICATION_ID --masterKey MASTER_KEY --databaseURI mongodb://mongo/test --publicServerURL http://localhost:1337/parse --mountGraphQL --mountPlayground
```

启动服务器后，您可以在浏览器中访问http://localhost:1337/playground来开始使用GraphQL API。

***注意:*** ***不要*** 在生产环境中使用 --mountPlayground 选项 [Parse Dashboard](https://github.com/parse-community/parse-dashboard) 具有内置的GraphQL Playground，生产应用程序推荐使用它。

### 使用 Express.js

您还可以将GraphQL API与REST API或单独的安装在Express.js应用程序中。您首先需要创建一个新项目并安装所需的依赖项：

```bash
$ mkdir my-app
$ cd my-app
$ npm install parse-server express --save
```

接着，创建 `index.js` 文件，写入以下内容：

```js
const express = require('express');
const { default: ParseServer, ParseGraphQLServer } = require('parse-server');

const app = express();

const parseServer = new ParseServer({
  databaseURI: 'mongodb://localhost:27017/test',
  appId: 'APPLICATION_ID',
  masterKey: 'MASTER_KEY',
  serverURL: 'http://localhost:1337/parse',
  publicServerURL: 'http://localhost:1337/parse'
});

const parseGraphQLServer = new ParseGraphQLServer(
  parseServer,
  {
    graphQLPath: '/graphql',
    playgroundPath: '/playground'
  }
);

app.use('/parse', parseServer.app); // (Optional) Mounts the REST API
parseGraphQLServer.applyGraphQL(app); // Mounts the GraphQL API
parseGraphQLServer.applyPlayground(app); // (Optional) Mounts the GraphQL Playground - do NOT use in Production

app.listen(1337, function() {
  console.log('REST API running on http://localhost:1337/parse');
  console.log('GraphQL API running on http://localhost:1337/graphql');
  console.log('GraphQL Playground running on http://localhost:1337/playground');
});
```

最后启动应用：

```bash
$ npx mongodb-runner start
$ node index.js
```

启动后，您可以访问http://localhost:1337/playground 在浏览器中调试GraphQL API。

***注意:*** ***不要*** 在生产环境中使用 --mountPlayground 选项 [Parse Dashboard](https://github.com/parse-community/parse-dashboard) 具有内置的GraphQL Playground，生产应用程序推荐使用它。

## 检查API运行状况

运行以下命令：

```graphql
query Health {
  health
}
```

您应该会收到以下回复：

```json
{
  "data": {
    "health": true
  }
}
```

## 创建您的第一个class

由于您的应用程序还没有任何schema，因此您可以使用`createClass` mutation来创建您的第一个class。运行以下命令：

```graphql
mutation CreateClass {
  createClass(
    name: "GameScore"
    schemaFields: {
      addStrings: [{ name: "playerName" }]
      addNumbers: [{ name: "score" }]
      addBooleans: [{ name: "cheatMode" }]
    }
  ) {
    name
    schemaFields {
      name
      __typename
    }
  }
}
```

您应该会收到以下回复：

```json
{
  "data": {
    "createClass": {
      "name": "GameScore",
      "schemaFields": [
        {
          "name": "objectId",
          "__typename": "SchemaStringField"
        },
        {
          "name": "updatedAt",
          "__typename": "SchemaDateField"
        },
        {
          "name": "createdAt",
          "__typename": "SchemaDateField"
        },
        {
          "name": "playerName",
          "__typename": "SchemaStringField"
        },
        {
          "name": "score",
          "__typename": "SchemaNumberField"
        },
        {
          "name": "cheatMode",
          "__typename": "SchemaBooleanField"
        },
        {
          "name": "ACL",
          "__typename": "SchemaACLField"
        }
      ]
    }
  }
}
```

## 使用自动生成的操作

Parse Server从您创建的第一个class中学到了，现在您的schema中有了`GameScore` class。您现在可以开始使用自动生成的操作！

运行以下命令以创建第一个对象：

```graphql
mutation CreateGameScore {
  createGameScore(
    fields: {
      playerName: "Sean Plott"
      score: 1337
      cheatMode: false
    }
  ) {
    id
    updatedAt
    createdAt
    playerName
    score
    cheatMode
    ACL
  }
}
```

You should receive a response similar to this:

```json
{
  "data": {
    "createGameScore": {
      "id": "XN75D94OBD",
      "updatedAt": "2019-09-17T06:50:26.357Z",
      "createdAt": "2019-09-17T06:50:26.357Z",
      "playerName": "Sean Plott",
      "score": 1337,
      "cheatMode": false,
      "ACL": null
    }
  }
}
```

您还可以对这个class进行查询：

```graphql
query GameScores {
  gameScores {
    results {
      id
      updatedAt
      createdAt
      playerName
      score
      cheatMode
      ACL
    }
  }
}
```

You should receive a response similar to this:

```json
{
  "data": {
    "gameScores": {
      "results": [
        {
          "id": "XN75D94OBD",
          "updatedAt": "2019-09-17T06:50:26.357Z",
          "createdAt": "2019-09-17T06:50:26.357Z",
          "playerName": "Sean Plott",
          "score": 1337,
          "cheatMode": false,
          "ACL": null
        }
      ]
    }
  }
}
```

## 自定义您的GraphQL Schema

Parse GraphQL Server允许您使用自己的查询和mutations来创建自定义GraphQL模式，并将其与自动生成的查询合并。您可以使用常规的云函数来解决这些操作。

要开始创建自定义schema，您需要编写`schema.graphql` 文件并使用 `--graphQLSchema`和`--cloud`选项初始化Parse Server ：

```bash
$ parse-server --appId APPLICATION_ID --masterKey MASTER_KEY --databaseURI mongodb://localhost/test --publicServerURL http://localhost:1337/parse --cloud ./cloud/main.js --graphQLSchema ./cloud/schema.graphql --mountGraphQL --mountPlayground
```

### 创建您的第一个自定义查询

在您的 `schema.graphql` 和 `main.js` 文件中添加如下代码，然后重启您的Parse Server.

```graphql
# schema.graphql
extend type Query {
  hello: String! @resolve
}
```

```js
// main.js
Parse.Cloud.define('hello', async () => {
  return 'Hello world!';
});
```

您现在可以通过GraphQL Playground来运行您的自定义查询：

```graphql
query {
  hello
}
```

You should receive the response below:

```json
{
  "data": {
    "hello": "Hello world!"
  }
}
```

## 了解更多

[Parse GraphQL 指南](http://docs.parseplatform.org/graphql/guide/) 是学习如何使用Parse GraphQL API的很好的资料。

您的GraphQL Playground内部也有一个非常强大的工具。请查看您的GraphQL Playground的右侧。您将看到DOCS和SCHEMA菜单。它们是通过分析您的应用程序架构自动生成的。请参考它们，并详细了解您可以使用Parse GraphQL API进行的所有操作。

另外，[GraphQL Learn 章节](https://graphql.org/learn/)也是学习GraphQL语言功能的很好的资源。

# 升级到 3.0.0

从3.0.0开始，parse-server使用JS SDK版本2.0。简而言之，parse SDK v2.0删除了backbone样式的回调以及Parse.Promise对象，支持使用原生promise。
所有的云函数接口也已经应用这些变动，所有的backbone样式的响应对象已经移除并由Promise代替。

我们已经编写了一份[迁移指南](3.0.0.md)，希望它可以帮助您过渡到下一个主要版本。

# 维护

请参阅[维护文档](https://github.com/parse-community/.github/blob/master/SUPPORT.md)。

如果您找到了Parse Server的bug，请确保您 [提交bug]前检查了所有下列提示(https://github.com/parse-community/parse-server/issues):

- [ ] 您已经阅读了 [先决条件]](http://docs.parseplatform.org/parse-server/guide/#prerequisites)。

- [ ] 您使用的是Parse Server [最新版本](https://github.com/parse-community/parse-server/releases)。

- [ ] 您已经搜索过所有[已知issues](https://github.com/parse-community/parse-server/issues?utf8=%E2%9C%93&q=)。您的问题很可能以前已经被上报或解决过。

# 想体验抢先版吗？

出于多种原因，我们建议您使用已发布到npm的生产版本，但是如果要使用parse-server尚未发布的最新版本，则可以直接依赖于此分支：

```
npm install parse-community/parse-server.git#master
```

## 试验中

您还可以使用自己的fork版本，并指定它们为当前进行中的分支：

```
npm install github:myUsername/parse-server#my-awesome-feature
```

并且不要忘记，如果您打算远程部署它，您应该在使用 `npm install`时带上 `--save`参数。

# 贡献

我们真心希望您可以加入Parse，见证它在开源社区中成长并蓬勃发展。请参阅[Contributing to Parse Server guide](CONTRIBUTING.md)。

# 贡献者

这个项目的存在要归功于所有的贡献者。

<a href="../../graphs/contributors"><img src="https://opencollective.com/parse-server/contributors.svg?width=890&button=false" /></a>

# Sponsors

Support this project by becoming a sponsor. Your logo will show up here with a link to your website. [Become a sponsor!](https://opencollective.com/parse-server#sponsor)

<a href="https://opencollective.com/parse-server/sponsor/0/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/0/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/1/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/1/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/2/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/2/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/3/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/3/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/4/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/4/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/5/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/5/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/6/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/6/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/7/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/7/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/8/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/8/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/9/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/9/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/10/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/10/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/11/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/11/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/12/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/12/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/13/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/13/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/14/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/14/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/15/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/15/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/16/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/16/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/17/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/17/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/18/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/18/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/19/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/19/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/20/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/20/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/21/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/21/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/22/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/22/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/23/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/23/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/24/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/24/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/25/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/25/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/26/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/26/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/27/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/27/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/28/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/28/avatar.svg"></a>
<a href="https://opencollective.com/parse-server/sponsor/29/website" target="_blank"><img src="https://opencollective.com/parse-server/sponsor/29/avatar.svg"></a>

# 赞助者

每月捐款支持我们，并帮助我们继续开展活动。[成为支持者！](https://opencollective.com/parse-server#backer)

<a href="https://opencollective.com/parse-server#backers" target="_blank"><img src="https://opencollective.com/parse-server/backers.svg?width=890" /></a>

-----

自2017年4月5日起，Parse, LLC已将此代码移交给parse-community组织，并且将不再贡献或分发此代码。
