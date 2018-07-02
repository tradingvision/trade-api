This Document is used to describe the REST API for the futures
## REST API

endpoint: `https://api.coin-weight.com/v1`


### **Authentication & Authorization**

HTTP Header:

- exp-authorization: {JWT_token}
	
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

### /futures/open _[POST, Authentication]_

operation:
  - open a future

example: to create new order
```javascript
{
  "streetOrderId": "1234567",
  "originalOrderId": "",
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


### /futures/close _[POST, Authentication]_

operation:
  - close a future

example: to create new order
```javascript
{
  "streetOrderId": "765",
  "originalOrderId": "`123`",
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
