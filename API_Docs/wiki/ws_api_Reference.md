# WebSocket API Reference

## 订阅 KLine 数据 market.$symbol.kline.$period

成功建立和 WebSocket API 的连接之后，向 Server 发送如下格式的数据来订阅数据：
```
{
  "sub": "market.$symbol.kline.$period",
  "id": "id generate by client"
}
```

| 参数名称         | 是否必须  | 类型     | 描述                | 默认值   | 取值范围                                     |
| ------------ | ----- | ------ | ----------------- | ----- | ---------------------------------------- |
| symbol       | true  | string | 交易对                |       | ethbtc, ltcbtc, etcbtc, bchbtc......以下新的symbol会在生效日当天可以请求：huobi10 - 指数，hb10 - ETF净值 |
| period       | true  | string | K线周期          |       | 1min, 5min, 15min, 30min, 60min, 1day, 1mon, 1week, 1year |

**正确订阅的例子**

正确订阅
```
{
  "sub": "market.btcusdt.kline.1min",
  "id": "id1"
}
```

订阅成功返回数据的例子
```
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcusdt.kline.1min",
  "ts": 1489474081631
}
```

之后每当 KLine 有更新时，client 会收到数据，例子
```json
{
  "ch": "market.btcusdt.kline.1min",
  "ts": 1489474082831,
  "tick": {
    "id": 1489464480,
    "amount": 0.0,
    "count": 0,
    "open": 7962.62,
    "close": 7962.62,
    "low": 7962.62,
    "high": 7962.62,
    "vol": 0.0
  }
}
```

tick 说明
```
  "tick": {
    "id": K线id,
    "amount": 成交量,
    "count": 成交笔数,
    "open": 开盘价,
    "close": 收盘价,当K线为最晚的一根时，是最新成交价
    "low": 最低价,
    "high": 最高价,
    "vol": 成交额, 即 sum(每一笔成交价 * 该笔的成交量)
  }

```

**错误订阅的例子**

错误订阅(错误的 symbol)
```
{
  "sub": "market.invalidsymbol.kline.1min",
  "id": "id2"
}
```

订阅失败返回数据的例子
```json
{
  "id": "id2",
  "status": "error",
  "err-code": "bad-request",
  "err-msg": "invalid topic market.invalidsymbol.kline.1min",
  "ts": 1494301904959
}
```

错误订阅(错误的 topic)
```
{
  "sub": "market.btcusdt.kline.3min",
  "id": "id3"
}
```

订阅失败返回数据的例子
```json
{
  "id": "id3",
  "status": "error",
  "err-code": "bad-request",
  "err-msg": "invalid topic market.btcusdt.kline.3min",
  "ts": 1494310283622
}
```

## 请求 KLine 数据 market.$symbol.kline.$period  

```
{
  "req": "market.$symbol.kline.$period",
  "id": "id generated by client",
  "from": 1533536947, //optional, type: long, 2017-07-28T00:00:00+08:00 至 2050-01-01T00:00:00+08:00 之间的时间点，单位：秒
  "to": 1533536947 //optional, type: long, 2017-07-28T00:00:00+08:00 至 2050-01-01T00:00:00+08:00 之间的时间点，单位：秒，必须比 from 大
}
```

| 参数名称         | 是否必须  | 类型     | 描述                | 默认值   | 取值范围                                     |
| ------------ | ----- | ------ | ----------------- | ----- | ---------------------------------------- |
| symbol       | true  | string | 交易对                |       | btcusdt, ethusdt, ltcusdt, etcusdt, bchusdt, ethbtc, ltcbtc, etcbtc, bchbtc... 以下新的symbol会在生效日当天可以请求：huobi10 - 指数，hb10 - ETF净值|
| period       | true  | string | K线周期          |       | 1min, 5min, 15min, 30min, 60min, 1day, 1mon, 1week, 1year |

##### 关于ETF请求参数的说明

Symbol|说明
--|--|
huobi10|指数|
hb10| ETF净值|

其他请求参数`period`,`from`,`to`保持不变。

* 响应结果

