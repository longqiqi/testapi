### 接口列表

  |**权限类型**|   **接口数据类型**   |**请求方法** |**类型**   |**描述**                     |**需要验签**        |                                                                                                                                            
  |----------- |------------------ |------------------------------------------------------------------------------------------------------------------------------------------------------------------- |---------- |---------------------------- |--------------|
  |读取   |市场行情接口 |         market.$symbol.kline.$period|                                                                                                            sub        |订阅 KLine 数据              |否|
  |读取   |市场行情接口|           market.$symbol.kline.$period|                                                                                                                    req        |请求 KLine 数据              |否|
  |读取   |市场行情接口           |market.$symbol.depth.$type |                                                                                                                     sub        |订阅 Market Depth 数据       |否|
 |读取   |市场行情接口           |market.$symbol.detail|                                                                                                                     sub        |订阅 Market detail 数据       |否|
  |读取   |市场行情接口           |market.$symbol.trade.detail |                                                                                                                     req        |请求 Trade detail 数据       |否|
  |读取   |市场行情接口           |market.$symbol.trade.detail|        sub |订阅 Trade Detail 数据  | 否  | 
 |交易   |交易接口           |orders.$symbol|        sub |订阅订单成交数据  | 是  | 
 |读取   |资产接口           |accounts.$symbol|        sub |订阅某个品种下的资产变动信息  | 是  | 
 |读取   |资产接口          |positions.$symbol|        sub |订阅某个品种下的持仓变动信息  | 是  | 
#### 合约订阅地址
合约站行情请求以及订阅地址为：wss://www.hbdm.com/ws

合约站订单推送订阅地址：wss://api.hbdm.com/notification

如果这个两个地址访问不了，可使用：
合约站行情请求以及订阅地址为：wss://www.btcgateway.pro/ws

合约站订单推送订阅地址：wss://api.btcgateway.pro/notification



