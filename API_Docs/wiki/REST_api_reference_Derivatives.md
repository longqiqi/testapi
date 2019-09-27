### 接口列表

 |**权限类型**|    **接口数据类型**   |**请求方法**     |                                                                                                                                                   **类型**  | **描述**                    | **需要验签**|
  |----------- |------------------ |------------------------------------------------------------------------------------------------------------------------------------------------------------------- |---------- |---------------------------- |--------------|
  |读取     |基础信息接口           |api/v1/contract_contract_info |                                                                                                                     GET        |获取合约信息                 |否|
  |读取     |基础信息接口          |api/v1/contract_index |                                                                                                                             GET        |获取合约指数信息             |否|
  |读取     |基础信息接口           |api /v1/contract_price_limit|                                                                                                                     GET        |获取合约最高限价和最低限价   |否|
  |读取     |基础信息接口           | api/v1/contract_open_interest|                                                                                                                      GET        |获取当前可用合约总持仓量     |否|
  |读取     |基础信息接口           | api/v1/contract_delivery_price|                                                                                                                      GET        |获取预估交割价    |否|
  |读取  |市场行情接口           | /market/depth |                                                                                                                                    GET        |获取行情深度数据            |否|
  |读取     |市场行情接口          |/market/history/kline |                                                                                                                            GET        |获取K线数据                  |否|
  |读取     |市场行情接口          | /market/detail/merged |                                                                                                                         GET        |获取聚合行情                 |否|
  |读取     |市场行情接口           | /market/trade |                                                                                                                                    GET        |获取市场最近成交记录         |否|
  |读取     |市场行情接口           |/market/history/trade |                                                                                                                             GET        |批量获取最近的交易记录       |否|
  |读取     |资产接口           | api/v1/contract_account_info |                                                                                                                   POST       |获取用户账户信息             |是|
  |读取  |资产接口           |api/v1/contract_position_info |                                                                                                                    POST       |获取用户持仓信息             |是|
  |交易     |交易接口           |api/v1/contract_order |                                                                                                                            POST       |合约下单                     |是|
  |交易     |交易接口           |api/v1/contract_batchorder |                                                                                                                        POST       |合约批量下单                 |是|
  |交易      |交易接口           |api/v1/contract_cancel |                                                                                                                            POST       |撤销订单                     |是|
  |交易     |交易接口           |api/v1/contract_cancelall |                                                                                                                         POST       |全部撤单                     |是|
  |读取     |交易接口          |api/v1/contract_order_info |                                                                                                                       POST       |获取合约订单信息             |是|
  |读取    |交易接口           | api/v1/contract_order_detail |                                                                                                                   POST       |获取订单明细信息             |是|
  |读取     |交易接口           | api/v1/contract_openorders |                                                                                                                       POST       |获取合约当前未成交委托       |是|
  |读取     |交易接口           |api/v1/contract_hisorders |                                                                                                                        POST       |获取合约历史委托             |是|
 |读取     |交易接口           |api/v1/contract_matchresults |                                                                                                                        POST       |获取历史成交记录            |是|
 |交易     |账户接口           |v1/futures/transfer |                                                                                                                        POST       |币币账户和合约账户间进行资金的划转            |是|
 |读取     |账户接口           |api/v1/contract_sub_account_list |                                                                                                                        POST       |查询母账户下所有子账户资产信息            |是|
 |读取     |账户接口           |api/v1/contract_sub_account_info |                                                                                                                        POST       |查询单个子账户资产信息            |是|
 |读取     |账户接口           |api/v1/contract_sub_position_info |                                                                                                                        POST       |查询单个子账户持仓信息的            |是|
 |读取     |账户接口           |api/v1/contract_financial_record |                                                                                                                        POST       |查询用户财务记录            |是|
 |读取     |账户接口           |api/v1/contract_order_limit |                                                                                                                        POST       |查询用户当前的下单量限制            |是|
 |读取     |账户接口           |api/v1/contract_fee |                                                                                                                        POST       |查询用户当前的手续费费率            |是|
 |读取     |账户接口           |api/v1/contract_transfer_limit |                                                                                                                        POST       |用户查询用户当前的划转限制            |是|
 |读取     |账户接口           |api/v1/contract_position_limit |                                                                                                                        POST       |用户持仓量限制的查询            |是|
 |读取     |账户接口           |api/v1/contract_risk_info |                                                                                                                        GET       |查询合约风险准备金余额和预估分摊比例            |是|
 |读取     |账户接口           |api/v1/contract_insurance_fund |                                                                                                                        GET       |查询合约风险准备金余额历史数据            |是|
 |读取     |账户接口           |api/v1/contract_adjustfactor |                                                                                                                        GET       |查询平台阶梯调整系数            |是|
 |交易     |账户接口           |api/v1/lightning_close_position |                                                                                                                        POST       |闪电平仓下单            |是|

 ### 访问地址
| 访问地址 | 适用站点 | 适用功能 | 适用交易对 |
|----|----|----|----|
| https://api.hbdm.com| 火币合约|   行情     | 火币合约的交易品种  |
### 签名认证

#### <a name="199">认证方式</a>

用户私有接口(资产接口、交易接口)跟用户相关的所有接口都需加密验签，签证用户合法性

签名认证方式跟pro现货签名一致，详情请参考https://github.com/huobiapi/API_Docs/wiki/REST_authentication

#### <a name="200">访问次数限制</a>

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

### 市场行情接口

#### 获取合约信息 

URL api/v1/contract_contract_info

**请求参数**

|**参数名称**     |**参数类型**   |**必填**   |**描述**|
|---------------- |-------------- |---------- |------------------------------------------------------------|
  |symbol           |string         |false|      "BTC","ETH"...|
  |contract_type   |string         |false|      合约类型: （this_week:当周 next_week:下周 quarter:季度）|
  |contract_code   |string         |false|      BTC180914|

**备注**： 如果不填，默认查询所有所有合约信息;
如果contract_code填了值，那就按照contract_code去查询;
如果contract_code没有填值，则按照symbol+contract_type去查询;

**返回参数**

  |**参数名称**               |**是否必须**   |**类型**   |**描述**                          |**取值范围**|
  |-------------------------- |-------------- |---------- |--------------------------------- |--------------------------------------------------------------------------------------------------------------------|
  |status                     |true           |string     |请求处理结果                      |"ok" , "error"|
  |\<list\>(属性名称: data)    |                |           |                               | |
  |symbol                     |true           |string     |品种代码                          |"BTC","ETH"...|
  |contract_code             |true           |string     |合约代码                          |"BTC180914" ...|
  |contract_type             |true           |string     |合约类型                          |当周:"this_week", 次周:"next_week", 季度:"quarter"|
  |contract_size             |true           |decimal    |合约面值，即1张合约对应多少美元   |10, 100...|
  |price_tick                |true           |decimal    |合约价格最小变动精度             | 0.001, 0.01...|
  |delivery_date             |true           |string     |合约交割日期                     | 如"20180720"|
  |create_date               |true           |string     |合约上市日期                      |如"20180706"|
  |contract_status           |true           |int        |合约状态                          |合约状态: 0:已下市、1:上市、2:待上市、3:停牌，4:暂停上市中、5:结算中、6:交割中、7:结算完成、8:交割完成、9:暂停上市|
  |\</list\>    |             |               |                     |        |                 
  |ts                         |true           |long       |响应生成时间点，单位：毫秒  |      

**示例**

    GET https://api.hbdm.com/api/v1/contract_contract_info
    # Response
    {
      "status": "ok",
      "data": [
        {
          "symbol": "BTC",
          "contract_code": "BTC180914",
          "contract_type": "this_week",
          "contract_size": 100,
          "price_tick": 0.001,
          "delivery_date": "20180704",
          "create_date": "20180604",
          "contract_status": 1
         }
        ],
      "ts":158797866555
    }

#### 获取合约指数信息

URL  api/v1/contract_index

**请求参数**

  |**参数名称**   |**参数类型**   |**必填**   |**描述**|
  |-------------- |-------------- |---------- |----------------|
  |symbol         |string         |true       |"BTC","ETH"...|

**返回参数**

  |**参数名称**               |**是否必须**   |**类型**   |**描述**                     |**取值范围**|
  |--------------------------| --------------| ----------| ---------------------------- |----------------|
  |status                    | true           |string     |请求处理结果                 |"ok" , "error"|
  |\<list\>(属性名称: data)    |                |           |                           ||
  |symbol                     |true           |string     |指数代码                    | "BTC","ETH"...|
  |index_price               |true           |decimal    |指数价格   |                  |
  |index_ts                |true           |long   |响应生成时间点，单位：毫秒   |                  |
  |\</list\>               |                |           |                           ||                                                            
  |ts                         |true           |long       |时间戳，单位：毫秒   |

**示例**

    GET  https://api.hbdm.com/api/v1/contract_index?symbol=BTC
    # Response
    {
      "status":"ok",
      "data": [
         {
           "symbol": "BTC",
           "index_price":471.0817,
           "index_ts": 1490759594752
          }
        ],
      "ts": 1490759594752
    }

#### 获取合约最高限价和最低限价

URL api/v1/contract_price_limit

**请求参数**

  |**参数名称**     |**参数类型**   |**必填**   |**描述**|
  |----------------| --------------| ---------- |-----------------------------------------------------------------|
  |symbol           |string         |false      |"BTC","ETH"...|
  |contract_type   |string         |false      |合约类型 (当周:"this_week", 次周:"next_week", 季度:"quarter")|
  |contract_code   |string         |false      |BTC180914 ...|

**备注**：如果contract_code填了值，那就按照contract_code去查询，如contract_code没有填值，则按照symbol+contract_type去查询，两个查询条件必填一个

**返回参数**

  |**参数名称**               |**是否必须**   |**类型**   |**描述**                     |**取值范围**|
  |-------------------------- |-------------- |---------- |---------------------------- |------------------------------------------------------|
  status|true|string|请求处理结果|"ok" ,"error”|
  \<list\>(属性名称: data)||||
  symbol|true|string|品种代码|"BTC","ETH" ...
  high_limit|true|decimal|最高买价|    |
  low_limit|true| decimal|最低卖价|    |
  contract_code|  true|string|合约代码|如"BTC180914" ...
  contract_type|  true|string|合约类型|当周:"this_week", 次周:"next_week", 季度:"quarter"
  \<list\>||||
  ts|    true|long|  响应生成时间点，单位：毫秒   

