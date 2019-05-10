# REST API    

## 开始使用    

REST，即Representational State Transfer的缩写，是目前最流行的一种互联网软件架构。它结构清晰、符合标准、易于理解、扩展方便，正得到越来越多网站的采用。其优点如下：    
- 在RESTful架构中，每一个URL代表一种资源；    
- 客户端和服务器之间，传递这种资源的某种表现层；    
- 客户端通过四个HTTP指令，对服务器端资源进行操作，实现“表现层状态转化”。   

建议开发者使用REST API进行币币交易或者资产提现等操作。    
    
## 请求交互    

REST访问的根URL：<https://api.hotbit.io/api/v1>  

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


## 函数列表

|函数名|描述|url|
| :-----    | :-----   | :-----   | 
|[server.time](#servertime)| 获取系统时间 |https://api.hotbit.io/api/v1/server.time| |
|[balance.query](#balancequery)| 获取用户资产 |https://api.hotbit.io/api/v1/balance.query| api_key=5eae7322-6f92-873a-9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&assets=["BTC","ETH"]|
|[asset.list](#assetlist)|获取平台所有资产类型和精度，prec为精确到小数点后多少位|https://api.hotbit.io/api/v1/asset.list| |
|[order.put_limit](#orderput_limit)|限价交易|https://api.hotbit.io/api/v1/order.put_limit|api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=ETH/BTC&side=1&amount=10&price=100 |
|[order.cancel](#ordercancel)|取消交易|https://api.hotbit.io/api/v1/order.cancel| api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=BTC/ETH&order_id=1|
|[order.batch_cancel](#orderbatch_cancel)|批量取消交易|https://api.hotbit.io/api/v1/order.batch_cancel| api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=BTC/ETH&orders_id=[1,2]|
|[order.deals](#orderdeals)|获取已成交的订单细节|https://api.hotbit.io/api/v1/order.deals| api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&order_id=100&limit=10|
|[order.finished_detail](#orderfinished_detail)|根据订单号查询已完成订单|https://api.hotbit.io/api/v1/order.finished_detail| api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&order_id=1|
|[order.book](#orderbook)|获取交易列表|https://api.hotbit.io/api/v1/order.book|market=ETH/BTC&side=1&offset=0&limit=10 |
|[order.depth](#orderdepth)|获取交易深度|https://api.hotbit.io/api/v1/order.depth|market=ETH/BTC&limit=100&interval=1e-8 |
|[order.pending](#orderpending)|查询未实施订单|https://api.hotbit.io/api/v1/order.pending| api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=ETH/BTC&offset=0&limit=100|
|[order.finished](#orderfinished)|查询用户的已完成订单|https://api.hotbit.io/api/v1/order.finished|api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=ETH/BTC&start_time=1511967657&end_time =1512050400&offset=0&limit=100&side=1 |
|[market.list](#marketlist)|获取交易对列表|https://api.hotbit.io/api/v1/market.list| |
|[market.last](#marketlast)|获取指定交易对的最新价格|https://api.hotbit.io/api/v1/market.last| market=ETH/BTC|
|[market.deals](#marketdeals)|查询交易对交易记录|https://api.hotbit.io/api/v1/market.deals| market=ETH/BTC&limit=10&last_id=1521100930|
|[market.user_deals](#marketuser_deals)|查询用户交易记录|https://api.hotbit.io/api/v1/user_deals| |
|[market.kline](#marketkline)|k线查询|https://api.hotbit.io/api/v1/market.kline|market=ETH/BTC&start_time=1521100000&end_time=1521101193&interval=60 |
|[market.status](#marketstatus)|获取过去指定时间段market当前最新涨跌幅，交易量，最高/最低价格等状态|https://api.hotbit.io/api/v1/market.status|market=ETH/BTC&period=10 |
|[market.status_today](#marketstatus_today)|获取今天market状态|https://api.hotbit.io/api/v1/market.status_today|market=ETH/BTC |
|[market.status24h](#marketstatus24h)|获取过去24小时内的market涨跌幅，交易量，最高/最低价格等状态|https://api.hotbit.io/api/v1/market.status24h||
|[market.summary](#marketsummary)|market概要|https://api.hotbit.io/api/v1/market.summary||
|[allticker](#allticker)|获取全市场交易对的最新成交信息|https://api.hotbit.io/api/v1/allticker||

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

**系统类API，获取系统资源**



### server.time

| 方法名 | 方法类型 | 描述 |
| --- | --- | --- |
| server.time | get  | 获取系统时间 |

请求参数：无

示例：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/server.time |  |

响应数据：


| 参数名称 | 数据类型 | 是否必须 | 描述 |
| --- | --- | --- | --- |
| result | long | true | 获取系统时间 |

示例：

```
 Response:{"error": null, "result": 1520919059}
```



## Asset API

**用户资产类API，实现针对各种类型用户资产的查询，更新，所有和用户资产相关操作历史记录获取等。**

### balance.query

| 方法名 | 方法类型 | 描述 |
| --- | --- | --- |
| balance.query | post  | 获取用户资产 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| api_key | string | 用户申请的API KEY |
| sign | string | 请求字符串的签名值 |
| assets | json array | 代币符号数组，空数组表示获取全部代币资产 |

示例：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/balance.query  | api_key=5eae7322-6f92-873a-9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&assets=["BTC","ETH"] |



### balance.history

| 方法名 | 方法类型 | 描述 |
| --- | --- | --- |
| balance.history | post  | 获取用户资金变动流水 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| api_key | string | 用户API KEY |
| sign | string | 用户签名值 |
| asset | string | 资产名称，如：&quot;BTC&quot;,&quot;ETH&quot;等，&quot;&quot;表示所有资产名称 |
| business | string | 业务名称，如：&quot;deposit&quot;或&quot;trade&quot;，&quot;&quot;表示所有业务名称 |
| start_time | Integer | 开始时间(秒) |
| end_time | Integer | 截止时间(秒) |
| offset | Integer | 偏移位置，如果设置为0，表示从最新近一笔业务开始算起，往之前时间的所有交易记录，总笔数不能大于limit；。如果设置为1，表示从次新一笔业务开始算起，往之前时间的所有交易记录总笔数不能大于limit；以此类推 |
| limit | Integer | 返回的最大交易笔数 |

示例：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/balance.history  | api_key=5eae7322-6f92-873a-9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&asset=BTC&business=deposit&start_time=1511967657&end_time =1512050400&offset=0&limit=100 |



### asset.list

| 方法名 | 方法类型 | 描述 |
| --- | --- | --- |
| asset.list | get  | 获取平台所有资产类型和精度，prec为精确到小数点后多少位 |

请求参数：无

示例：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/asset.list  |  |

响应数据：

示例：


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



## Trade API

**交易类API**

### order.put_limit

| 方法名 | 方法类型 | 描述 |
| --- | --- | --- |
| order.put_limit | post  | 限价交易 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| api_key | string | 用户API KEY |
| sign | string | 用户签名值 |
| market | string  | market名称，如："BTC/USDT","ETH/USDT" |
| side | Integer  | 1 = &quot;sell&quot;，2=&quot;buy&quot; |
| amount | double  | 申请交易的数量 (**注意必须是最小量的倍数**)|
| price | double  | 交易价格 |

**同一交易对下面只能同时存在200个挂单**

示例：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/order.put_limit  | api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=ETH/BTCH&side=1&amount=10&price=100  |
响应数据：

示例：


```
Response:
{
    "error": null,
    "result": 
	 {
	   "id":8688803,    #order-ID
	    "market":"ETHBTC",
	    "source":"web",    #数据请求来源标识
	    "type":1,	       #下单类型 1-限价单
	    "side":2,	       #买卖方标识 1-卖方，2-买方
	    "user":15731,
	    "ctime":1526971722.164765, #订单创建时间(秒)
	    "mtime":1526971722.164765, #订单更新时间(秒)
	    "price":"0.080003",
	    "amount":"0.4",
	    "taker_fee":"0.0025",
	    "maker_fee":"0",
	    "left":"0.4",
	    "deal_stock":"0",
	    "deal_money":"0",
	    "deal_fee":"0",
	    "status":0     #订单状态标志 当与0x8为真的时候表示当前订单是取消的
        },
    "id": 1521169460
}
```


### order.cancel

| 方法名 | 方法类型 | 描述 |
| --- | --- | --- |
| order.cancel | post  | 取消交易 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| api_key | string | 用户API KEY |
| sign | string | 用户签名值 |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| order_id | Integer  | 要取消交易的id。参看&quot;order.put_limit&quot;方法的返回结果。|

示例：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/order.cancel  | api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=ETH/BTCH&order_id=1  |

响应数据：

示例：


```
Response:
{
    "error": null,
    "result": 
	 {
	   "id":8688803,    #order-ID
	    "market":"ETHBTC",
	    "source":"web",    #数据请求来源标识
	    "type":1,	       #下单类型 1-限价单
	    "side":2,	       #买卖方标识 1-卖方，2-买方
	    "user":15731,
	    "ctime":1526971722.164765, #订单创建时间(秒)
	    "mtime":1526971722.164765, #订单更新时间(秒)
	    "price":"0.080003",
	    "amount":"0.4",
	    "taker_fee":"0.0025",
	    "maker_fee":"0",
	    "left":"0.4",
	    "deal_stock":"0",
	    "deal_money":"0",
	    "deal_fee":"0",
	     "status":0     #订单状态标志 当与0x8为真的时候表示当前订单是取消的
        },
    "id": 1521169460
}
```

### order.batch_cancel

| 方法名 | 方法类型 | 描述 |
| --- | --- | --- |
| order.batch_cancel | post  | 批量取消交易 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| api_key | string | 用户API KEY |
| sign | string | 用户签名值 |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| orders_id | arrary  | 要取消交易的id,数量最多10个订单取消，参看&quot;order.put_limit&quot;方法的返回结果。|

示例：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/order.batch_cancel  | api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=ETH/BTC&orders_id=[1,2]  |

响应数据：

示例：


```
Response:
{
    "error": null,
    "result": 
	 [
            {#正确反馈
                   "id":8688803,    #order-ID
                    "market":"ETHBTC",
                    "source":"web",    #数据请求来源标识
                    "type":1,	       #下单类型 1-限价单
                    "side":2,	       #买卖方标识 1-卖方，2-买方
                    "user":15731,
                    "ctime":1526971722.164765, #订单创建时间(秒)
                    "mtime":1526971722.164765, #订单更新时间(秒)
                    "price":"0.080003",
                    "amount":"0.4",
                    "taker_fee":"0.0025",
                    "maker_fee":"0",
                    "left":"0.4",
                    "deal_stock":"0",
                    "deal_money":"0",
                    "deal_fee":"0",
		     "status":0     #订单状态标志 当与0x8为真的时候表示当前订单是取消的
            },
            {	#发生错误反馈
                "error": {	
		   "code":10
		   "message":"order not found"
		}
	  	"result":null,
	         "id": 1521169460
            }
        ],
    "id": 1521169460
}
```

### order.deals

| 方法名 | 方法类型 | 描述 |
| --- | --- | --- |
| order.deals | post  | 获取已成交的订单细节 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| api_key | string | 用户API KEY |
| sign | string | 用户签名值 |
| order_id | Integer  | 交易ID，参看 "order.put_limit"方法的返回结果|
| offset | Integer  | 等于0，表示从最近一次交易往之前查找 |
| limit | Integer  | 最多返回 &quot;records&quot;的数量 |

示例：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/order.deals | api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&order_id=100&limit=10 |

响应数据：

示例：


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
                "time": 1521107410.357024,#(秒)
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


### order.finished_detail

| 方法名 | 方法类型 | 描述 |
| --- | --- | --- |
| order.finished_detail | post  | 根据订单号查询已完成订单 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| api_key | string | 用户API KEY |
| sign | string | 用户签名值 |
| order_id | Integer  | 交易ID，参看 "order.put_limit"方法的返回结果|


示例：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/order.finished_detail | api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&order_id=1 |

响应数据：

示例：


```
Response:
{
    "error": null,
    "result": {
        "id": 1,
        "ctime": 1535545564.4409361,#(秒)
        "ftime": 1535545564.525017,#(秒)
        "user": 15731,
        "market": "YCCETH",
        "source": "test",
        "type": 1,
        "side": 2,
        "price": "0.0000509",
        "amount": "1",
        "taker_fee": "0.001",
        "maker_fee": "0.001",
        "deal_stock": "1",
        "deal_money": "0.0000509",
        "deal_fee": "0.001",
	 "status":0     #订单状态标志 当与0x8为真的时候表示当前订单是取消的
    },
    "id": 1536050997
}
```


### order.book

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
| https://api.hotbit.io/api/v1/order.book?market=ETH/BTC&side=1&offset=0&limit=10   |   |

响应数据：

示例：


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



### order.depth

| 方法名 | 方法类型 | 描述 |
| --- | --- | --- |
| order.depth | get  | 获取交易深度 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| limit | Integer |  最多返回 &quot;records&quot;的数量，可取值：1, 5, 10, 20, 30, 50, 100 |
| interval | string  | 价格精度，可取值：0，0.1，0.01，0.001，……，0.0000000001 |

示例：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/order.depth?market=ETH/BTC&limit=100&interval=1e-8  |   |

响应数据：

示例：


```
Response:
{
	"error": null, 
	"result": 
		{
			"asks": [["0.0733858", "0.319"], ["0.0741178", "0.252"], ["0.0742609", "0.03"], ... ["0.1250465", "0.272"]], 
			"bids": [["0.0730197", "0.275"], ["0.0723", "1.052"], ["0.0722876", "0.302"], ... ["2.0e-7", "1"]]}, 
	"id": 1527559250
}
```



### order.pending

| 方法名 | 方法类型 | 描述 |
| --- | --- | --- |
| order.pending | post  | 查询未实施订单 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| api_key | string | 用户API KEY |
| sign | string | 用户签名值 |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| offset | Integer | 偏移位置，如果设置为0，表示从最新近一笔业务开始算起，往之前时间的所有交易记录，总笔数不能大于limit；。如果设置为1，表示从次新一笔业务开始算起，往之前时间的所有交易记录总笔数不能大于limit；以此类推..... |
| limit | Integer  | 最多返回 &quot;records&quot;的数量 |
示例：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/order.pending  | api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=fdcafaf85a38970e4d84f6f286a2879e&market=ETH/BTC&offset=0&limit=100  |

响应数据：

示例：

```
{
    "error":null,
    "result":{
        "ETHBTC":{
            "limit":50,
            "offset":0,
            "total":1,
            "records":[
                {
                    "id":8688803,    #order-ID
                    "market":"ETHBTC",
                    "source":"web",    #数据请求来源标识
                    "type":1,	       #下单类型 1-限价单
                    "side":2,	       #买卖方标识 1-卖方，2-买方
                    "user":15731,
                    "ctime":1526971722.164765, #订单创建时间
                    "mtime":1526971722.164765, #订单更新时间
                    "price":"0.080003",
                    "amount":"0.4",
                    "taker_fee":"0.0025",
                    "maker_fee":"0",
                    "left":"0.4",
                    "deal_stock":"0",
                    "deal_money":"0",
                    "deal_fee":"0",
		     "status":0     #订单状态标志 当与0x8为真的时候表示当前订单是取消的
                }
            ]
        }
    },
    "id":1526971756
}
```


### order.finished

| 方法名 | 方法类型 | 描述 |
| --- | --- | --- |
| order.finished | post  | 查询用户的已完成订单 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| api_key | string | 用户API KEY |
| sign | string | 用户签名值 |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| start_time | Integer | 开始时间(秒) |
| end_time | Integer | 截止时间(秒) |
| offset | Integer | 偏移位置，如果设置为0，表示从最新近一笔业务开始算起，往之前时间的所有交易记录，总笔数不能大于limit；。如果设置为1，表示从次新一笔业务开始算起，往之前时间的所有交易记录总笔数不能大于limit；以此类推 |
| limit | Integer  | 最多返回 &quot;records&quot;的数量 |
| side | Integer  | 1 = &quot;sell&quot;，2=&quot;buy&quot; |

示例：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/order.finished  | api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=ETH/BTC&start_time=1511967657&end_time =1512050400&offset=0&limit=100&side=1  |



## Market API

**市场类API**

### market.list

| 方法名 | 方法类型 | 描述 |
| --- | --- | --- |
| market.list | get  | 获取交易对列表 |

请求参数：无

示例：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/market.list | |

响应数据：

示例：


```
Response:
{
    "error": null,
    "result": [
        {
            "name": "QASHBTC",
            "stock": "QASH",
            "money": "BTC",
            "fee_prec": 4,  #税率精度4位小数
            "stock_prec": 2, #stock精度
            "money_prec": 8, #money精度
            "min_amount": "0.1" #下单最小值
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



### market.last
| 方法名 | 方法类型 | 描述 |
| --- | --- | --- |
| market.last | get  | 获取指定交易对的最新价格 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |

示例：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/market.last?market=ETH/BTC |  |

响应数据：

示例：


```
Response:
{
    "error": null,
    "result": "0.07413600",
    "id": 1521169525
}
```



### market.deals

| 方法名 | 方法类型 | 描述 |
| --- | --- | --- |
| market.deals | get  | 查询交易对交易记录 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| limit | Integer  | 查询个数限制(limit <= 1000) |
| last_id | Integer  | 返回大于order_id > last_id的交易数据 |

示例：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/market.deals?market=ETH/BTC&limit=10&last_id=1521100930 |  |

响应数据：

示例：


```
 Response:
{
    "error": null,
    "result": [],
    "id": 1521169562
}
```



### market.user_deals

| 方法名 | 方法类型 | 描述 |
| --- | --- | --- |
| market.user_deals | post  | 查询用户交易记录 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| offset | Integer  |偏移位置，如果设置为0，表示从最新一个订单开始算起，往之前时间的所有交易记录，总笔数不能大于limit；如果设置为1，表示从次新一个订单开始算起，往之前时间的所有满足条件订单记录，总数不能大于limit；以此类推..... |
| limit | Integer   | 查询个数限制(limit <= 1000) |

示例：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/user_deals | |


### market.kline

| 方法名 | 方法类型 | 描述 |
| --- | --- | --- |
| market.kline | get  | k线查询 |

请求参数：

| 参数名 | 参数类型 | 描述 |
| --- | --- | --- |
| market | string  | market名称，如：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| start_time | Integer  | 起始时间戳(秒)  |
| end_time | Integer  | 结束时间戳(秒)  |
| Interval | Integer   | 周期间隔,单位秒, (起始时间到结束时间，总周期数) < 1000 |

示例：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/market.kline?market=ETH/BTC&start_time=1521100000&end_time=1521101193&interval=60 | |

响应数据：

示例：


```
Response:
{
    "error": null,
    "result": [],
    "id": 1521169586
}
```



### market.status

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
| https://api.hotbit.io/api/v1/market.status?market=ETH/BTC&period=10 | |

响应数据：

示例：


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



### market.status_today

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
| https://api.hotbit.io/api/v1/market.status_today?market=ETH/BTC | |

响应数据：

示例：


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



### market.status24h

| 方法名           | 方法类型 | 描述                                                        |
| ---------------- | -------- | ----------------------------------------------------------- |
| market.status24h | get      | 获取过去24小时内的market涨跌幅，交易量，最高/最低价格等状态 |

请求参数：无

示例：

| url                                           | body |
| --------------------------------------------- | ---- |
| https://api.hotbit.io/api/v1/market.status24h |      |

响应数据：

示例：


```
Response:
{
    "TRXETH": {
        "period": 86400,
        "last": "0.00013199",
        "open": "0.00013523",
        "close": "0.00013199",
        "high": "0.00013723",
        "low": "0.00013199",
        "volume": "887054.18",
        "deal": "119.2565600483"
    },
    "ATNETH": {
        "period": 86400,
        "last": "0.00069484",
        "open": "0.00069776",
        "close": "0.00069484",
        "high": "0.00069952",
        "low": "0.00069449",
        "volume": "153483.514",
        "deal": "106.97614821094"
    },
    "TNBETH": {
        "period": 86400,
        "last": "0.00010258",
        "open": "0.00009194",
        "close": "0.00010258",
        "high": "0.00010538",
        "low": "0.00008869",
        "volume": "761802.93",
        "deal": "73.4726442434"
    },
……
    "GVTETH": {
        "period": 86400,
        "last": "0.034525",
        "open": "0.032989",
        "close": "0.034525",
        "high": "0.034567",
        "low": "0.032878",
        "volume": "612.44",
        "deal": "20.60413469"
    }
}
```



### market.summary

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
| https://api.hotbit.io/api/v1/market.summary | |


响应数据：

示例：


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

### allticker

| 方法名 | 方法类型 | 描述 |
| --- | --- | --- |
| allticker | get  | 获取全市场交易对的最新成交信息 |


请求参数：

无

示例：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/allticker | |


响应数据：

示例：


```
Response:
{
    ticker: [
    {
        symbol: "HTB_ETH",      # 交易对名称
        buy: "0.0000077393",    # 买一价
        sell: "0.0000078169",   # 卖一价
        open: "0.0000078205",   # 开盘价
        close: "0.0000077506",  # 收盘价
        high: "0.0000080946",   # 最高价
        low: "0.000007551",     # 最低价
        last: "0.0000077506",   # 最新价
        vol: "98842592"         # 成交量
    },
    {
        symbol: "DELTA_ETH",
        buy: "6.86e-8",
        sell: "6.89e-8",
        open: "7.13e-8",
        close: "6.87e-8",
        high: "7.19e-8",
        low: "6.87e-8",
        last: "6.87e-8",
        vol: "143957900"
    }]
}
```
