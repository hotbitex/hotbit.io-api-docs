# hotbit.io-api-docs
Official Documentation for the hotbit.io APIs. https://www.hotbit.io/

#  Instructions    

## API Guidelines    

Hotbit provides all users with a complete set of simple yet powerful development tools, which aims to support all users to rapidly and efficiently intergrate all trading functions of Hotbit into their own Apps.

Hotbit interface plays the role as the foundation regarding the provision of all services. After creating their own accounts on Hotbit website, developers may establish API with different permission levels based on their own needs, and then conduct automatic transactions or withdrawals by using their API.
 

The following functions can be rapidly deployed by the use of API: 
- Obtain latest market trends 
Obtain relevant information regarding the depth of buy and sell transactions.
- Check the volume of available and frozen assets
- Check all currently unsettled order(s) that the user placed
Buy and sell rapidly
- Cancel orders in large quantities
- Rapidly withdraw assets from Hotbit to your verified address(es)

After obtaining interface permission(s), users may start their own development processes by reading and following the instructions of this document. 
    
## Instructions Regarding the Methods of Interface Call

Hotbit provides its users with two methods of interface call, between which developers may select the appropriate method that suits themselves to check market trends, conduct transactions or withdrawals based on their own application scenarios or preferences.

#### REST API    

REST，which is the abbreviation of "Representational State Transfer"，is currently one of the most popular types of Internet software frameworks. REST is currently being adopted by an increasing number of websites due to its clear structure, compliance with standards, simpleness and outstanding scalability. The advantages of REST are listed as follows:    
- Each URL represents one type of resource in the framework of RESTful;
- A certain type of presentation layer of the resource is delivered between the client end and the server;
- The client end conduct operations on the resources of server end through four HTTP instructions for the "transformation on the status of presentation layer".

The developers are recommended to use REST API for operations such as the transactions between different types of cryptocurrency tokens or asset withdrawals.

#### WebSocket API    

WebSocket is a new type of HTML5 protocol. WebSocket enables the full-duplex communication between client end and server end, which allows data to be transferred rapidly through two-way communications. By adopting WebSocket, the connection between the client end and the server end can be established through a simple handshake, and that the server may actively push information to the client end based on relevant business rules and regulations. The advantages of WebSocket are listed as below:   
- During the data transfers between the client end and server end, the size of the information in the request header is comparatively small, which is approximately 2 bytes only;   
- Both the client end and the server end may actively send data to each other.
- No need to establish multiple TCP requests and destroys, which saves the resources of broadband connections and servers. 

**_Attention:The feedback data of websocket is compressed by the compression algorithm of zlib._**

The developers are strongly recommended to obtain relevant information such as market trends and the depth of buy and sell transactions by adopting WebSocket API

## Enable API Permission

Users may obtain their API permissions by visiting "Account Management->API Management" section of our website. By clicking "Apply For API", our users will obtain their API permissions. The apiKey is the encrypted access key that Hotbit provides for all API users, and the secretKey functions as the private key that allows users to sign the requested parameters.   

**_Attention: Please do not disclose these two parameters to anyone, as these two parameters determines the security of your account(s)._**    
     
## Parameter Signature  

Apart from "sign", all data that users submitted must participate in the signature.

First，**sort the strings that require to be signed according to the parameter names（first compare the first letter of all parameter names and sort them based on alphabetical order; in case that the first letter of more than one parameters is the same, sort these parameters based on the second letter of their names according to alphabetical order, and so on).**   

For example: the requested parameter of asset inquiry interface balance.query(please refer to the instructions of HTTP interface)

| Parameter Name | Type of Parameter | Description | Examples |
| --- | --- | --- | --- | 
| api_key | string | The API KEY applied by the user | 6b97d781-5ffd-958f-576d96d0bbebc8c6 |
| sign | string | The signature value of the requested string | C88F04701D3349D0A93A0164DC5A4CD9 |
| assets | json array | The array of token abbreviation, blank array means obtain all token assets | ["BTC","ETH"]|
   	
The process of the abovementioned requests on signature generation is listed as below

- Generate strings to be signed

```
api_key=6b97d781-5ffd-958f-576d96d0bbebc8c6&assets=["BTC","ETH"]
```

- Add parameter of private key to the strings to be signed and generate final strings to be signed. For example:

```
api_key=6b97d781-5ffd-958f-576d96d0bbebc8c6&assets=["BTC","ETH"]&secret_key=de8063ea6e99bc967ba6395d06fabf50
```

- Calculate MD5 value

```
MD5("api_key=6b97d781-5ffd-958f-576d96d0bbebc8c6&assets=["BTC","ETH"]&secret_key=de8063ea6e99bc967ba6395d06fabf50")
```

The calculated MD5 value is：c88f04701d3349d0a93a0164dc5a4cd9

- Change MD5 value into capital letters

```
to_upper(c88f04701d3349d0a93a0164dc5a4cd9)
```

The final signature value generated is：C88F04701D3349D0A93A0164DC5A4CD9

After finishing above processes，the requested parameter sent by balance.query is：

```
api_key=6b97d781-5ffd-958f-576d96d0bbebc8c6&assets=["BTC","ETH"]&sign=C88F04701D3349D0A93A0164DC5A4CD9
```

Description: The API Key and Secret Key mentioned above are examples only, during the actual adoption, users are required to substitute all API Key and Secret Key mentioned in the example with their own API Key.