**示例**

    GET  https://api.hbdm.com/api/v1/contract_price_limit?symbol=BTC&contract_type=this_week
    # Response
    {
      "status":"ok",
      "data": 
       [{
          "symbol":"BTC",
          "high_limit":443.07,
          "low_limit":417.09,
          "contract_code":"BTC180914",
          "contract_type":"this_week"
         }],
      "ts": 1490759594752
    }

#### 获取当前可用合约总持仓量 

URL api/v1/contract_open_interest

**请求参数**

  |**参数名称**|**参数类型**   |**必填**   |**描述**|
  |---------------- |-------------- |---------- |-----------------------------------------------------------------|
 | symbol|string|    false| "BTC","ETH"...|
  |contract_type|   string|    false| 合约类型 (当周:"this_week", 次周:"next_week", 季度:"quarter")|
  |contract_code|   string|    false| BTC180914|

**返回参数**

  |**参数名称**|    **是否必须**   |**类型**   |**描述**|**取值范围**|
 | -------------------------- |-------------- |---------- |----------------------------| ------------------------------------------------------|
  |status|true|string|请求处理结果| "ok" , "error"|
  |\<list\>(属性名称: data)||||
  |symbol|true|string|品种代码|"BTC", "ETH" ...|
  |contract_type|  true|string|合约类型|当周:"this_week", 次周:"next_week", 季度:"quarter"|
  |volume|true|decimal|    持仓量(张)||   
  |amount|true|decimal|    持仓量(币)||   
  |contract_code|  true|string|合约代码|如"BTC180914" ...|
  |\</list\>|||||
  |ts|    true|long|  响应生成时间点，单位：毫秒   |

**示例**

    GET  https://api.hbdm.com/api/v1/contract_open_interest?symbol=BTC&contract_type=this_week
    # Response
    {
      "status":"ok",
      "data":
        [{
          "symbol":"BTC",
          "contract_type": "this_week",
          "volume":123,
          "amount":106,
          "contract_code": "BTC180914"
         }],
      "ts": 1490759594752
    }
#### 获取预估交割价

URL api/v1/contract_delivery_price

**请求参数**

  |**参数名称**|**参数类型**   |**必填**   |**描述**|
  |---------------- |-------------- |---------- |-----------------------------------------------------------------|
 | symbol|string|    true| "BTC","ETH"...|


**返回参数**

  |**参数名称**|    **是否必须**   |**类型**   |**描述**|**取值范围**|
 | -------------------------- |-------------- |---------- |----------------------------| ------------------------------------------------------|
  |status|true|string|请求处理结果| "ok" , "error"|
  |\<list\>(属性名称: data)||||
  |delivery_price|  true|string|预估交割价| |
  |\</list\>|||||
  |ts|    true|long|  响应生成时间点，单位：毫秒   |

**示例**

    GET  https://api.hbdm.com/api/v1/contract_delivery_price?symbol=BTC
    # Response
    {
      "status":"ok",
      "data":
        {
          "delivery_price": 3806.4615259197324414715719,     
         },
      "ts": 1490759594752
    }
#### 获取行情深度数据

URL /market/depth

**请求参数**

  |**参数名称**   |**参数类型**   |**必填**   |**描述**|
  |-------------- |-------------- |---------- |--------------------------------------------------------------------------------|
  |symbol|    string|    true|  如"BTC_CW"表示BTC当周合约，"BTC_NW"表示BTC次周合约，"BTC_CQ"表示BTC季度合约|
  |type|string|    true|  (150档数据)  step0, step1, step2, step3, step4, step5（合并深度1-5）；step0时，不合并深度, (20档数据)  step6, step7, step8, step9, step10, step11（合并深度7-11）；step6时，不合并深度|

**返回参数**

| **参数名称** | **是否必须** | **数据类型** | **描述** | **取值范围** |
| -------- | -------- | -------- |--------------------------------------- | -------------- | 
| ch | true |string | 数据所属的 channel，格式： market.period | | 
| status | true |string | 请求处理结果 | "ok" , "error" | 
| asks | true | object |卖盘,[price(挂单价), vol(此价格挂单张数)], 按price升序 | | 
| bids | true| object | 买盘,[price(挂单价), vol(此价格挂单张数)], 按price降序 | | 
| mrid| true| string | 订单ID | | 
|ts | true | number | 响应生成时间点，单位：毫秒 | |

    tick 说明:

    "tick": {
      "id": 消息id.
      "ts": 消息生成时间，单位：毫秒.
      "bids": 买盘,[price(挂单价), vol(此价格挂单张数)], //按price降序.
      "asks": 卖盘,[price(挂单价), vol(此价格挂单张数)]  //按price升序.
      "ch": 数据所属的 channel,
      "mrid": 订单ID,
      "ts": 时间戳,
      "version": 版本
    }

**示例**

    GET https://api.hbdm.com/market/depth?symbol=BTC_CQ&type=step5
   # Response
    {
      "ch":"market.BTC_CQ.depth.step5",
      "status":"ok",
        "tick":{
          "asks":[
            [6580,3000],
            [70000,100]
            ],
          "bids":[
            [10,3],
            [2,1]
            ],
          "ch":"market.BTC_CQ.depth.step5",
          "id":1536980854,
          "mrid":6903717,
          "ts":1536980854171,
          "version":1536980854
        },
      "ts":1536980854585
    }

#### 获取K线数据

URL /market/history/kline

**请求参数**

  |**参数名称**   |**是否必须**  | **类型**   |**描述**   |**默认值**   |**取值范围**|
  |-------------- |-------------- |---------- |---------- |------------ |--------------------------------------------------------------------------------|
  |symbol|    true|string|合约名称||如"BTC_CW"表示BTC当周合约，"BTC_NW"表示BTC次周合约，"BTC_CQ"表示BTC季度合约|
  |period|    true|string|K线类型|| 1min, 5min, 15min, 30min, 60min,4hour,1day, 1mon|
  |size|true|integer    |获取数量   |150|[1,2000]|

**返回参数**

  |**参数名称**   |**是否必须**   |**数据类型**   |**描述**|   **取值范围**|
 | -------------- |-------------- |-------------- |------------------------------------------ |----------------|
  |ch|  true|string|    数据所属的 channel，格式： market.period   |
  |data|true|object|    KLine 数据|| 
  |status|    true|string|    请求处理结果|"ok" , "error"|
  |ts|  true|number|    响应生成时间点，单位：毫秒|| 

Data说明：
```
"data": [
  {
    "id": K线id,
    "vol": 成交量(张),
    "count": 成交笔数,
    "open": 开盘价,
    "close": 收盘价,当K线为最晚的一根时，是最新成交价
    "low": 最低价,
    "high": 最高价,
    "amount": 成交量(币), 即 sum(每一笔成交量(张)*单张合约面值/该笔成交价)
   }
]
```

**示例**

    GET  https://api.hbdm.com/market/history/kline?period=1min&size=200&symbol=BTC_CQ
    # Response
    {
      "ch": "market.BTC_CQ.kline.1min",
      "data": [
        {
          "vol": 2446,
          "close": 5000,
          "count": 2446,
          "high": 5000,
          "id": 1529898120,
          "low": 5000,
          "open": 5000,
          "amount": 48.92
         },
        {
          "vol": 0,
          "close": 5000,
          "count": 0,
          "high": 5000,
          "id": 1529898780,
          "low": 5000,
          "open": 5000,
          "amount": 0
         }
       ],
      "status": "ok",
      "ts": 1529908345313
    }

#### 获取聚合行情

URL /market/detail/merged

**请求参数**

  |**参数名称**   |**是否必须**   |**类型**   |**描述**   |**默认值**   |**取值范围**|
  |--------------| --------------| ---------- |----------| ------------ |--------------------------------------------------------------------------------|
  |symbol|    true|string|合约名称|如"BTC_CW"表示BTC当周合约，"BTC_NW"表示BTC次周合约，"BTC_CQ"表示BTC季度合约|

**返回参数**

  |**参数名称**   |**是否必须**  | **数据类型**   |**描述**|    **取值范围**|
 | -------------- |-------------- |-------------- |----------------------------------------------------------| ----------------|
  |ch|  true|string|    数据所属的 channel，格式： market.\$symbol.detail.merged   |
  |status|    true|string|    请求处理结果|"ok" , "error"|
  |tick|true|object|    K线数据||
  |ts|  true|number|    响应生成时间点，单位：毫秒|| 

tick说明:

    "tick": {
      "id": K线id,
      "vol": 成交量（张）,
      "count": 成交笔数,
      "open": 开盘价,
      "close": 收盘价,当K线为最晚的一根时，是最新成交价
      "low": 最低价,
      "high": 最高价,
      "amount": 成交量(币), 即 sum(每一笔成交量(张)*单张合约面值/该笔成交价)
      "bid": [买1价,买1量(张)],
      "ask": [卖1价,卖1量(张)]
     }

**示例**

    GET  https://api.hbdm.com/market/detail/merged?symbol=BTC_CQ
    # Response
    {
      "ch": "market.BTC_CQ.detail.merged",
      "status": "ok",
      "tick": 
        {
          "vol":"13305",
          "ask": [5001, 2],
          "bid": [5000, 1],
          "close": "5000",
          "count": "13305",
          "high": "5000",
          "id": 1529387841,
          "low": "5000",
          "open": "5000",
          "ts": 1529387842137,
          "amount": "266.1"
         },
      "ts": 1529387842137
    }



#### 获取市场最近成交记录

URL /market/trade

**请求参数**

  |**参数名称**   |**是否必须**   |**类型**   |**描述**   |**默认值**   |**取值范围**|
  |-------------- |-------------- |---------- |---------- |------------ |--------------------------------------------------------------------------------|
  |symbol|    true|string|合约名称|如"BTC_CW"表示BTC当周合约，"BTC_NW"表示BTC次周合约，"BTC_CQ"表示BTC季度合约|

**返回参数**

  |**参数名称**   |**是否必须**   |**类型**   |**描述**|  **默认值**   |**取值范围**|
  |--------------| --------------| ----------| ---------------------------------------------------------| ------------ |--------------|
  |ch|  true|string|数据所属的 channel，格式： market.\$symbol.trade.detail||
  |status|    true|string|||  "ok","error"
  |tick|true|object|Trade 数据|||   
  |ts|  true|number|发送时间|||

Tick说明：

    "tick": {
      "id": 消息id,
      "ts": 最新成交时间,
      "data": [
        {
       "id": 成交id,
        "price": 成交价钱,
         "amount": 成交量(张),
         "direction": 主动成交方向,
         "ts": 成交时间
        }
      ]
    }

