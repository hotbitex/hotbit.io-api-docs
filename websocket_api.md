协议整体描述：

本协议描述交易系统websocket端对外API的访问接口。

API基于websocket的json/rpc。

请求地址：
websoket访问的根URL：wss://ws.hotbit.io

请求：

通过websocket的send方法，将json/rpc数据发往服务器，有以下三个字段：

| 字段名 | 字段类型 | 描述 |
| --- | --- | --- |
| method | String |  API的方法名 |
| params | Json Array |  API的参数数组，不能为null |
| id | Integer | 详细描述看下面\*后内容 |



\* 没有包含&quot;id&quot;成员的请求对象为通知，作为通知的请求对象表明客户端对相应的响应对象并不感兴趣，本身也没有响应对象需要返回给客户端。服务端必须不回复一个通知。

\* 如果id成员存在，必须为一个无符号的整形数值，表示本次请求的标识码，远程返回时数据的标识码应该与本次请求标识码相同。

请求示例：

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


应答：

应答共有三个参数组成：



| 字段名 | 字段类型 | 描述 |
| --- | --- | --- |
| result | json object | API调用结果，结果出错时为null。 |
| error | json object | API调用错误内容，正确时为null。  |
| id | Request id | Integer |



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

**Request**

- method: method，String
- params: parameters，Array
- id: Request id, Integer

**Response**

- result: Json object，null for failure
- error: Json object，null for success, non-null for failure

1. code: error code
2. message: error message

- id: Request id, Integer

**Notify**

- method: method，String
- params: parameters，Array
- id: Null



System API

PING

示例：

| Request： {    &quot;method&quot;:&quot;server.ping&quot;,    &quot;params&quot;:[],    &quot;id&quot;:100}   |
| --- |
| Response:{    &quot;error&quot;:null,    &quot;result&quot;:&quot;pong&quot;,    &quot;id&quot;:100} |

System time

示例：

| Request： {    &quot;method&quot;:&quot;server.time&quot;,    &quot;params&quot;:[],    &quot;id&quot;:100}  |
| --- |
| Response:{    &quot;error&quot;:null,    &quot;result&quot;:&quot;1511941406&quot;,    &quot;id&quot;:100} |



ID verification api

示例：

| Request： {    &quot;method&quot;:&quot;server.auth&quot;,    &quot;params&quot;:[&quot;api_key&quot;,&quot;sign&quot;],    &quot;id&quot;:100}  |
| --- |
| Response:{    &quot;error&quot;: null,     &quot;result&quot;: {&quot;status&quot;: &quot;success&quot;},     &quot;id&quot;: 100} |

请求参数：

| 参数名 | 参数类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| api_key | String | 是 | 用户api_key  |
| sign | String | 是 | 签名值 |

Market API

Market inquiry

**K线数据请求。**

示例：

| Request：{&quot;method&quot;:&quot;kline.query&quot;,&quot;params&quot;:[&quot;BTCBCC&quot;,145231637,145232657,5],&quot;id&quot;:100}     |
| --- |
| Response:{    &quot;error&quot;: null,   &quot;result&quot;: [    [        1492358400, time        &quot;7000.00&quot;,  open        &quot;8000.0&quot;,   close        &quot;8100.00&quot;,  highest        &quot;6800.00&quot;,  lowest        &quot;1000.00&quot;   volume        &quot;123456.00&quot; amount        &quot;BTCCNY&quot;    market name   ]    ...],    &quot;id&quot;: 123} |

请求参数：

| 参数名 | 参数类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| market | String  | 是 | 市场名 |
| start | Integer | 是 | 开始时间 |
| end | Integer | 是 | 结束时间  |
| interval | Integer | 是 | 数据获取周期 |



返回值：



