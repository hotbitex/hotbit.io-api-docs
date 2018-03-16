# REST API    

## 开始使用    

REST，即Representational State Transfer的缩写，是目前最流行的一种互联网软件架构。它结构清晰、符合标准、易于理解、扩展方便，正得到越来越多网站的采用。其优点如下：    
- 在RESTful架构中，每一个URL代表一种资源；    
- 客户端和服务器之间，传递这种资源的某种表现层；    
- 客户端通过四个HTTP指令，对服务器端资源进行操作，实现“表现层状态转化”。   

建议开发者使用REST API进行币币交易或者资产提现等操作。    
    
## 请求交互    

REST访问的根URL：<https://www.10.10.1.4:18083.com/api/v1>  

所有请求基于Https协议，请求头信息中contentType需要统一设置为：application/x-www-form-urlencoded
    
    
请求交互说明    
1. 请求参数：根据接口请求参数规定进行参数封装。    
2. 提交请求参数：将封装好的请求参数通过GET或者POST方式提交至服务器。    
3. 服务器响应：服务器首先对用户请求数据进行参数安全校验，通过校验后根据业务逻辑将响应数据以JSON格式返回给用户。    
4. 数据处理：对服务器响应数据进行处理。 

## 请求 

|参数名|描述|
| -----    | -----  |  
|method|API方法名|
|params|参数|



## 应答
应答位于"200 ok"的body部分，共有三个参数组成：

|参数名|参数类型|描述|
| :-----    | :-----   | :-----    | 
|result|json object| API调用结果，结果出错时为null|
|error|json object| API调用错误内容，正确时为null|
|id|Integer|Request id|

```
结果正确示例：
{
    "error": null,
    "result": {.....},
    "id": 123
}


结果错误示例：
{
    "error": {
        "code": 1,
        "message": "invalid argument"
    },
    "result": null,
    "id": 123
}
```


## API参考  
**系统类api,  获取系统资源**

###system.time

 | 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | system.time | get  | 获取系统时间 |

请求参数：

| 参数名 | 参数类型  | 描述 |
| --- | ---  | --- |

示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/system.time |  |

```
 Response:{"error": null, "result": 1520919059}
```


##Asset API
**用户资产类api，实现针对各种类型用户资产的查询，更新，所有和用户资产相关操作历史记录获取等等**

###balance.query

 | 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | balance.query | post  | 获取用户资产 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| api_key | string | 用户apikey |
| sign | string | 用户签名值 |
| assets | json array | 代币符号数组 (ps:空数组为获取全部代币资产) |

示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/balance.query  | api_key=5eae7322-6f92-873a-9bc214fd61517ec&sign=fdcafaf85a38970e4d84f6f286a2879e&assets=["BTC","ETH"] |

```
 Response:{

 		 }
```

###balance.history

 | 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | balance.history | post  | 获取用户资产 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| api_key | string | 用户apikey |
| sign | string | 用户签名值 |
| asset | string | 资产名称，如：&quot;BTC&quot;,&quot;ETH&quot;等，&quot;&quot;表示所有资产名称 |
| business | string | 业务名称，如：&quot;deposit&quot;或&quot;trade&quot;，&quot;&quot;表示所有业务名称 |
| start_time | Integer | 开始时间 |
| end_time | Integer | 截止时间 |
| offset | Integer | 偏移位置，如果设置为0，表示从最新近一笔业务开始算起，往之前时间的所有交易记录，总笔数不能大于limit；。如果设置为1，表示从次新一笔业务开始算起，往之前时间的所有交易记录总笔数不能大于limit；以此类推..... |
| limit | Integer | 返回的最大交易笔数..... |

示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/balance.history  | api_key=5eae7322-6f92-873a-9bc214fd61517ec&sign=fdcafaf85a38970e4d84f6f286a2879e&assets=BTC&business=deposit&start_time=1511967657&end_time =1512050400&offset=0&limit=100 |

```
 Response:{

 		 }
```
###asset.list

| 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | asset.list | get  | 获取平台所有资产类型和精度，prec为精确到小数点后多少位 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |

示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/asset.list  |  |

```
 Response:
{
    "error": null,
    "result": [
        {
            "name": "BTC",
            "prec": 8
        },
        {
            "name": "ETH",
            "prec": 8
        }
    ],
    "id": 1521164549
}
```

###asset.summary

| 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | asset.summary | get  | 获取总资产概要，返回平台各种资产的总额 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| assets | json array  | 资产名称，如：&quot;BTC&quot;,&quot;ETH&quot;等，[]表示请求所有资产名称数据 |

示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/asset.summary?assets=[&quot;BTC&quot;,&quot;ETH&quot;]  |   |