**示例**

    GET  https://api.hbdm.com/market/trade?symbol=BTC_CQ
    # Response
    {
      "ch": "market.BTC_CQ.trade.detail",
      "status": "ok",
      "tick": {
        "data": [
          {
            "amount": "1",
            "direction": "sell",
            "id": 6010881529486944176,
            "price": "5000",
            "ts": 1529386945343
           }
         ],
        "id": 1529388202797,
        "ts": 1529388202797
        },
      "ts": 1529388202797
    }

#### 批量获取最近的交易记录

URL /market/history/trade

**请求参数：**

  |**参数名称**   |**是否必须**   |**数据类型**   |**描述**|  **默认值**   |**取值范围**|
  |-------------- |-------------- |-------------- |-------------------- |------------ |--------------------------------------------------------------------------------|
 | symbol|    true|string|    合约名称||如"BTC_CW"表示BTC当周合约，"BTC_NW"表示BTC次周合约，"BTC_CQ"表示BTC季度合约
  |size|false|number|    获取交易记录的数量  | 1| [1, 2000]

**返回参数**

  |**参数名称**   |**是否必须**   |**数据类型**   |**描述**|   **取值范围**|
  |--------------| --------------| --------------| ---------------------------------------------------------| ---------------|
  |ch|  true|string|    数据所属的 channel，格式： market.\$symbol.trade.detail   ||
  |data|true|object|    Trade 数据||
  |status|    true|string||    "ok"，"error"
  |ts|  true|number|    响应生成时间点，单位：毫秒||

data说明：

    "data": {
      "id": 消息id,
      "ts": 最新成交时间,
      "data": [
        {
          "id": 成交id,
          "price": 成交价,
          "amount": 成交量(张),
          "direction": 主动成交方向,
          "ts": 成交时间
        }
      ]
    }

**示例**

    GET  https://api.hbdm.com/market/history/trade?symbol=BTC_CQ&size=100
    # Response
    {
      "ch": "market.BTC_CQ.trade.detail",
      "status": "ok",
      "ts": 1529388050915,
      "data": [
        {
          "id": 601088,
          "ts": 1529386945343,
          "data": [
            {
             "amount": 1,
             "direction": "sell",
             "id": 6010881529486944176,
             "price": 5000,
             "ts": 1529386945343
             }
           ]
        }
       ]
    }


### 资产接口

#### 获取用户账户信息

URL  api/v1/contract_account_info

**请求参数**

  |**参数名称**   |**是否必须**   |**类型**   |**描述**   |**默认值**   |**取值范围**|
  |-------------- |-------------- |---------- |----------| ------------ |------------------------------------------|
  |symbol|    false|string|品种代码||"BTC","ETH"...如果缺省，默认返回所有品种|

**返回参数**

  |**参数名称**|    **是否必须**   |**类型**   |**描述**|   **取值范围**|
  |-------------------------- |-------------- |---------- |------------------------------------------ |----------------|
  |status|true|string|请求处理结果|"ok" , "error"|
  |\<list\>(属性名称: data)||||    
  |symbol|true|string|品种代码|"BTC","ETH"...|
  |margin_balance| true|decimal    |账户权益||   
  |margin_position|true|decimal    |持仓保证金（当前持有仓位所占用的保证金）   ||
  |margin_frozen|  true|decimal    |冻结保证金|| 
  |margin_available|true|decimal   |可用保证金|| 
  |profit_real|    true|decimal    |已实现盈亏|| 
  |profit_unreal|  true|decimal    |未实现盈亏|| 
  |risk_rate| true|decimal    |保证金率||   
  |liquidation_price|    true|decimal    |预估强平价|| 
  |withdraw_available|   true|decimal    |可划转数量|| 
  |lever_rate|true|decimal    |杠杠倍数|||   
  |adjust_factor|true|decimal    |调整系数|||   
  |\</list\>||||   
  |ts|    number|    long|  响应生成时间点，单位：毫秒|| 

**示例**

    POST  https://api.hbdm.com/api/v1/contract_account_info    
    # Response
    {
      "status": "ok",
      "data": [
        {
          "symbol": "BTC",
          "margin_balance": 1,
          "margin_position": 0,
          "margin_frozen": 3.33,
          "margin_available": 0.34,
          "profit_real": 3.45,
          "profit_unreal": 7.45,
          "withdraw_available":4.0989898,
          "risk_rate": 100,
          "liquidation_price": 100，
		  "adjust_factor": 0.1
         },
        {
          "symbol": "ETH",
          "margin_balance": 1,
          "margin_position": 0,
          "margin_frozen": 3.33,
          "margin_available": 0.34,
          "profit_real": 3.45,
          "profit_unreal": 7.45,
          "withdraw_available":4.7389859,
          "risk_rate": 100,
          "liquidation_price": 100,
		   "adjust_factor": 0.2
         }
       ],
      "ts":158797866555
    }


#### 获取用户持仓信息

URL api/v1/contract_position_info

**请求参数**

  |**参数名称**   |**是否必须**   |**类型**   |**描述**   |**默认值**   |**取值范围**|
  |-------------- |--------------| ---------- |----------| ------------ |------------------------------------------|
  |symbol|    false|string|品种代码||"BTC","ETH"...如果缺省，默认返回所有品种|

**返回参数**

  |**参数名称**|    **是否必须**  | **类型**  | **描述**|**取值范围**|
  |-------------------------- |-------------- |---------- |---------------------------- |------------------------------------------------------|
  |status|true|string|请求处理结果| "ok" , "error"|
  |\<list\>(属性名称: data)||||
  |symbol|true|string|品种代码|"BTC","ETH"...|
  |contract_code|  true|string|合约代码|"BTC180914" ...|
  |contract_type|  true|string|合约类型|当周:"this_week", 次周:"next_week", 季度:"quarter"|
  |volume|true|decimal    |持仓量|  |
  |available| true|decimal    |可平仓数量||   
  |frozen|true|decimal    |冻结数量||
  |cost_open|true|decimal    |开仓均价||
  |cost_hold| true|decimal    |持仓均价||
  |profit_unreal|  true|decimal    |未实现盈亏||   
  |profit_rate|    true|decimal    |收益率| | 
  |profit|true|decimal|    收益|    |
  |position_margin|true|decimal    |持仓保证金||   
  |lever_rate|true|int|   杠杠倍数||
  |direction|true|  string|"buy":买 "sell":卖|||
  |last_price|true|  decimal	  |最新价|||
  |\</list\>|||||
  |ts|    true|long|  响应生成时间点，单位：毫秒   ||

**示例**

    POST  https://api.hbdm.com/api/v1/contract_position_info
     # Response
    {
      "status": "ok",
      "data": [
        {
          "symbol": "BTC",
          "contract_code": "BTC180914",
          "contract_type": "this_week",
          "volume": 1,
          "available": 0,
          "frozen": 0.3,
          "cost_open": 422.78,
          "cost_hold": 422.78,
          "profit_unreal": 0.00007096,
          "profit_rate": 0.07,
          "profit": 0.97,
          "position_margin": 3.4,
          "lever_rate": 10,
          "direction":"buy",
          "last_price":7900.17
         }
        ],
     "ts": 158797866555
    }


### 交易接口


#### 合约下单 

URL api/v1/contract_order

**请求参数**

  |**参数名**|**参数类型**   |**必填**   |**描述**|
  |-------------------- |-------------- |----------| ---------------------------------------------------------------|
  |symbol|    string|    true| "BTC","ETH"...|
  |contract_type|  string|    true| 合约类型 ("this_week":当周 "next_week":下周 "quarter":季度)|
  |contract_code|  string|    true| BTC180914|
  |client_order_id |   long|false| 客户自己填写和维护，这次一定要大于上一次|
  |price|decimal|   true|  价格|
  |volume|    long|true|  委托数量(张)|
  |direction| string|    true|  "buy":买 "sell":卖|
  |offset|    string|    true|  "open":开 "close":平|
  |lever_rate|int| true|  杠杆倍数[“开仓”若有10倍多单，就不能再下20倍多单]|
  |order_price_type |  string|    true|  订单报价类型 "limit":限价 "opponent":对手价 "post_only":只做maker单,post only下单只受用户持仓数量限制,optimal_5：最优5档、optimal_10：最优10档、optimal_20：最优20档|

**备注**：如果contract_code填了值，那就按照contract_code去下单，如果contract_code没有填值，则按照symbol+contract_type去下单。
对手价下单price价格参数不用传，对手价下单价格是买一和卖一价,optimal_5：最优5档、optimal_10：最优10档、optimal_20：最优20档下单price价格参数不用传，"limit":限价，"post_only":只做maker单 需要传价格。

**开平方向**：

开多：买入开多(direction用buy、offset用open)

平多：卖出平多(direction用sell、offset用close)

开空：卖出开空(direction用sell、offset用open)

平空：买入平空(direction用buy、offset用close)


**返回参数**

  |**参数名称**|   **是否必须**   |**类型**   |**描述**|**取值范围**|
  |------------------- |-------------- |---------- |-------------------------------------------- |----------------|
  |status|   true|string|请求处理结果|"ok" , "error"|
  |order_id|true|long|  订单ID|| 
  |client_order_id  | true|long|  用户下单时填写的客户端订单ID，没填则不返回  | 
  |ts|  true|long|  响应生成时间点，单位：毫秒||   

**示例**

    POST  https://api.hbdm.com/api/v1/contract_order

  # Response
    {
      "status": "ok",
      "data": {
		"order_id": 88
	      },
      "ts": 158797866555
    }

#### 合约批量下单 

URL api/v1/contract_batchorder

**请求参数**

  |**参数名**|    **参数类型**   |**必填**   |**描述**|
  |---------------------------------- |-------------- |---------- |--------------------------------------------------------------|
  |\<list\>(属性名称: orders_data)|| ||  
  |symbol|   string|    false| "BTC","ETH"...|
  |contract_type|string|    false| 合约类型: "this_week":当周 "next_week":下周 "quarter":季度|
  |contract_code|string|    false| BTC180914|
  |client_order_id|  long|false|  客户自己填写和维护，这次一定要大于上一次|
  |price|  decimal|   true|  价格|
  |volume|  long|true|  委托数量(张)|
  |direction|string|    true|  "buy":买 "sell":卖|
  |offset|  string|    true|  "open":开 "close":平|
  |lever_rate|   int| true|  杠杆倍数[“开仓”若有10倍多单，就不能再下20倍多单]|
  |order_price_type| string|    true|  订单报价类型 "limit":限价 "opponent":对手价 "post_only":只做maker单,post only下单只受用户持仓数量限制,optimal_5：最优5档、optimal_10：最优10档、optimal_20：最优20档|
  |\</list\>||||