如果对合约订单推送订阅有疑问，可以参考[Demo](https://github.com/huobiapi/Futures-Java-demo)
#### 访问次数限制

公开行情接口和用户私有接口都有访问次数限制
* 普通用户，需要密钥的私有接口，每个UID 3秒最多30次请求(该UID的所有币种和不同到期日的合约的所有私有接口共享3秒30次的额度)
* 其他非行情类的公开接口，比如获取指数信息，限价信息，交割结算、平台持仓信息等，所有用户都是每个IP3秒最多60次请求（所有该IP的非行情类的公开接口请求共享3秒60次的额度）
* 行情类的公开接口，比如：获取K线数据、获取聚合行情、市场行情、获取市场最近成交记录：

    （1） restful接口：同一个IP,  1秒最多200个请求 

    （2）  websocket：req请求，同一时刻最多请求50次；sub请求，无限制，服务器主动推送数据

* WebSocket私有订单成交推送接口(需要API KEY验签)

    一个UID最多同时建立10个私有订单成交推送WS链接。该用户在一个品种(包含该品种的所有周期的合约)上，仅需要维持一个订单推送WS链接即可。

    注意: 订单推送WS的限频，跟用户RESTFUL私有接口的限频是分开的，相互不影响。

api接口response中的header返回以下字段
* ratelimit-limit： 单轮请求数上限，单位：次数
* ratelimit-interval：请求数重置的时间间隔，单位：毫秒
* ratelimit-remaining：本轮剩余可用请求数，单位：次数
* ratelimit-reset：请求数上限重置时间，单位：毫秒

市场行情接口
--------

#### 订阅 KLine 数据

成功建立和 WebSocket API 的连接之后，向 Server发送如下格式的数据来订阅数据：

    {
    "sub": "market.$symbol.kline.$period",
    "id": "id generate by client"
    }

  |**参数名称**|   **是否必须**  | **类型**  | **描述** |  **默认值**  | **取值范围**
  |--------------| -----------------| ---------- |----------| ------------| --------------------------------------------------------------------------------|
  |symbol  |       true         |  string  |   交易对   |               |如"BTC\_CW"表示BTC当周合约，"BTC\_NW"表示BTC次周合约，"BTC\_CQ"表示BTC季度合约|
  |period    |     true          | string   |  K线周期     |            |1min, 5min, 15min, 30min, 1hour,4hour,1day, 1mon|

正确订阅请求参数的例子：

    {
    "sub": "market.BTC_CQ.kline.1min",
    "id": "id1"
    }

订阅成功返回数据的例子

    {
    "id": "id1",
    "status": "ok",
    "subbed": "market.BTC_CQ.kline.1min",
    "ts": 1489474081631
    }

之后每当 KLine 有更新时，client 会收到数据，例子

    {
     "ch": "market.BTC_CQ.kline.1min",
     "ts": 1489474082831,
     "tick": 
        {
         "id": 1489464480,
         "mrid": 268168237,
         "vol": 100,
         "count": 0,
         "open": 7962.62,
         "close": 7962.62,
         "low": 7962.62,
         "high": 7962.62,
         "amount": 0.3 //单位BTC 币种的数量
        }
    }

    tick 说明

    "tick": 
      {
       "id": K线id,
       "mrid": 268168237,
       "vol": 成交量张数,
       "count": 成交笔数,
       "open": 开盘价,
       "close": 收盘价,当K线为最晚的一根时，是最新成交价
       "low": 最低价,
       "high": 最高价,
       "amount": BTC, 即 sum(每一笔 该合约面值* 该笔的成交量/成交价)
    }

**错误订阅的列子**

错误订阅(错误的 symbol)

    {
     "sub": "market.invalidsymbol.kline.1min",
     "id": "id2"
    }

订阅失败返回数据的例子

    {
     "id": "id2",
     "status": "error",
     "err-code": "bad-request",
     "err-msg": "invalid topic market.invalidsymbol.kline.1min",
     "ts": 1494301904959
    }

错误订阅(错误的 topic)

    {
     "sub": "market.BTC_CQ.kline.3min",
     "id": "id3"
    }

订阅失败返回数据的例子

    {
     "id": "id3",
     "status": "error",
     "err-code": "bad-request",
     "err-msg": "invalid topic market.BTC_CQ.kline.3min",
     "ts": 1494310283622
    }

#### 请求 KLine 数据 </a>

成功建立和 WebSocket API 的连接之后，向Server发送如下格式的数据来请求数据：

    {
     "req": "market.$symbol.kline.$period",
     "id": "id generated by client",
     "from": " type: long, 2017-07-28T00:00:00+08:00 至2050-01-01T00:00:00+08:00 之间的时间点，单位：秒",
      "to": "type: long, 2017-07-28T00:00:00+08:00 至2050-01-01T00:00:00+08:00 之间的时间点，单位：秒，必须比 from 大"}

| **参数名称** | **是否必须** | **类型** | **描述** | **默认值** |**取值范围** | 
| -------- | -------- | ------ | ------ | ------- |---------------------------------------- | 
| symbol | true | string |交易对 | |如"BTC\_CW"表示BTC当周合约，"BTC\_NW"表示BTC次周合约，"BTC\_CQ"表示BTC季度合约|
 | period | true | string | K线周期 | | 1min, 5min, 15min, 30min, 1hour,4hour,1day, 1mon|
 | from| true | long| 开始时间 | | |
 | to| true | long| 结束时间 | | |
    [t1, t5] 假设有 t1  ~ t5 的K线：

    from: t1, to: t5, return [t1, t5].
    from: t5, to: t1, which t5  > t1, return [].
    from: t5, return [t5].
    from: t3, return [t3, t5].
    to: t5, return [t1, t5].
    from: t which t3  < t  <t4, return [t4, t5].
    to: t which t3  < t  <t4, return [t1, t3].
    from: t1 and to: t2, should satisfy 1325347200  < t1  < t2  < 2524579200.

**备注**：一次最多2000条

请求 KLine 数据请求参数的例子：

    {
    "req": "market.BTC_CQ.kline.1min",
    "id": "id4"
    }

请求成功返回数据的例子：

    {
     "rep": "market.BTC_CQ.kline.1min",
     "status": "ok",
     "id": "id4",
     "tick": [
       {
        "vol": 100,
        "count": 27,
        "id": 1494478080,
        "open": 10050.00,
        "close": 10058.00,
        "low": 10050.00,
        "high": 10058.00,
        "amount": 175798.757708
       },
       {
        "vol": 300,
        "count": 28,
        "id": 1494478140,
        "open": 10058.00,
        "close": 10060.00,
        "low": 10056.00,
        "high": 10065.00,
        "amount": 158331.348600
       }
     ]
    }

#### 订阅 Market Depth 数据 </a>

成功建立和 WebSocket API 的连接之后，向 Server发送如下格式的数据来订阅数据：

    {
    "sub": "market.$symbol.depth.$type",
    "id": "id generated by client"
    }

  |**参数名称**   |**是否必须**   |**类型**   |**描述**     |**默认值**   |**取值范围**|
  |-------------- |-------------- |---------- |------------ |------------ |---------------------------------------------------------------------------------|
  |symbol         |true           |string     |交易对            |        |如"BTC\_CW"表示BTC当周合约，"BTC\_NW"表示BTC次周合约，"BTC\_CQ"表示BTC季度合约.|
  |type           |true           |string     |Depth 类型        |        |(150档数据)  step0, step1, step2, step3, step4, step5（合并深度1-5）,step0时，不合并深度;(20档数据)  step6, step7, step8, step9, step10, step11（合并深度7-11）；step6时，不合并深度|

**备注**：用户选择“合并深度”时，一定报价精度内的市场挂单将予以合并显示。合并深度仅改变显示方式，不改变实际成交价格。

正确订阅请求参数的例子：

    {
    "sub": "market.BTC_CQ.depth.step0",
    "id": "id5"
    }

订阅成功返回数据的例子：

    {
      "id": "id5",
      "status": "ok",
      "subbed": "market.BTC_CQ.depth.step0",
      "ts": 1489474081631
    }

之后每当 depth 有更新时，client 会收到数据，例子：

    {
     "ch": "market.BTC_CQ.depth.step0",
     "ts": 1489474082831,
     "tick":
       { 
        "mrid": 269073229,
         "id": 1539843937,
            "bids": [
             [9999.9101,1], 
             [9992.3089,2]
                    ],
             "asks": [
              [10010.9800,10],
              [10011.3900,15]
                     ],
	      "ts": 1539843937417,
	       "version": 1539843937,
	       "ch": "market.BTC_CQ.depth.step0"
        }
    }

    tick 说明：
    "tick": {
      "bids": [
        [买1价,买1量]
        [买2价,买2量]
         //more data here
       ],
       "asks": [
        [卖1价,卖1量]
        [卖2价,卖2量]
        //more data here
       ]
    }

#### 订阅 Market Detail 数据 </a>

成功建立和 WebSocket API 的连接之后，向 Server发送如下格式的数据来请求数据:

    {
     "sub": "market.$symbol.detail",
     "id": "id generated by client"
    }

  |**参数名称**   |**是否必须**   |**类型**   |**描述**     |**默认值**   |**取值范围**|
  |-------------- |-------------- |---------- |------------ |------------ |--------------------------------------------------------------------------------|
  |symbol         |true           |string     |交易对      |              |如"BTC\_CW"表示BTC当周合约，"BTC\_NW"表示BTC次周合约，"BTC\_CQ"表示BTC季度合约|

订阅 Market Detail 数据请求参数的例子：

    {
     "sub": "market.BTC_CQ.detail",
     "id": "id6"
    }

请求成功返回数据的例子：

    {
	"ch": "market.BTC_CW.detail",
	"ts": 1539842340724,
	"tick": {
		"id": 1539842340,
		"mrid": 268041138,
		"open": 6740.47,
		"close": 7800,
		"high": 7800,
		"low": 6726.13,
		"amount": 477.1200312075244664773339914558562673572,
		"vol": 32414,
		"count": 1716
	        }
       }

tick数据说明：

    "tick": 
         { 
           "id": id, 
           "mrid": 1494496390000, 
           "vol": 成交量(张), 
           "count": 成交笔数, 
           "open": 开盘价, 
           "close": 收盘价,当K线为最晚的一根时，是最新成交价 
           "low": 最低价, 
           "high": 最高价, 
           "amount": 成交量(币), 即 sum(每一笔成交量(张)*单张合约面值/该笔成交价)
         }

#### 请求 Trade Detail 数据

成功建立和 WebSocket API 的连接之后，向 Server 发送如下格式的数据来请求数据：

    {
     "req": "market.$symbol.trade.detail",
     "id": "id generated by client"
    }

仅返回当前 Trade Detail

请求 Market Detail 数据请求参数的例子：

    {
     "req": "market.BTC_CQ.trade.detail",
     "id": "id8"
    }

请求成功返回数据的例子：



    {
     "ch": "market.BTC_CQ.trade.detail",
     "ts": 1489474082831,
     "data": [
              {
               "id":601595424,
               "price":10195.64,
               "amount":100,
               "direction":"buy",
               "ts":1494495766000
               },
              {
              "id":601595423,
              "price":10195.64,
              "amount":200,
              "direction":"buy",
              "ts":1494495711000
              }
            ]
     }
    
tick数据说明：

    "data": [
      {
       "id": 消息ID,
       "price": 成交价,
       "amount": 成交量（张）,
       "direction": 成交方向,
       "ts": 时间戳
      }
     ]

#### 订阅 Trade Detail 数据 </a>

成功建立和 WebSocket API 的连接之后，向 Server发送如下格式的数据来订阅数据：

    {
    "sub": "market.$symbol.trade.detail",
    "id": "id generated by client"
    }

**备注**：仅能获取最近 300 个 Trade Detail 数据。

  |**参数名称**   |**是否必须**   |**类型**   |**描述**   |**默认值**   |**取值范围**|
  |-------------- |-------------- |---------- |---------- |------------ |--------------------------------------------------------------------------------|
  |symbol         |true           |string     |合约名称    |            |如"BTC\_CW"表示BTC当周合约，"BTC\_NW"表示BTC次周合约，"BTC\_CQ"表示BTC季度合约|

正确订阅请求参数的例子：

    {
     "sub": "market.BTC_CQ.trade.detail",
     "id": "id7"
    }

订阅成功返回数据的例子：

    {
     "id": "id7",
     "status": "ok",
     "subbed": "market.BTC_CQ.trade.detail",
     "ts": 1489474081631
    }

之后每当 Trade Detail 有更新时，client 会收到数据，例子：

    {
	"ch": "market.BTC_NW.trade.detail",
	"ts": 1539831709042,
	"tick": {
		"id": 265842227,
		"ts": 1539831709001,
		"data": [{
			"amount": 20,
			"ts": 1539831709001,
			"id": 265842227259096443,
			"price": 6742.25,
			"direction": "buy"
		        }]
	          }
     }

data 说明：

    "data": [
      {
       "id": 消息ID,
       "price": 成交价,
       "amount": 成交量（张）,
       "direction": 成交方向,
       "ts": 时间戳
      }
     ]


### 订单推送访问地址

* 统一服务地址

  ```
  wss://api.hbdm.com/notification
  ```
正常ws请求连接不能同时超过10个
### 数据压缩

WebSocket API 返回的所有数据都进⾏了 GZIP 压缩，需要 client 在收到数据之后解压

### 请求与响应数据说明

* 字符编码：UTF-8

* 大小写敏感，包含所有参数名和返回值

* 数据类型：使用JSON传输数据

* 所有请求数据都有固定格式，具体接口说明文档中只会重点介绍非通用部分，

  请求数据结构如下，

  ```
   {
  "op": string, // 必填;Client 请求的操作类型(Server 会原样返回)，详细操作
  类型列列表请参考附录
  "cid": string, // 选填;当前请求唯一 ID(Client 自⽣成并保证本地唯一性，
  Server 会原样返回) 
  // 其余必填/可选字段
  }
  ```

* 所有响应/推送数据都会以固定的结构返回，具体接口说明文档中只会重点介绍data部分，请求响应数据结构如下，

  ```
   
  {
  "op": string, // 必填;本次响应 Client 请求的操作类型
  "cid": string, // 选填;Client 请求唯一 ID
  "ts": long, // 必填;Server 应答时本地时间戳
  "err-code": integer, // 必填;响应码，0 代表成功;非0 代表出错，详细响应码列表请参考错误码表。
  "err-msg": string, 只在出错情况下有此信息，表明详细的出错信息 
  "data": object // 选填;返回数据对象，请求处理成功时有效
  }
  ```

  推送数据结构如下，

  ```
  {
  "op": "string", // 必填;Server 推送的操作类型，详细操作类型列表请参考附录
  "ts": long, // 必填;Server 推送时本地时间戳
  "data": object // 必填;返回数据对象
  }
  ```

### 市场行情心跳

WebSocket Server 发送心跳：
```
{"ping": 18212558000}
```
 WebSocket Client 应该返回：
```
 {"pong": 18212558000}
```
注：返回的数据里面的 `"pong"` 的值为收到的 `"ping"` 的值
注：WebSocket Client 和 WebSocket Server 建立连接之后，WebSocket Server 每隔 `5s`（这个频率可能会变化） 会向 WebSocket Client 发起一次心跳，WebSocket Client 忽略心跳2次后，WebSocket Server 将会主动断开连接；WebSocket Client发送最近2次心跳message中的其中一个`ping`的值，WebSocket Server都会保持WebSocket连接。

### 订单推送心跳

WebSocket API 支持单向心跳，Server 发起 ping message，Client 返回 pong message。
WebSocket Server 发送⼼心跳:

```
{
    "op": "ping",
    "ts": 1492420473058
}
```

WebSocket Client 应该返回:

```
{
    "op": "pong"
    "ts": 1492420473058
}
```

注：

* `"pong"`操作返回数据里面的`"ts"`的值为`"ping"`推送收到的`"ts"`值
* WebSocket Client 和 WebSocket Server 建⽴立连接之后，WebSocket Server 每隔 5s(这个频率可能会变化) 会向 WebSocket Client 发起⼀一次⼼心跳，WebSocket Client 忽略心跳 3 次后，WebSocket Server 将会主动断开连接。

异常情况WebSocket Server 会返回错误信息，比如

```
{
    "op": "pong"
    "ts": 1492420473027,
    "err-code": 2011，
    "err-msg": “详细出错信息”
}
```

### 服务方主动断开连接

在建连和鉴权期间，如果出错，服务方会主动断开连接，在关闭之前推送数据结构如下,

```
{
    "op": "close", // 表明是服务⽅方主动断开连接 
    "ts": long   // Server 推送时本地时间戳
}
```

### 服务方返回错误，但不断开连接

鉴权成功后，在客户方提供非法Op或者某些内部错误的情况下，服务方会返回错误，但并不断开连接

```
{
    "op": "error", // 表明是收到非法op或者内部错误 
    "ts": long// Server 推送时本地时间戳
}
```
#### 鉴权-Authentication

用户自⼰在火币网⽣成Access Key和Secret Key，Secret Key由用户自⼰保存，⽤户需提供Access Key。目前关于 apikey 申请和修改，请在“账户 - API 管理 ” 创建新API Key 填写备注(可选择绑定 ip)点击创建。其中 Access Key 为 API 访问密钥，Secret Key 为用户对请求进⾏签名的密钥(仅申请时可见)。用户按规则生成签名(Signature)。 

交易功能 websocket 版本接⼝建立连接时首先要做鉴权操作，具体格式如下，

重要提示：这两个密钥与账号安全紧密相关，无论何时都请勿向其它人透露。 

#### 鉴权请求数据格式

```
{
  "op": "auth",
  "type": "api",
  "AccessKeyId": "e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx",
  "SignatureMethod": "HmacSHA256",
  "SignatureVersion": "2",
  "Timestamp": "2017-05-11T15:19:30",
  "Signature": "4F65x5A2bLyMWVQj3Aqp+B4w+ivaA7n5Oi2SuYtCJ9o=",
}
```
#### 鉴权请求数据格式说明

| 字段名称         | 类型   | 说明                                                         |
| :--------------- | :----- | :----------------------------------------------------------- |
| op               | string | 必填；操作名称，鉴权固定值为auth                             |
| type             | string | 必填；认证方式 api表示接口认证，ticket 表示终端认证          |
| cid              | string | 选填；Client请求唯一ID                                       |
| AccessKeyId      | string | type的值为api时必填；API 访问密钥, 您申请的 APIKEY 中的 AccessKey |
| SignatureMethod  | string | type的值为api时必填；签名方法, 用户计算签名的基于哈希的协议，此处使用 HmacSHA256 |
| SignatureVersion | string | type的值为api时必填；签名协议的版本，此处使用 2              |
| Timestamp        | string | type的值为api时必填；时间戳, 您发出请求的时间 (UTC 时区) 。在查询请求中包含此值有助于防止第三方截取您的请求。如:2017-05-11T16:22:06。再次强调是 (UTC 时区) |
| Signature        | string | type的值为api时必填；签名, 计算得出的值，用于确保签名有效和未被篡改 |
| ticket           | string | type的值为ticket时必填；登陆时返回                           |

注意：

* 为了减少已有用户的接入工作量，此处使用了与REST接口同样的签名算法进行鉴权。
* 请注意大小写
* 当type为api时，参数op，type，cid，Signature不参加签名计算
* 此处签名计算中请求方法固定值为`GET`,其余值请参考REST接口签名算法文档

步骤：

 示例例参数签名(Signature)计算过程如下，

* 规范要计算签名的请求 因为使用 HMAC 进⾏签名计算时，使⽤不同内容计算得到的结果会完全
  不同。所以在进⾏签名计算前，请先对请求进⾏规范化处理。

* 请求方法(GET 或 POST)，后面添加换行符 `\n` 。

  ```
  GET\n
  ```

* 添加小写的访问地址，后面添加换行符`\n`。

  ```
  api.hbdm.com\n
  ```

* 访问方法的路径，后面添加换行符`\n`。

  ```
  /notification\n
  ```

* 按照ASCII码的顺序对参数名进行排序(使⽤用 UTF-8 编码，且进⾏了 URI 编码，十六进制字符必须
  大写，如‘:’会被编码为'%3A'，空格被编码为'%20')。例如，下面是请求参数的原始顺序，进⾏过
  编码后。

  ```
  AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-
  7xxxx&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-
  11T15%3A19%3A30
  ```

* 按照以上顺序，将各参数使用字符’&’连接。 
  * 组成最终的要进行签名计算的字符串如下:
    * 计算签名，将以下两个参数传入加密哈希函数: 要进行签名计算的字符串，进行签名
      的密钥(SecretKey) 
    * 得到签名计算结果并进行 Base64编码
  * 将上述值作为参数Signature的取值添加到 API 请求中。 将此参数添加到请求时，必须将该值进
    ⾏ URI 编码。

#### 鉴权应答数据格式说明

| 名称     | 类型    | 说明                                                 |
| :------- | :------ | :--------------------------------------------------- |
| op       | string  | 必填；操作名称，鉴权固定值为 auth                    |
| type     | string  | 必填；根据请求的参数进行返回。                       |
| cid      | string  | 选填；请求时携带则会返回。                           |
| err-code | integer | 成功返回 0, 失败为其他值，详细响应码列列表请参考附录 |
| err-msg  | string  | 可选，若出错表示详细错误信息                         |
| ts       | long    | 服务端应答时间戳                                     |
| user-id  | long    | ⽤户 id                                              |

鉴权成功应答数据示例

```
 
{
  "op": "auth",
  "type":"api",
  "ts": 1489474081631,
  "err-code": 0,
  "data": {
    "user-id": 12345678
  }
}
```

鉴权失败应答返回数据

```
{
"op": "auth",
"type":"api",
"ts": 1489474081631, 
"err-code": xxxx， 
"err-msg": ”详细的错误信息“
}
```

#### 订阅订单成交数据（sub）

成功建立和 WebSocket API 的连接之后，向 Server 发送如下格式的数据来订阅数据:

#### 订阅请求数据格式

```
{
  “op”: “sub”,
  "cid": "id generated by client”,
  “topic": "topic to sub”
}
```

#### 订阅请求数据格式说明

| 字段名称 | 类型   | 说明                                        |
| :------- | :----- | :------------------------------------------ |
| op       | string | 必填；操作名称，订阅固定值为sub             |
| cid      | string | 选填;Client 请求唯一 ID                     |
| topic    | string | 必填；订阅主题名称，详细主题列表请参考附录; |

#### 订阅请求示例

正确的订阅请求

```
{
  "op": "sub",
  "cid": "40sG903yz80oDFWr",
  "topic": "orders.btc"
}
```

订阅成功返回数据示例

```
{
  "op": "sub",
  "cid": "40sG903yz80oDFWr",
  "err-code": 0,
  "ts": 1489474081631,
  "topic": "orders.btc"
}
```

错误订阅请求示例（错误的topic）

```
{
  "op": "sub",
  "topic": "orders.bar",
  "cid": "40sG903yz80oDFWr"
}
```

订阅失败会返回下面示例数据

```
{
  "op": "sub",
  "topic": "orders.bar",
  "cid": "40sG903yz80oDFWr",
  "err-code": xxxx, //具体编码请参考响应码表 
  "err-msg": “详细的错误信息”，
  "ts": 1489474081631
}
```

#### 成交详情通知数据格式说明

```
{
"op": "notify", // 操作名称
"topic": "orders.btc", // 主题
"ts": 1489474082831, // 时间戳 
"symbol": "BTC", //品种
"contract_type": "this_week", //合约类型
"contract_code": "BTC180914", //合约代码
"volume": 111, //委托数量
"price": 1111, //委托价格 
"order_price_type": "limit", //订单报价类型 "limit":限价 "opponent":对手价 "post_only":只做maker单,post only下单只受用户持仓数量限制
"direction": "buy", //"buy":买 "sell":卖
"offset": "open", //"open":开 "close":平
"status": 6 //订单状态( 3已提交 4部分成交 5部分成交已撤单 6全部成交 7已撤单)
"lever_rate": 10, //杠杆倍数
"order_id": 106837, //订单ID
"client_order_id": 10683, //客户订单ID
"order_source": "web", //订单来源   （system:系统 web:用户网页 api:用户API m:用户M站 risk:风控系统）
"order_type": 1, //订单类型  (1:报单 、2:撤单 、3:强平、4:交割)
"created_at": 1408076414000, //订单创建时间
"trade_volume": 1, //累计成交数量
"trade_turnover": 1200, //累计成交总金额
"fee": 0, //手续费
"trade_avg_price": 10, //成交均价
"margin_frozen": 10, //冻结保证金
"profit": 2, //收益
"trade":[{
    "trade_id":112, //撮合结果id
    "trade_volume":1, //成交量
    "trade_price":123.4555, //撮合价格
    "trade_fee":0.234, //成交手续费
    "trade_turnover":34.123, //成交金额 
    "created_at": 1490759594752, //成交创建时间
    "role": "maker" //taker或maker
  }]
}
```

#### 成交推送请求数据格式说明

| 字段名称                | 类型    | 说明                                                         |
| ----------------------- | ------- | ------------------------------------------------------------ |
| op                      | string  | 必填;操作名称，推送固定值为 notify;                          |
| topic                   | string  | 必填;推送的主题                                              |
| ts                      | long    | 服务端应答时间戳                                             |
| symbol                  | string  | 品种ID                                                       |
| contract_type           | string  | 合约类型                                                     |
| contract_code           | string  | 合约代码                                                     |
| volume                  | decimal | 委托数量                                                     |
| price                   | decimal | 委托价格                                                     |
| order_price_type        | string  | 订单报价类型 "limit":限价 "opponent":对手价 "post_only":只做maker单,post only下单只受用户持仓数量限制                  |
| direction               | string  | "buy":买 "sell":卖                                           |
| offset                  | string  | "open":开 "close":平                                         |
| status                  | int     | 订单状态(1准备提交 2准备提交 3已提交 4部分成交 5部分成交已撤单 6全部成交 7已撤单 11撤单中) |
| lever_rate              | int     | 杠杆倍数                                                     |
| order_id                | long    | 订单ID                                                       |
| client_order_id         | long    | 客户订单ID                                                   |
| order_source            | int     | 订单来源（system:系统 web:用户网页 api:用户API m:用户M站 risk:风控系统） |
| order_type              | int     | 订单类型  1:报单 、 2:撤单 、 3:强平、4:交割                 |
| created_at              | long    | 订单创建时间                                                 |
| trade_volume            | decimal | 成交数量                                                     |
| trade_turnover          | decimal | 成交总金额                                                   |
| fee                     | decimal | 手续费                                                       |
| trade_avg_price         | decimal | 成交均价                                                     |
| margin_frozen           | decimal | 冻结保证金                                                   |
| profit                  | decimal | 收益                                                         |
| <list>(属性名称: trade) |         |                                                              |
| trade_id                | long    | 撮合结果id 非唯一，可重复，注意：一个撮合结果代表一个taker单和N个maker单的成交记录的集合，如果一个taker单吃了N个maker单，那这N笔trade都是一样的撮合结果id                                                  |
| trade_volume            | decimal | 成交量                                                       |
| trade_price             | decimal | 撮合价格                                                     |
| trade_fee               | decimal | 成交手续费                                                   |
| trade_turnover          | decimal | 成交金额                                                     |
| created_at              | long    | 成交创建时间                                                 |
| role             | string  | taker或maker                                                 |
| </list>                  |         |                                                             |


### 取消订阅数据（ubsub）

成功建⽴和 WebSocket API 的连接之后，向 Server 发送如下格式的数据来取消订阅数据:

#### 取消订阅请求数据格式

```
{
  “op”: “unsub”,
  “topic": "topic to unsub”,
  "cid": "id generated by client”,
}
```

#### 取消订阅请求数据格式说明

| 字段名称 | 类型   | 说明                                               |
| :------- | :----- | :------------------------------------------------- |
| op       | string | 必填;操作名称，订阅固定值为 unsub;                 |
| cid      | string | 选填;Client 请求唯一 ID                            |
| topic    | string | 必填;待取消订阅主题名称，详细主题列列表请参考附录; |

### 取消订阅请求示例

正确的取消订阅请求

```
{
  "op": "unsub",
  "topic": "orders.btc",
  "cid": "40sG903yz80oDFWr"
}
```

取消订阅成功返回数据示例

```
{
  "op": "unsub",
  "topic": "orders.btc",
  "cid": "id generated by client",
  "err-code": 0,
  "ts": 1489474081631
}
```

#### 订阅与取消订阅规则说明

| 订阅(sub)      | 取消订阅(ubsub) | 规则   |
| -------------- | --------------- | ------ |
| orders.*       | orders.*        | 允许   |
| orders.symbol1 | orders.*        | 允许   |
| orders.symbol1 | orders.symbol1  | 允许   |
| orders.symbol1 | orders.symbol2  | 不允许 |
| orders.*       | orders.symbol1  | 不允许 |


### 资产变动数据（sub）

成功建立和 WebSocket API 的连接之后，向 Server 发送如下格式的数据来订阅数据:

#### 订阅请求数据格式

```
{
  "op": "sub",
  "cid": "id generated by client”,
  “topic": "topic to sub”
}
```

#### 订阅请求数据格式说明

| 字段名称 | 类型   | 说明                                        |
| :------- | :----- | :------------------------------------------ |
| op       | string | 必填；操作名称，订阅固定值为sub             |
| cid      | string | 选填;Client 请求唯一 ID                     |
| topic    | string | 必填；订阅主题名称，必填 (accounts.$symbol)  订阅、取消订阅某个品种下的资产变更信息，当 $symbol值为 * 时代表订阅所有品种; |

#### 订阅请求示例

正确的订阅请求

```
{
  "op": "sub",
  "cid": "40sG903yz80oDFWr",
  "topic": "accounts.btc"
}
```

订阅成功返回数据示例

```
{
  "op": "sub",
  "cid": "40sG903yz80oDFWr",
  "err-code": 0,
  "ts": 1489474081631,
  "topic": "accounts.btc"
}
```

错误订阅请求示例（错误的topic）

```
{
  "op": "sub",
  "topic": "accounts.bar",
  "cid": "40sG903yz80oDFWr"
}
```

订阅失败会返回下面示例数据

```
{
  "op": "sub",
  "topic": "accounts.bar",
  "cid": "40sG903yz80oDFWr",
  "err-code": xxxx, //具体编码请参考响应码表 
  "err-msg": “详细的错误信息”，
  "ts": 1489474081631
}
```

#### 当资产有更新时，返回的参数示例如下

```
{
    "op": “notify”,             // 操作名称
    "topic": “accounts”,     // 主题
    "ts": 1489474082831,    
    "event": "order.match",
    "data":[
       {
    "symbol": "BTC",
    "margin_balance": 1,
    "margin_static": 1,
    "margin_position": 0,
    "margin_frozen": 3.33,
    "margin_available": 0.34,
    "profit_real": 3.45,
    "profit_unreal": 7.45,
    "withdraw_available":4.0989898,
    "risk_rate": 100,
    "liquidation_price": 100,
    "lever_rate": 10,
    "adjust_factor": 0.1
     }
   ]
}
```

#### 字段说明
| 字段名称                | 类型    | 说明                                                         |
| ----------------------- | ------- | ------------------------------------------------------------ |
| ts                        | long  | 响应生成时间点，单位：毫秒                           |
| event                     | string  | 资产变化通知相关事件说明，比如订单创建开仓(order.open) 、订单成交(order.match)（除开强平和结算交割）、结算交割(settlement)、订单强平成交(order.liquidation)（对钆和接管仓位）、订单撤销(order.cancel) 、合约账户划转（contract.transfer)（包括外部划转）、系统（contract.system)、其他资产变化(other)   、初始资金（init）                                              |
| symbol                    | string    | 品种代码 ,"BTC","ETH"...，当 $symbol值为 * 时代表订阅所有品种                                             |
| margin_balance            | decimal  | 账户权益                                                       |
| margin_static             | decimal  | 静态权益                                                     |
| margin_position           | decimal  | 持仓保证金（当前持有仓位所占用的保证金）                                                     |
| margin_frozen             | decimal | 冻结保证金                                                     |
| margin_available          | decimal | 可用保证金                                                     |
| profit_real               | decimal  | 已实现盈亏                |
| profit_unreal             | decimal  | 未实现盈亏                                          |
| risk_rate                 | decimal  |保证金率                                        |
| liquidation_price         | decimal     | 预估爆仓价 |
| withdraw_available        | decimal     | 可划转数量                                                     |
| lever_rate                | decimal    | 杠杆倍数                                                       |
| adjust_factor                | decimal    | 调整系数                                                       |


