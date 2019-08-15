# REST API    

## Begin to Use

REST is the abbreviation of Representational State Transfer, which is one of the most popular types of Internet software frameworks. REST is currently being adopted by an increasing number of websites due to its clear structure, compliance with standards, simpleness and outstanding scalability. The advantages of REST are listed as follows:
- Each URL represents one type of resource in the framework of RESTful;
- A certain type of presentation layer of the resource is delivered between the client end and the server;
- The client end conduct operations on the resources of server end through four HTTP instructions for the "transformation on the status of presentation layer".
The developers are recommended to use REST API for operations such as the transactions between different types of cryptocurrency tokens or asset withdrawals. 
   
## Interaction Request

The root URL that REST visits: <https://api.hotbit.io/api/v1>
All requests are based on Https protocol, the contentType of request header information is required to be configured as: application/x-www-form-urlencoded

Description of Interaction Request

1. Request parameter: conduct parameter encapsulation according to the parameter regulated by interface request.
2. Submit the requested parameter: Submit the encapsulated parameter to the server through GET or POST.
3. Server Response: Firsrt, the server conducts parameter security verification on the data requested by the user, after the verification, the server returns the responded data to the user in JSON format based on business logic.
4. Data Process: Process the data responded by the server

## Request

|Parameter Name|Description|
| -----    | -----  |
|method|Name of API Method|
|params|Parameters|


## List of Functions