响应结果的结构保持不变，唯一的区别是响应结果中的amount, count 和 vol都是0，因为在这里的amount, count 和 vol是交易的相关的 值，而指数和ETF净值没有相应的数据。Houbi10 指数和 Hb10净值每15秒更新一次。 

`[t1, t5]`
假设有 t1 ~ t5 的K线.

* from: t1, to: t5, return [t1, t5].
* from: t5, to: t1, which t5 > t1, return [].
* from: t5, return [t5].
* from: t3, return [t3, t5].
* to: t5, return [t1, t5].
* from: t which t3 < t <t4, return [t4, t5].
* to: t which t3 < t <t4, return [t1, t3].
* from: t1 and to: t2, should satisfy 1501171200 < t1 < t2 < 2524579200.

```
一次最多300条
```

#### 请求 KLine 数据的例子

```
{
  "req": "market.btcusdt.kline.1min",
  "id": "id10"
}
```

返回数据的例子
```
{
  "rep": "market.btcusdt.kline.1min",
  "status": "ok",
  "id": "id10",
  "tick": [
    {
      "amount": 17.4805,
      "count":  27,
      "id":     1494478080,
      "open":   10050.00,
      "close":  10058.00,
      "low":    10050.00,
      "high":   10058.00,
      "vol":    175798.757708
    },
    {
      "amount": 15.7389,
      "count":  28,
      "id":     1494478140,
      "open":   10058.00,
      "close":  10060.00,
      "low":    10056.00,
      "high":   10065.00,
      "vol":    158331.348600
    },
    // more KLine data here
  ]
}
```

## 订阅 Market Depth 数据 market.$symbol.depth.$type 

成功建立和 WebSocket API 的连接之后，向 Server 发送如下格式的数据来订阅数据：
```
{
  "sub": "market.$symbol.depth.$type",
  "id": "id generated by client"
}
```

| 参数名称         | 是否必须  | 类型     | 描述                | 默认值   | 取值范围                                     |
| ------------ | ----- | ------ | ----------------- | ----- | ---------------------------------------- |
| symbol       | true  | string | 交易对                |       | btcusdt, ethusdt, ltcusdt, etcusdt, bchusdt, ethbtc, ltcbtc, etcbtc, bchbtc... |
| type         | true  | string | Depth 类型          |       | step0, step1, step2, step3, step4, step5（合并深度0-5）；step0时，不合并深度 |