| &quot;result&quot;: [    [        1492358400, time        //时间戳        &quot;7000.00&quot;,  open                &quot;8000.0&quot;,   close               &quot;8100.00&quot;,  highest             &quot;6800.00&quot;,  lowest               &quot;1000.00&quot;   volume              &quot;123456.00&quot; amount              &quot;BTCCNY&quot;    market name    ]    ...] |
| --- |

Market subscription

**K线数据订阅。**

示例：

| Request： {    &quot;method&quot;:&quot;kline.subscribe&quot;,    &quot;params&quot;:[    &quot;BTCBCC&quot;,     60   ],    &quot;id&quot;:100}  |
| --- |
| Response: {    &quot;error&quot;: null,    &quot;result&quot;: {        &quot;status&quot;: &quot;success&quot;    },    &quot;id&quot;: 100} |

请求参数：

| 参数名 | 参数类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| market | String  | 是 | 市场名 |
| interval | Integer | 是 | 数据获取周期 |

subcribtion成功后，订阅数据将会被推送到客户端。内容如 **Market notification** 所述。

Market notification

**订阅成功后，通过websocket向客户端主动推送数据内容。**

示例：

| {    &quot;method&quot;: &quot;kline.update&quot;,     &quot;params&quot;: [[1513135140, &quot;8000&quot;, &quot;8000&quot;, &quot;8000&quot;, &quot;8000&quot;, &quot;0&quot;, &quot;0&quot;, &quot;BTCBCC&quot;]],    &quot;id&quot;: null} |
| --- |



返回值：



| &quot;params&quot;: [    [        1492358400, time        //时间戳        &quot;7000.00&quot;,  open                &quot;8000.0&quot;,   close               &quot;8100.00&quot;,  highest             &quot;6800.00&quot;,  lowest               &quot;1000.00&quot;   volume              &quot;123456.00&quot; amount              &quot;BTCBCC&quot;    market name    ]] |
| --- |

**Cancel subscription**

**取消订阅。**

示例：

| Request：{    &quot;method&quot;:&quot;kline.unsubscribe&quot;,    &quot;params&quot;:[&quot;BTCBCC&quot;],    &quot;id&quot;:100}   |
| --- |
| Response:{&quot;error&quot;: null,&quot;result&quot;: {&quot;status&quot;: &quot;success&quot;},&quot;id&quot;: 100} |



Price API

Acquire latest price

**获取当前价格。**

示例：

| Request：        {    &quot;method&quot;:&quot;price.query&quot;,    &quot;params&quot;:[&quot;BTCBCC&quot;],    &quot;id&quot;:100} |
| --- |
| Response: {    &quot;error&quot;: null,    &quot;result&quot;: &quot;8000.00000000&quot;,    &quot;id&quot;: 100} |



请求参数：



| &quot;params&quot;:[    &quot;BTCBCC&quot;                                    # 市场名] |
| --- |

返回值：

|  &quot;result&quot;: &quot;8000.00000000&quot;,                        # 价格 |
| --- |

Latest price subscription

**订阅价格。**

示例：

| Request：        {    &quot;method&quot;:&quot;price.subscribe&quot;,    &quot;params&quot;:[&quot;BTCBCC&quot;],    &quot;id&quot;:100} |
| --- |
| Response: {    &quot;error&quot;: null,    &quot;result&quot;: {        &quot;status&quot;: &quot;success&quot;    },    &quot;id&quot;: 100} |



请求参数：



| &quot;params&quot;:[    &quot;BTCBCC&quot;                                    # 市场名] |
| --- |



Latest price notification

**订阅成功后，通过websocket向客户端主动推送数据内容。**

示例：

| {    &quot;method&quot;: &quot;price.update&quot;,     &quot;params&quot;: [&quot;BTCBCC&quot;, &quot;8000.00000000&quot;],     &quot;id&quot;: null} |
| --- |



参数说明:

|  &quot;params&quot;: [    &quot;BTCBCC&quot;,                    # market name    &quot;8000.00000000&quot;            # 价格]           |
| --- |





Cancel subscription

**取消订阅。**

示例：

| Request：{    &quot;method&quot;:&quot;price.unsubscribe&quot;,    &quot;params&quot;:[&quot;BTCBCC&quot;],    &quot;id&quot;:100}   |
| --- |
| Response:{&quot;error&quot;: null,&quot;result&quot;: {&quot;status&quot;: &quot;success&quot;},&quot;id&quot;: 100} |







Market status API

Acquire market status

**获取当前market状态。**

示例：

| Request：        {    &quot;method&quot;:&quot;state.query&quot;,    &quot;params&quot;:[&quot;BTCBCC&quot;,86400],    &quot;id&quot;:100} |
| --- |
| Response: {     &quot;error&quot;: null,     &quot;result&quot;:        {             &quot;period&quot;: 86400,             &quot;last&quot;: &quot;8000&quot;,             &quot;open&quot;: &quot;0&quot;,             &quot;close&quot;: &quot;0&quot;,             &quot;high&quot;: &quot;0&quot;,            &quot;low&quot;: &quot;0&quot;,             &quot;volume&quot;: &quot;0&quot;,            &quot;deal&quot;: &quot;0&quot;       },      &quot;id&quot;: 100} |



请求参数：



| &quot;params&quot;:[    &quot;BTCBCC&quot;,                                    # 市场名    86400                                            # 周期 ，Integer, e.g. 86400 for last 24 hours] |
| --- |

返回值：

| {             &quot;period&quot;: 86400,       # 周期 ，Integer, e.g. 86400 for last 24 hours            &quot;last&quot;: &quot;8000&quot;,            #             &quot;open&quot;: &quot;0&quot;,             &quot;close&quot;: &quot;0&quot;,             &quot;high&quot;: &quot;0&quot;,            &quot;low&quot;: &quot;0&quot;,             &quot;volume&quot;: &quot;0&quot;,            &quot;deal&quot;: &quot;0&quot;       }, |
| --- |



**获取24小时多个market状态。**

示例：

| Request：        {    &quot;method&quot;:&quot;state.querys&quot;,    &quot;params&quot;:[&quot;BTCBCC&quot;,&quot;EOSUSDT&quot;],    &quot;id&quot;:100} |
| --- |
| Response: {     &quot;error&quot;: null,     &quot;result&quot;: [        &quot;BTCBCC&quot;,           {              &quot;period&quot;: 86400,               &quot;last&quot;: &quot;8000&quot;,               &quot;open&quot;: &quot;0&quot;,               &quot;close&quot;: &quot;0&quot;,               &quot;high&quot;: &quot;0&quot;,               &quot;low&quot;: &quot;0&quot;,               &quot;volume&quot;: &quot;0&quot;,               &quot;deal&quot;: &quot;0&quot;         },         &quot;EOSUSDT&quot;,          {              &quot;period&quot;: 86400,               &quot;last&quot;: &quot;8000&quot;,               &quot;open&quot;: &quot;0&quot;,               &quot;close&quot;: &quot;0&quot;,               &quot;high&quot;: &quot;0&quot;,               &quot;low&quot;: &quot;0&quot;,               &quot;volume&quot;: &quot;0&quot;,               &quot;deal&quot;: &quot;0&quot;         }       ],       &quot;id&quot;: 100} |



请求参数：



| &quot;params&quot;:[    &quot;BTCBCC&quot;,                                    # 市场名    &quot;EOSUSDT&quot;                                   #] |
| --- |

返回值：

| {             &quot;period&quot;: 86400,       # 周期 ，Integer, e.g. 86400 for last 24 hours            &quot;last&quot;: &quot;8000&quot;,            #             &quot;open&quot;: &quot;0&quot;,             &quot;close&quot;: &quot;0&quot;,             &quot;high&quot;: &quot;0&quot;,            &quot;low&quot;: &quot;0&quot;,             &quot;volume&quot;: &quot;0&quot;,            &quot;deal&quot;: &quot;0&quot;       }, |
| --- |



Market 24H status subscription

**24小时市场状态订阅。**

示例：

| Request：        {    &quot;method&quot;:&quot;state.subscribe&quot;,    &quot;params&quot;:[&quot;BTCBCC&quot;,&quot;BTCUSD&quot;],    &quot;id&quot;:100} |
| --- |
| Response: {    &quot;error&quot;: null,    &quot;result&quot;: {        &quot;status&quot;: &quot;success&quot;    },    &quot;id&quot;: 100} |

请求参数：

params: market list

Market 24H status notification

**订阅成功后，状态数据会被交易服务器推送给客户端。**

示例：

| Result:{    &quot;method&quot;: &quot;state.update&quot;,     &quot;params&quot;: [       &quot;BTCBCC&quot;,     {&quot;period&quot;: 86400, &quot;last&quot;: &quot;8000&quot;, &quot;open&quot;: &quot;0&quot;, &quot;close&quot;: &quot;0&quot;, &quot;high&quot;: &quot;0&quot;, &quot;low&quot;: &quot;0&quot;, &quot;volume&quot;: &quot;0&quot;, &quot;deal&quot;: &quot;0&quot;}       ],     &quot;id&quot;: null} |
| --- |



返回值参考state.query方法。

Cancel subscription

**取消订阅。**

示例：

| Request：{    &quot;method&quot;:&quot;state.unsubscribe&quot;,    &quot;params&quot;:[],    &quot;id&quot;:100}   |
| --- |
| Response:{    &quot;error&quot;: null,    &quot;result&quot;: {        &quot;status&quot;: &quot;success&quot;    },    &quot;id&quot;: 100} |



Market status in Today API

Acquire Market today status

**请求今天的市场状态。**

示例：

| Request:{    &quot;method&quot;:&quot;today.query&quot;,    &quot;params&quot;:[&quot;BTCBCC&quot;],    &quot;id&quot;:100} |
| --- |
| Response:{     &quot;error&quot;: null,     &quot;result&quot;:    {         &quot;open&quot;: &quot;8000&quot;,         &quot;last&quot;: &quot;8000&quot;,         &quot;high&quot;: &quot;8000&quot;,         &quot;low&quot;: &quot;8000&quot;,         &quot;volume&quot;: &quot;0&quot;,         &quot;deal&quot;: &quot;0&quot;     },     &quot;id&quot;: 100} |

请求参数：

| 参数名 | 参数类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| market | String  | 是 | 市场名 |

  返回值参考state.query方法。

Market today status subscription

**当天market状态订阅。**

示例：

| Request：{    &quot;method&quot;:&quot;today.subscribe&quot;,    &quot;params&quot;:[&quot;BTCUSD&quot;,&quot;BTCBCC&quot;],    &quot;id&quot;:100} |
| --- |
| Response:{    &quot;error&quot;: null,    &quot;result&quot;: {        &quot;status&quot;: &quot;success&quot;    },    &quot;id&quot;: 100} |

请求参数：

| 参数名 | 参数类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| market list | String  | 是 | 市场名列表 |

Market Today status notification

请求订阅成功后，交易服务器返回给客户端数据。

| {    &quot;method&quot;: &quot;today.update&quot;,     &quot;params&quot;: [        &quot;BTCBCC&quot;,        {&quot;open&quot;: &quot;8000&quot;, &quot;last&quot;: &quot;8000&quot;, &quot;high&quot;: &quot;8000&quot;, &quot;low&quot;: &quot;8000&quot;, &quot;volume&quot;: &quot;0&quot;, &quot;deal&quot;: &quot;0&quot;}   ],     &quot;id&quot;: null} |
| --- |



 返回值参考state.query方法。



Cancel subscription

**取消订阅。**

示例：

| Request：{    &quot;method&quot;:&quot;today.unsubscribe&quot;,    &quot;params&quot;:[],    &quot;id&quot;:100}   |
| --- |
| Response:{    &quot;error&quot;: null,    &quot;result&quot;: {        &quot;status&quot;: &quot;success&quot;    },    &quot;id&quot;: 100} |



Deal API

Acquire latest executed list

**查询最近已经执行交易列表**

示例：

| Request:{    &quot;method&quot;:&quot;deals.query&quot;,    &quot;params&quot;:[&quot;BTCBCC&quot;,5,1],    &quot;id&quot;:100}  |
| --- |
| Response:{       &quot;error&quot;: null,     &quot;result&quot;: [            { &quot;id&quot;: 26, &quot;time&quot;: 1512454847.188796, &quot;price&quot;: &quot;8000&quot;, &quot;amount&quot;: &quot;1&quot;, &quot;type&quot;: &quot;buy&quot; },         { &quot;id&quot;: 25, &quot;time&quot;: 1512445625.751971, &quot;price&quot;: &quot;8000&quot;, &quot;amount&quot;: &quot;0.125&quot;, &quot;type&quot;: &quot;buy&quot; },        { &quot;id&quot;: 24, &quot;time&quot;: 1512442938.956193, &quot;price&quot;: &quot;8000&quot;, &quot;amount&quot;: &quot;0.125&quot;, &quot;type&quot;: &quot;buy&quot; },        { &quot;id&quot;: 23, &quot;time&quot;: 1512442929.0405071, &quot;price&quot;: &quot;8000&quot;, &quot;amount&quot;: &quot;0.125&quot;, &quot;type&quot;: &quot;buy&quot; },     { &quot;id&quot;: 22, &quot;time&quot;: 1512442927.2021289, &quot;price&quot;: &quot;8000&quot;, &quot;amount&quot;: &quot;0.125&quot;, &quot;type&quot;: &quot;buy&quot; }                     ],     &quot;id&quot;: 100} |



请求参数：

| 参数名 | 参数类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| market | String  | 是 | 市场名 |
| limit | Integer | 是 | 最大返回数量 |
| last\_id | integer | 是 | 从最近一个订单id开始往前（id从大到小）最后一个id,例如一共有1，2，3，4，5，一共5个id，last\_id设为3，limit即使设为5也只返回id为4，5两组数据。 |



返回值：

|   &quot;result&quot;: [            {                  &quot;id&quot;: 26,                                        # deal\_id                  &quot;time&quot;: 1512454847.188796,   # 交易时间                   &quot;price&quot;: &quot;8000&quot;,                          # 交易价格                   &quot;amount&quot;: &quot;1&quot;,                            # 成交数量                   &quot;type&quot;: &quot;buy&quot;                               # 成交类型            },            ...... ] |
| --- |



Latest order list subscription

**最新订单列表订阅。**

示例：

| {    &quot;method&quot;:&quot;deals.subscribe&quot;,    &quot;params&quot;:[&quot;BTCBCC&quot;,&quot;BTCUSD&quot;],    &quot;id&quot;:100} |
| --- |
| {    &quot;error&quot;: null,     &quot;result&quot;: {            &quot;status&quot;: &quot;success&quot;    },     &quot;id&quot;: 100} |



Latest order list update

**最新订单列表推送，交易服务器发送给客户端。**



示例：

| {       &quot;method&quot;: &quot;deals.update&quot;,    &quot;params&quot;: [            &quot;BTCBCC&quot;, [                    {&quot;id&quot;: 26, &quot;time&quot;: 1512454847.188796, &quot;price&quot;: &quot;8000&quot;, &quot;amount&quot;: &quot;1&quot;, &quot;type&quot;: &quot;buy&quot;},                                     {&quot;id&quot;: 25, &quot;time&quot;: 1512445625.751971, &quot;price&quot;: &quot;8000&quot;, &quot;amount&quot;: &quot;0.125&quot;, &quot;type&quot;: &quot;buy&quot;},                     {&quot;id&quot;: 24, &quot;time&quot;: 1512442938.956193, &quot;price&quot;: &quot;8000&quot;, &quot;amount&quot;: &quot;0.125&quot;, &quot;type&quot;: &quot;buy&quot;},                     ..................                    {&quot;id&quot;: 22, &quot;time&quot;: 1512442927.2021289, &quot;price&quot;: &quot;8000&quot;, &quot;amount&quot;: &quot;0.125&quot;, &quot;type&quot;: &quot;buy&quot;},                   {&quot;id&quot;: 3, &quot;time&quot;: 1512442109.0283101, &quot;price&quot;: &quot;8000&quot;, &quot;amount&quot;: &quot;0.125&quot;, &quot;type&quot;: &quot;buy&quot;},                    {&quot;id&quot;: 2, &quot;time&quot;: 1512441964.500567, &quot;price&quot;: &quot;8000&quot;, &quot;amount&quot;: &quot;0.125&quot;, &quot;type&quot;: &quot;buy&quot;},               {&quot;id&quot;: 1, &quot;time&quot;: 1512441751.9356339, &quot;price&quot;: &quot;8000&quot;, &quot;amount&quot;: &quot;0.125&quot;, &quot;type&quot;: &quot;buy&quot;}                 ]           ], &quot;id&quot;: null} |
| --- |



返回值：



| 参考deals.query的返回结果。 |
| --- |



Cancel subscription

**取消订阅。**

| Request:{    &quot;method&quot;:&quot;deals.unsubscribe&quot;,    &quot;params&quot;:[],    &quot;id&quot;:100} |
| --- |
| Response:{    &quot;error&quot;: null,     &quot;result&quot;: {        &quot;status&quot;: &quot;success&quot;    },     &quot;id&quot;: 100} |



Depth API



Acquire depth

**请求depth。**

示例：

| Request:{    &quot;method&quot;:&quot;depth.query&quot;,    &quot;params&quot;:[&quot;EOSUSDT&quot;,100,&quot;1&quot;],    &quot;id&quot;:100} |
| --- |
| Response:{     &quot;error&quot;: null,     &quot;result&quot;: { &quot;asks&quot;: [[ &quot;8000&quot;, &quot;20&quot;] ], &quot;bids&quot;: [[ &quot;800&quot;, &quot;4&quot;] ] },     &quot;id&quot;: 100} |

请求参数：

| 参数名 | 参数类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| market | String  | 是 | 市场名 |
| limit | Integer | 是 | 最大返回数量 |
| **interval** | string | 是 |  精度，比如&quot;0.001&quot;，获取的数据就是：12.975这样的数字。再比如&quot;5&quot;，就会得到&quot;15&quot; &quot;20&quot; &quot;10&quot; 这样的数字。 |

返回结果：

|  &quot;result&quot;: {         &quot;asks&quot;: [                [                     &quot;8000&quot;,                     &quot;20&quot;                ]          ],         &quot;bids&quot;: [                [&quot;800&quot;, &quot;4&quot;]         ] } |
| --- |

Depth subscription

**订阅Depth。**

示例：

| Request：        {    &quot;method&quot;:&quot;depth.subscribe&quot;,    &quot;params&quot;:[&quot;EOSUSDT&quot;,100,&quot;0.1&quot;],    &quot;id&quot;:100} |
| --- |
| Response: {    &quot;error&quot;: null,    &quot;result&quot;: {        &quot;status&quot;: &quot;success&quot;    },    &quot;id&quot;: 100} |



请求参数：



| &quot;params&quot;:[    &quot;BTCBCC&quot; ,     # 市场名    100,                 # limit,取值：1, 5, 10, 20, 30, 50, 100     &quot;10&quot;                # interval, 取值：&quot;0&quot;,&quot;0.00000001&quot;,&quot;0.0000001&quot;,&quot;0.000001&quot;,&quot;0.00001&quot;,                            # &quot;0.0001&quot;, &quot;0.001&quot;, &quot;0.01&quot;, &quot;0.1&quot;] |
| --- |



Depth notification

**Depth数据上传。**

示例：

| {    &quot;method&quot;: &quot;depth.update&quot;,     &quot;params&quot;: [true, {&quot;asks&quot;: [[&quot;8000&quot;, &quot;20&quot;]], &quot;bids&quot;: [[&quot;800&quot;, &quot;4&quot;]]}, &quot;BTCBCC&quot;],     &quot;id&quot;: null} |
| --- |



返回结果：

|  &quot;params&quot;: [            true,  #clean: Boolean, true: complete result，false: last returned updated result           {                  &quot;asks&quot;: [[                          &quot;8000&quot;,      # 价格                           &quot;20&quot;           # 数量                   ]],                   &quot;bids&quot;: [[&quot;800&quot;, &quot;4&quot;]]          },           &quot;BTCBCC&quot;] |
| --- |



Cancel subscription

**取消订阅。**

示例：

| Request:{    &quot;method&quot;:&quot;depth.unsubscribe&quot;,    &quot;params&quot;:[],    &quot;id&quot;:100} |
| --- |
| Response:{    &quot;error&quot;: null,     &quot;result&quot;: {        &quot;status&quot;: &quot;success&quot;    },     &quot;id&quot;: 100} |





Order API (Authentication required before connection)

**Order API 调用需要auth认证，请先调用server.auth方法请求认证，然后再调用。**

Unexecuted order inquiry

示例：

| Request:{    &quot;method&quot;:&quot;order.query&quot;,    &quot;params&quot;:[&quot;BTCBCC&quot;,0,50],    &quot;id&quot;:100} |
| --- |
| Response:{      &quot;error&quot;: null,     &quot;result&quot;: {         &quot;limit&quot;: 50,         &quot;offset&quot;: 0,         &quot;total&quot;: 0,         &quot;records&quot;: []     },     &quot;id&quot;: 100 } |



请求参数：

| 参数名 | 参数类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| market | String  | 是 | 市场名 |
| offset | Integer | 是 | offset |
| limit | Integer | 是 | 最大返回数量,小于101 |

返回值：

| &quot;result&quot;: {    &quot;offset&quot;:    &quot;limit&quot;:    &quot;records&quot;: [        {            &quot;id&quot;: Executed ID            &quot;time&quot;: timestamp            &quot;user&quot;: user ID            &quot;role&quot;: role，1：Maker, 2: Taker            &quot;amount&quot;: count            &quot;price&quot;: price            &quot;deal&quot;: order amount            &quot;fee&quot;: fee            &quot;deal\_order\_id&quot;: Counterpart transaction ID   交易对方的order id        }    ...]} |
| --- |

Executed order inquiry

示例：

| {    &quot;method&quot;:&quot;order.history&quot;,    &quot;params&quot;:[&quot;BTCBCC&quot;,1511941006,1511941406,0,50],    &quot;id&quot;:100} |
| --- |
| Response:{     &quot;error&quot;: null,     &quot;result&quot;: { &quot;offset&quot;: 1021, &quot;limit&quot;: 1, &quot;records&quot;: [] },     &quot;id&quot;: 100 } |



请求参数：

| 参数名 | 参数类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| market | String  | 是 | 市场名 |
| start\_time | Integer | 是 | 开始时间  |
| end\_time | Integer | 是  | 结束时间 |
| offset | Integer | 是 | offset |
| limit | Integer | 是 | 最大返回数量,小于101 |



返回值：参考order.query方法返回值。



Order subscription

**订阅Order。**

示例：

| Request：        {    &quot;method&quot;:&quot;order.subscribe&quot;,    &quot;params&quot;:[&quot;BTCBCC&quot;,&quot;BTCETH&quot;],    &quot;id&quot;:100} |
| --- |
| Response: {    &quot;error&quot;: null,    &quot;result&quot;: {        &quot;status&quot;: &quot;success&quot;    },    &quot;id&quot;: 100} |



请求参数：order list



Order notification

**个人交易数据推送。**

**当用户交易数据订阅成功后，交易数据会从交易服务器推送给客户端。**

示例：

| {    &quot;method&quot;: &quot;order.update&quot;,     &quot;params&quot;: [        1,                                                        # event: event type，Integer, 1: PUT, 2: UPDATE, 3: FINISH        {                                                          # order: order detail，Object，see HTTP protocol            &quot;id&quot;: 422458,             &quot;market&quot;: &quot;BTCBCC&quot;,             &quot;source&quot;: &quot;&quot;,             &quot;type&quot;: 1,                                           # 1 --- limit order ;   2 --- market order            &quot;side&quot;: 1,                                            # 1 --- ask         2 --- bid            &quot;user&quot;: 5,             &quot;ctime&quot;: 1513819599.987308,         # create\_time            &quot;mtime&quot;: 1513819599.987308,       # update\_time            &quot;price&quot;: &quot;8000&quot;,             &quot;amount&quot;: &quot;10&quot;,             &quot;taker\_fee&quot;: &quot;0.002&quot;,             &quot;maker\_fee&quot;: &quot;0.001&quot;,             &quot;left&quot;: &quot;10&quot;,             &quot;deal\_stock&quot;: &quot;0&quot;,             &quot;deal\_money&quot;: &quot;0&quot;,             &quot;deal\_fee&quot;: &quot;0&quot;        }   ],     &quot;id&quot;: 102} |
| --- |





Cancel subscription

**取消订阅。**

示例：

| Request:{    &quot;method&quot;:&quot;order.unsubscribe&quot;,    &quot;params&quot;:[],    &quot;id&quot;:100} |
| --- |
| Response:{    &quot;error&quot;: null,     &quot;result&quot;: {        &quot;status&quot;: &quot;success&quot;    },     &quot;id&quot;: 100} |







Asset API (Authentication required before connection)

Asset API **调用需要auth认证，请先调用server.auth方法请求认证，然后再调用。**

Asset inquiry

示例：

| {     &quot;method&quot;:&quot;asset.query&quot;,    &quot;params&quot;:[&quot;ETH&quot;,&quot;BTC&quot;],    &quot;id&quot;:100} |
| --- |
| {     &quot;error&quot;: null,     &quot;result&quot;: {         &quot;ETH&quot;: { &quot;available&quot;: &quot;0&quot;, &quot;freeze&quot;: &quot;0&quot; },         &quot;BTC&quot;: { &quot;available&quot;: &quot;0&quot;, &quot;freeze&quot;: &quot;0&quot; }     },     &quot;id&quot;: 100} |

请求参数：asset list。

返回值：参考上面示例。

Asset.subscribe

**资产信息订阅**

示例：

| Request：        {    &quot;method&quot;:&quot;asset.subscribe&quot;,    &quot;params&quot;:[&quot;BTC&quot;,&quot;ETH&quot;,&quot;BCC&quot;],    &quot;id&quot;:100} |
| --- |
| Response: {    &quot;error&quot;: null,    &quot;result&quot;: {        &quot;status&quot;: &quot;success&quot;    },    &quot;id&quot;: 100} |

请求参数：

params: asset list



Asset notification

Asset数据上传。



示例：

| {    &quot;method&quot;: &quot;asset .update&quot;,     &quot;result&quot;: {         &quot;ETH&quot;: { &quot;available&quot;: &quot;0&quot;, &quot;freeze&quot;: &quot;0&quot; },         &quot;BTC&quot;: { &quot;available&quot;: &quot;0&quot;, &quot;freeze&quot;: &quot;0&quot; }     },     &quot;id&quot;: 100} |
| --- |





Cancel subscription

**取消订阅。**

示例：

| Request:{    &quot;method&quot;:&quot;asset.unsubscribe&quot;,    &quot;params&quot;:[],    &quot;id&quot;:100} |
| --- |
| Response:{    &quot;error&quot;: null,     &quot;result&quot;: {        &quot;status&quot;: &quot;success&quot;    },     &quot;id&quot;: 100} |







Asset history

**获取资产历史。**

示例：

| Request: {    &quot;method&quot;:&quot;asset.history&quot;,    &quot;params&quot;:[&quot;BTC&quot;,&quot;deposit&quot;,1501940406,1513328112,0,100],    &quot;id&quot;:100} |
| --- |
| Response: {     &quot;error&quot;: null,      &quot;result&quot;: {         &quot;offset&quot;: 0,          &quot;limit&quot;: 100,          &quot;records&quot;: [             {                &quot;time&quot;: 1511856143.3754101,                &quot;asset&quot;: &quot;USD&quot;,                &quot;business&quot;: &quot;deposit&quot;,                &quot;change&quot;: &quot;100000&quot;,                &quot;balance&quot;: &quot;300000&quot;,                &quot;detail&quot;: {                    &quot;id&quot;: 10003                }            },            {                &quot;time&quot;: 1511856059.013973,                &quot;asset&quot;: &quot;USD&quot;,                &quot;business&quot;: &quot;deposit&quot;,                &quot;change&quot;: &quot;100000&quot;,                &quot;balance&quot;: &quot;200000&quot;,                &quot;detail&quot;: {                    &quot;id&quot;: 10002                }            },           ........         ]      },      &quot;id&quot;: 100 } |

请求参数：

| 参数名 | 参数类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| asset | String  | 是 | asset name, which can be null |
| business | String  | 是 | 业务名 |
| start\_time | Integer | 是 | 开始时间  |
| end\_time | Integer | 是  | 结束时间 |
| offset | Integer | 是 | offset |
| limit | Integer | 是 | 最大返回数量,小于101 |
