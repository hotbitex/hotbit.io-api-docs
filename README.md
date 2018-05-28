# hotbit.io-api-docs
Official Documentation for the hotbit.io APIs. https://www.hotbit.io/

# 入门指引    

## API概述    

HOTBIT为用户提供了一整套简单而又强大的开发工具，旨在帮助用户快速、高效地将HOTBIT交易功能整合到自己的应用当中。    

HOTBIT接口是提供服务的基础，开发者在HOTBIT网站创建账号后，可以根据自身需求建立不同权限的API，并利用API进行自动交易或者提现。   

通过API可以快速实现以下功能：    
- 获取市场最新行情    
- 获取买卖深度信息    
- 查询可用和冻结金额    
- 查询自己当前尚未成交的挂单    
- 快速买进卖出    
- 批量撤单    
- 快速提现到您的认证地址    

获取接口权限后，可以通过阅读本接口文档来帮助开发。    
    
## 接口调用方式说明    

HOTBIT为用户提供了两种调用接口的方式，开发者可根据自己的使用场景和偏好选择适合自己的方式来查询行情、进行交易或提现。    

#### REST API    

REST，即Representational State Transfer的缩写，是目前最流行的一种互联网软件架构。它结构清晰、符合标准、易于理解、扩展方便，正得到越来越多网站的采用。其优点如下：    
- 在RESTful架构中，每一个URL代表一种资源；    
- 客户端和服务器之间，传递这种资源的某种表现层；    
- 客户端通过四个HTTP指令，对服务器端资源进行操作，实现“表现层状态转化”。    

建议开发者使用REST API进行币币交易或者资产提现等操作。 

#### WebSocket API    

WebSocket是HTML5一种新的协议(Protocol)。它实现了客户端与服务器全双工通信，使得数据可以快速地双向传播。通过一次简单的握手就可以建立客户端和服务器连接，服务器根据业务规则可以主动推送信息给客户端。其优点如下：    
- 客户端和服务器进行数据传输时，请求头信息比较小，大概2个字节;    
- 客户端和服务器皆可以主动地发送数据给对方；    
- 不需要多次创建TCP请求和销毁，节约宽带和服务器的资源。    

强烈建议开发者使用WebSocket API获取市场行情和买卖深度等信息。    

## 开启API权限    

用户的API权限在网站的账户管理->API管理内获取。点击申请API即可获得，其中apiKey是HOTBIT提供给API用户的访问密钥，secretKey用于对请求参数签名的私钥。    

**_注意： 请勿向任何人泄露这两个参数，这两个参数关乎您账号的安全。_**    
     
## 参数签名    

用户提交的参数除sign外，都要参与签名。    

首先，将待签名字符串要求按照参数名进行排序(首先比较所有参数名的第一个字母，按abcd顺序排列，若遇到相同首字母，则看第二个字母，以此类推)。   

例如：对于如下的参数进行签名   

   	string[] parameters={"api_key":"api_key_string","id": 123,"method": "balance.query","params": [1, "BTC"]};
   	
生成待签名的字符串    

	api_key=api_key_string&id=123&method=balance.query&params=1,BTC

然后，将待签名字符串添加私钥参数生成最终待签名字符串。例如：

	api_key=api_key_string&id=123&method=balance.query&params=1,BTC&secret_key=secretKey    

注意，`&secret_key=secretKey` 为签名必传参数，放在字符串的最后位置。   

最后，是利用32位MD5算法，对最终待签名字符串进行签名运算，从而得到签名结果字符串(该字符串赋值于参数sign)，MD5计算结果中字母全部大写。    