### 取消订阅数据（ubsub）

成功建⽴和 WebSocket API 的连接之后，向 Server 发送如下格式的数据来取消订阅数据:

#### 取消订阅请求数据格式

```
{
  “op”: “unsub”,
  “topic": "topic to unsub”,
  "cid": "id generated by client”,
}
```

#### 取消订阅请求数据格式说明

| 字段名称 | 类型   | 说明                                               |
| :------- | :----- | :------------------------------------------------- |
| op       | string | 必填;操作名称，订阅固定值为 unsub;                 |
| cid      | string | 选填;Client 请求唯一 ID                            |
| topic    | string | 必填;必填；必填；订阅主题名称，必填 (accounts.$symbol)  订阅、取消订阅某个品种下的资产变更信息，当 $symbol值为 * 时代表订阅所有品种; |

### 取消订阅请求示例

正确的取消订阅请求

```
{
  "op": "unsub",
  "topic": "accounts.btc",
  "cid": "40sG903yz80oDFWr"
}
```

取消订阅成功返回数据示例

```
{
  "op": "unsub",
  "topic": "accounts.btc",
  "cid": "id generated by client",
  "err-code": 0,
  "ts": 1489474081631
}
```

#### 订阅与取消订阅规则说明

| 订阅(sub)      | 取消订阅(ubsub) | 规则   |
| -------------- | --------------- | ------ |
| accounts.*       | accounts.*        | 允许   |
| accounts.symbol1 | accounts.*        | 允许   |
| accounts.symbol1 | accounts.symbol1  | 允许   |
| accounts.symbol1 | accounts.symbol2  | 不允许 |
| accounts.*       | accounts.symbol1  | 不允许 |


