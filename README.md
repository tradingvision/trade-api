# Trading-Platform Document

## Websocket API

### connect to websocket

- public user(without login)
```
wss://api.coin-weight.com/ws?uid=0
```

- login user

```
wss://api.coin-weight.com/ws?uid={uid}&token={jwt}
```

```
example: 

wss://api.coin-weight.com/ws?uid=1&token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1aWQiOjEsImlzcyI6ImV4IiwiZXhwIjoxNTI5MTcxMDE0LCJncm91cCI6Imdyb3VwMSJ9.s-wIDiBExMGCCyl-srynv1TrRtRi74lbqy3zx0Iyb44
```

### Subscription command

Notice:
  - use `seq` to track the response for the request

- Subscribe Topic
  - Request
    ```
    {"type":"SUB","seq":1,"data":{"symbol":"BTC-USDT","counterParty":"binance"}}
    ```
  - Response
    ```
    {"errorCode":0,"errorMsg":"ok","seq":1}
    ```

- UnSubscribe Topic
  - Request
    ```
    {"type":"UNSUB","seq":2,"data":{"symbol":"BTC-USDT","counterParty":"binance"}}
    ```
  - Response
    ```
    {"errorCode":0,"errorMsg":"ok","seq":2}
    ```

### Data 

- Depth price data
```
{
	"type": "streetPrice",
	"data": {
		"counterParty": "binance",
		"symbol": "BTC-USDT",
		"bids": [{
			"price": "6323.98000000",
			"quantity": "2.87027100"
		}, {
			"price": "6322.99000000",
			"quantity": "1.78365300"
		}, {
			"price": "6316.98000000",
			"quantity": "0.63262900"
		}, {
			"price": "6316.61000000",
			"quantity": "0.37062700"
		}, {
			"price": "6315.65000000",
			"quantity": "0.01000000"
		}],
		"asks": [{
			"price": "6324.86000000",
			"quantity": "0.02246900"
		}, {
			"price": "6325.00000000",
			"quantity": "3.13870000"
		}, {
			"price": "6325.50000000",
			"quantity": "0.10000000"
		}, {
			"price": "6325.81000000",
			"quantity": "0.24000000"
		}, {
			"price": "6326.00000000",
			"quantity": "0.08772600"
		}]
	}
}
```

- User active order update data

TBD

## REST API

### **Authentication & Authorization**

HTTP Header:

- Authorization: Bearer {JWT_token}
	
JWT payload 内容如下
	
```javascript
{
	"uid": 2,			// user id
	"iss": "ex",		// issuer (system)
	"exp": 1517148660,	// JWT token expiry time
	"group": "group1"	// user group
}
```
	
JWT token 在用户 login/signup 的时候获取
	
- super-token: {super\_key\_string} （用于调试时绕过用户 token 的获取，当系统配置了 super token 并且客户端 header 带有同样的 super token，鉴权即可成功）

### /order/process _[POST, Authentication]_

operation:
  - ORDER_NEW
  - ORDER_UPDATE
  - ORDER_CANCEL

example: to create new order
```javascript
{
  "streetOrderId": "1234567",
  "originalOrderId": "orgin12345",
  "operation": "ORDER_NEW",
  "type": "LIMIT",
  "orderPrice": "0.00123",
  "totalQuantity": "10.000",
  "symbol": "BTC-USDT",
  "side": "BID",
  "owner": "2",
  "counterParty": "binance"
}
```

response

```javascript
{
  "success": false,
  "seq_no": 0,
  "error_code": 403,
  "data": null
}
```

| error_code | reason |
|:---:|---|
| 0 | OK |
| 403 | Authentication failure |


### /user/login [POST]

Request Body

```javascript
{
  "userName": "abc",
  "password": "abc_password"
}
```

Response

```javascript
{
  "success": true,
  "seq_no": 0,
  "error_code": 0,
  "data": {
    "uid": 2,
    "user_name": "abc",
    "group": "group1",
    "auth_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1aWQiOjIsImlzcyI6ImV4IiwiZXhwIjoxNTE3MTQ4NjYwLCJncm91cCI6Imdyb3VwMSJ9.YrYttRaGcFWro6WFsGkm3arnSQXyF1ZOWfT2-Uve6tI"
  }
}
```

error response

```javascript
{
  "success": false,
  "seq_no": 0,
  "error_code": 2,
  "data": null
}
```

| error_code | reason |
|:---:|---|
| 0 | OK |
| 1 | Account not exist |
| 3 | Password error |
| 4 | Parameters error |

### /user/signup _[POST]_

request

```javascript
{
  "email": "abc@gmail.com",
  "phone": "12345",
  "userName": "abc"
}
```
ok response

```javascript
{
  "success": true,
  "seq_no": 0,
  "error_code": 0,
  "data": {
    "uid": 2,
    "user_name": "abc",
    "group": "group1",
    "auth_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1aWQiOjQsImlzcyI6ImV4IiwiZXhwIjoxNTE3MTQ4OTQwLCJncm91cCI6Imdyb3VwMSJ9.Z6GXtZGPCxt2v-e5E_lQ_8sXrgQHu4uwKJjrdof0Cvc"
  }
}
```

error response

```javascript
{
  "success": false,
  "seq_no": 0,
  "error_code": 5,
  "data": null
}
```

| error_code | reason |
|:---:|---|
| 0 | OK |
| 1 | Username duplication |
| 2 | Email duplication |
| 3 | Phone duplication |
| 4 | Account duplication |
| 5 | Verification code error ||


### /secret/{owner} _[GET]_

ok response

```javascript
{
    "success": true,
    "seqNo": 0,
    "errorCode": 0,
    "errorMsg": "ok",
    "data": [
        {
            "counterParty": "huobi",
            "owner": "1",
            "apiKey": "123456",
            "secretKey": "k****k"  
        }
    ]
}
```

| error_code | reason |
|:---:|---|
| 0 | OK |
| 500 | Unexpected Error (wrong owner) |

### /secret/add _[POST]_

request
```javascript
{
  "counterParty": "huobi",
  "owner": "1",
  "apiKey": "123456",
  "secretKey": "kkkkkk"
}
```

ok response

```javascript
{
    "success": true,
    "seqNo": 0,
    "errorCode": 0,
    "errorMsg": "ok",
    "data": null
}
```

| error_code | reason |
|:---:|---|
| 0 | OK |
| 1 | Duplicated secret key |
| 500 | Unexpected Error (wrong owner) |


### /secret/delete _[POST]_

request
```javascript
{
  "counterParty": "huobi",
  "owner": "1"
}
```

ok response

```javascript
{
    "success": true,
    "seqNo": 0,
    "errorCode": 0,
    "errorMsg": "ok",
    "data": null
}
```

| error_code | reason |
|:---:|---|
| 0 | OK |
| 500 | Unexpected Error (wrong owner) |