*用户选择“合并深度”时，一定报价精度内的市场挂单将予以合并显示（具体合并规则见[GET /market/symbols](https://api.huobi.br.com/market/symbols)）。合并深度仅改变显示方式，不改变实际成交价。*

正确订阅例子
```
{
  "sub": "market.btcusdt.depth.step0",
  "id": "id1"
}
```

订阅成功返回数据的例子
```
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcusdt.depth.step0",
  "ts": 1489474081631
}
```

之后每当 depth 有更新时，client 会收到数据，例子
```
{
  "ch": "market.btcusdt.depth.step0",
  "ts": 1489474082831,
  "tick": {
    "bids": [
    [9999.3900,0.0098], // [price, amount]
    [9992.5947,0.0560],
    // more Market Depth data here
    ]
    "asks": [
    [10010.9800,0.0099]
    [10011.3900,2.0000]
    //more data here
    ]
  }
}
```

tick 说明
```
  "tick": {
    "bids": [
    [买1价,买1量]
    [买2价,买2量]
    //more data here
    ]
    "asks": [
    [卖1价,卖1量]
    [卖2价,卖2量]
    //more data here
    ]
  }
```

## 请求 Market Depth 数据 market.$symbol.depth.$type 

```
{
  "req": "market.$symbol.depth.$type",
  "id": "id generated by client"
}
```

#### 请求 Market Depth 数据的例子
```
{
  "req": "market.btcusdt.depth.step0",
  "id": "id10"
}
```

返回数据的例子：
```
{
  "rep": "market.btcusdt.depth.step0",
  "status": "ok",
  "id": "id10",
  "tick": {
    "bids": [
    [9999.3900,0.0098], // [price, amount]
    [9992.5947,0.0560],
    // more Market Depth data here
    ]
    "asks": [
    [10010.9800,0.0099]
    [10011.3900,2.0000]
    //more data here
    ]
  }
}
```

## 订阅 Trade Detail 数据 market.$symbol.trade.detail  

```
{
  "sub": "market.$symbol.trade.detail",
  "id": "id generated by client"
}
```

| 参数名称         | 是否必须  | 类型     | 描述                | 默认值   | 取值范围                                     |
| ------------ | ----- | ------ | ----------------- | ----- | ---------------------------------------- |
| symbol       | true  | string | 交易对                |       |btcusdt, ethusdt, ltcusdt, etcusdt, bchusdt, ethbtc, ltcbtc, etcbtc, bchbtc... |

正确订阅例子：

```
{
  "sub": "market.btcusdt.trade.detail",
  "id": "id1"
}
```

订阅成功返回数据的例子：
```
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcusdt.trade.detail",
  "ts": 1489474081631
}
```

之后每当 Trade Detail 有更新时，client 会收到数据，例子：

```
{
  "ch": "market.btcusdt.trade.detail",
  "ts": 1489474082831,
  "tick": {
        "id": 14650745135,
        "ts": 1533265950234,
        "data": [
            {
                "amount": 0.0099,
                "ts": 1533265950234,
                "id": 146507451359183894799,
                "price": 401.74,
                "direction": "buy"
            },
            // more Trade Detail data here
        ]
    }
  
  ]
  }
}
```

data 说明：
```
  "data": [
    {
      "id":        消息ID,
      "price":     成交价,
      "time":      成交时间,
      "amount":    成交量,
      "direction": 成交方向,
      "tradeId":   成交ID,
      "ts":        时间戳
    }
  ]
```

## 请求 Trade Detail 数据 market.$symbol.trade.detail 

```
{
  "req": "market.$symbol.trade.detail",
  "id": "id generated by client"
}
```

* 仅能获取最近 300 个 Trade Detail 数据

#### 请求 Trade Detail 数据的例子
```
{
  "req": "market.btcusdt.trade.detail",
  "id": "id11"
}
```

返回数据的例子：
```
{
  "rep": "market.btcusdt.trade.detail",
  "status": "ok",
  "id": "id11",
  "data": [
    {
      "id":        601595424,
      "price":     10195.64,
      "time":      1494495766,
      "amount":    0.2943,
      "direction": "buy",
      "tradeId":   601595424,
      "ts":        1494495766000
    },
    {
      "id":        601595423,
      "price":     10195.64,
      "time":      1494495711,
      "amount":    0.2430,
      "direction": "buy",
      "tradeId":   601595423,
      "ts":        1494495711000
    },
    // more Trade Detail data here
  ]
}
```

## 请求 Market Detail 数据 market.$symbol.detail 
```
{
  "req": "market.$symbol.detail",
  "id": "id generated by client"
}
```

* 仅返回当前 Market Detail

#### 请求 Market Detail 数据的例子

```
{
  "req": "market.btcusdt.detail",
  "id": "id12"
}
```

返回数据的例子：
```json
{
  "rep": "market.btcusdt.detail",
  "status": "ok",
  "id": "id12",
  "tick": {
    "amount": 12224.2922,
    "open":   9790.52,
    "close":  10195.00,
    "high":   10300.00,
    "ts":     1494496390000,
    "id":     1494496390,
    "count":  15195,
    "low":    9657.00,
    "vol":    121906001.754751
  }
}
```

## 订阅accounts
订阅accounts资产变动更新。

**订阅请求数据格式说明**

  |字段名称  | 类型   |  说明|
  |----------| -------- |----------------------------------------------|
  |op         |string   |必填；操作名称，订阅固定值为 sub；|
  |cid        |string   |选填；Client 请求唯一 ID|
  |topic      |string   |必填；订阅主题名称，详细主题列表请参考附录；|
  |model       |string|选填；订阅账户balance类型。0 代表可用，即type=trade的balance; 1 代表 total，即该账户的总余额，包括type=trade 和type=frozen的余额的和。当mode缺省时，默认值为0.|

_建议：如需同时订阅可用和总余额，需要为 0 和 1 各开启一条 websocket 连接，如果使用同一条连接，后订阅的 topic 会覆盖前一个 topic_

### 订阅请求示例
正确的订阅**可用余额**请求

    {
      "op": "sub",
      "cid": "40sG903yz80oDFWr",
      "topic": "accounts",
      "model": "0"
    }

订阅成功返回数据示例

    {
      "op": "sub",
      "cid": "40sG903yz80oDFWr",
      "err-code": 0,
      "ts": 1489474081631,
      "topic": "accounts"
    }

之后每当 Account **可用余额**有更新时，Client 会收到数据，比如，

    {
      "op": "notify",
      "ts": 1522856623232,
      "topic": "accounts",
      "data": {
        "event": "order.place",
        "list": [
          {
            "account-id": 419013,
            "currency": "usdt",
            "type": "trade",
            "balance": "500009195917.4362872650" (可用）
          }
        ]
      }
    }

正确的订阅**总余额**请求

    {
      "op": "sub",
      "cid": "40sG903yz80oDFWr",
      "topic": "accounts"
      "model": "1"
    }

订阅成功返回数据示例

    {
      "op": "sub",
      "cid": "40sG903yz80oDFWr",
      "err-code": 0,
      "ts": 1489474081631,
      "topic": "accounts"
    }

之后每当 Account **总余额**有更新时，即一个account id 下各类子账户的总余额， 括：交易子账户（trade)的总余额(可用+冻结），借贷子账户（loan），利息子账户（interest)。比如当用户交易下单时，交易子账户可用余额会发生变化，但是总余额不会有变化。

    {
      "op": "notify",
      "ts": 1522856623232,
      "topic": "accounts",
      "data": {
        "event": "order.match",
        "list": [
          {"account-id":4717043,
           "currency":"ada",
           "type":"trade",
           "balance":"45.576059983015876879" （可用+冻结）
          },
          {"account-id":4717043,
           "currency":"usdt",
           "type":"trade",
           "balance":"181.025751112881267723"} （可用+冻结）
          }
        ]
      }
    }