### 持仓变动更新数据（sub）

成功建立和 WebSocket API 的连接之后，向 Server 发送如下格式的数据来订阅数据:

#### 订阅请求数据格式

```
{
  “op”: “sub”,
  "cid": "id generated by client”,
  “topic": "topic to sub”
}
```

#### 订阅请求数据格式说明

| 字段名称 | 类型   | 说明                                        |
| :------- | :----- | :------------------------------------------ |
| op       | string | 必填；操作名称，订阅固定值为sub             |
| cid      | string | 选填;Client 请求唯一 ID                     |
| topic    | string | 必填；订阅主题名称，必填 (positions.$symbol)  订阅、取消订阅某个品种下的持仓变更信息，当 $symbol值为 * 时代表订阅所有品种 |

#### 订阅请求示例

正确的订阅请求

```
{
  "op": "sub",
  "cid": "40sG903yz80oDFWr",
  "topic": "positions.btc"
}
```

订阅成功返回数据示例

```
{
  "op": "sub",
  "cid": "40sG903yz80oDFWr",
  "err-code": 0,
  "ts": 1489474081631,
  "topic": "positions.btc"
}
```

错误订阅请求示例（错误的topic）

```
{
  "op": "sub",
  "topic": "positions.bar",
  "cid": "40sG903yz80oDFWr"
}
```