**备注**：如果contract_code填了值，那就按照contract_code去下单，如果contract_code没有填值，则按照symbol+contract_type去下单。
对手价下单price价格参数不用传，对手价下单价格是买一和卖一价,optimal_5：最优5档、optimal_10：最优10档、optimal_20：最优20档下单price价格参数不用传，"limit":限价，"post_only":只做maker单 需要传价格。

**返回参数**

  |**参数名称**|  **是否必须**   |**类型**   |**描述**|**取值范围**|
  |----------------------------- |-------------- |---------- |--------------------------------------------| ----------------|
  |status|   true|string|请求处理结果| "ok" , "error"|
  |\<list\>(属性名称: errors)|||| 
  |index|    true|int|   订单索引||
  |err_code|true|int|   错误码||
  |err_msg| true|string|错误信息||
  |\</list\>||||  
  |\<list\>(属性名称: success)||||
  |index|    true|int|   订单索引||
  |order_id|true|long|  订单ID|| 
  |client_order_id|  true|long|  用户下单时填写的客户端订单ID，没填则不返回  | 
  |\</list\>||||
  |ts|  true|long|  响应生成时间点，单位：毫秒|

**示例**

    POST  https://api.hbdm.com/api/v1/contract_batchorder
    # Response
    {
      "status": "ok",
      "data": {
        "errors":[
          {
            "index":1,
            "err_code": 200417,
            "err_msg": "invalid symbol"
           },
          {
            "index":3,
            "err_code": 200415,
            "err_msg": "invalid symbol"
           }
         ],
        "success":[
          {
            "index":2,
            "order_id":161256,
            "client_order_id":1344567
           },
          {
            "index":3,
            "order_id":161257,
            "client_order_id":1344569
           }
         ]
       },
      "ts": 1490759594752
    }

#### 撤销订单 

URL api/v1/contract_cancel

**请求参数**

  |**参数名称**|   **是否必须**   |**类型**   |**描述**|
  |------------------- |-------------- |---------- |--------------------------------------------------------------|
  |order_id|false|string|订单ID（ 多个订单ID中间以","分隔,一次最多允许撤消50个订单 ）|
  |client_order_id|  false|string|客户订单ID(多个订单ID中间以","分隔,一次最多允许撤消50个订单)|
  |symbol|   true|string|"BTC","ETH"...|

**备注**：
order_id和client_order_id都可以用来撤单，同时只可以设置其中一种，如果设置了两种，默认以order_id来撤单。

**返回参数**

  |**参数名称**| **是否必须**  | **类型**  | **描述**| **取值范围**|
  |---------------------------- |-------------- |---------- |--------------------------------------------------| ----------------|
  |status|  true|string|请求处理结果| "ok" , "error"|
  |\<list\>(属性名称: errors)||||
  |order_id|    true|string|订单ID||   
  |err_code|   true|int|   错误码||   
  |err_msg|true|string|错误信息|| 
  |\</list\>||||
  |successes|   true|string|撤销成功的订单的order_id或client_order_id列表  | |
  |ts|true|long|  响应生成时间点，单位：毫秒| |


**示例**

    POST  https://api.hbdm.com/api//v1/contract_cancel
    # Response
    {
      "status": "ok",
      "data": {
        "errors":[
          {
            "order_id":"161251",
            "err_code": 200417,
           "err_msg": "invalid symbol"
           },
          {
          "order_id":161253,
          "err_code": 200415,
          "err_msg": "invalid symbol"
           }
         ],
        "success":[161256,1344567]
       },
      "ts": 1490759594752
    }

#### 全部撤单 

URL api/v1/contract_cancelall

**请求参数**

  |**参数名称**   |**是否必须**   |**类型**   |**描述**|
  |-------------- |-------------- |---------- |----------------------------|
  |symbol|    true|string|品种代码，如"BTC","ETH"...|
  |contract_code|    false|string|合约code|
  |contract_type|    false|string|合约类型|
 
**备注**： 只传symbol，撤该该品种下所有周期的合约
只要有contract_code，则撤销该code的合约
只传symbol+contract_type， 则撤销二者拼接所成的合约订单

**返回参数**

  |**参数名称**| **是否必须**   |**类型**  | **描述**|**取值范围**|
  |---------------------------- |-------------- |---------- |---------------------------- |----------------|
  |status|  true|string|请求处理结果| "ok" , "error"| 
  |\<list\>(属性名称: errors)||||
  |order_id|    true|String|订单id| | 
  |err_code|    true|int|   订单失败错误码| |   
  |err_msg|true|int|   订单失败信息|| 
  |\</list\>|||| 
  |successes|    true|string|成功的订单||   
  |ts| true|long|  响应生成时间点，单位：毫秒  || 

**示例**

    POST  https://api.hbdm.com.com/api/v1/contract_cancelall
    # Response
    #多笔订单返回结果(成功订单ID,失败订单ID)
    {
      "status": "ok",
      "data": {
        "errors":[
          {
            "order_id":"161251",
            "err_code": 200417,
            "err_msg": "invalid symbol"
           },
          {
            "order_id":161253,
            "err_code": 200415,
            "err_msg": "invalid symbol"
           }
          ],
        "successes":[161256,1344567]
       },
      "ts": 1490759594752
    }


#### 获取合约订单信息

URL api/v1/contract_order_info

**请求参数**

  |**参数名称**|   **是否必须**   |**类型**   |**描述**|
  |------------------- |--------------| ---------- |-------------------------------------------------------------|
  |order_id|false|string|订单ID（ 多个订单ID中间以","分隔,一次最多允许查询50个订单 ）|
  |client_order_id   |false|string|客户订单ID(多个订单ID中间以","分隔,一次最多允许查询50个订单)|
  |symbol|   true|string|"BTC","ETH"...|

**备注**：order_id和client_order_id都可以用来查询，同时只可以设置其中一种，如果设置了两种，默认以order_id来查询。
周五交割结算后，会把结束状态的订单（5部分成交已撤单 6全部成交 7已撤单）删除掉。

**返回数据**

  |**参数名称**|    **是否必须**   |**类型**   |**描述**| **取值范围**|
  |-------------------------- |-------------- |---------- |-------------------------------------------------------------------------------------------- |----------------------------------------------------|
  |status|true|string|请求处理结果|  "ok" , "error"|
  |\<list\>(属性名称: data)||||| 
  |symbol|true|string|品种代码||| 
  |contract_type|  true|string|合约类型|当周:"this_week", 周:"next_week", 季度:"quarter"|
  |contract_code|  true|string|合约代码| "BTC180914" ...|
  |volume|true|decimal    |委托数量|| 
  |price| true|decimal    |委托价格|| 
  |order_price_type|    true|string|订单报价类型 "limit":限价 "opponent":对手价 "post_only":只做maker单,post only下单只受用户持仓数量限制||   
  |direction|  true|string|"buy":买 "sell":卖||
  |offset|true|string|"open":开 "close":平||
  |lever_rate|true|int|   杠杆倍数|1\\5\\10\\20|
  |order_id|  true|long|  订单ID|| 
  |client_order_id|true|long|  客户订单ID||  
  |created_at|true|long|  成交时间||
  |trade_volume|   true|decimal|    成交数量||
  |trade_turnover| true|decimal    成交总金额||    
  |fee|   true|decimal|    手续费||   
  |trade_avg_price|true|decimal|    成交均价|| 
  |margin_frozen|  true|decimal|    冻结保证金||   
  |profit|true|decimal|    收益||
  |status|true|int|   订单状态|(1准备提交 2准备提交 3已提交 4部分成交 5部分成交已撤单 6全部成交 7已撤单 11撤单中) |  
  |order_type|true|string|   订单类型|  1:报单 、 2:撤单 、 3:强平、4:交割
  |order_source|   true|string|订单来源|（1:system、2:web、3:api、4:m 5:risk、6:settlement） |   
  |\</list\>|||||
  |ts|    true|long|  时间戳||   

**示例**

    POST  https://api.hbdm.com.com/api/v1/contract_order_info
  # Response
    {
      "status": "ok",
      "data":[
        {
          "symbol": "BTC",
          "contract_type": "this_week",
          "contract_code": "BTC180914",
          "volume": 111,
          "price": 1111,
          "order_price_type": "limit",
          "direction": "buy",
          "offset": "open",
          "lever_rate": 10,
          "order_id": 106837,
          "client_order_id": 10683,
          "order_source": "web",
          "order_type": "1",
          "created_at": 1408076414000,
          "trade_volume": 1,
          "trade_turnover": 1200,
          "fee": 0,
          "trade_avg_price": 10,
          "margin_frozen": 10,
          "profit ": 10,
          "status": 0
         },
        {
          "symbol": "ETH",
          "contract_type": "this_week",
          "contract_code": "ETH180921",
          "volume": 111,
          "price": 1111,
          "order_price_type": "limit",
          "direction": "buy",
          "offset": "open",
          "lever_rate": 10,
          "order_id": 106837,
          "client_order_id": 10683,
          "order_source": "web",
           "order_type": "1",
          "created_at": 1408076414000,
          "trade_volume": 1,
          "trade_turnover": 1200,
          "fee": 0,
          "trade_avg_price": 10,
          "margin_frozen": 10,
          "profit ": 10,
          "status": 0
         }
        ],
      "ts": 1490759594752
    }

#### 获取订单明细信息

URL api/v1/contract_order_detail

**请求参数**

  |**参数名称**   |**是否必须**   |**类型**   |**描述**|
  |-------------- |-------------- |---------- |------------------------|
  |symbol|    true|string|"BTC","ETH"...|
  |order_id| true|long|  订单id|
  |created_at|  true|long|  下单时间戳|
  |order_type|  true|int|  订单类型，1:报单 、 2:撤单 、 3:强平、4:交割|
  |page_index|    false|int|   第几页,不填第一页|
  |page_size|false|int|   不填默认20，不得多于50|

