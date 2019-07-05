# WebSocket API 

**WebSocket**是一种在TCP连接上进行通讯的协议。WebSocket通信协议于2011年被[IETF](https://zh.wikipedia.org/wiki/Internet_Engineering_Task_Force)定为标准RFC 6455，并由RFC7936补充规范。

WebSocket使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在WebSocket API中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。参见[WebSocket](https://zh.wikipedia.org/wiki/WebSocket)详细介绍。

本协议描述Hotbit交易系统对外提供的WebSocket API接口，大致可分为三类：

- 系统接口：用于客户端发送心跳、获取交易系统时间、用户验证等操作。
- 交易数据查询接口：提供K线、行情、市场状态、成交和定单数据的条件查询。
- 订阅/取消订阅接口：包括行情、成交、K线和市场状态等数据的订阅和取消订阅接口。调用订阅接口以后，交易系统会持续推送数据到客户端，直到客户端调用相应的取消订阅接口。

**_注意：websocket 反馈数据采取zlib压缩算法进行压缩_**

## 请求地址

- wss://ws.hotbit.io

## 请求数据格式

WebSocket请求分为三部分

| 字  段 | 字段类型   | 描述 |
| ------ | ---------- | ---- |
| method | String     | API的方法名  |
| params | Json Array | API的参数数组，可以为空数组，但不能为null |
| id     | Integer    | 详细描述见下面说明 |

**说明**：

- 没有包含"id"成员的请求对象为通知，服务端不回复一个通知。
- 如果id成员存在，必须为一个无符号的整形数值，表示本次请求的标识码，远程返回时数据的标识码应该与本次请求标识码相同。

请求示例1：

```
{
    "method":kline.query,
    "params":[
        "BTCBCC",
        145231637,
        145232657,
        5
    ],
    "id":100
}
```

请求示例2：

```
{
    "method":"server.ping",
    "params":[],
    "id":100
}
```

## 应答数据格式

应答同样也分为三个部分

| 字段   | 字段类型    | 描述                            |
| ------ | ----------- | ------------------------------- |
| result | json object | API调用结果，结果出错时为null。 |
| error  | json object | API调用错误内容，正确时为null。 |
| id     | request id  | integer                         |

结果正确示例：

```
{
    "error":null,
    "result":[.........],
    "id":100
}
```

结果错误示例：

```
{
    "error":{
        "code": 1,
        "message":"invalid argument"
	 },
    "result":null,
    "id":100
}
```


# API Reference

## 系统接口

### 发送心跳

请求

- method: server.ping
- params: []

示例：

```
{
    "method":"server.ping",
    "params":[],
    "id":100
}
```

响应：

```
{
    "error":null,
    "result":"pong",
    "id":100
}
```



### 获取系统时间

请求

- method: system.time
- params: []

示例：

```
{
    "method":"server.time",
    "params":[],
    "id":100
}
```



响应：

```
{
    "error":null,
    "result":1511941406,
    "id":100
}
```



### 用户验证

请求

- method: server.auth
- params: 

请求参数：

| 参数名  | 参数类型 | 必填 | 描述        |
| ------- | -------- | ---- | ----------- |
| api_key | String   | 是   | 用户api_key |
| sign    | String   | 是   | 签名值      |

示例：

```
{
    "method":"server.auth",
    "params":[api_key,sign],
    "id":100
}
```

响应：

```
{
    "error": null,
    "result": {
        "status": "success"
    },
    "id": 100
}
```



## 查询接口

### K线数据查询

- method: kline.query
- params:

请求参数：

| 参数名   | 参数类型 | 必填 | 描述         |
| -------- | -------- | ---- | ------------ |
| market   | String   | 是   | 市场名       |
| start    | Integer  | 是   | 开始时间     |
| end      | Integer  | 是   | 结束时间     |
| interval | Integer  | 是   | 数据获取周期 |

示例：

```
{
    "method":"kline.query",
    "params":["EOSETH",145231637,145232657,5],
    "id":100
}
```

返回值：

```
{
    "error": null,
    "result": [
        [
            1525798800,    # 时间戳
            "0.00001790",  # 开盘价
            "0.00001881",  # 收盘价
            "0.00001886",  # 最高价
            "0.00001790",  # 最低价
            "169033000",   # 成交量
            "3025.69",     # 成交额
            "ACATETH"      # 交易对市场名称
        ],
        ...],
    "id": 123
} 
```

### 最新价格查询

- method: price.query
- params: ["market_name"]

示例：

```
{
    "method":"price.query", 
    "params":["BTCBCC"], 
    "id":100
}
```

响应：

```
{
    "error": null, 
    "result": "8000.00000000", 
    "id": 100
}
```

### 当前市场状态查询

- method: state.query
- params: 

请求参数：

| 参数名   | 参数类型 | 必填 | 描述         |
| -------- | -------- | ---- | ------------ |
| market list  | json array   | 是   | 交易对市场名称列表       |
| interval    | Integer  | 是   | 周期，单位：秒，例如：86400表示24小时  |

示例：

```
{
    "method":"state.query", 
    "params":["BTCBCC",86400], 
    "id":100
}
```

响应：

```
{
    "period": 86400,  # 周期，单位秒
    "last": "8000",   # 最新价
    "open": "0",      # 开盘价
    "close": "0",     # 收盘价
    "high": "0",      # 最高价
    "low": "0",       # 最低价
    "volume": "0",    # 成交量
    "deal": "0"       # 成交额
},
```

### 当天的市场状态查询

**该接口查询自然日的市场状态，例如当前时间是2017-12-20 14:21:35，调用该接口返回的是2017-12-20这一天的数据，这点和state.query接口不同，state.query返回的是指定时间内的数据，可以跨自然日**

- method: today.query
- params: 


请求参数：

| 参数名 | 参数类型 | 必填 | 描述   |
| ------ | -------- | ---- | ------ |
| market | String   | 是   | 市场名 |

示例：

```
{
    "method":"today.query", 
    "params":["BTCBCC"], 
    "id":100
}
```

响应：

```
{
    "last": "8000",   # 最新价
    "open": "0",      # 开盘价
    "close": "0",     # 收盘价
    "high": "0",      # 最高价
    "low": "0",       # 最低价
    "volume": "0",    # 成交量
    "deal": "0"       # 成交额
},
```

### 用户成交查询

- method: deals.query
- params: 

请求参数：

| 参数名   | 参数类型 | 必填 | 描述                                                         |
| -------- | -------- | ---- | ------------------------------------------------------------ |
| market   | String   | 是   | 市场名                                                       |
| limit    | Integer  | 是   | 最大返回数量                                                 |
| last\_id | integer  | 是   | 从最近一个订单id开始往前（id从大到小）最后一个id,例如一共有1，2，3，4，5，一共5个id，last\_id设为3，limit即使设为5也只返回id为4，5两组数据。 |

请求：

```
{
    "method":"deals.query",
    "params":["BTCBCC",5,1], 
    "id":100
}
```

响应：

```
{ 
    "error": null, 
    "result": [ 
    { "id": 26, "time": 1512454847.188796, "price": "8000", "amount": "1", "type": "buy" }, 
    { "id": 25, "time": 1512445625.751971, "price": "8000", "amount": "0.125", "type": "buy" }, 
    { "id": 24, "time": 1512442938.956193, "price": "8000", "amount": "0.125", "type": "buy" }, 
    { "id": 23, "time": 1512442929.0405071, "price": "8000", "amount": "0.125", "type": "buy" }, 
    { "id": 22, "time": 1512442927.2021289, "price": "8000", "amount": "0.125", "type": "buy" } ], 
    "id": 100
}
```

返回值说明：

```
"result": [ 
    { 
        "id": 26,                  # deal_id
        "time": 1512454847.188796, # 交易时间 
        "price": "8000",           # 交易价格 
        "amount": "1",             # 成交数量 
        "type": "buy"              # 成交类型 
    }, 
    ...... ]
```

### 定单深度查询

- method: depth.query
- params: 

请求参数：

| 参数名       | 参数类型 | 必填 | 描述  |
| ------------ | -------- | ---- | ----- |
| market       | String   | 是   | 市场名     |
| limit        | Integer  | 是   | 最大返回数量  |
| **interval** | String   | 是   | 精度，比如"0.001"，获取的数据就是：12.975这样的数字。再比如"5"，就会得到"15" "20" "10" 这样的数字。 |

示例：

```
{ 
    "method":"depth.query", 
    "params":["EOSUSDT",100,"1"], 
    "id":100
}
```

响应：

```
{ 
    "error": null, 
    "result": { "asks": [[ "8000", "20"] ], "bids": [[ "800", "4"] ] }, 
    "id": 100
}
```

返回结果：

```
"result": { 
    "asks": [ [ "8000", "20" ] ], 
    "bids": [ ["800", "4"] ]
}
```

### 用户未完成定单查询

**Order API 调用需要auth认证，请先调用server.auth方法请求认证，然后再调用。**
- method: order.query
- params:

请求参数：

| 参数名 | 参数类型 | 必填 | 描述                 |
| ------ | -------- | ---- | -------------------- |
| market list | list   | 是   | 市场名列表，如果为空的话，返回所有市场的数据|
| offset | Integer  | 是   | offset               |
| limit  | Integer  | 是   | 最大返回数量,小于101 |


示例：

```
{ 
    "method":"order.query", 
    "params":[["BTCBCC","BTCETH"],0,50], 
    "id":100
}
或
{
    "method":"order.query", 
    "params":[[],0,50], 
    "id":100
}
```

响应：

```
{ 
    "error": null, 
    "result": { "limit": 50, "offset": 0, "total": 0, "records": [] }, 
    "id": 100 
}
```

返回值说明：

```     
{
    "error": null, 
    "result": {
        "ACATETH": {
            "limit": 100, 
            "offset": 0, 
            "total": 3, 
            "records": [
                {
                    "id": 8675864,               # 定单ID
                    "market": "ACATETH",         # 市场名称
                    "type": 1,                   # 定单类型 1-限价单 2-市价单
                    "side": 1,                   # 买卖方向 1-ASK 2-Bid
                    "user": 15731,               # 用户ID
                    "ctime": 1524482296.075341,  # 定单创建时间
                    "mtime": 1524482296.075341,  # 定单修改时间
                    "price": "0.00001899",       # 价格
                    "amount": "1",               # 数量
                    "taker_fee": "0",            # taker的费率
                    "maker_fee": "0",            # maker的费率
                    "left": "1",                 # 剩余未成交量
                    "deal_stock": "0e-8",        # 成交量
                    "deal_money": "0e-16",       # 成交金额
                    "deal_fee": "0e-12"          # 成交费用
                },
                ...
            ]
        }, 
        ……,
        "SPHTXBTC":{……}
    }
    "id": 13
}
```

### 用户已完成定单查询

- method: order.history
- params: 

请求参数：

| 参数名      | 参数类型 | 必填 | 描述                 |
| ----------- | -------- | ---- | -------------------- |
| market      | String   | 是   | 市场名               |
| start\_time | Integer  | 是   | 开始时间             |
| end\_time   | Integer  | 是   | 结束时间             |
| offset      | Integer  | 是   | offset               |
| limit       | Integer  | 是   | 最大返回数量,小于101 |

示例：

```
{ 
    "method":"order.history", 
    "params":["BTCBCC",1511941006,1511941406,0,50], 
    "id":100}
```

响应：

```
{ 
    "error": null, 
    "result": { "limit": 50, "offset": 0, "total": 0, "records": [] }, 
    "id": 100 
}
```

**返回值：参考order.query方法返回值。**

### 用户资产查询

**调用需要auth认证，请先调用server.auth方法请求认证，然后再调用。**

- method: asset.query
- params: []

示例：

```
{
    "method":"asset.query", 
    "params":["ETH","BTC"], 
    "id":100
}
```

响应：

```
{
    "error": null, 
    "result": { 
        "ETH": { 
            "available": "0",  # 可用资金
            "freeze": "0"      # 冻结资金
        }, 
        "BTC": { "available": "0", "freeze": "0" } 
    }, 
    "id": 100
}
```

请求参数：asset list。

返回值：参考上面示例。


### 用户资产变动历史查询

- method: asset.history
- params:

请求参数：

| 参数名      | 参数类型 | 必填 | 描述                          |
| ----------- | -------- | ---- | ----------------------------- |
| asset       | String   | 是   | asset name, which can be null |
| business    | String   | 是   | 业务名                        |
| start\_time | Integer  | 是   | 开始时间                      |
| end\_time   | Integer  | 是   | 结束时间                      |
| offset      | Integer  | 是   | offset                        |
| limit       | Integer  | 是   | 最大返回数量,小于101          |

示例：

```
{
    "method":"asset.history", 
    "params":["BTC","deposit",1501940406,1513328112,0,100], 
    "id":100
}
```

响应：

```
{
    "error": null, 
    "result": { 
        "offset": 0, 
        "limit": 100, 
        "records": [ 
            { 
              "time": 1511856143.3754101,   # 时间戳
              "asset": "USD",               # 资产类型
              "business":"deposit",         # 业务标识 deposit-充值，freeze-冻结/解冻，withdraw-提币
              "change": "100000",           # 变动金额
              "balance": "300000",          # 变动后用户资金余额
              "detail": { "id": 10003 }     # 其他详情
            }, 
            ... 
         ] 
    }, 
    "id": 100 
}
```

## 订阅/取消订阅接口


### 行情订阅

- method: kline.subscribe
- params: 

请求参数：

| 参数名   | 参数类型 | 必填 | 描述         |
| -------- | -------- | ---- | ------------ |
| market   | String   | 是   | 市场名       |
| interval | Integer  | 是   | 数据获取周期 |

示例：

```
{
    "method":"kline.subscribe", 
    "params":[ "BTCBCC", 60 ], 
    "id":100
}
```

响应：

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

订阅成功后，订阅数据将会被推送到客户端。内容如 **[行情推送]** 所述。

### 行情推送

示例：

```
{
    "method": "kline.update", 
    "params": [[1513135140, "8000", "8000", "8000", "8000", "0", "0", "BTCBCC"]], 
    "id": null
}
```

参数说明：

```
"params": [ 
    [ 
      1492358400,  # 时间戳
      "7000.00",   # 开盘价
      "8000.0",    # 收盘价
      "8100.00",   # 最高价
      "6800.00",   # 最低价
      "1000.00"    # 成交量
      "123456.00", # 成交金额
      "BTCBCC"     # 交易币对市场名称
    ]
]
```

### 取消行情订阅

- method: kline.unsubscribe
- params: []

示例：

```
{
    "method":"kline.unsubscribe", 
    "params":[], 
    "id":100
}
```

响应：

```
{
    "error": null,
    "result": {"status": "success"},
    "id": 100
}
```

### 最新价订阅

- method: price.subscribe
- params: 

请求参数：

| 参数名   | 参数类型 | 必填 | 描述         |
| -------- | -------- | ---- | ------------ |
| market   | String   | 是   | 市场名       |

请求

```
{
    "method":"price.subscribe", 
    "params":["BTCBCC"], 
    "id":100
}
```

响应

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### 最新价推送

**订阅成功后，向客户端主动推送数据内容。**

- method: price.update

示例

```
{
    "method": "price.update", 
    "params": ["BTCBCC", "8000.00000000"], 
    "id": null
}
```

参数说明:

```
"params": [ 
    "BTCBCC",       # 市场名称 
    "8000.00000000" # 最新价格
]
```

### 取消最新价订阅

- method: price.unsubscribe
- params: []

示例：

```
{
    "method":"price.unsubscribe", 
    "params":[], 
    "id":100
}
```

响应

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### 市场状态订阅

**24小时市场状态订阅。**

- method: state.subscribe
- params: 交易币对市场名称列表

请求

```
{
    "method":"state.subscribe", 
    "params":["BTCBCC","BTCUSD"], 
    "id":100
}
```

响应

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### 市场状态推送

**订阅成功后，状态数据会被交易服务器推送给客户端。**

- method: state.update

示例：

```
{
    "method": "state.update", 
    "params": [ 
        "BTCBCC", {
            "period": 86400, 
            "last": "8000", 
            "open": "0", 
            "close": "0", 
            "high": "0", 
            "low": "0", 
            "volume": "0", 
            "deal": "0"
        } 
    ], 
    "id": null
}
```

返回值参考state.query方法。

### 取消市场状态推送

- method: state.unsubscribe
- params: []

请求

```
{
    "method":"state.unsubscribe", 
    "params":[], 
    "id":100
}
```

响应

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### 当日市场状态订阅

**当天market状态订阅。**

- method: today.subscribe
- params: 

请求参数：

| 参数名      | 参数类型 | 必填 | 描述       |
| ----------- | -------- | ---- | ---------- |
| market list | String   | 是   | 市场名列表 |

请求

```
{
    "method":"today.subscribe", 
    "params":["BTCUSD","BTCBCC"], 
    "id":100
}
```

响应

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### 当日市场状态推送

订阅成功后，服务器主动向客户端推送数据。

- method: today.update

示例

```
{
    "method": "today.update", 
    "params": [ 
        "BTCBCC", {
            "open": "8000", 
            "last": "8000", 
            "high": "8000", 
            "low": "8000", 
            "volume": "0", 
            "deal": "0"
         }
     ], 
     "id": null
}
```

返回值参考state.query方法。

### 取消当日市场状态订阅

- method: today.unsubscribe
- params: []

请求

```
{
    "method":"today.unsubscribe", 
    "params":[], 
    "id":100
}
```

响应

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### 成交订阅

- method: deals.subscribe
- params: 交易币对市场名称列表

请求

```
{
    "method":"deals.subscribe", 
    "params":["BTCBCC","BTCUSD"], 
    "id":100
}
```

响应

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### 成交推送

示例：

```
{
    "method": "deals.update", 
    "params": [ 
        "BTCBCC", [ 
             {"id": 26, "time": 1512454847.188796, "price": "8000", "amount": "1", "type": "buy"},
             {"id": 25, "time": 1512445625.751971, "price": "8000", "amount": "0.125", "type": "buy"},
             {"id": 24, "time": 1512442938.956193, "price": "8000", "amount": "0.125", "type": "buy"}, 
             ... 
             {"id": 22, "time": 1512442927.2021289, "price": "8000", "amount": "0.125", "type": "buy"},
             {"id": 3, "time": 1512442109.0283101, "price": "8000", "amount": "0.125", "type": "buy"},
             {"id": 2, "time": 1512441964.500567, "price": "8000", "amount": "0.125", "type": "buy"},
             {"id": 1, "time": 1512441751.9356339, "price": "8000", "amount": "0.125", "type": "buy"} 
         ]
     ], 
    "id": null
}
```

参考deals.query的返回结果。 

### 取消成交订阅

- method: deals.unsubscribe
- params: []

请求

```
{ 
    "method":"deals.unsubscribe", 
    "params":[], 
    "id":100
}
```

响应

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

#### 定单深度订阅

- method: depth.subscribe
- params: 

请求参数：

```
"params":[ 
    "BTCBCC",  # 市场名 
    100,       # 定单价格,取值：1, 5, 10, 20, 30, 50, 100 
    "0.0001"   # 价格区间精度, 取值："0","0.00000001","0.0000001","0.000001","0.00001", 
               # "0.0001", "0.001", "0.01", "0.1"
]
```

请求

```
{
    "method":"depth.subscribe", 
    "params":["EOSUSDT",100,"0.1"], 
    "id":100
}
```

响应

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### 定单深度数据推送

- method: depth.update

示例：

```
{
    "method": "depth.update", 
    "params": [true, {"asks": [["8000", "20"]], "bids": [["800", "4"]]}, "BTCBCC"], 
    "id": null
}
```

返回结果说明：

以价格区间精度为0.1为例。

```
"params": [ 
    true, #clean: Boolean, true: complete result，false: last returned updated result 
    { 
        "asks": [
            [
                "8000", # 价格 
                "20"    # 数量 
            ],            
            [
                "8000.1", # 价格 
                "20"      # **价格≥8000，小于8000.1的所有定单数量和之和**
            ]
         ], 
        "bids": 
        [
            [
                "800", 
                "4"
            ],
            [
                "800.1", 
                "5"
            ]
        ] 
     },
    "BTCBCC"             # 交易币对的市场名称
]
```

### 取消定单深度订阅

- method: depth.unsubscribe
- params: []

请求

```
{
    "method":"depth.unsubscribe", 
    "params":[], 
    "id":100
}
```

响应

```
{ 
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### 用户定单状态订阅

- method: order.subscribe
- params: 市场名称列表

请求

```
{ 
    "method":"order.subscribe", 
    "params":["BTCBCC","BTCETH"], 
    "id":100
}
```

响应

```
{ 
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### 用户定单变化推送

**定单的每次状态变化都会推送给用户端**

示例：

```
{
    "method": "order.update", 
    "params": [ 
        1, # 定单状态变化事件类型，整数值 1-新定单, 2-更新定单, 3-完全成交 
        {
            "id": 422458, 
            "market": "BTCBCC", 
            "source": "", 
            "type": 1, # 1-limit order 2 -market order
            "side": 1, # 1-Ask 2-Bid 
            "user": 5, 
            "ctime": 1513819599.987308, # create_time 
            "mtime": 1513819599.987308, # update_time 
            "price": "8000", 
            "amount": "10", 
            "taker_fee": "0.002", 
            "maker_fee": "0.001", 
            "left": "10", 
            "deal_stock": "0", 
            "deal_money": "0", 
            "deal_fee": "0" } 
    ], 
    "id": 102
}
```

定单的详细字段说明参考order.query。

### 取消定单状态变化订阅

- method: order.unsubscribe
- params: []

请求

```
{
    "method":"order.unsubscribe", 
    "params":[], 
    "id":100
}
```

响应

```
{ 
    "error": null,
    "result": { "status": "success" }, 
    "id": 100
}
```

### 用户资产信息订阅

- method: asset.subscribe
- params: 资产名称列表

请求

```
{
    "method":"asset.subscribe",
    "params":["BTC","ETH","BCC"], 
    "id":100
}
```

响应

```
{ 
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### 用户资产变化信息推送

- method: asset.update

示例：

```
{ 
    "method": "asset.update", 
    "params": { 
        "ETH": { "available": "0", "freeze": "0" }, 
        "BTC": { "available": "0", "freeze": "0" } 
    }, 
    "id": 100
}
```

### 用户资产信息订阅

- method: asset.unsubscribe

请求

```
{
    "method":"asset.unsubscribe", 
    "params":[], 
    "id":100
}
```

响应

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```