订阅失败会返回下面示例数据

```
{
  "op": "sub",
  "topic": "positions.bar",
  "cid": "40sG903yz80oDFWr",
  "err-code": xxxx, //具体编码请参考响应码表 
  "err-msg": “详细的错误信息”，
  "ts": 1489474081631
}
```

#### 当持仓有更新时，返回的参数示例如下

```
{ 

      “op”: “notify”,             // 操作名称
      “topic”: “positions”,     // 主题
      "ts": 1489474082831,   
     "event": "order.match"
     "data":[
                   {
      "symbol": "BTC",
      "contract_code": "BTC180914",
      "contract_type": "this_week",
      "volume": 1,
      "available": 0,
      "frozen": 1,
      "cost_open": 422.78,
      "cost_hold": 422.78,
      "profit_unreal": 0.00007096,
      "profit_rate": 0.07,
      "profit": 0.97,
      "position_margin": 3.4,
      "lever_rate": 10,
      "direction":"buy"
                 }
             ]
}
```

#### 字段说明

**返回参数**

| 字段名称                | 类型    | 说明                                                         |
| ----------------------- | ------- | ------------------------------------------------------------ |
| ts                     | long  | 响应生成时间点，单位：毫秒                           |
| event                  | string  | 持仓变化通知相关事件说明，比如订单创建平仓(order.close) 、订单成交(order.match)（除开强平和结算交割）、结算交割(settlement)、订单强平成交(order.liquidation)（对钆和接管仓位）、订单撤销(order.cancel)  、初始持仓（init）                                             |
| symbol                 | string    | 品种代码 ,"BTC","ETH"...，当 $symbol值为 * 时代表订阅所有品种                                             |
| contract_code          | decimal  | 合约代码                                                       |
| contract_type          | decimal  | 合约类型,当周:"this_week", 次周:"next_week", 季度:"quarter"，已下市：“delivered”                                                    |
| volume                 | decimal  | 持仓量                                                     |
| available              | decimal | 可平仓数量                                                     |
| frozen                 | decimal | 冻结数量                                                      |
| cost_open              | decimal  | 开仓均价                |
| cost_hold              | decimal  | 持仓均价                                          |
| profit_unreal          | decimal  |未实现盈亏                                        |
| profit_rate            | decimal     | 收益率 |
| profit                 | decimal     | 收益                                                     |
| position_margin        | decimal    | 持仓保证金                                                       |
| lever_rate             | decimal     | 杠杆倍数                                                      |
| direction              | decimal    | 仓位方向   "buy":买 "sell":卖                                                     |


