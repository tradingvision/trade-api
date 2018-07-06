
## Endpoint: 
`https://api.coin-weight.com/v1`

## Order API

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

### /order/history _[GET, Authentication]_
Request Parameters
- owner: 1234
- pageIndex: 0
- pageSize: 10

Response body
```
TBD
```

### /order/cancel-all _[POST, Authentication]_
Request body
```javascript
{
	"owner": "123",
	"canceledCounterParty": ["huobi", "binance"]
}
```


## User API

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

## API key & Secret key API

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

## Strategy API

### /strategy/get [GET]

ok response

```javascript
{"1": {
      'name': 'octmm',
      'exchange': 'binance',
      'strategyType': 'SingleMarketMaker',
      'symbols': [{
        'name': 'OCT-ETH',
        'currency': 'ETH',
        'strategy': 'octmm',
        'margin': 0.1,
        'orderNumber': 10,
        'spreadPct': 0.03,
        'orderQty': 10,
        'qtyVar': 0.01,
        'priceStep': 0.0001,
        'priceVar': 0.0001
      }, {
        'name': 'OCT-BTC',
        'currency': 'BTC',
        'strategy': 'octmm',
        'margin': 0.01,
        'orderNumber': 10,
        'spreadPct': 0.03,
        'orderQty': 10,
        'qtyVar': 0.01,
        'priceStep': 0.0001,
        'priceVar': 0.0001
      }]
    }}
```

| error_code | reason |
|:---:|---|
| 0 | OK |
| 500 | Unexpected Error (wrong owner) |


### /strategy/update _[POST]_

request

```javascript
{
  "userid": "123",
  "strategy": {"1": {
      'name': 'octmm',
      'exchange': 'binance',
      'strategyType': 'SingleMarketMaker',
      'symbols': [{
        'name': 'OCT-ETH',
        'currency': 'ETH',
        'strategy': 'octmm',
        'margin': 0.1,
        'orderNumber': 10,
        'spreadPct': 0.03,
        'orderQty': 10,
        'qtyVar': 0.01,
        'priceStep': 0.0001,
        'priceVar': 0.0001
      }, {
        'name': 'OCT-BTC',
        'currency': 'BTC',
        'strategy': 'octmm',
        'margin': 0.01,
        'orderNumber': 10,
        'spreadPct': 0.03,
        'orderQty': 10,
        'qtyVar': 0.01,
        'priceStep': 0.0001,
        'priceVar': 0.0001
      }]
    }}
}
```

ok response

```javascript
{
  "success": true,
  "seq_no": 0,
  "error_code": 0,
  "data": {
    "strategy": {"1": {
      'name': 'octmm',
      'exchange': 'binance',
      'strategyType': 'SingleMarketMaker',
      'symbols': [{
        'name': 'OCT-ETH',
        'currency': 'ETH',
        'strategy': 'octmm',
        'margin': 0.1,
        'orderNumber': 10,
        'spreadPct': 0.03,
        'orderQty': 10,
        'qtyVar': 0.01,
        'priceStep': 0.0001,
        'priceVar': 0.0001
      }, {
        'name': 'OCT-BTC',
        'currency': 'BTC',
        'strategy': 'octmm',
        'margin': 0.01,
        'orderNumber': 10,
        'spreadPct': 0.03,
        'orderQty': 10,
        'qtyVar': 0.01,
        'priceStep': 0.0001,
        'priceVar': 0.0001
      }]
    }}
  }
}
```


| error_code | reason |
|:---:|---|
| 0 | OK |
| 500 | Unexpected Error (wrong owner) |