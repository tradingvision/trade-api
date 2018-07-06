
## Endpoint: 
`https://api.coin-weight.com/v1`

## Order API

### basic models

operation:
  - ORDER_NEW
  - ORDER_UPDATE
  - ORDER_CANCEL

orderType:
  - LIMIT
  - MARKET

side:
  - BID
  - ASK

executionType:
  - EXECUTION_REJECTED
  - EXECUTION_ACK
  - EXECUTION_PARTIAL_FILLED
  - EXECUTION_FILLED
  - EXECUTION_CANCELING
  - EXECUTION_CANCELED
  - EXECUTION_PARTIAL_CANCELED

Order Model
```javascript
{
  "streetOrderId":"1234567",
  "orderType":"LIMIT",
  "symbol":"BTC-USDT",
  "side":"BID",
  "owner":"2",
  "counterParty":"binance",
  "price":"0.00123",
  "quantity":"10.000",
  "filledPrice":"0.00",
  "filledQuantity":"0.00",
  "executionType":"EXECUTION_ACK",
  "createTime":1530861306000,
  "executionTime":1530861306000
}
```

General Response Model

- **ok** response
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data": {
    // ...
  }
}
```


- **error** response

```javascript
{
  "success":false,
  "errorCode":403,
  "errorMsg":"authentication failure",
  "data":null
}
```


### /order/create _[POST, Authentication]_

```javascript
{  
  "orderType":"LIMIT",
  "symbol":"BTC-USDT",
  "side":"BID",
  "owner":"2",
  "counterParty":"binance",
  "price":"0.00123",
  "quantity":"10.000"
}
```

Response

```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data":{
    "streetOrderId":"1234567",
    "orderType":"LIMIT",
    "symbol":"BTC-USDT",
    "side":"BID",
    "owner":"2",
    "counterParty":"binance",
    "price":"0.00123",
    "quantity":"10.000",
    "filledPrice":"0.00",
    "filledQuantity":"0.00",
    "executionType":"EXECUTION_ACK",
    "createTime":1530861306000,
    "executionTime":1530861306000
  }
}
```

### /order/update _[POST, Authentication]_
Only unfilled order can be updated, update field only support for:
  - price
  - quantity

Request Body
```javascript
{
  "streetOrderId": "1234567",
  "price": "0.00145",
  "quantity": "10.000",
  "owner": "2",
  "counterParty": "binance"
}
```

Response: successfully updated

```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data":{
    "streetOrderId":"1234567",
    "orderType":"LIMIT",
    "symbol":"BTC-USDT",
    "side":"BID",
    "owner":"2",
    "counterParty":"binance",
    "price":"0.00145",
    "quantity":"10.000",
    "filledPrice":"0.00",
    "filledQuantity":"0.00",
    "executionType":"EXECUTION_ACK",
    "createTime":1530861306000,
    "executionTime":1530861306000
  }
}
```

Response: failed to update

```javascript
{
  "success":false,
  "errorCode":1000,
  "errorMsg":"cannot update the order",
  "data":{
    "streetOrderId":"1234567",
    "orderType":"LIMIT",
    "symbol":"BTC-USDT",
    "side":"BID",
    "owner":"2",
    "counterParty":"binance",
    "price":"0.00123",
    "quantity":"10.000",
    "filledPrice":"0.00",
    "filledQuantity":"0.00",
    "executionType":"EXECUTION_PARTIAL_FILLED",
    "createTime":1530861306000,
    "executionTime":1530861306000
  }
}
```


### /order/cancel _[POST, Authentication]_
Request Body

```javascript
{
  "streetOrderId": "1234567",
  "owner": "2",
  "counterParty": "binance"
}
```

Response

```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data":{
    "streetOrderId":"1234567",
    "orderType":"LIMIT",
    "symbol":"BTC-USDT",
    "side":"BID",
    "owner":"2",
    "counterParty":"binance",
    "price":"0.00123",
    "quantity":"10.000",
    "filledPrice":"0.00",
    "filledQuantity":"0.00",
    "executionType":"EXECUTION_CANCELED",
    "createTime":1530861306000,
    "executionTime":1530861306000
  }
}
```

### /order/cancel-all _[POST, Authentication]_

Request Body:

```javascript
{
  "owner":"123",
  "selectedCounterParty":[
    "huobi",
    "binance"
  ]
}
```

Response:

```javascript
{
  "owner":"123",
  "canceledCounterParty":[
    "huobi",
    "binance"
  ]
}
```

### /order/info _[POST, Authentication]_

Request Body
- owner: 1234
- type: active (optional, value=`active`, `filled`, `all`, default value=`all`)
- pageIndex: 0 (optional, default value=`0`)
- pageSize: 10 (optional, default value=`10`)

Request Example:
```
/order/info?owner=1234&pageIndex=0&pageSize=20
```

Response body
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data":{
    "totalPage":10,
    "hasNext":true,
    "content":[
      {
        "streetOrderId":"1234567",
        "orderType":"LIMIT",
        "symbol":"BTC-USDT",
        "side":"BID",
        "owner":"2",
        "counterParty":"binance",
        "price":"0.00123",
        "quantity":"10.000",
        "filledPrice":"0.00",
        "filledQuantity":"0.00",
        "executionType":"EXECUTION_CANCELED",
        "createTime":1530861306000,
        "executionTime":1530861306000
      },
      {
        "streetOrderId":"1345678",
        "orderType":"LIMIT",
        "symbol":"BTC-USDT",
        "side":"ASK",
        "owner":"2",
        "counterParty":"huobi",
        "price":"0.00123",
        "quantity":"10.000",
        "filledPrice":"0.00",
        "filledQuantity":"0.00",
        "executionType":"EXECUTION_FILLED",
        "createTime":1530861306000,
        "executionTime":1530861306000
      }
    ]
  }
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
  "errorCode": 0,
  "data": {
    "uid": 2,
    "userName": "abc",
    "group": "group1",
    "authToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1aWQiOjIsImlzcyI6ImV4IiwiZXhwIjoxNTE3MTQ4NjYwLCJncm91cCI6Imdyb3VwMSJ9.YrYttRaGcFWro6WFsGkm3arnSQXyF1ZOWfT2-Uve6tI"
  }
}
```
error response

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
  "errorCode": 0,
  "data": {
    "uid": 2,
    "userName": "abc",
    "group": "group1",
    "authToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1aWQiOjQsImlzcyI6ImV4IiwiZXhwIjoxNTE3MTQ4OTQwLCJncm91cCI6Imdyb3VwMSJ9.Z6GXtZGPCxt2v-e5E_lQ_8sXrgQHu4uwKJjrdof0Cvc"
  }
}
```

error response

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