### 取消订阅数据（ubsub）

成功建⽴和 WebSocket API 的连接之后，向 Server 发送如下格式的数据来取消订阅数据:

#### 取消订阅请求数据格式

```
{
  “op”: “unsub”,
  “topic": "topic to unsub”,
  "cid": "id generated by client”,
}
```

#### 取消订阅请求数据格式说明

| 字段名称 | 类型   | 说明                                               |
| :------- | :----- | :------------------------------------------------- |
| op       | string | 必填;操作名称，订阅固定值为 unsub;                 |
| cid      | string | 选填;Client 请求唯一 ID                            |
| topic    | string | 必填;必填；必填；订阅主题名称，必填 (positions.$symbol)  订阅、取消订阅某个品种下的资产变更信息，当 $symbol值为 * 时代表订阅所有品种; |

### 取消订阅请求示例

正确的取消订阅请求

```
{
  "op": "unsub",
  "topic": "positions.btc",
  "cid": "40sG903yz80oDFWr"
}
```

取消订阅成功返回数据示例

```
{
  "op": "unsub",
  "topic": "positions.btc",
  "cid": "id generated by client",
  "err-code": 0,
  "ts": 1489474081631
}
```

#### 订阅与取消订阅规则说明

| 订阅(sub)      | 取消订阅(ubsub) | 规则   |
| -------------- | --------------- | ------ |
| positions.*       | positions.*        | 允许   |
| positions.symbol1 | positions.*        | 允许   |
| positions.symbol1 | positions.symbol1  | 允许   |
| positions.symbol1 | positions.symbol2  | 不允许 |
| positions.*       | positions.symbol1  | 不允许 |

