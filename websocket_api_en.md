# WebSocket API 

**WebSocket** is a type of protocol that conducts communication on TCP connection. WebSocket communication protocol is defined as standard RFC 6455 by [IETF](https://zh.wikipedia.org/wiki/Internet_Engineering_Task_Force) in 2011, and is complemented and standardized by RFC7936.

WebSocket makes the data exchange between client end and server become more simple, and allows server end to actively push data to client end. In WebSocket API，by finishing one handshake only, a permanent connection can be directly established between browser and server, which also enables two-way data communication. Please refer to [WebSocket](https://zh.wikipedia.org/wiki/WebSocket) for detailed introduction.

This protocol describes the WebSocket API interface that Hotbit trading system provides for external users, which can generally be categorized into three types:

- System interface: used for operations of client end, such as sending HeartBeat, obtain the time of trading system and user verification etc.
- Trading data inquiry interface: provides criteria queries of K Chart, market trend, market status, order settlement and number of orders.
- Subscribe/unsubscribe interface: includes the subscription and unsubscription of the interfaces such as the interfaces of market trend, order settlement, K Chart and market status etc. After calling subscribed interface, the trading system will continuously push data to client end, until the client end calls relevant interface to cancel the subscription. 

**_Note：websocket feedback data is compressed by zlib compression algorithm_**

## Request Address

- wss://ws.hotbit.io

## Request Data Format

WebSocket request is divided into three sections

| Field | Type of Field   | Description |
| ------ | ---------- | ---- |
| method | String     | Name of API Method  |
| params | Json Array | The array of API parameter can be empty, but cannot be null|
| id     | Integer    | Please refer to the following instruction for detailed description |

**Description**：

- The requested object that does not include "id" member is notice, the server end does not reply to a notice.
- If id member exists, it must be an integer value with no symbol that represents the identifier of the request. During the remote return process, the identifier of the data shall be the same as the identifier of the request.

Request Example 1：

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

Request Exmaple 2：

```
{
    "method":"server.ping",
    "params":[],
    "id":100
}
```

## Format of Response Data

The response is also divided into three sections

| Field   | Type of Field    | Description                            |
| ------ | ----------- | ------------------------------- |
| result | json object | API result call，when the result is incorrect, it is null。 |
| error  | json object | API call for incorrect information，when correct, it is null。 |
| id     | request id  | integer                         |

Example for correct result：

```
{
    "error":null,
    "result":[.........],
    "id":100
}
```

Example for incorrect result：

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

## System Interfce

### Send HeartBeat

Request

- method: server.ping
- params: []

Example：

```
{
    "method":"server.ping",
    "params":[],
    "id":100
}
```

Response：

```
{
    "error":null,
    "result":"pong",
    "id":100
}
```



### Obtain System Time

Request

- method: system.time
- params: []

Example：

```
{
    "method":"server.time",
    "params":[],
    "id":100
}
```



Response：

```
{
    "error":null,
    "result":1511941406,
    "id":100
}
```



### User Verification

Request

- method: server.auth
- params: 

Request Parameter：

| Name of Parameter  | Type of parameter | Mandatory | Description        |
| ------- | -------- | ---- | ----------- |
| api_key | String   | Yes   | User api_key |
| sign    | String   | Yes   | Signature Value      |

Example：

```
{
    "method":"server.auth",
    "params":[api_key,sign],
    "id":100
}
```

Response：

```
{
    "error": null,
    "result": {
        "status": "success"
    },
    "id": 100
}
```



## Inquire Interface

### K Chart Data Inquiry

- method: kline.query
- params:

Request Parameter：

| Name of parameter   | Type of parameter | Mandatory | Description         |
| -------- | -------- | ---- | ------------ |
| market   | String   | Yes   | Name of market       |
| start    | Integer  | Yes   | Starting Time     |
| end      | Integer  | Yes   | Ending Time     |
| interval | Integer  | Yes   | Period for data obtention |

Example：

```
{
    "method":"kline.query",
    "params":["EOSETH",145231637,145232657,5],
    "id":100
}
```

Return Value：

```
{
    "error": null,
    "result": [
        [
            1525798800,    # Timestamp
            "0.00001790",  # Opening price
            "0.00001881",  # Closing price
            "0.00001886",  # Highest price
            "0.00001790",  # Lowest price
            "169033000",   # Trading volume
            "3025.69",     # Turnover
            "ACATETH"      # The name of transaction pair market
        ],
        ...],
    "id": 123
} 
```

### Inquiry on Latest Price

- method: price.query
- params: ["market_name"]

Example：

```
{
    "method":"price.query", 
    "params":["BTCBCC"], 
    "id":100
}
```

Response：

```
{
    "error": null, 
    "result": "8000.00000000", 
    "id": 100
}
```

### Inquiry on Current Market Status

- method: state.query
- params: 

Request parameter：

| name of parameter   | type of parameter | mandatory | description         |
| -------- | -------- | ---- | ------------ |
| market list  | json array   | yes   | The list of the name of transaction pair market       |
| interval    | Integer  | yes   | period，unit：second，for example：86400 means 24 hours  |

example：

```
{
    "method":"state.query", 
    "params":["BTCBCC",86400], 
    "id":100
}
```

response：

```
{
    "period": 86400,  # period，unit second
    "last": "8000",   # latest price
    "open": "0",      # opening price
    "close": "0",     # closing price
    "high": "0",      # highest price
    "low": "0",       # lowest price
    "volume": "0",    # Trading volume
    "deal": "0"       # Turnover
},
```

### Inquiry on the Market Status of the Day

**The interface inquires the market status of calendar day，for example, the current time is 2017-12-20 14:21:35，the data returned by calling this interface is the data of the calendar day of 2017-12-20，which is different from state.query interface，the data that state.query return is the data within a designated period of time, which may straddle more than one calendar days.**

- method: today.query
- params: 


Request parameter：

| name of parameter | type of parameter | mandatory | description   |
| ------ | -------- | ---- | ------ |
| market | String   | yes   | market name |

Example：

```
{
    "method":"today.query", 
    "params":["BTCBCC"], 
    "id":100
}
```

Response：

```
{
    "last": "8000",   # Latest price
    "open": "0",      # Opening price
    "close": "0",     # Closing price
    "high": "0",      # Highest price
    "low": "0",       # Lowest price
    "volume": "0",    # Trading volume
    "deal": "0"       # Turnover
},
```

### Inquiry on User's Settled Order

- method: deals.query
- params: 

Request parameter：

| name of parameter   | type of parameter | mandatory | description                                                     |
| -------- | -------- | ---- | ------------------------------------------------------------ |
| market   | String   | yes   | market name                                                       |
| limit    | Integer  | yes   | Maximum number returned                                                 |
| last\_id | integer  | yes   |  the last id from the most recent id to previous id (id from large to small),for example there are 5 id together, namely 1,2,3,4,5, if last\_id is set to be 3，even if limit is set to be 5, only the two groups of data under id 4,5 will be returned. |

Request：

```
{
    "method":"deals.query",
    "params":["BTCBCC",5,1], 
    "id":100
}
```

Response：

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

Return value description：

```
"result": [ 
    { 
        "id": 26,                  # deal_id
        "time": 1512454847.188796, # trading time 
        "price": "8000",           # trading price 
        "amount": "1",             # amount of transaction settled 
        "type": "buy"              # type of transaction settled 
    }, 
    ...... ]
```

### Inquiry on the Depth of Order

- method: depth.query
- params: 

Request parameter：

| name of parameter       | type of parameter | mandatory | description  |
| ------------ | -------- | ---- | ----- |
| market       | String   | yes   | market name     |
| limit        | Integer  | yes   | maximum number returned  |
| **interval** | String   | yes   | precision，for example "0.001"，the data obtained will be：such number as 12.975. Also, for example "5"，will obtain numbers such as "15" "20" "10". |

Example：

```
{ 
    "method":"depth.query", 
    "params":["EOSUSDT",100,"1"], 
    "id":100
}
```

Response：

```
{ 
    "error": null, 
    "result": { "asks": [[ "8000", "20"] ], "bids": [[ "800", "4"] ] }, 
    "id": 100
}
```

Result returned：

```
"result": { 
    "asks": [ [ "8000", "20" ] ], 
    "bids": [ ["800", "4"] ]
}
```

### Inquiry on User's Unfinished Orders

**Order API call requires authentication，please call server.auth method first for the request of authentication，then call.**
- method: order.query
- params:

Parameter request：

| name of parameter | type of parameter | mandatory | description                 |
| ------ | -------- | ---- | -------------------- |
| market list | list   | yes   | list of market names，if empty，return the data of all markets|
| offset | Integer  | yes   | offset               |
| limit  | Integer  | yes   | Maximum number of return,less than 101 |


Example：

```
{ 
    "method":"order.query", 
    "params":[["BTCBCC","BTCETH"],0,50], 
    "id":100
}
or
{
    "method":"order.query", 
    "params":[[],0,50], 
    "id":100
}
```

Response：

```
{ 
    "error": null, 
    "result": { "limit": 50, "offset": 0, "total": 0, "records": [] }, 
    "id": 100 
}
```

Description on return value：

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
                    "id": 8675864,               # Order ID
                    "market": "ACATETH",         # Market Name
                    "type": 1,                   # Type of order 1-limit order 2-market order
                    "side": 1,                   # Direction of buy and sell 1-ASK 2-Bid
                    "user": 15731,               # User ID
                    "ctime": 1524482296.075341,  # Time of Order establishment
                    "mtime": 1524482296.075341,  # Time of order modification
                    "price": "0.00001899",       # price
                    "amount": "1",               # amount
                    "taker_fee": "0",            # taker fee 
                    "maker_fee": "0",            # maker fee
                    "left": "1",                 # unsettled amount left
                    "deal_stock": "0e-8",        # settled amount
                    "deal_money": "0e-16",       # Value settled
                    "deal_fee": "0e-12"          # fee settled
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

### Inquiry on User's Finished Orders

- method: order.history
- params: 

request parameter：

| name of parameter      | type of parameter | mandatory | description                 |
| ----------- | -------- | ---- | -------------------- |
| market      | String   | yes   | market name               |
| start\_time | Integer  | yes   | starting time             |
| end\_time   | Integer  | yes   | ending time             |
| offset      | Integer  | yes   | offset               |
| limit       | Integer  | yes   | maximum number returned, less than101 |

Example：

```
{ 
    "method":"order.history", 
    "params":["BTCBCC",1511941006,1511941406,0,50], 
    "id":100}
```

response：

```
{ 
    "error": null, 
    "result": { "limit": 50, "offset": 0, "total": 0, "records": [] }, 
    "id": 100 
}
```

**return value：refer to the return value of order.query method.**

### Inquiry on User's Asset

**The call requires authentication，please call the method of server.auth first for authentication request，then call.**

- method: asset.query
- params: []

for example：

```
{
    "method":"asset.query", 
    "params":["ETH","BTC"], 
    "id":100
}
```

response：

```
{
    "error": null, 
    "result": { 
        "ETH": { 
            "available": "0",  # available asset
            "freeze": "0"      # freeze asset
        }, 
        "BTC": { "available": "0", "freeze": "0" } 
    }, 
    "id": 100
}
```

request parameter：asset list。

return value：refer to the example above。


### Inquiry on Historical Changes of User Assets

- method: asset.history
- params:

request parameter：

| name of parameter      | type of parameter | mandatory | description                          |
| ----------- | -------- | ---- | ----------------------------- |
| asset       | String   | yes   | asset name, which can be null |
| business    | String   | yes   | business name                        |
| start\_time | Integer  | yes   | starting time                      |
| end\_time   | Integer  | yes   | ending time                      |
| offset      | Integer  | yes   | offset                        |
| limit       | Integer  | yes   | 最大返回数量maximum number returned, less than 101          |

example：

```
{
    "method":"asset.history", 
    "params":["BTC","deposit",1501940406,1513328112,0,100], 
    "id":100
}
```

response：

```
{
    "error": null, 
    "result": { 
        "offset": 0, 
        "limit": 100, 
        "records": [ 
            { 
              "time": 1511856143.3754101,   # timestamp
              "asset": "USD",               # asset type
              "business":"deposit",         # business symbol deposit-deposit，freeze-freeze/unfreeze，withdraw-提币
              "change": "100000",           # asset change
              "balance": "300000",          # user balance after the change
              "detail": { "id": 10003 }     # other details
            }, 
            ... 
         ] 
    }, 
    "id": 100 
}
```

## Subscripbe/Unsubscribe Interface


### Subscribe Market Trends

- method: kline.subscribe
- params: 

request parameter：

| name of parameter   | type of parameter | mandatory | description         |
| -------- | -------- | ---- | ------------ |
| market   | String   | yes   | market name       |
| interval | Integer  | yes   | period of data obtention |

example：

```
{
    "method":"kline.subscribe", 
    "params":[ "BTCBCC", 60 ], 
    "id":100
}
```

response：

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

After successfully subscribing, the subscribed data will be pushed to client end. Refer the content to **[market trend push]**.

### Market Trend Push

example：

```
{
    "method": "kline.update", 
    "params": [[1513135140, "8000", "8000", "8000", "8000", "0", "0", "BTCBCC"]], 
    "id": null
}
```

parameter description：

```
"params": [ 
    [ 
      1492358400,  # timestamp
      "7000.00",   # opening price
      "8000.0",    # closing price
      "8100.00",   # highest price
      "6800.00",   # lowest price
      "1000.00"    # 成交量 Turnover
      "123456.00", # 成交金额 Value settled
      "BTCBCC"     # 交易币对市场名称 Market name of transaction pair
    ]
]
```

### Cancel the Subsctiption on Market Trend

- method: kline.unsubscribe
- params: []

example：

```
{
    "method":"kline.unsubscribe", 
    "params":[], 
    "id":100
}
```

response：

```
{
    "error": null,
    "result": {"status": "success"},
    "id": 100
}
```

### Subscription of Latest Price

- method: price.subscribe
- params: 

request parameter：

| name of parameter   | type of parameter | mandatory | description         |
| -------- | -------- | ---- | ------------ |
| market   | String   | yes   | market name       |

request

```
{
    "method":"price.subscribe", 
    "params":["BTCBCC"], 
    "id":100
}
```

response

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### Push of Latest Price

**After successfully subscribing，the data content will be actively pushed to client end.**

- method: price.update

example

```
{
    "method": "price.update", 
    "params": ["BTCBCC", "8000.00000000"], 
    "id": null
}
```

parameter description:

```
"params": [ 
    "BTCBCC",       # market name 
    "8000.00000000" # latest price
]
```

### Cancel the Subscription of Latest Price

- method: price.unsubscribe
- params: []

example：

```
{
    "method":"price.unsubscribe", 
    "params":[], 
    "id":100
}
```

response

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### The Subscription of Market Status

**The subscription of 24-hour Market Status.**

- method: state.subscribe
- params: The list of market names of transaction pairs

request

```
{
    "method":"state.subscribe", 
    "params":["BTCBCC","BTCUSD"], 
    "id":100
}
```

response

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### The Push of Market Status

**After successfully subscribing，the status data will be pushed to client end by trading server.**

- method: state.update

example：

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

Please refer to the method of state.query for return value.

### Cancel the Push of Market Status

- method: state.unsubscribe
- params: []

request

```
{
    "method":"state.unsubscribe", 
    "params":[], 
    "id":100
}
```

response

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### The Subscription of the Current Day's Market Status

**The subscription of the current day's market status.**

- method: today.subscribe
- params: 

request parameter：

| name of parameter      | type of parameter | mandatory | description       |
| ----------- | -------- | ---- | ---------- |
| market list | String   | yes   | list of market names |

request

```
{
    "method":"today.subscribe", 
    "params":["BTCUSD","BTCBCC"], 
    "id":100
}
```

response

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### The Push of the Current Day's Market Status

After successfully subscribing， the server actively pushes data to client end.

- method: today.update

example

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

Return value refer to the method of state.query.

### Cancel the Subscription of Current Day's Market Status

- method: today.unsubscribe
- params: []

Request

```
{
    "method":"today.unsubscribe", 
    "params":[], 
    "id":100
}
```

Response

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### Subscription of Settled Deals

- method: deals.subscribe
- params: The list of market names of transaction pairs

request

```
{
    "method":"deals.subscribe", 
    "params":["BTCBCC","BTCUSD"], 
    "id":100
}
```

response

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### The Push of Settled Deals

example：

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

Refer to the returned result of deals.query. 

### Cancel the Subscription on Settled Deals

- method: deals.unsubscribe
- params: []

request

```
{ 
    "method":"deals.unsubscribe", 
    "params":[], 
    "id":100
}
```

response

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

#### Subscription on Order Depth

- method: depth.subscribe
- params: 

request parameter：

```
"params":[ 
    "BTCBCC",  # market name 
    100,       # order price,value：1, 5, 10, 20, 30, 50, 100 
    "0.0001"   #  the precision of price range, value："0","0.00000001","0.0000001","0.000001","0.00001", 
               # "0.0001", "0.001", "0.01", "0.1"
]
```

request

```
{
    "method":"depth.subscribe", 
    "params":["EOSUSDT",100,"0.1"], 
    "id":100
}
```

response

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

#### Subscription on Orders Depth

- method: depths.subscribe
- params: 

request parameter：

```
"params":[
 [ 
    "ETCBTC",  # market name 
    100,       # order price,value：1, 5, 10, 20, 30, 50, 100 
    "0.0001"   #  the precision of price range, value："0","0.00000001","0.0000001","0.000001","0.00001", 
               # "0.0001", "0.001", "0.01", "0.1"
 ],[ 
    "BTCBCC",  # market name 
    100,       # order price,value：1, 5, 10, 20, 30, 50, 100 
    "0.0001"   #  the precision of price range, value："0","0.00000001","0.0000001","0.000001","0.00001", 
               # "0.0001", "0.001", "0.01", "0.1"
 ]
]
```

request

```
{
    "method":"depths.subscribe", 
    "params":[["ETCUSDT",100,"0.1"],["EOSUSDT",100,"0.1"]], 
    "id":100
}
```

response

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```


### The Push of Order Depth Data

- method: depth.update

example：

```
{
    "method": "depth.update", 
    "params": [true, {"asks": [["8000", "20"]], "bids": [["800", "4"]]}, "BTCBCC"], 
    "id": null
}
```

Description of returned result：

Take the example of the precision of price range as 0.1.

```
"params": [ 
    true, #clean: Boolean, true: complete result，false: last returned updated result 
    { 
        "asks": [
            [
                "8000", # price 
                "20"    # number 
            ],            
            [
                "8000.1", # price 
                "20"      # **the sum of the number of all orders with price≥8000，less than 8000.1**
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
    "BTCBCC"             # the market name of transaction pair
]
```

### Cancel the Subscription of Order Depth

- method: depth.unsubscribe
- params: []

request

```
{
    "method":"depth.unsubscribe", 
    "params":[], 
    "id":100
}
```

response

```
{ 
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### The subscription of User's Order Status

- method: order.subscribe
- params: list of market names

request

```
{ 
    "method":"order.subscribe", 
    "params":["BTCBCC","BTCETH"], 
    "id":100
}
```

response

```
{ 
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### The Push on Changes of User's Order

**Every change in the status of order will be pushed to client end**

example：

```
{
    "method": "order.update", 
    "params": [ 
        1, # the event type of change on order status，integer value 1-new order, 2-update order, 3-settled completely 
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

refer to order.query for the detailed description on order field.

### Cancel the Subscription of Changes on Order Status

- method: order.unsubscribe
- params: []

request

```
{
    "method":"order.unsubscribe", 
    "params":[], 
    "id":100
}
```

response

```
{ 
    "error": null,
    "result": { "status": "success" }, 
    "id": 100
}
```

### Subscription on User's Asset Information

- method: asset.subscribe
- params: list of asset names

request

```
{
    "method":"asset.subscribe",
    "params":["BTC","ETH","BCC"], 
    "id":100
}
```

response

```
{ 
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```

### Push of Changes on User Asset

- method: asset.update

example：

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

### Subscription of User Asset Information

- method: asset.unsubscribe

request

```
{
    "method":"asset.unsubscribe", 
    "params":[], 
    "id":100
}
```

response

```
{
    "error": null, 
    "result": { "status": "success" }, 
    "id": 100
}
```