```
 Response:
 {
    "error": null,
    "result": [
        {
            "name": "BTC",
            "total_balance": "2513530477.51917879303",
            "available_count": 33,
            "available_balance": "2513414167.80335382403",
            "freeze_count": 6,
            "freeze_balance": "116309.715824969"
        },
        {
            "name": "SPHTX",
            "total_balance": "601388997.97133994",
            "available_count": 18,
            "available_balance": "601388308.60133994",
            "freeze_count": 3,
            "freeze_balance": "689.37"
        }
    ],
    "id": 1521169086
}
```

##Trade API
**交易类api**

###order.put_limit

| 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | order.put_limit | post  | 限价交易 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| api_key | string | 用户apikey |
| sign | string | 用户签名值 |
| market | string  | market名称，如："BTC/USD","BCC/USD" |
| side | Integer  | 1 = &quot;sell&quot;，2=&quot;buy&quot; |
| amount | Integer  | 申请交易的数量 |
| price | Integer  | 交易价格 |

示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/asset.put_limit  | api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=fdcafaf85a38970e4d84f6f286a2879e&market=BTC/ETH&side=1&amount=10&price=100  |

```
 Response:{
 
  }
```

###order.cancel

| 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | order.cancel | post  | 取消交易 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| api_key | string | 用户apikey |
| sign | string | 用户签名值 |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| order_id | Integer  | 要取消交易的id。参看&quot;order.put_market&quot;方法的返回结果。|

示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/asset.summary  | api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=fdcafaf85a38970e4d84f6f286a2879e&market=BTC/ETH&side=1&amount=10&price=100  |

```
 Response:{
 
  }
```

###order.deals
| 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | order.cancel | get  | 获取订单细节 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| api_key | string | 用户apikey |
| sign | string | 用户签名值 |
| order_id | Integer  | 要取消交易的id。参看&quot;order.put_market&quot;方法的返回结果。|
| limit | Integer  | 最多返回 &quot;records&quot;的数量 |

示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/asset.summary?api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=fdcafaf85a38970e4d84f6f286a2879e&order_id=1000&limit=10
  |  | 

```
 Response:
{
    "error": null,
    "result": {
        "offset": 10,
        "limit": 10,
        "records": [
            {
                "time": 1521107411.116817,
                "user": 15643,
                "id": 1385154,
                "role": 1,
                "price": "0.02",
                "amount": "0.071",
                "deal": "0.00142",
                "fee": "0",
                "deal_order_id": 2337658
            },
            {
                "time": 1521107410.357024,
                "user": 15643,
                "id": 1385151,
                "role": 1,
                "price": "0.02",
                "amount": "0.081",
                "deal": "0.00162",
                "fee": "0",
                "deal_order_id": 2337653
            }
        ]
    },
    "id": 1521169460
}
```


###order.book
| 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | order.book | get  | 获取交易列表 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| side | Integer  | 1 = &quot;sell&quot;，2=&quot;buy&quot; |
| offset | Integer | 偏移位置，如果设置为0，表示从最新近一笔业务开始算起，往之前时间的所有交易记录，总笔数不能大于limit；。如果设置为1，表示从次新一笔业务开始算起，往之前时间的所有交易记录总笔数不能大于limit；以此类推..... |
| limit | Integer  | 最多返回 &quot;records&quot;的数量 |

示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/order.book?market=ETH/BTC&side=1&offset=0&limit=10   |   |

```
 Response: 
 {
    "error": null,
    "result": {
        "offset": 0,
        "limit": 10,
        "total": 0,
        "orders": []
    },
    "id": 1521169117
}
```


###order.depth
| 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | order.depth | get  | 获取交易深度 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| limit | Integer |  最多返回 &quot;records&quot;的数量 |
| interval | string  | 时间间隔 |

示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/order.depth?market=ETH/BTC&limit=100&interval=1000  |   |

```
 Response:
 {
    "error": null,
    "result": {
        "asks": [],
        "bids": []
    },
    "id": 1521169159
}
```

###order.pending
| 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | order.pending | post  | 查询未实施订单 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| api_key | string | 用户apikey |
| sign | string | 用户签名值 |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| offset | Integer | 偏移位置，如果设置为0，表示从最新近一笔业务开始算起，往之前时间的所有交易记录，总笔数不能大于limit；。如果设置为1，表示从次新一笔业务开始算起，往之前时间的所有交易记录总笔数不能大于limit；以此类推..... |
| limit | Integer  | 最多返回 &quot;records&quot;的数量 |
示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/order.pending  | api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=fdcafaf85a38970e4d84f6f286a2879emarket=ETH/BTC&offset=0&limit=100  |

```
 Response:{
 
  }
```