### 附录

### 操作类型（OP）说明

| 类型   | 描述                 |
| ------ | -------------------- |
| ping   | ⼼跳发起(server)     |
| pong   | 心跳应答             |
| auth   | 鉴权                 |
| sub    | 订阅消息             |
| unsub  | 取消订阅消息         |
| notify | 推送订阅消息(server) |

#### 主题（topic）类型说明

| 类型           | 使用操作类型 | 描述                                                         |
| -------------- | ------------ | ------------------------------------------------------------ |
| orders.$symbol | sub,ubsub    | 订阅、取消订阅指定交易易对的订单变更更消息，当 $symbol 值为 * 时代表订阅所有交易易对 |

#### 响应码（Err-Code）说明

| 返回码 | 返回描述                                 |
| ------ | ---------------------------------------- |
| 0      | Request successfully.                    |
| 2001   | Invalid authentication.                  |
| 2002   | Authentication required.                 |
| 2003   | Authentication failed.                   |
| 2004   | Number of visits exceeds limit.          |
| 2005   | Connection has been authenticated.       |
| 2010   | Topic error.                             |
| 2011   | Contract doesn't exist.                  |
| 2012   | Topic not subscribed.                    |
| 2013   | Authentication type doesn't exist.       |
| 2014   | Repeated subscription.                   |
| 2030   | Exceeds connection limit of single user. |
| 2040   | Missing required parameter.              |