### Accounts变化通知数据格式说明

 | 名称     |    类型 |   说明|
|-------|-----|------|
  |event      |  string   |资产变化通知相关事件说明，比如订单创建(order.place) 、订单成交(order.match)、订单成交退款（order.refund)、订单撤销(order.cancel) 、点卡抵扣交易手续费（order.fee-refund)、杠杆账户划转（margin.transfer)、借贷本金（margin.loan)、借贷计息（margin.interest)、归还借贷本金利息(margin.repay)、其他资产变化(other)|
 | account-id   |long  |   账户 id|
  |currency   |  string |  币种|
  |type        | string  | 账户类型, 交易子账户（trade),借贷子账户（loan），利息子账户（interest)|
  |balance      |string   |账户余额 (当订阅mode=0时，该余额为**可用**余额；当订阅mode=1时，该余额为**总**余额）|

## 订阅orders.$symbol

订阅订单更新

**订阅请求数据格式说明**

  |字段名称  | 类型   |  说明|
  |----------| -------- |----------------------------------------------|
  |op         |string   |必填；操作名称，订阅固定值为 sub；|
  |cid        |string   |选填；Client 请求唯一 ID|
  |topic      |string   |必填；订阅主题名称，详细主题列表请参考附录；|

正确的订阅请求

    {
      "op": "sub",
      "cid": "40sG903yz80oDFWr",
      "topic": "orders.htusdt"
     
    }

订阅成功返回数据示例

    {
      "op": "sub",
      "cid": "40sG903yz80oDFWr",
      "err-code": 0,
      "ts": 1489474081631,
      "topic": "orders.htusdt"
    }


之后每当 Order 有更新时，Client 会收到数据，比如，

    {
      "op": "notify",
      "topic": "orders.htusdt",
      "ts": 1522856623232,
      "data": {
        "seq-id": 94984,
        "order-id": 2039498445,
        "symbol": "htusdt",
        "account-id": 100077,
        "order-amount": "5000.000000000000000000",
        "order-price": "1.662100000000000000",
        "created-at": 1522858623622,
        "order-type": "buy-limit",
        "order-source": "api",
        "order-state": "filled",
        "role": "taker|maker",
        "price": "1.662100000000000000",
        "filled-amount": "5000.000000000000000000",
        "unfilled-amount": "0.000000000000000000",
        "filled-cash-amount": "8301.357280000000000000",
        "filled-fees": "8.000000000000000000"
      }
    }

### 订单变化通知数据格式说明

  |名称   |              类型   |  说明|
  |-------------------- |--------| --------------------------------------|
  |seq-id               |long     |流水号(不连续)|
  |order-id             |long     |订单 id|
  |symbol               |string   |交易对|
  |account-id           |long     |账户 id|
  |order-amount         |string   |订单数量|
  |order-price          |string   |订单价格|
  |created-at           |long     |订单创建时间|
  |order-type           |string   |订单类型，请参考订单类型说明|
  |order-source         |string   |订单来源，请参考订单来源说明|
  |order-state          |string   |订单状态，请参考订单状态说明|
  |role                 |string   |maker, taker|
  |price                |string   |成交价格|
  |filled-amount        |string   |单次成交数量|
  |unfilled-amount      |string   |单次未成交数量|
  |filled-cash-amount   |string   |单次成交金额|
  |filled-fees          |string   |单次成交手续费（买入为币，卖出为钱）|


错误订阅请求示例（错误的 topic）

    {
      "op": "sub",
      "topic": "foo.bar",
      "cid": "40sG903yz80oDFWr"
    }

订阅失败会返回下面示例数据

    {
      "op": "sub",
      "topic": "foo.bar",
      "cid": "40sG903yz80oDFWr",
      "err-code": 1001, //具体编码请参考响应码表
      "err-msg": “详细的错误信息”，
      "ts": 1489474081631
    }


## 取消订阅数据(unsub)

成功建立和 WebSocket API 的连接之后，向 Server
发送如下格式的数据来取消订阅数据：

### 取消订阅请求数据格式

    {
      “op”: “unsub”,
      “topic": "topic to unsub”,
      "cid": "id generated by client”,
    }

**取消订阅请求数据格式说明**

  |字段名称 |  类型     |说明|
  |----------| --------| ----------------------------------------------------|
  |op         |string   |必填；操作名称，订阅固定值为 unsub；|
  |cid        |string   |选填；Client 请求唯一 ID|
  |topic      |string   |必填；待取消订阅主题名称，详细主题列表请参考附录；|

### 取消订阅请求示例

正确的取消订阅请求

    {
      "op": "unsub",
      "topic": "accounts",
      "cid": "40sG903yz80oDFWr"
    }

取消订阅成功返回数据示例

    {
      "op": "unsub",
      "topic": "accounts",
      "cid": "id generated by client",
      "err-code": 0,
      "ts": 1489474081631
    }

订阅与取消订阅规则说明

  |订阅(sub)       | 取消订阅(unsub)  | 规则|
  |----------------| -----------------| --------|
  |accounts         |accounts          |允许|
  |orders.\*        |orders.\*         |允许|
  |orders.symbol1   |orders.\*         |允许|
  |orders.symbol1   |orders.symbol1    |允许|
  |orders.symbol1   |orders.symbol2    |不允许|
  |orders.\*        |orders.symbol1    |不允许|



## 订阅orders.$symbol.update（新）

相比现有用户订单更新推送主题“orders.$symbol”， 新增主题“orders.$symbol.update”拥有更低的数据延迟以及更准确的消息顺序。建议API用户订阅此新主题接收订单更新推送，以替代现有订阅主题 “orders.$symbol”。（现有订阅主题 “orders.$symbol”仍将在Websocket API服务中被保留直至另行通知。）

**订阅请求数据格式说明**

  |字段名称  | 类型   |  说明|
  |----------| -------- |----------------------------------------------|
  |op         |string   |必填；操作名称，订阅固定值为 sub；|
  |cid        |string   |选填；Client 请求唯一 ID|
  |topic      |string   |必填；订阅主题名称，详细主题列表请参考附录；|

正确的订阅请求

    {
      "op": "sub",
      "cid": "40sG903yz80oDFWr",
      "topic": "orders.htusdt.update"
    }

订阅成功返回数据示例

    {
      "op": "sub",
      "ts": 1489474081631,
      "topic": "orders.htusdt.update",
      "err-code": 0,
      "cid": "40sG903yz80oDFWr"  
    }

之后每当 Order 有更新时，Client 会收到数据，比如，

    {
      "op": "notify",
      "ts": 1522856623232,
      "topic": "orders.htusdt.update",  
      "data": {
        "unfilled-amount": "0.000000000000000000",
        "filled-amount": "5000.000000000000000000",
        "price": "1.662100000000000000",
        "order-id": 2039498445,
        "symbol": "htusdt",
        "match-id": 94984,
        "filled-cash-amount": "8301.357280000000000000",
        "role": "taker|maker",
        "order-state": "filled"
          }
    }

### 订单变化通知数据格式说明

  |名称   |              类型   |  说明|
  |-------------------- |--------| --------------------------------------|
  |match-id               |long     |最近撮合编号（当order-state = submitted, canceled, partial-canceled时，match-id 为消息序列号；当order-state = filled, partial-filled 时，match-id 为最近撮合编号。）|
  |order-id             |long     |订单编号|
  |symbol               |string   |交易代码|
  |order-state          |string   |订单状态，请参考订单状态说明|
  |role                 |string   |最近撮合角色（当order-state = submitted, canceled, partial-canceled时，role 为缺省值taker；当order-state = filled, partial-filled 时，role 取值为taker 或maker。）|
  |price                |string   |最新价（当order-state = submitted 时，price 为订单价格；当order-state = canceled, partial-canceled 时，price 为零；当order-state = filled, partial-filled 时，price 为最近成交价。当role = taker，且该订单同时与多张对手方订单撮合时，price 为多笔成交均价。）|
  |filled-amount        |string   |最近成交数量|
  |filled-cash-amount   |string   |最近成交数额|
  |unfilled-amount      |string   |最近未成交数量（当order-state = submitted 时，unfilled-amount 为原始订单量；当order-state = canceled OR partial-canceled 时，unfilled-amount 为未成交数量；当order-state = filled 时，如果 order-type = buy-market，unfilled-amount 可能为一极小值；如果order-type <> buy-market 时，unfilled-amount 为零；当order-state = partial-filled AND role = taker 时，unfilled-amount 为未成交数量；当order-state = partial-filled AND role = maker 时，unfilled-amount 为零。（后续将支持此场景下的未成交量，时间另行通知。））|




## 请求用户资产 - accounts.list

查询当前用户的所有账户余额数据

成功建立和 WebSocket API 的连接之后，向 Server发送如下格式的数据来查询账户数据：

**账户查询请求数据格式说明**

  |字段名称  | 类型   |  说明|
  |----------| --------| -------------------------------------------------------|
  |op         |string  | 必填；操作名称，固定值为 req；|
  |cid        |string  | 选填；Client 请求唯一 ID|
  |topic      |string   |必填；固定值为accounts.list，详细主题列表请参考附录；|

账户查询请求示例

请求内容

    {
      "op": "req",
      "topic": "accounts.list",
      "cid": "40sG903yz80oDFWr"
    }

调用成功返回数据

    {
      "op": "req",
      "topic": "accounts.list",
      "cid": "40sG903yz80oDFWr"
      "err-code": 0,
      "ts": 1489474082831,
      "data": [
        {
          "id": 419013,
          "type": "spot",
          "state": "working",
          "list": [
            {
              "currency": "usdt",
              "type": "trade",
              "balance": "500009195917.4362872650"
            },
            {
              "currency": "usdt",
              "type": "frozen",
              "balance": "9786.6783000000"
            }
          ]
        },
        {
          "id": 35535,
          "type": "point",
          "state": "working",
          "list": [
            {
              "currency": "eth",
              "type": "trade",
              "balance": "499999894616.1302471000"
            },
            {
              "currency": "eth",
              "type": "frozen",
              "balance": "9786.6783000000"
            }
          ]
        }
      ]
    }

调用失败应答数据示例

    {
      "op": "req",
      "topic": "foo.bar",
      "cid": "40sG903yz80oDFWr"
      "err-code": 12001, //具体编码需要参考响应码表
      “err-msg": "详细的错误信息”，
      "ts": 1489474081631
    }

## 请求交易订单 - orders.list

查询当前委托、历史委托

成功建立和 WebSocket API 的连接之后，向 Server发送如下格式的数据来查询委托数据：

##### 订单列表查询请求数据格式

    {
      "op": "req",
      "topic": "orders.list",
      "cid": "40sG903yz80oDFWr",
      "account-id": "$account-id",
      "symbol": "$symbol",
      "types": "$order-type",
      "start-date": "$start-date",
      "end-date": "$end-date",  
      "states": "$order-state",
      "from": "$from",
      "direct": "$direct",
      "size": "$size"
    }

**订单查询请求数据格式说明**

  |参数名称 |    是否必须 |  类型  |   描述 |                                          默认值  | 取值范围|
  |-------| ----------| --------| ----------------------------------- |-------- |----------------------------|
  |op           |true       |string   |操作名称，固定值为 req            |||                      
  |cid          |false      |string   |Client 请求唯一 ID               |||                       
  |topic        |true       |string   |固定值为 orders.list，详细主题列表请参考附录    |||        
  |account-id   |true       |long     |账户 id                             |||                    
  |symbol       |true       |string   |交易对                            ||  btcusdt, ltcbtc, etcbtc, htusdt......|
  |types        |false      |string   |查询的订单类型组合，使用','分割         ||                参考订单类型说明|
  |start-date   |false      |string   |查询开始日期, 日期格式yyyy-mm-dd        |||                
  |end-date     |false      |string   |查询结束日期, 日期格式yyyy-mm-dd         |||               
  |states       |true       |string   |查询的订单状态组合，使用','分割            |||             
  |from         |false      |string   |查询起始 ID                             |||                
  |direct       |false      |string   |查询方向                                ||              prev 向前，next 向后|
  |size         |false      |string   |查询记录大小                             |||              

订单列表查询请求示例

    {
      "op": "req",
      "topic": "orders.list",
      "cid": "40sG903yz80oDFWr",
      "symbol": "htusdt",
      "states": "submitted,partial-filled"
    }

调用成功返回数据

    {
      "op": "req",
      "topic": "orders.list",
      "cid": "40sG903yz80oDFWr",
      "err-code": 0,
      "ts": 1522856623232,
      "data": [
        {
          "id": 2039498445,
          "symbol": "htusdt",
          "account-id": 100077,
          "amount": "5000.000000000000000000",
          "price": "1.662100000000000000",
          "created-at": 1522858623622,
          "type": "buy-limit",
          "filled-amount": "5000.000000000000000000",
          "filled-cash-amount": "8301.357280000000000000",
          "filled-fees": "8.000000000000000000",
          "finished-at": 1522858624796,
          "source": "api",
          "state": "filled",
          "canceled-at": 0
        }
      ]
    }

应答数据格式说明，

  |名称                 |类型 |    说明|
  |-------------------- |--------| ------------------------------------|
  |id                   |long    | 订单ID|
  |symbol               |string   |交易对|
  |account-id           |long     |账户ID|
  |amount               |string   |订单数量|
  |price                |string   |订单价格|
  |created-at           |long     |订单创建时间|
  |type                 |string   |订单类型，请参考订单类型说明|
  |filled-amount        |string   |已成交数量|
  |filled-cash-amount   |string   |已成交总金额|
  |filled-fees          |string   |已成交手续费|
  |finished-at          |string   |最后成交时间|
  |source               |string   |订单来源，请参考订单来源说明|
  |state                |string   |订单状态，请参考订单状态说明|
  |cancel-at            |long     |撤单时间|

## 请求某个订单详情 - orders.detail
查询某个订单详情

成功建立和 WebSocket API 的连接之后，向 Server发送如下格式的数据来查询某个订单详情数据：

订单详情查询请求数据格式

    {
      "op": "req",
      "topic": "orders.detail",
      "order-id": "$order-id"
      "cid": "40sG903yz80oDFWr"
    }

**订单详情查询请求数据格式说明**

  |参数名称  | 是否必须 |  类型    | 描述       |                                      默认值  | 取值范围|
  |----------| ----------| --------| ------------------------------------------------ |--------| ----------|
  |op         |true       |string   |操作名称，固定值为 req    |||                                
  |cid        |true       |string   |Client 请求唯一 ID        |||                                
  |topic      |false      |string   |固定值为 orders.detail，详细主题列表请参考附录  |||          
  |order-id   |true       |string   |订单ID    |||                                                

##### 订单详情查询请求示例

请求内容

    {
      "op": "req",
      "topic": "orders.detail",
      "order-id": "2039498445"
      "cid": "40sG903yz80oDFWr"
    }

调用成功返回数据示例

    {
      "op": "req",
      "topic": "orders.detail",
      "cid": "40sG903yz80oDFWr"
      "err-code": 0,
      "ts": 1522856623232,
      "data": {
        "id": 2039498445,
        "symbol": "htusdt",
        "account-id": 100077,
        "amount": "5000.000000000000000000",
        "price": "1.662100000000000000",
        "created-at": 1522858623622,
        "type": "buy-limit",
        "filled-amount": "5000.000000000000000000",
        "filled-cash-amount": "8301.357280000000000000",
        "filled-fees": "8.000000000000000000",
        "finished-at": 1522858624796,
        "source": "api",
        "state": "filled",
        "canceled-at": 0
      }
    }

## 请求数据（req)限流策略

为鼓励用户使用订阅接收数据推送，对查询请求做了限流策略， 规则如下：

-   accounts.list
    查询请求，同一个用户(不论几个连接），每2次请求间隔不能小于25秒（此数字可能发生变化），否则会接收到“too
    many request"的错误应答。
-   orders.list 和 orders.detail
    查询请求，同一个用户（不论几个连接），每2次请求间隔不能小于5秒（此数字可能发生变化），否则会接收到“too
    many request"的错误应答。

## 附录

### 操作类型（OP ）说明

  |类型   |  描述|
  |--------| ----------------------|
  |ping     |心跳发起(server)|
  |pong     |心跳应答|
  |auth     |鉴权|
  |sub      |订阅消息|
  |unsub    |取消订阅消息|
  |req      |请求数据|
  |notify   |推送订阅消息(server)|

### 主题 (Topic) 类型说明

| 类型 | 适用操作类型 | 描述 | 
|------------|------|------------| 
|accounts | sub, unsub | 订阅、取消订阅所有资产变更消息 | 
|orders.$symbol | sub, unsub | 订阅、取消订阅指定交易对的订单变更消息，当 $symbol值为 `*` 时代表订阅所有交易对 |
|orders.$symbol.update | sub, unsub | 订阅、取消订阅指定交易对的订单变更消息（新），当 $symbol值为 `*` 时代表订阅所有交易对 |
 | accounts.list | req | 账户数据请求 |
 |orders.list | req | 订单列表查询请求 |
 | orders.detail | req |订单详情查询请求 |

### 响应码（Err-Code）说明

> 具体码表请参照`error.json`

### 订单类型（Order Type）说明

  |类型         | 描述|
  |------------- |--------------------|
  |buy-market    |市价买|
  |sell-market   |市价卖|
  |buy-limit     |限价买|
  |sell-limit    |限价卖|
  |buy-ioc       |买入立即执行或撤销|
  |sell-ioc      |卖出立即执行或撤销|
  |buy-limit-maker |限价买入(只能成为maker)|
  |Sell-limit-maker |限价卖出(只能成为maker)|

### 订单状态（Order State）说明

  |类型              | 描述|
  |------------------| --------------------|
  |submitted         | 已提交|
  |partial-filled    | 部分成交|
  |partial-canceled  | 部分成交撤销(终态)|
  |filled            | 完全成交(终态)|
  |canceled          | 已撤销(终态)|

### 订单来源（Order Source）说明

  |类型         |描述|
  |------------ |--------------------------|
  |spot-web     |现货 Web 交易单|
  |spot-api     |现货 Api 交易单|
  |spot-app     |现货 App 交易单|
  |margin-web   |借贷 Web 交易单|
  |margin-api   |借贷 Api 交易单|
  |margin-app   |借贷 App 交易单|
  |fl-sys       |借贷强制平仓单（爆仓单）|