**返回数据**

  |**参数名称**|  **是否必须**   |**类型**   |**描述**| **取值范围**|
  |----------------------------- |-------------- |---------- |--------------------------------------------- |------------------------------------------------------|
  |status|   true|string|请求处理结果| "ok" , "error"|
  |\<object\> (属性名称: data)|||| 
  |symbol|   true|string|品种代码|| 
  |contract_type|true|string|合约类型|当周:"this_week", 次周:"next_week", 季度:"quarter"|
  |contract_code|true|string|合约代码|"BTC180914" ...|
  |lever_rate|   true|int|   杠杆倍数| 1\5\10\20|
  |direction|true|string|"buy":买 "sell":卖||  
  |offset|   true|string|"open":开 "close":平||
  |volume|   true|decimal    |委托数量|| 
  |price|    true|decimal    |委托价格|| 
  |created_at|   true|long|  成交时间||
  |order_source| true|string|订单来源|| 
  |order_price_type| true|string|订单报价类型 |订单报价类型 "limit":限价 "opponent":对手价 "post_only":只做maker单,post only下单只受用户持仓数量限制   |  
  |margin_frozen|true|decimal    |冻结保证金||    
  |profit|   true|decimal    |收益||
  |total_page|   true|int|   总共页数|||
  |current_page| true|int|   当前页数|| 
  |total_size|   true|int|   总条数||   
  |\<list\> (属性名称: trades)|||| 
  |trade_id|true|long|  撮合结果id 非唯一，可重复，注意：一个撮合结果代表一个taker单和N个maker单的成交记录的集合，如果一个taker单吃了N个maker单，那这N笔trade都是一样的撮合结果id||    
  |trade_price|  true|decimal|    撮合价格||
  |trade_volume| true|decimal|   成交量||  
  |trade_turnover|    true|decimal|    成交金额|| 
  |trade_fee|   true|decimal|    成交手续费||    
  |created_at|   true|long|  创建时间||| 
  |role|   true|string|  taker或maker||| 
  |\</list\>||||   
  |\</object \>||||
  |ts|  true|long|  时间戳||

**示例**

    POST  https://api.hbdm.com/api/v1/contract_order_detail
 # Response
    {
      "status": "ok",
      "data":{
        "symbol": "BTC",
        "contract_type": "this_week",
        "contract_code": "BTC180914",
        "volume": 111,
        "price": 1111,
        "order_price_type": "limit",
        "direction": "buy",
        "offset": "open",
        "status": 1,
        "lever_rate": 10,
        "trade_avg_price": 10,
        "margin_frozen": 10,
        "profit": 10,
        "order_id": 106837,
        "order_source": "web",
        "created_at": 1408076414000,
        "trades":[
          {
            "trade_id":112,
            "trade_volume":1,
            "trade_price":123.4555,
            "trade_fee":0.234,
            "trade_turnover":34.123,
            "role": "maker",
            "created_at": 1490759594752
           }
          ],
        "total_page":15,
        "total_size":3,
        "current_page":3
        },
      "ts": 1490759594752
    }
    错误:
    {
     "status":"error",
     "err_code":20029,
     "err_msg": "invalid symbol",
     "ts": 1490759594752
    }

#### 获取合约当前未成交委托 

URL  api/v1/contract_openorders

**请求参数**

  |**参数名称**   |**是否必须**   |**类型**   |**描述**| **默认值**   |**取值范围**|
  |-------------- |-------------- |---------- |------------------------ |------------ |----------------|
  |symbol|    true|string|品种代码|    |"BTC","ETH"...|
  |page_index   | false|int|   ||页码，不填默认第1页| 1| 
  |page_size|false|int|   ||不填默认20，不得多于50 |

**返回参数**

  |**参数名称**|    **是否必须**   |**类型**   |**描述**|   **取值范围**|
  |-------------------------- |-------------- |---------- |--------------------------------------------------------------- |------------------------------------------------------|
  |status|true|string|请求处理结果||
  |\<list\>(属性名称: data)||||   
  |symbol|true|string|品种代码||  
  |contract_type|  true|string|合约类型|当周:"this_week", 次周:"next_week", 季度:"quarter"|
  |contract_code|  true|string|合约代码|"BTC180914" ...|
  |volume|true|decimal    |委托数量||
  |price| true|decimal    |委托价格||   
  |order_price_type|    true|string|订单报价类型 "limit":限价 "opponent":对手价 "post_only":只做maker单,post only下单只受用户持仓数量限制|
  |direction|  true|string|"buy":买 "sell":卖||   
  |offset|true|string|"open":开 "close":平||  
  |lever_rate||true|int|   杠杆倍数|   1\\5\\10\\20|
  |order_id||  true|long|  订单ID||
  |client_order_id|true|long|  客户订单ID||
  |created_at|true|long|  订单创建时间||
  |trade_volume|   true|decimal    |成交数量||  
  |trade_turnover| true|decimal    |成交总金额|| 
  |fee|   true|decimal    |手续费||
  |trade_avg_price|true|decimal    |成交均价||  
  |margin_frozen|  true|decimal    |冻结保证金|| 
  |profit|true|decimal   | 收益||  
  |status|true|int|   订单状态|(3未成交 4部分成交 5部分成交已撤单 6全部成交 7已撤单) |  
  |order_source|   true|string|订单来源||
  |\</list\>||||
  |total_page|true|int|   总页数||
  |current_page|   true|int|   当前页||
  |total_size|true|int|   总条数||
  |ts|    true|long|  时间戳||

**示例**

    POST https://www.hbdm.com/api/v1/contract_openorders
 # Response
    {
      "status": "ok",
      "data":{
        "orders":[
          {
             "symbol": "BTC",
             "contract_type": "this_week",
             "contract_code": "BTC180914",
             "volume": 111,
             "price": 1111,
             "order_price_type": "limit",
             "direction": "buy",
             "offset": "open",
             "lever_rate": 10,
             "order_id": 106837,
             "client_order_id": 10683,
             "order_source": "web",
             "created_at": 1408076414000,
             "trade_volume": 1,
             "trade_turnover": 1200,
             "fee": 0,
             "trade_avg_price": 10,
             "margin_frozen": 10,
             "status": 1
            }
           ],
        "total_page":15,
        "current_page":3,
        "total_size":3
       },
      "ts": 1490759594752
    }


#### 获取合约历史委托

URL api/v1/contract_hisorders

**请求参数**

  |**参数名称**   |**是否必须**   |**类型**   |**描述**| **默认值**   |**取值范围**|
  |--------------| --------------| ---------- |------------------------| ------------| ------------------------------------------------------------------------------------------------------|
  |symbol|    true|string|品种代码|   "BTC","ETH"...|
  |trade_type |   true|int|   交易类型|    0:全部,1:买入开多,2: 卖出开空,3: 买入平空,4: 卖出平多,5: 卖出强平,6: 买入强平,7:交割平多,8: 交割平空|
  |type|true|int|   类型|1:所有订单,2:结束状态的订单|
  |status|    true|int|   订单状态|    0:全部,3:未成交, 4: 部分成交,5: 部分成交已撤单,6: 全部成交,7:已撤单|
  |create_date |  true|int|   日期|  7，90（7天或者90天）|
  |page_index  |  false|int|   |页码，不填默认第1页| 1| 
  |page_size|false|int|   |不填默认20，不得多于50   20|

**返回参数**

  |**参数名称**| **是否必须**   |**类型**   |**描述**|**取值范围**|
  |---------------------------- |-------------- |---------- |--------------------------------------------- |------------------------------------------------------|
  |status|  true|string|请求处理结果||  
  |\<object\>(属性名称: data)|||| 
  |\<list\>(属性名称: orders)|||| 
  |order_id|    true|long|  订单ID|  
  |symbol|  true|string|品种代码|
  |contract_type|    true|string|合约类型| 当周:"this_week", 次周:"next_week", 季度:"quarter"|
  |contract_code|    true|string|合约代码| "BTC180914" ...|
  |lever_rate|  true|int|   杠杆倍数| 1\\5\\10\\20|
  |direction|    true|string|"buy":买 "sell":卖||  
  |offset|  true|string|"open":开 "close":平||
  |volume|  true|decimal    |委托数量||
  |price|   true|decimal    |委托价格|| 
  |create_date| true|long|  创建时间|| 
  |order_source|true|string|订单来源|| 
  |order_price_type|true|string|1限价单，3对手价，4闪电平仓，5计划委托，6post_only |  
  |margin_frozen|    true|decimal    |冻结保证金||    
  |profit|  true|decimal    |收益||
  |trade_volume|true|decimal    |成交数量|| 
  |trade_turnover|   true|decimal    |成交总金额||    
  |fee||true|decimal    |手续费||   
  |trade_avg_price| true|decimal    |成交均价|| 
  |status|  true|int|   订单状态|| 
  |order_type|  true|int|   订单类型|1:报单 、 2:撤单 、 3:强平、4:交割|
  |\</list\>||||  
  |\</object\>||||
  |total_page|  true|int|   总页数||   
  |current_page|true|int|   当前页||   
  |total_size|  true|int|   总条数||  
  |ts|true|long|  时间戳||  

**示例**

    POST https://api.hbdm.com/api/v1/contract_hisorders
    # Response
    {
      "status": "ok",
      "data":{
        "orders":[
          {
            "symbol": "BTC",
            "contract_type": "this_week",
            "contract_code": "BTC180914",
            "volume": 111,
            "price": 1111,
            "order_price_type": "limit",
            "direction": "buy",
            "offset": "open",
            "lever_rate": 10,
            "order_id": 106837,
            "order_source": "web",
            "created_at": 1408076414000,
            "trade_volume": 1,
            "trade_turnover": 1200,
            "fee": 0,
            "trade_avg_price": 10,
            "margin_frozen": 10,
            "profit": 10,
            "status": 1,
            "order_type": 1
          }
         ],
        "total_page":15,
        "current_page":3,
        "total_size":3
        },
      "ts": 1490759594752
    }

#### 获取历史成交记录

URL api/v1/contract_matchresults

**请求参数**

| **参数名称**    | **是否必须** | **类型** | **描述**        | **默认值** | **取值范围**                                 |
| ----------- | -------- | ------ | ------------- | ------- | ---------------------------------------- |
| symbol      | true     | string | 品种代码          |         | "BTC","ETH"...                           |
| contract_code      | true     | string | 合约code          |         |                          |
| trade_type  | true     | int    | 交易类型          |         | 0:全部,1:买入开多,2: 卖出开空,3: 买入平空,4: 卖出平多,5: 卖出强平,6: 买入强平 |
| create_date | true     | int    | 日期            |         | 7，90（7天或者90天）                            |
| page_index  | false    | int    | 页码，不填默认第1页    | 1       |                                          |
| page_size   | false    | int    | 不填默认20，不得多于50 | 20      |                                          |

**返回参数**
 