|Function Name|Description|url|
| :-----    | :-----   | :-----   | 
|[server.time](#servertime)| Obtain System Time |https://api.hotbit.io/api/v1/server.time| |
|[balance.query](#balancequery)| Obtain User Assets |https://api.hotbit.io/api/v1/balance.query| api_key=5eae7322-6f92-873a-9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&assets=["BTC","ETH"]|
|[asset.list](#assetlist)|Obtain the types and precisions of all assets on the platform, prec refers to the number of decimal digits after the decimal point|https://api.hotbit.io/api/v1/asset.list| |
|[order.put_limit](#orderput_limit)|Limit Order Transaction|https://api.hotbit.io/api/v1/order.put_limit|api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=ETH/BTC&side=1&amount=10&price=100&isfee=0 |
|[order.cancel](#ordercancel)|Cancel Transaction|https://api.hotbit.io/api/v1/order.cancel| api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=BTC/ETH&order_id=1|
|[order.batch_cancel](#orderbatch_cancel)|Cancel transactions in large quantities|https://api.hotbit.io/api/v1/order.batch_cancel| api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=BTC/ETH&orders_id=[1,2]|
|[order.deals](#orderdeals)|Obtain the details of settled orders|https://api.hotbit.io/api/v1/order.deals| api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&order_id=100&limit=10&offset=0|
|[order.finished_detail](#orderfinished_detail)|Check finished order according to order number|https://api.hotbit.io/api/v1/order.finished_detail| api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&order_id=1|
|[order.book](#orderbook)|Obtain Transaction List|https://api.hotbit.io/api/v1/order.book|market=ETH/BTC&side=1&offset=0&limit=10 |
|[order.depth](#orderdepth)|Obtain Transaction Depth|https://api.hotbit.io/api/v1/order.depth|market=ETH/BTC&limit=100&interval=1e-8 |
|[order.pending](#orderpending)|Check unexecuted orders|https://api.hotbit.io/api/v1/order.pending| api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=ETH/BTC&offset=0&limit=100|
|[order.finished](#orderfinished)|Check the user's finished orders|https://api.hotbit.io/api/v1/order.finished|api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=ETH/BTC&start_time=1511967657&end_time =1512050400&offset=0&limit=100&side=1 |
|[market.list](#marketlist)|Obtain the list of transaction pairs|https://api.hotbit.io/api/v1/market.list| |
|[market.last](#marketlast)|Obtain the latest price of designated transaction pair|https://api.hotbit.io/api/v1/market.last| market=ETH/BTC|
|[market.deals](#marketdeals)|Check the transaction records of the transaction pair|https://api.hotbit.io/api/v1/market.deals| market=ETH/BTC&limit=10&last_id=1521100930|
|[market.user_deals](#marketuser_deals)|Check the user's transaction records|https://api.hotbit.io/api/v1/market.user_deals|api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=ETH/BTC&offset=0&limit=100 |
|[market.kline](#marketkline)|Check K Chart|https://api.hotbit.io/api/v1/market.kline|market=ETH/BTC&start_time=1521100000&end_time=1521101193&interval=60 |
|[market.status](#marketstatus)|Obtain latest market status within designated period in the past, such as latest range of increase and decline, transaction volume, highest/lowest price etc.|https://api.hotbit.io/api/v1/market.status|market=ETH/BTC&period=10 |
|[market.status_today](#marketstatus_today)|Obtain today's market status|https://api.hotbit.io/api/v1/market.status_today|market=ETH/BTC |
|[market.status24h](#marketstatus24h)|Obtain the market status within the previous 24 hours, such as range of increase and decline, transaction volume, highest and lowest price etc.|https://api.hotbit.io/api/v1/market.status24h||
|[market.summary](#marketsummary)|Market summary|https://api.hotbit.io/api/v1/market.summary||
|[allticker](#allticker)|Obtain the latest trading information of all transaction pairs|https://api.hotbit.io/api/v1/allticker||

## Response
Response can be found in the body section of "200 ok", which consists of three parameters:
|Parameter Name|Parameter Type|Description|
| :-----    | :-----   | :-----    |
|result|json object| The result of API call, when error occurs, the result is null|
|error|json object| The information of API call error, when API call is correct, the information is null|
|id|Integer|Request id|

```
Example of correct result:
{
    "error": null,
    "result": {.....},
    "id": 123
}

Example of incorrect result:
{
    "error": {
        "code": 1,
        "message": "invalid argument"
    },
    "result": null,
    "id": 123
}
```



## API Reference

**System API, obtains system resource**



### server.time

| Name of Method | Type of Method | Description |
| --- | --- | --- |
| server.time | get  | Obtain System Time |

Parameter requested: No

Example:

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/server.time |  |

Data Responded:


| Name of Parameter | Type of Data | Must or not | Description |
| --- | --- | --- | --- |
| result | long | true | Obtain System Time |

Example:

```
 Response:{"error": null, "result": 1520919059}
```



## Asset API

**User asset API enables the checking and update of all types of user assets and the obtention of all history records of any operations that are related to the user's assets**

### balance.query

| Name of Method | Type of Method | Description |
| --- | --- | --- |
| balance.query | post  | Obtain User Assets |

Requested parameter:

| Name of Parameter | Type of Parameter | Description |
| --- | --- | --- |
| api_key | string | The API KEY applied by the user |
| sign | string | Request the signature value of the string |
| assets | json array | The array of token abbreviation, empty arran means to obtain all token assets |

Example:

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/balance.query  | api_key=5eae7322-6f92-873a-9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&assets=["BTC","ETH"] |



### balance.history

| Name of Method | Type of Method | Description |
| --- | --- | --- |
| balance.history | post  | Obtain the records regarding the changes in user assets |

Requested Parameter:

| Parameter Name | Parameter Type | Description |
| --- | --- | --- |
| api_key | string | User's API KEY |
| sign | string | User's signature value |
| asset | string | Asset name, such as: &quot;BTC&quot;,&quot;ETH&quot;，&quot;&quot;means the names of all assets |
| business | string | Business name, such as：&quot;deposit&quot;or&quot;trade&quot;，&quot;&quot;means all business names |
| start_time | Integer | Start time(second) |
| end_time | Integer | End Time(second) |
| offset | Integer | Offset position，if value set as 0，it means that the total number of transactions from the earliest transaction to the most recent transaction cannot be greater than limit;。if value set as 1, it means that the total number of transactions from the earliest transaction to the second most recent transaction cannot be greater than limit; and so on |
| limit | Integer | Maximum number of transactions returned |

Example:

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/balance.history  | api_key=5eae7322-6f92-873a-9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&asset=BTC&business=deposit&start_time=1511967657&end_time =1512050400&offset=0&limit=100 |



### asset.list

| Name of Method | Type of Method | Description |
| --- | --- | --- |
| asset.list | get  | Obtain the types and precisions of all assets on the platform, prec refers to the number of decimal digits after the decimal point|

Parameter Requested: No

Example:

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/asset.list  |  |

ata Responded:

Example:


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

**Trade API**

### order.put_limit

| Name of Method | Type of Method | Description |
| --- | --- | --- |
| order.put_limit | post  | Limit Order Transaction |

Requested Parameter:

| Name of Parameter | Type of Parameter | Description |
| --- | --- | --- |
| api_key | string | User's API KEY |
| sign | string | User's signature value |
| market | string  | market name，such as："BTC/USDT","ETH/USDT" |
| side | Integer  | 1 = &quot;sell&quot;，2=&quot;buy&quot; |
| amount | double  | Amount of tokens applied for trading (**Note that the amount must be the multiple of minimum amount**)|
| price | double  | Trading price |
| isfee | Integer  | Use deductable token to deduct or not 0 = &quot;no(no)&quot;，1=&quot;yes(yes)&quot;|

**Only 200 orders are allowed to be placed simultaneously under the same transaction pair**

Example:

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/order.put_limit  | api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=ETH/BTC&side=1&amount=10&price=100&isfee=0  |
Data Responded:

Example:


```
Response:
{
    "error": null,
    "result": 
	 {
	   "id":8688803,    #order-ID
	    "market":"ETHBTC",
	    "source":"web",    #The source identification of data request
	    "type":1,	       #Type of order pladement 1-limit order
	    "side":2,	       #Identification of buyers and sellers 1-Seller，2-buyer
	    "user":15731,
	    "ctime":1526971722.164765, #Time of order establishment(second)
	    "mtime":1526971722.164765, #Time of order update(second)
	    "price":"0.080003",
	    "amount":"0.4",
	    "taker_fee":"0.0025",
	    "maker_fee":"0",
	    "left":"0.4",
	    "deal_stock":"0",
	    "deal_money":"0",
	    "deal_fee":"0",
	    "status":0    , #Sign of order status when 0x8 is true, it means the current order is cancelled, when 0x80 is true, it means that the current order is deducted by deductable tokens	    "fee_stock":"HTB",	#Name of deductable token
	    "alt_fee":"0.5",	#The discount of deductable tokens
	    "deal_fee_alt":"0.123" #Amount deducted
        },
    "id": 1521169460
}
```


### order.cancel

| Name of Method | Type of Method | Description |
| --- | --- | --- |
| order.cancel | post  | Cancel Transaction |

Parameter Requested:

| Name of Parameter | Type of Parameter | Description |
| --- | --- | --- |
| api_key | string | User's API KEY |
| sign | string | User's signature value |
| market | string  | market name，such as：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| order_id | Integer  | The id whose transactions are required to be cancelled 。Refer to&quot;order.put_limit&quot;for the returned result of the method。|

Example:

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/order.cancel  | api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=ETH/BTC&order_id=1  |

Data Responded:

Example:


```
Response:
{
    "error": null,
    "result": 
	 {
	   "id":8688803,    #order-ID
	    "market":"ETHBTC",
	    "source":"web",    #The source identification of data request
	    "type":1,	       #Type of order pladement 1-limit order
	    "side":2,	       #The sign of buyer and seller 1-seller，2-buyer
	    "user":15731,
	    "ctime":1526971722.164765, #Time of order establishment(second)
	    "mtime":1526971722.164765, #Time of order update(second)
	    "price":"0.080003",
	    "amount":"0.4",
	    "taker_fee":"0.0025",
	    "maker_fee":"0",
	    "left":"0.4",
	    "deal_stock":"0",
	    "deal_money":"0",
	    "deal_fee":"0",
	    "status":0    , #Sign of order status  when 0x8 is true, it means the current order is cancelled, when 0x80 is true, it means that the current order is deducted by deductable tokens	    "fee_stock":"HTB",	#Name of deductable token
	    "alt_fee":"0.5",	#The discount of deductable tokens
	    "deal_fee_alt":"0.123" #The amount deducted
        },
    "id": 1521169460
}
```

### order.batch_cancel

| Name of Method | Type of Method | Description |
| --- | --- | --- |
| order.batch_cancel | post  | Cancel transactions in large quantities |

Parameter requested:

| Name of Parameter | Type of parameter | Description |
| --- | --- | --- |
| api_key | string | User's API KEY |
| sign | string | User's signature value |
| market | string  | market name，such as：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| orders_id | arrary  | For those id that requires to cancel transactions, the maximum number of orders allowed to be cancelled is 10，refer to&quot;order.put_limit&quot;for the returned result of the method.|

Example:

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/order.batch_cancel  | api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=ETH/BTC&orders_id=[1,2]  |

Data Responded:

Example:


```
Response:
{
    "error": null,
    "result": 
	 [
            {#Correct feedback
                   "id":8688803,    #order-ID
                    "market":"ETHBTC",
                    "source":"web",    #The source identification of data request
                    "type":1,	       #Type of order placement 1-limit order
                    "side":2,	       #sign of buyer and seller 1-seller，2-buyer
                    "user":15731,
                    "ctime":1526971722.164765, #Time of order establishment(second)
                    "mtime":1526971722.164765, #Time of order update(second)
                    "price":"0.080003",
                    "amount":"0.4",
                    "taker_fee":"0.0025",
                    "maker_fee":"0",
                    "left":"0.4",
                    "deal_stock":"0",
                    "deal_money":"0",
                    "deal_fee":"0",
		    "status":0    , #Sign of order status  when 0x8 is true, it means the current order is cancelled, when 0x80 is true, it means that the current order is deducted by deductable tokens		    "fee_stock":"HTB",	#Name of deductable token
	 	    "alt_fee":"0.5",	#The discount of deductable token
	            "deal_fee_alt":"0.123" #The amount deducted
            },
            {	#Error feedback occured
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

| Name of method | Type of method | Description |
| --- | --- | --- |
| order.deals | post  | Obtain the details of settled orders |

Parameter requested:

| Name of parameter | Type of parameter | Description |
| --- | --- | --- |
| api_key | string | User's API KEY |
| sign | string | User's signature value |
| order_id | Integer  | Transaction ID，refer to "order.put_limit"for the returned result of the method|
| offset | Integer  | equals to 0，means search from the latest transaction to previous transactions |
| limit | Integer  | Maximum number of &quot;records&quot; returned |

Example:

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/order.deals | api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&order_id=100&limit=10&offset=0 |

Data responded:

Example:


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

| Name of method | Type of method | Description |
| --- | --- | --- |
| order.finished_detail | post  | Check finished orders according to order number |

请求参数：Parameter requested:

| Name of parameter | Type of parameter | Description |
| --- | --- | --- |
| api_key | string | User's API KEY |
| sign | string | User's signature value |
| order_id | Integer  | Transaction ID，refer to "order.put_limit"for the returned result of the method|


Example:

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/order.finished_detail | api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&order_id=1 |

Data responded:

Example:


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
	"status":0    , #Sign of order status when 0x8 is true, it means the current order is cancelled, when 0x80 is true, it means that the current order is deducted by deductable tokens   	"fee_stock":"HTB",	#Name of deductable token
   	"alt_fee":"0.5",	#The discount of deductable token
   	"deal_fee_alt":"0.123" #Amount deducted
    },
    "id": 1536050997
}
```


### order.book

| Name of the method | type of method | description |
| --- | --- | --- |
| order.book | get  | obtain list of transaction |

Requested parameter:

| name of parameter | type of parameter | description |
| --- | --- | --- |
| market | string  | market name，such as：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| side | Integer  | 1 = &quot;sell&quot;，2=&quot;buy&quot; |
| offset | Integer |  Offset position，if value set as 0，it means that the total number of transactions from the earliest transaction to the most recent transaction cannot be greater than limit;。if value set as 1, it means that the total number of transactions from the earliest transaction to the second most recent transaction cannot be greater than limit; and so on.....|
| limit | Integer  | The maximum number of &quot;records&quot;to be returned |

Example:

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/order.book?market=ETH/BTC&side=1&offset=0&limit=10   |   |

Data responded

Example:


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

| Name of method | type of method | description |
| --- | --- | --- |
| order.depth | get  | obtain trading depth |

parameter requested:

| name of parameter | type of parameter | description |
| --- | --- | --- |
| market | string  | market name，such as：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| limit | Integer |  maximum number of &quot;records&quot;to be returned，value allowed：1, 5, 10, 20, 30, 50, 100 |
| interval | string  | price precision，value allowed：0，0.1，0.01，0.001，……，0.0000000001 |

Example:

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/order.depth?market=ETH/BTC&limit=100&interval=1e-8  |   |

data responded:

Example:


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

| name of method | type of method | description |
| --- | --- | --- |
| order.pending | post  | Check unexecuted order |

parameter requested:

| name of parameter | type of parameter | description |
| --- | --- | --- |
| api_key | string | user's API KEY |
| sign | string | user's signature value |
| market | string  | market name，such as：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| offset | Integer | Offset position，if value set as 0，it means that the total number of transactions from the earliest transaction to the most recent transaction cannot be greater than limit;。if value set as 1, it means that the total number of transactions from the earliest transaction to the second most recent transaction cannot be greater than limit; and so on.....|
| limit | Integer  | maximum number of &quot;records&quot;to be returned |
Example:

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/order.pending  | api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=fdcafaf85a38970e4d84f6f286a2879e&market=ETH/BTC&offset=0&limit=100  |

Data responded:

Example:

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
                    "source":"web",    #source identification of data request
                    "type":1,	       #type of order placement 1-limit order
                    "side":2,	       #sign of buyer and seller 1-seller，2-buyer
                    "user":15731,
                    "ctime":1526971722.164765, #Time of order establishment
                    "mtime":1526971722.164765, #Time of order establishment
                    "price":"0.080003",
                    "amount":"0.4",
                    "taker_fee":"0.0025",
                    "maker_fee":"0",
                    "left":"0.4",
                    "deal_stock":"0",
                    "deal_money":"0",
                    "deal_fee":"0",
		    "status":0    , #Sign of order status when 0x8 is true, it means the current order is cancelled, when 0x80 is true, it means that the current order is deducted by deductable tokens		    "fee_stock":"HTB",	#name of deductable token
	 	    "alt_fee":"0.5",	#Discount of the deductable token
	            "deal_fee_alt":"0.123" #amount deducted
                }
            ]
        }
    },
    "id":1526971756
}
```


### order.finished

| name of method | type of method | description |
| --- | --- | --- |
| order.finished | post  | Check the user's finished orders |

parameter requested:

| name of parameter | type of parameter | description |
| --- | --- | --- |
| api_key | string | user's API KEY |
| sign | string | user's signature value |
| market | string  | market name，such as：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| start_time | Integer | starting time(second) |
| end_time | Integer | ending time(second) |
| offset | Integer | Offset position，if value set as 0，it means that the total number of transactions from the earliest transaction to the most recent transaction cannot be greater than limit;。if value set as 1, it means that the total number of transactions from the earliest transaction to the second most recent transaction cannot be greater than limit; and so on |
| limit | Integer  | Maximum number of &quot;records&quot;returned |
| side | Integer  | 1 = &quot;sell&quot;，2=&quot;buy&quot; |

Example:

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/order.finished  | api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=ETH/BTC&start_time=1511967657&end_time =1512050400&offset=0&limit=100&side=1  |



## Market API

**Market API**

### market.list

| Name of method | type of method | description |
| --- | --- | --- |
| market.list | get  | Obtain the list of transaction pairs |

Parameter requested: no

Example:

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/market.list | |

Data responded:

Example:


```
Response:
{
    "error": null,
    "result": [
        {
            "name": "QASHBTC",
            "stock": "QASH",
            "money": "BTC",
            "fee_prec": 4,  #the precision of the rate of transaction fee is 4 digits after decimal point
            "stock_prec": 2, #stock precision
            "money_prec": 8, #money precision
            "min_amount": "0.1" #Minimum value of order placement
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
| name of method | type of method | description |
| --- | --- | --- |
| market.last | get  | obtain the latest price of designated transaction pair |

parameter requested:

| name of parameter | type of parameter | description |
| --- | --- | --- |
| market | string  | market name，such as：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |

Example：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/market.last?market=ETH/BTC |  |

Data responded：

Example：


```
Response:
{
    "error": null,
    "result": "0.07413600",
    "id": 1521169525
}
```



### market.deals

| name of method | type of method | description |
| --- | --- | --- |
| market.deals | get  | check the transaction records of the transaction pair |

parameter requested：

| name of parameter | type of parameter | description |
| --- | --- | --- |
| market | string  | market name，such as：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| limit | Integer  | check limit in number(limit <= 1000) |
| last_id | Integer  | return to the trading data which is greater than order_id > last_id |

Example：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/market.deals?market=ETH/BTC&limit=10&last_id=1521100930 |  |

Data responded：

Example：


```
 Response:
{
    "error": null,
    "result": [],
    "id": 1521169562
}
```



### market.user_deals

| name of method | type of method | description |
| --- | --- | --- |
| market.user_deals | post  | check the user's transaction records |

requested parameter：

| name of parameter | type of parameter | description |
| --- | --- | --- |
| api_key | string | user's API KEY |
| sign | string | user's signature value |
| market | string  | market name，such as：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| offset | Integer  | Offset position，if value set as 0，it means that the total number of transactions from the earliest transaction to the most recent transaction cannot be greater than limit;。if value set as 1, it means that the total number of all orders that meet the requirements from the second most recent order to the earliest order cannot be greater than limit; and so on.....|
| limit | Integer   | Check limit in number(limit <= 1000) |

Example：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/market.user_deals | api_key=5eae7322-6f92-873a-e9bc214fd61517ec&sign=FDCAFAF85A38970E4D84F6F286A2879E&market=ETH/BTC&offset=0&limit=100|


### market.kline

| name of method | type of method | description |
| --- | --- | --- |
| market.kline | get  | Check K Chart |

Parameter requested：

| name of parameter | type of parameter | description |
| --- | --- | --- |
| market | string  | market name，such as：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| start_time | Integer  | Starting timestamp(second) |
| end_time | Integer  | ending timestamp(second)  |
| Interval | Integer   | interval of periods,unit second, (starting time to ending time，total number of periods) < 1000 |

Example：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/market.kline?market=ETH/BTC&start_time=1521100000&end_time=1521101193&interval=60 | |

Data responded：

Example：


```
Response:
{
    "error": null,
    "result": [   
    [1525067600, "11714.04", "11710.01", "11778.69", "11697.18", "13.604065", "159329.23062211", "BTCUSDT"], 
    [1565067660, "11703.47", "11716.65", "11720.55", "11703.47", "14.401973", "168649.82127032", "BTCUSDT"], 
    [1565067720, "11714.24", "11715.09", "11724.5", "11707.78", "12.287975", "143952.77384769", "BTCUSDT"]],#time ,open, close, high, low ,volume, deal, market
    "id": 1521169586
}
```



### market.status

| name of method | type of method | description |
| --- | --- | --- |
| market.status | get  | obtain the latest status of the market during the designated period of time in the past,l such as latest range of increase and decline, trading volume, highest/lowest price etc. |

parameter requested:

| name of parameter | type of parameter | description |
| --- | --- | --- |
| market | string  | market name，such as：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |
| period | Integer  | period of inquiry，unit is second. It refers to the time from now on to the past，for example：86400，means the previous 24 hours. |

Example：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/market.status?market=ETH/BTC&period=10 | |

Data responded：

example：


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

| name of method | type of method | description |
| --- | --- | --- |
| market.status_today | get  | obtain today's market status |

 parameter requested：

| name of parameter | type of parameter | description |
| --- | --- | --- |
| market | string  | market name，such as：&quot;BTC/USDT&quot;,&quot;BCC/USDT&quot; |

example：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/market.status_today?market=ETH/BTC | |

data responded：

example：


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

| name of method           | type of method | description                                                        |
| ---------------- | -------- | ----------------------------------------------------------- |
| market.status24h | get      | Obtain the market status within the previous 24 hours, such as the range of increase and decline, trading volume, highest/lowest price etc. |

Requested parameter: no

Example：

| url                                           | body |
| --------------------------------------------- | ---- |
| https://api.hotbit.io/api/v1/market.status24h |      |

data responded：

example：


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

| name of method | type of method | description |
| --- | --- | --- |
| market.summary | get  | market summary |

 parameter requested：

| name of parameter | type of parameter | description |
| --- | --- | --- |
| markets | json array  | marketnamejson array，such as：["BTCUSD","BCCUSD"]，in case of empty array:[],return to all market。 |

example：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/market.summary | |


data responded：

example：


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

| name of method | type of method | description |
| --- | --- | --- |
| allticker | get  | obtain the latest trading informnation of all transaction pairs in the market |


requested parameter：

no

example：

| url | body |
| --- | --- |
| https://api.hotbit.io/api/v1/allticker | |


data responded：

example：


```
Response:
{
    ticker: [
    {
        symbol: "HTB_ETH",      # name of transaction pair
        buy: "0.0000077393",    # buy one price
        sell: "0.0000078169",   # sell one price
        open: "0.0000078205",   # open price
        close: "0.0000077506",  # close price
        high: "0.0000080946",   # highest price
        low: "0.000007551",     # lowest price
        last: "0.0000077506",   # latest price
        vol: "98842592"         # transaction volume
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