###order.finished
| 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | order.finished | post  | 查询用户的已完成订单 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| api_key | string | 用户apikey |
| sign | string | 用户签名值 |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| start_time | Integer | 开始时间 |
| end_time | Integer | 截止时间 |
| offset | Integer | 偏移位置，如果设置为0，表示从最新近一笔业务开始算起，往之前时间的所有交易记录，总笔数不能大于limit；。如果设置为1，表示从次新一笔业务开始算起，往之前时间的所有交易记录总笔数不能大于limit；以此类推..... |
| limit | Integer  | 最多返回 &quot;records&quot;的数量 |
| side | Integer  | 1 = &quot;sell&quot;，2=&quot;buy&quot; |

示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/order.pending  | api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=fdcafaf85a38970e4d84f6f286a2879emarket=ETH/BTC&start_time=1511967657&end_time =1512050400&offset=0&limit=100&side=1  |

```
 Response:{
 
  }
```


##Market API
**市场类api**

###market.last
| 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | market.last | get  | 获取指定market的最新价格 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |

示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/market.last?market=ETH/BTC |  |

```
 Response:
{
    "error": null,
    "result": "0.07413600",
    "id": 1521169525
}
```

###market.deals
| 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | market.last | get  | 查询市场交易记录 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| api_key | string | 用户apikey |
| sign | string | 用户签名值 |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| limit | Integer  | 查询个数限制(limit <= 1000) |
| last_id | Integer  | 返回大于order_id > last_id的交易数据 |

示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/market.deals?market=ETH/BTC&limit=10&last_id=1521100930 |  |

```
 Response:
{
    "error": null,
    "result": [],
    "id": 1521169562
}
```

###market.user_deals
| 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | market.user_deals | post  | 查询用户交易记录 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| offset | Integer  |偏移位置，如果设置为0，表示从最新一个订单开始算起，往之前时间的所有交易记录，总笔数不能大于limit；如果设置为1，表示从次新一个订单开始算起，往之前时间的所有满足条件订单记录，总数不能大于limit；以此类推.....
 |
| limit | Integer   | 查询个数限制(limit <= 1000) |

示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/user_deals | |

```
 Response:
 {

}
```

###market.kline
| 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | market.kline | get  | k线查询 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| start_time | Integer  | 起始时间戳  |
| end_time | Integer  | 结束时间戳  |
| Interval | Integer   | 周期间隔,单位秒, (起始时间到结束时间，总周期数) < 1000 |

示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/market.kline?market=ETH/BTC&start_time=1521100000&end_time=1521101193&interval=60 | |

```
 Response:
{
    "error": null,
    "result": [],
    "id": 1521169586
}
```

###market.status
| 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | market.status | get  | 获取过去指定时间段market当前最新涨跌幅，交易量，最高/最低价格等状态 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| period | Integer  | 查询周期，以秒为单位。即从现在开始往前推的时间，例如：86400，是过去24小时。 |

示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/market.status?market=ETH/BTC&period=10 | |

```
 Response:
{
    "error": null,
    "result": {
        "period": 10,
        "last": "0.0743",
        "open": "0.074162",
        "close": "0.0743",
        "high": "0.0743",
        "low": "0.074162",
        "volume": "0.314",
        "deal": "0.023315531"
    },
    "id": 1521169247
}
```


###market.status_today
| 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | market.status_today | get  | 获取今天market状态 |
 
 请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |

示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/market.status_today?market=ETH/BTC | |

```
 Response:
{
    "error": null,
    "result": {
        "open": "0.074015",
        "last": "0.074287",
        "high": "0.074485",
        "low": "0.073",
        "volume": "1141.63",
        "deal": "83.11985574"
    },
    "id": 1521169293
}
```

###market.list
| 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | market.list | get  | 获取market列表 |
 
 请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |

示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/market.list | |

```
 Response:
{
    "error": null,
    "result": [
        {
            "name": "QASHBTC",
            "stock": "QASH",
            "money": "BTC",
            "fee_prec": 4,
            "stock_prec": 2,
            "money_prec": 8,
            "min_amount": "0.1"
        },
        {
            "name": "QASHETH",
            "stock": "QASH",
            "money": "ETH",
            "fee_prec": 4,
            "stock_prec": 2,
            "money_prec": 8,
            "min_amount": "0.0001"
        }
    ],
    "id": 1521169333
}
```

###market.summary
| 方法名 | 方法类型 | 描述 | 
 | --- | --- | --- |
 | market.summary | get  | market概要 |
 
 请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
 | markets | json array  | market名称json array，如：["BTCUSD","BCCUSD"]，如为空数组:[],返回所有market。 |

示例：

| url | body |
| --- | --- |
| https://www.hotbit.io/api/v1/market.list | |

```
 Response:
{
    "error": null,
    "result": [
        {
            "name": "ETHBTC",
            "ask_count": 0,
            "ask_amount": "0",
            "bid_count": 0,
            "bid_amount": "0"
        },
        {
            "name": "QUNBTC",
            "ask_count": 2,
            "ask_amount": "28",
            "bid_count": 2,
            "bid_amount": "23"
        }
    ],
    "id": 1521169429
}
```