| **参数名称**               | **是否必须** | **类型**  | **描述**             | **取值范围**     |
| ---------------------- | -------- | ------- | ------------------ | ------------ |
| status                 | true     | string  | 请求处理结果             |              |
| \<object\>(属性名称: data) |          |         |                    |              |
| \<list\>(属性名称: trades) |          |         |                    |              |
| match_id               | true     | long    | 成交ID               |              |
| order_id               | true     | long    | 订单ID               |              |
| symbol                 | true     | string  | 品种代码               |              |
| contract_type          | true     | string  | 合约类型               |当周:"this_week", 次周:"next_week", 季度:"quarter" |
| contract_code          | true     | string  | 合约代码               |"BTC180914" ...       |
| direction              | true     | string  | "buy":买 "sell":卖         |              |
| offset                 | true     | string  | "open":开 "close":平           |              |
| trade_volume           | true     | decimal | 成交数量               |              |
| trade_price                  | true     | decimal | 成交价格               |              |
| trade_turnover                  | true     | decimal | 成交总金额               |              |
| create_date            | true     | long    | 成交时间               |              |
| offset_profitloss                 | true     | decimal | 平仓盈亏                 |              |
| traded_fee                    | true     | decimal | 成交手续费                |              |
|role|   true|string|  taker或maker||| 
| \</list\>              |          |         |                    |              |
| total_page             | true     | int     | 总页数                |              |
| current_page           | true     | int     | 当前页                |              |
| total_size             | true     | int     | 总条数                |              |
| \</object\>            |          |         |                    |              |
| ts                     | true     | long    | 时间戳                |              |

**备注**

- 如果不传page_index和page_size，默认只查第一页的20条数据，详情请看参数说明；
获取该用户在某品种上的最新成交记录，可以按照交易类型进行过滤筛选。注意，该接口是需要API KEY验签的私有接口，只能查询属于该用户自己的最新成交记录

**示例**

    POST https://api.hbdm.com/api/v1/contract_matchresults
    
 # Response
    {
     "data": {
		"current_page": 1,
		"total_page": 1,
		"total_size": 2,
		"trades": [{
			"contract_code": "EOS190419",
			"contract_type": "this_week",
			"create_date": 1555553626736,
			"direction": "sell",
			"match_id": 3635853382,
			"offset": "close",
			"offset_profitloss": 0.15646398812252696,
			"order_id": 1118,
			"symbol": "EOS",
			"trade_fee": -0.002897500905469032,
			"trade_price": 5.522,
			"trade_turnover": 80,
			"role": "maker",
			"trade_volume": 8
		}]
	},
	"status": "ok",
	"ts": 1555654870867
    }

#### 币币账户和合约账户间进行资金的划转

URL https://api.huobi.pro/v1/futures/transfer

此接口用户币币现货账户与合约账户之间的资金划转。

从现货现货账户转至合约账户，类型为`pro-to-futures`; 从合约账户转至现货账户，类型为`futures-to-pro`

该接口的访问频次的限制为1分钟10次。

**请求参数**

  |**参数名称**   |**是否必须**   |**类型**   |**描述**| **默认值**   |**取值范围**|
  |--------------| --------------| ---------- |------------------------| ------------| ------------------------------------------------------------------------------------------------------|
  |currency|    true|string|币种|   e.g. btc|
  |amount |   true|Decimal|   划转金额|    |
  |type|true|string|   划转类型|从合约账户到现货账户：“futures-to-pro”，从现货账户到合约账户： “pro-to-futures”|

**备注**
注意请求地址为火币Global地址

**返回参数**

  |**参数名称**| **是否必须**   |**类型**   |**描述**|**取值范围**|
  |---------------------------- |-------------- |---------- |--------------------------------------------- |------------------------------------------------------|
  |status|  true|string|状态|ok, error|  
  |data|    true|long|  生成的划转订单id|  |
  |err-code|  true|string|错误码|具体错误码请见列表|
  |err-msg|    true|string|错误消息|具体错误码请见列表|


**示例**

    POST https://api.huobi.pro/v1/futures/transfer
    # Response
    正确的返回
	{
	"status": "ok",
	"data":56656,
    }
	错误的返回
	{
	"status": "error",
	"data":null,
	"err-code":"dw-account-transfer-error",
	"err-msg":"由于其他服务不可用导致的划转失败"
    }
##### err-code列表

err-code | err-msg(中文） | err-msg(Englis)|补充说明
------ | ------- | -----|-------------
|base-msg|||其他错误，具体的err-msg, 请参照对应的错误消息列表。
|base-currency-error|币种无效|The currency is invalid|
|frequent-invoke|操作过于频繁，请稍后重试。（如果超过1分钟10次，系统返回该error-code）|the operation is too frequent. Please try again later|如果请求次数超过1分钟10次，系统返回该error-code
|banned-by-blacklist|黑名单限制|Blacklist restriction|
|dw-insufficient-balance|可划转余额不足，最大可划转 {0}。（币币账户的余额不足。）|Insufficient balance. You can only transfer {0} at most.|币币账户的余额不足。
|dw-account-transfer-unavailable|转账暂时不可用|account transfer unavailable|该接口暂时不可用
|dw-account-transfer-error|由于其他服务不可用导致的划转失败|account transfer error|
|dw-account-transfer-failed|划转失败。请稍后重试或联系客服 |Failed to transfer. Please try again later.|由于系统异常导致的划转失败
|dw-account-transfer-failed-account-abnormality|账户异常，划转失败。请稍后重试或联系客服|Account abnormality, failed to transfer。Please try again later.|

##### base-msg对应的err-msg列表
err-code | err-msg(中文） | err-msg(Englis)|补充说明
------ | ------- | -----|-------------
|base-msg|用户没有入金权限|Unable to transfer in currently. Please contact customer service.|
|base-msg|用户没有出金权限|Unable to transfer out currently. Please contact customer service.|
|base-msg|合约状态异常，无法出入金|Abnormal contracts status. Can’t transfer.|
|base-msg|子账号没有入金权限，请联系客服|Sub-account doesn't own the permissions to transfer in. Please contact customer service.|
|base-msg|子账号没有出金权限，请联系客服|Sub-account doesn't own the permissions to transfer out. Please contact customer service.|
|base-msg|子账号没有划转权限，请登录主账号授权|The sub-account does not have transfer permissions. Please login main account to authorize.|
|base-msg|可划转余额不足|Insufficient amount available.|合约账户的余额不足
|base-msg|单笔转出的数量不能低于{0}{1}|The single transfer-out amount must be no less than {0}{1}.|
|base-msg|单笔转出的数量不能高于{0}{1}|The single transfer-out amount must be no more than {0}{1}.|
|base-msg|单笔转入的数量不能低于{0}{1}|The single transfer-in amount must be no less than {0}{1}.|
|base-msg|单笔转入的数量不能高于{0}{1}|The single transfer-in amount must be no more than {0}{1}.|
|base-msg|您当日累计转出量超过{0}{1}，暂无法转出|Your accumulative transfer-out amount is over the daily maximum, {0}{1}. You can't transfer out for the time being.|
|base-msg|您当日累计转入量超过{0}{1}，暂无法转入|Your accumulative transfer-in amount is over the daily maximum, {0}{1}. You can't transfer in for the time being.|
|base-msg|您当日累计净转出量超过{0}{1}，暂无法转出|Your accumulative net transfer-out amount is over the daily maximum, {0}{1}. You can't transfer out for the time being.|
|base-msg|您当日累计净转入量超过{0}{1}，暂无法转入|Your accumulative net transfer-in amount is over the daily maximum, {0}{1}. You can't transfer in for the time being.|
|base-msg|超过平台当日累计最大转出量限制，暂无法转出|The platform's accumulative transfer-out amount is over the daily maximum. You can't transfer out for the time being.|
|base-msg|超过平台当日累计最大转入量限制，暂无法转入|The platform's accumulative transfer-in amount is over the daily maximum. You can't transfer in for the time being.|
|base-msg|超过平台当日累计最大净转出量限制，暂无法转出|The platform's accumulative net transfer-out amount is over the daily maximum. You can't transfer out for the time being.|
|base-msg|超过平台当日累计最大净转入量限制，暂无法转入|The platform's accumulative net transfer-in amount is over the daily maximum. You can't transfer in for the time being.|
|base-msg|划转失败，请稍后重试或联系客服|Transfer failed. Please try again later or contact customer service.|
|base-msg|服务异常，划转失败，请稍后再试|Abnormal service, transfer failed. Please try again later.|
|base-msg|您尚未开通合约交易，无访问权限|You don’t have access permission as you have not opened contracts trading.|
|base-msg|合约品种不存在|This contract type doesn't exist.|没有相应币种的合约



##### 查询母账户下所有子账户资产信息
**请求参数**
URL：api/v1/contract_sub_account_list

| **参数名称**    | **是否必须** | **类型** | **描述**        | **默认值** | **取值范围**                                 |
| ----------- | -------- | ------ | ------------- | ------- | ---------------------------------------- |
| symbol      | false     | string | 品种代码          |         | "BTC","ETH"...  ,如果缺省，默认返回所有品种                         |

**返回参数**
 
| **参数名称**               | **是否必须** | **类型**  | **描述**             | **取值范围**     |
| ---------------------- | -------- | ------- | ------------------ | ------------ |
| status | true | string | 请求处理结果	 | "ok" , "error" |
| ts | true  | long | 响应生成时间点，单位：毫秒 |  |
| <data> |  |  |  |  |
| sub_uid | true  | long | 子账户UID |  |
| <list> |  |  |  |  |
| symbol | true | string | 品种代码 | "BTC","ETH"... |
| margin_balance | true | decimal | 账户权益 |  |
| liquidation_price | true | decimal | 预估强平价 |  |
| risk_rate | true | decimal | 保证金率 |  |
| </list> |  |  |  |  |
| </data> |  |  |  |  |

**返回示例**

```
{
        "status": "ok",
        "ts": 1499223904680,
	"data": [
	  {
	    "sub_uid": 9910049,
	    "list": [
              {
	         "symbol": "BTC",
                 "margin_balance": 1,
                 "liquidation_price": 100,
	         "risk_rate": 100,
	      },
	      {
	         "symbol": "ETH",
                 "margin_balance": 1,
                 "liquidation_price": 100,
	         "risk_rate": 100,
	      }
	      ]
	  },
          {
	    "sub_uid": 9910048,
	    "list": [
              {
	         "symbol": "BTC",
                 "margin_balance": 1,
                 "liquidation_price": 100,
	         "risk_rate": 100,
	      },
	      {
	         "symbol": "ETH",
                 "margin_balance": 1,
                 "liquidation_price": 100,
	         "risk_rate": 100,
	      }
	      ]
          }
	]
}
```
**备注**

 只返回已经开通合约交易的子账户数据.


##### 查询单个子账户资产信息

- URL：api/v1/contract_sub_account_info

**请求参数**


| **参数名称**    | **是否必须** | **类型** | **描述**        | **默认值** | **取值范围**                                 |
| ----------- | -------- | ------ | ------------- | ------- | ---------------------------------------- |
| symbol | false | string | 品种代码	 | "BTC","ETH"...，如果缺省，默认返回所有品种 |
| sub_uid | true | long | 子账户的UID	 |  |

**返回参数**

| **参数名称**               | **是否必须** | **类型**  | **描述**             | **取值范围**     |
| ---------------------- | -------- | ------- | ------------------ | ------------ |
| status | true | string | 请求处理结果	 | "ok" , "error" |
| ts                       | true | long | 响应生成时间点，单位：毫秒 |  |
| <data> |  |  |  |  |
| symbol                  | true     | string  | 品种代码               | "BTC","ETH"...，当 $symbol值为 * 时代表订阅所有品种 |
| margin_balance                  | true     | decimal  | 账户权益               |                |
| margin_position                 | true     | decimal  | 持仓保证金（当前持有仓位所占用的保证金）               |                |
| margin_frozen                 | true     | decimal  | 冻结保证金               |                |
| margin_available                 | true     | decimal  | 可用保证金               |                |
| profit_real                 | true     | decimal  | 已实现盈亏               |                |
| profit_unreal                 | true     | decimal  | 未实现盈亏               |                |
| risk_rate                 | true     | decimal  | 保证金率               |                |
| liquidation_price                | true     | decimal  | 预估爆仓价               |                |
| withdraw_available                | true     | decimal  | 可划转数量               |                |
| lever_rate                | true     | decimal  | 杠杆倍数               |                |
| adjust_factor                | true     | decimal  | 调整系数               |                |
| </data> |  |  |  |  |

**返回示例**
```
{
  "status": "ok",
  "data":  [ 
     {
        "symbol": "BTC",
        "margin_balance": 1,
        "margin_position": 0,
        "margin_frozen": 3.33,
        "margin_available": 0.34,
        "profit_real": 3.45,
        "profit_unreal": 7.45,
        "withdraw_available":4.0989898,
        "risk_rate": 100,
        "liquidation_price": 100,
		"adjust_factor": 0.1
      }
    ],
  "ts":158797866555
}
```
**备注**

   只能查询到开通合约交易的子账户信息；
  
   子账户来过合约系统但是未开通合约交易也不返回对应的数据；

##### 查询单个子账户持仓信息的

- URL：api/v1/contract_sub_position_info

**请求参数**

| **参数名称**    | **是否必须** | **类型** | **描述**        | **默认值** | **取值范围**                                 |
| ----------- | -------- | ------ | ------------- | ------- | ---------------------------------------- |
| symbol | false | string | 品种代码	 | "BTC","ETH"...，如果缺省，默认返回所有品种 |
| sub_uid | true | long | 子账户的UID	 |  |


**返回参数**

| **参数名称**               | **是否必须** | **类型**  | **描述**             | **取值范围**     |
| ---------------------- | -------- | ------- | ------------------ | ------------ |
| status | true | string | 请求处理结果	 | "ok" , "error" |
| ts                       | true | long | 响应生成时间点，单位：毫秒 |  |
| <data> |  |  |  |  |
| symbol                  | true     | string  | 品种代码               | "BTC","ETH"... |
| contract_code                | true     | string  |  合约代码             | "BTC180914" ... |
| contract_type                | true     | string  | 合约类型              | 当周:"this_week", 次周:"next_week", 季度:"quarter" |
| volume                | true     | decimal	  |  持仓量             |  |
| available               | true     | decimal	  | 可平仓数量              |  |
| frozen               | true     | decimal	  |  冻结数量             |  |
| cost_open               | true     | decimal	  |  开仓均价             |  |
| cost_hold               | true     | decimal	  | 持仓均价              |  |
| profit_unreal               | true     | decimal	  | 未实现盈亏              |  |
| profit_rate               | true     | decimal	  | 收益率              |  |
| profit               | true     | decimal	  | 收益              |  |
| position_margin               | true     | decimal	  | 持仓保证金              |  |
| lever_rate               | true     | int	  | 杠杆倍数              |  |
| direction               | true     | string	  |   仓位方向           |  "buy":多 "sell":空 |
| </data> |  |  |  |  |

**返回示例**
```
{
  "status": "ok",
  "ts": 158797866555
  "data":[ 
     {
         "symbol": "BTC",
         "contract_code": "BTC180914",
         "contract_type": "this_week",
         "volume": 1,
         "available": 0,
         "frozen": 0.3,
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

##### 查询用户财务记录

 URL：api/v1/contract_financial_record
 
**请求参数**

| **参数名称**                | **是否必须** | **类型**  | **描述**             | **取值范围**       |
| ----------------------- | -------- | ------- | ------------------ | -------------- |
| symbol | true | string | 品种代码	 | "BTC","ETH"... |
| type | false | string | 不填查询全部类型,【查询多类型中间用，隔开】 | 平多：3，平空：4，开仓手续费-吃单：5，开仓手续费-挂单：6，平仓手续费-吃单：7，平仓手续费-挂单：8，交割平多：9，交割平空：10，交割手续费：11，强制平多：12，强制平空：13，从币币转入：14，转出至币币：15，结算未实现盈亏-多仓：16，结算未实现盈亏-空仓：17，穿仓分摊：19，系统：26，活动奖励：28，返利：29 |
| create_date | false | int | 7，90 (7天 ，90天) ，不填默认为7|  |
| page_index | false | int | 第几页,不填默认第一页 |  |
| page_size | false | int | 不填默认20，不得多于50 |  |

**返回参数**

| **参数名称**                | **是否必须** | **类型**  | **描述**             | **取值范围**       |
| ----------------------- | -------- | ------- | ------------------ | -------------- |
| status | true | string | 请求处理结果	 | "ok" , "error" |
| ts | true  | long | 响应生成时间点，单位：毫秒 |  |
| <data> |  |  | 字典类型 |  |
| <financial_record> |  |  |  |  |
| id | true  | long |  |  |
| ts | true  | long | 创建时间 |  |
| symbol | true  | string | 品种代码 | "BTC","ETH"... |
| type | true  | int | 交易类型 | 平多：3，平空：4，开仓手续费-吃单：5，开仓手续费-挂单：6，平仓手续费-吃单：7，平仓手续费-挂单：8，交割平多：9，交割平空：10，交割手续费：11，强制平多：12，强制平空：13，从币币转入：14，转出至币币：15，结算未实现盈亏-多仓：16，结算未实现盈亏-空仓：17，穿仓分摊：19，系统：26，活动奖励：28，返利：29 |
| amount | true  | decimal | 金额 |  |
| </financial_record> |  |  |  |  |
| total_page | true  | int | 总页数 |  |
| current_page | true  | int | 当前页 |  |
| total_size | true  | int | 总条数 |  |
| </data> |  |  |  |  |

**返回示例**
```
{
    	"status": "ok",
	"data":{
		"financial_record" : [
			{
			"id": 192838272,
			"ts": 1408076414000,
			"symbol":"BTC",
		        "type":1, 
			"amount":1,
			},
			{
				.........
			}
		],
	       "total_page":15,
	       "current_page":3，
	       "total_size":3
        },
	"ts": 1490759594752
}
```

##### 查询用户当前的下单量限制

 URL：api/v1/contract_order_limit
 
 
**请求参数**

| **参数名称**                | **是否必须** | **类型**  | **描述**             | **取值范围**       |
| ----------------------- | -------- | ------- | ------------------ | -------------- |
| symbol | false | string | 品种代码	 | "BTC","ETH"...，如果缺省，默认返回所有品种 |
| order_price_type | true  | string | 订单报价类型 | "limit":限价，"opponent":对手价，"lightning":闪电平仓，"optimal_5":最优5档，"optimal_10":最优10档，"optimal_20":最优20档 |

**返回参数**

| **参数名称**                | **是否必须** | **类型**  | **描述**             | **取值范围**       |
| ----------------------- | -------- | ------- | ------------------ | -------------- |
| status | true | string | 请求处理结果	 | "ok" , "error" |
| ts | true  | long | 响应生成时间点，单位：毫秒 |  |
| <data> |  |  |  |  |    
| order_price_type | true  | string | 订单报价类型 | "limit":限价，"opponent":对手价，"lightning":闪电平仓，"optimal_5":最优5档，"optimal_10":最优10档，"optimal_20":最优20档 |
| <list> |  |  |  |  |
| symbol | true  | string | 品种代码 | "BTC","ETH"... |
| <types> |  |  |  |  |
| contract_type| true | string | 合约类型 | 当周:"this_week", 次周:"next_week", 季度:"quarter" |
| open_limit | true | long | 合约开仓单笔下单量最大值 |  |
| close_limit | true | long | 合约平仓单笔下单量最大值 |  |
| </type|  |  |  |  |
| </list|  |  |  |  |
| </data> |  |  |  |  |



**返回示例**
```
{
  "status": "ok",
  "data":  {
      "order_price_type": "limit",
      "list":[
      {
          "symbol": "BTC",
          "types": [
             {
              "contract_type": "this_week",
              "open_limit": 3000,
              "close_limit": 3000,
             },
             {
              "contract_type": "next_week",
              "open_limit": 3000,
              "close_limit": 3000,
             },     
             {
              "contract_type": "quarter",
              "open_limit": 3000,
              "close_limit": 3000,
             }
           ]
      }
      ]
   }
 "ts": 158797866555
}

```

##### 查询用户当前的手续费费率

 URL：api/v1/contract_fee
 
 
**请求参数**

| **参数名称**                | **是否必须** | **类型**  | **描述**             | **取值范围**       |
| ----------------------- | -------- | ------- | ------------------ | -------------- |
| symbol | false | string | 品种代码	 | "BTC","ETH"...，如果缺省，默认返回所有品种 |

**返回参数**

| **参数名称**                | **是否必须** | **类型**  | **描述**             | **取值范围**       |
| ----------------------- | -------- | ------- | ------------------ | -------------- |
| status | true | string | 请求处理结果	 | "ok" , "error" |
| ts | true  | long | 响应生成时间点，单位：毫秒 |  |
| <data> |  |  |  |  |
| symbol | true  | string | 品种代码 | "BTC","ETH"... |
| open_maker_fee | true | string | 开仓挂单的手续费费率，小数形式 | |
| open_taker_fee | true | string | 开仓吃单的手续费费率，小数形式 | |
| close_maker_fee | true | string | 平仓挂单的手续费费率，小数形式 | |
| close_taker_fee | true | string | 平仓吃单的手续费费率，小数形式 | |
| delivery_fee | true | string | 交割的手续费费率，小数形式 | |
| </data|  |  |  |  |




**返回示例**
```
POST  https://api.hbdm.com/api/v1/contract_fee
 # Response
{
  "status": "ok",
  "data": [
    {
      "symbol": "BTC",
      "open_maker_fee": "-0.00025",
      "open_taker_fee": "0.00075",
      "close_maker_fee": "-0.00025",
      "close_taker_fee": "0.00075",
      "delivery_fee": "0.0005",
    }
 ],
 "ts": 158797866555
}


```


##### 用户查询用户当前的划转限制

 URL：api/v1/contract_transfer_limit
 
 
**请求参数**

| **参数名称**                | **是否必须** | **类型**  | **描述**             | **取值范围**       |
| ----------------------- | -------- | ------- | ------------------ | -------------- |
| symbol | false | string | 品种代码	 | "BTC","ETH"...，如果缺省，默认返回所有品种 |

**返回参数**

| **参数名称**                | **是否必须** | **类型**  | **描述**             | **取值范围**       |
| ----------------------- | -------- | ------- | ------------------ | -------------- |
| status | true | string | 请求处理结果	 | "ok" , "error" |
| ts | true  | long | 响应生成时间点，单位：毫秒 |  |
| <data> |  |  |  |  |
| symbol | true  | string | 品种代码 | "BTC","ETH"... |
| transfer_in_max_each | true | decimal | 单笔最大转入量 |  |
| transfer_in_min_each | true | decimal | 单笔最小转入量 |  |
| transfer_out_max_each | true | decimal | 单笔最大转出量 |  |
| transfer_out_min_each | true | decimal | 单笔最小转出量 |  |
| transfer_in_max_daily | true | decimal | 单日累计最大转入量 |  |
| transfer_out_max_daily | true | decimal | 单日累计最大转出量 |  |
| net_transfer_in_max_daily | true | decimal | 单日累计最大净转入量 |  |
| net_transfer_out_max_daily | true | decimal | 单日累计最大净转出量 |  |
| </data|  |  |  |  |


**返回示例**
```
POST  https://api.hbdm.com/api/v1/contract_position_limit
 # Response
{
  "status": "ok",
  "data": [
    {
      "symbol": “BTC”,
      "transfer_in_max_each": 5000,
      "transfer_in_min_each": 5000,
      "transfer_out_max_each": 5000,
      "transfer_out_min_each": 5000,
      "transfer_in_max_daily": 5000,
      "transfer_out_max_daily": 5000,
      "net_transfer_in_max_daily": 5000,
      "net_transfer_out_max_daily": 5000,
    }
 ],
 "ts": 158797866555
}



```


##### 用户持仓量限制的查询

 URL：api/v1/contract_position_limit
 
 
**请求参数**

| **参数名称**                | **是否必须** | **类型**  | **描述**             | **取值范围**       |
| ----------------------- | -------- | ------- | ------------------ | -------------- |
| symbol | false | string | 品种代码	 | "BTC","ETH"...，如果缺省，默认返回所有品种 |


**返回参数**

| **参数名称**                | **是否必须** | **类型**  | **描述**             | **取值范围**       |
| ----------------------- | -------- | ------- | ------------------ | -------------- |
| status | true | string | 请求处理结果	 | "ok" , "error" |
| ts | true  | long | 响应生成时间点，单位：毫秒 |  |
| <data> |  |  |  |  |
| symbol | true  | string | 品种代码 | "BTC","ETH"... |
| <list> |  |  |  |  |
| contract_type| true | string | 合约类型 | 当周:"this_week", 次周:"next_week", 季度:"quarter"，所有合约:“all” |
| buy_limit | true | decimal | 合约多仓持仓的最大值，单位为张 |  |
| sell_limit | true | decimal | 合约空仓持仓的最大值，单位为张 |  |
| </list> |  |  |  |  |
| </data> |  |  |  |  |




**返回示例**
```
POST  https://api.hbdm.com/api/v1/contract_position_limit
 # Response
{
  "status": "ok",
  "data": [
    {
      "symbol": "BTC",
      "list": [
        {
         "contract_type": "all",
         "buy_limit": 5000,
         "sell_limit": 5000,
       },
        {
         "contract_type": "this_week",
         "buy_limit": 3000,
         "sell_limit": 3000,
       },
        {
         "contract_type": "next_week",
         "buy_limit": 3000,
         "sell_limit": 3000,
       },     
        {
         "contract_type": "quarter",
         "buy_limit": 3000,
         "sell_limit": 3000,
       }
       ]
    }
 ],
 "ts": 158797866555
}

```

##### 查询合约风险准备金余额和预估分摊比例

 URL：api/v1/contract_risk_info

 
**请求参数**

| **参数名称**                | **是否必须** | **类型**  | **描述**             | **取值范围**       |
| ----------------------- | -------- | ------- | ------------------ | -------------- |
| symbol | false | string | 品种代码	 | "BTC","ETH"...，如果缺省，默认返回所有品种 |


**返回参数**

| **参数名称**                | **是否必须** | **类型**  | **描述**             | **取值范围**       |
| ----------------------- | -------- | ------- | ------------------ | -------------- |
| status | true | string | 请求处理结果	 | "ok" , "error" |
| ts | true  | long | 响应生成时间点，单位：毫秒 |  |
| <data> |  |  |  |  |
| symbol | true  | string | 品种代码 | "BTC","ETH"... |
| insurance_fund | true  | decimal | 风险准备金余额 |  |
| estimated_clawback | true  | decimal | 预估分摊比例 |  |
| </data> |  |  |  |  |




**返回示例**
```
GET  https://api.hbdm.com/api/v1/contract_risk_info
 # Response
{
  "status": "ok",
  "ts": 158797866555,
  "data": [
    {
      "symbol": "ETH",
      "insurance_fund": 3806.4615259197324414715719,
      "estimated_clawback": 0.0023,
    }
  ]
}


```


##### 查询合约风险准备金余额历史数据

 URL：api/v1/contract_insurance_fund

 
**请求参数**

| **参数名称**                | **是否必须** | **类型**  | **描述**             | **取值范围**       |
| ----------------------- | -------- | ------- | ------------------ | -------------- |
| symbol | true | string | 品种代码	 | "BTC","ETH"... |


**返回参数**

| **参数名称**                | **是否必须** | **类型**  | **描述**             | **取值范围**       |
| ----------------------- | -------- | ------- | ------------------ | -------------- |
| status | true | string | 请求处理结果	 | "ok" , "error" |
| ts | true  | long | 响应生成时间点，单位：毫秒 |  |
| <data> |  |  |  | 字典数据 |
| symbol | true  | string | 品种代码 | "BTC","ETH"... |
| < tick > |  |  |  |  |
| insurance_fund | true  | decimal | 风险准备金余额 |  |
| ts | true  | long | 数据时间点，单位：毫秒 |  |
| </tick > |  |  |  |  |
| </data> |  |  |  |  |





**返回示例**
```
GET  https://api.hbdm.com/api/v1/contract_insurance_fund
 # Response
{
  "status": "ok",
  "ts": 158797866555,
  "data":   {
      "symbol": "ETH",
      "tick": [
        {
          "insurance_fund": 3806.4615259197324414715719,
          "ts": 158797866555,
         }
       ]
  }
}


```

##### 查询平台阶梯调整系数

 URL：api/v1/contract_adjustfactor

 
**请求参数**

| **参数名称**                | **是否必须** | **类型**  | **描述**             | **取值范围**       |
| ----------------------- | -------- | ------- | ------------------ | -------------- |
| symbol | false | string | 品种代码	 | "BTC","ETH"...，如果缺省，默认返回所有品种 |


**返回参数**

| **参数名称**                | **是否必须** | **类型**  | **描述**             | **取值范围**       |
| ----------------------- | -------- | ------- | ------------------ | -------------- |
| status | true | string | 请求处理结果	 | "ok" , "error" |
| ts | true  | long | 响应生成时间点，单位：毫秒 |  |
| <data> |  |  |  |  |
| symbol | true  | string | 品种代码 | "BTC","ETH"... |
| <list> |  |  |  |  |
| lever_rate   | true     | decimal  | 杠杆倍数               |                |
| <ladderDetail> |  |  |  |  |
| min_size | true | decimal | 净持仓量的最小值 |  |
| max_size | true | decimal | 净持仓量的最大值 |  |
| ladder | true | int | 档位 |  |
| adjust_factor | true | decimal | 调整系数 |  |
| </ladderDetail> |  |  |  |  |
| </list> |  |  |  |  |
| </data> |  |  |  |  |


**返回示例**
```
GET  https://api.hbdm.com/api/v1/contract_adjustfactor
 # Response
{
  "status": "ok",
  "data": [
   {
      "symbol": "BTC",
      "list": [
       {
          "lever_rate": 10,
          "ladders": [
           {
             "ladder": 1,
             "min_size": 0,
             "max_size": 100,
             "adjust_factor": 0.1,
           },
           {
             "ladder": 2,
             "min_size": 101,
             "max_size": 500,
             "adjust_factor": 0.2,
           }
           ]
       }
       ]
   }
   ],
   "ts": 158797866555
}

```

##### 闪电平仓下单

 URL：/v1/lightning_close_position

 
**请求参数**

| **参数名称**                | **是否必须** | **类型**  | **描述**             | **取值范围**       |
| ----------------------- | -------- | ------- | ------------------ | -------------- |
| symbol | false | String | 品种代码	 | "BTC","ETH"... |
| contract_type | false | String | 合约类型 | “this_week”:当周，“next_week”:次周，“quarter”:季度|
| contract_code | false | String | 合约代码 | BTC190903 |
| volume | true | Number | 委托数量（张） |  |
| direction | true | String | “buy”:买，“sell”:卖 |  |
| client_order_id | false | Number | （API）客户自己填写和维护，必须保持唯一 |  |



**返回参数**

| **参数名称**                | **是否必须** | **类型**  | **描述**             | **取值范围**       |
| ----------------------- | -------- | ------- | ------------------ | -------------- |
| status | true | string | 请求处理结果	 | "ok" :成功, "error"：失败 |
| ts | true  | long | 响应生成时间点，单位：毫秒 |  |
| <data> |  |  |  | 字典 |
| order_id | true  | Number | 订单ID[用户级别的，不同的用户order_id可能相同] |  |
| client_order_id | false | Number | 用户自己的订单id |  |
| </data> |  |  |  |  |



**返回示例**
```
POST http://www.huobiapps.com/contract-order/v1/lightning_close_position

- Response
{
  "status": "ok",
  "data": {
    "order_id": 986,
    "client_order_id": 9086
  }
     "ts": 158797866555
}

错误：
{
    "status": "error"，
    "err_code": 20012，
    "err_msg": "invalid symbol"，
    "ts": 1490759594752
}



```