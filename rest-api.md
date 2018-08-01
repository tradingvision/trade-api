
## Endpoint: 
`https://api.coin-weight.com/exp/v2`

## Basic Models

orderType:
- LIMIT
- MARKET

side:
- BID  (BUY)
- ASK  (SELL)

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
  "processedTime":1530861306000
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

## Global API

### /test-auth _[GET, Authentication]_
Success Response
```javascript
{
  "uid": 1,
  "group": "group1"
}
```

Error Response: `HTTP code 403`
```javascript
{
  "success": false,
  "errorCode": 403,
  "errorMsg": "Authentication Failure",
  "data": null
}
```

## Config API

### /config/symbols _[GET]_

```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data":[
    {
      "counterParty":"huobi",
      "symbols":[
        {
          "name":"ETH-USDT", // symbol name
          "base":"ETH",
          "quote":"USDT",
          "priceDecimal":2, // when create order, price precision at most 0.01
          "quantityDecimal":4, // when create order, quantity precision at most 0.0001
          "quantityMin":0.001, // when create order, minimum quantity 0.001
          "quantityMax":10000.0 // when create order, maximum quantity 10000.0
        }
      ]
    },
    {
      "counterParty":"binance",
      "symbols":[
        {
          "name":"ETH-USDT",
          "base":"ETH",
          "quote":"USDT",
          "priceDecimal":2,
          "quantityDecimal":4,
          "quantityMin":0.001,
          "quantityMax":10000.0
        }
      ]
    }
  ]
}
```

### /config/market/default _[GET]_

Response Body
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data":[
    {
      "category":"BTC", // tab name
      "items":[
        {
          "counterParty":"binance",
          "symbol":"ETH-BTC",
          “favorite”: false
        },
        {
          "counterParty":"huobi",
          "symbol":"ETH-BTC",
          “favorite”: false
        }
      ]
    },
    {
      "category":"ETH",
      "items":[
        {
          "counterParty":"binance",
          "symbol":"EOS-ETH",
          “favorite”: false
        },
        {
          "counterParty":"huobi",
          "symbol":"EOS-ETH",
          “favorite”: false
        }
      ]
    }
  ]
}
```

### /config/market/custom/{uid} _[GET, Authentication]_

Response Body
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data":[
    {
      "category":"BTC", // tab name
      "items":[
        {
          "counterParty":"binance",
          "symbol":"ETH-BTC",
          “favorite”: true
        },
        {
          "counterParty":"huobi",
          "symbol":"ETH-BTC",
          “favorite”: false
        }
      ]
    },
    {
      "category":"ETH",
      "items":[
        {
          "counterParty":"binance",
          "symbol":"EOS-ETH",
          “favorite”: false
        },
        {
          "counterParty":"huobi",
          "symbol":"EOS-ETH",
          “favorite”: true
        }
      ]
    }
  ]
}
```

### /config/market/custom/{uid}/favorite _[GET, Authentication]_
Response Body
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data":[
    {
      "counterParty":"binance",
      "symbol":"ETH-BTC"，
      “favorite”: true
    },
    {
      "counterParty":"huobi",
      "symbol":"EOS-USDT",
      “favorite”: true
    }
  ]
}
```

### /config/market/custom/{uid}/favorite/add _[POST, Authentication]_
Request Body
```javascript
{
  "counterParty": "binance",
  "symbol": "ETH-BTC"
}
```

Response Body
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data": null
}
```

### /config/strategy/default/items _[get]_

#### full list of strategy types list can refer [REST API document](strategy.md)

Response Body
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data": [   
    {
      "name": "VolumeMarketMaker",
      "default": {
        "name": "octmm",
        "exchange": "binance",
        "owner": "strategy-10",
        "strategyType": "VolumeMarketMaker",
        "symbols": [{
            "name": "test",
            "symbol": "ETH-BTC",
            "currency1": "ETH",
            "margin1": 0.2,
            "currency2": "BTC",
            "margin2": 0.02,
            "orderQty": 0.1,
            "qtyVar": 0.0,
            "priceVar": 0.0,
            "middlePrice": 0,
            "replaceDelay": 60.0,
            "delayVar": 10
          }
        ]
      }
    }, 
    {
      "name": "SingleMarketMaker",
      "default": {
        "name": "octmm",
        "exchange": "binance",
        "strategyType": "SingleMarketMaker",
        "owner": "strategy-11",
        "symbols": [{
            "name": "test",
            "symbol": "ETH-BTC",
            "side": "Both",
            "currency1": "ETH",
            "margin1": 0.1,
            "currency2": "BTC",
            "margin2": 0.01,
            "orderNumber": 1,
            "spreadPct": 0.04,
            "orderQty": 0.1,
            "qtyVar": 0.0,
            "priceStep": 0.0001,
            "priceVar": 0.0,
            "middlePrice": 0,
            "replaceDelay": 1.0,
            "middlePriceStep": 0.0001,
            "toleranceVar": 0.001
          }
        ]
      }
    }
  ]
}
```

### /config/strategy/custom/{uid}/items _[GET, Authentication]_

#### list of strategy items list can refer [REST API document](strategy.md)

Response Body
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data": [
    {
      'id': "1",
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
    }
  ]
}
```

### /config/strategy/custom/{uid}/item/update _[POST, Authentication]_
Request Body
```javascript
{
  "id": "1",
  "name": "octmm",
  "exchange": "binance",
  "owner": "strategy-10",
  "strategyType": "VolumeMarketMaker",
  "symbols": [{
      "name": "test",
      "symbol": "ETH-BTC",
      "currency1": "ETH",
      "margin1": 0.2,
      "currency2": "BTC",
      "margin2": 0.02,
      "orderQty": 0.1,
      "qtyVar": 0.0,
      "priceVar": 0.0,
      "middlePrice": 0,
      "replaceDelay": 60.0,
      "delayVar": 10
    }
  ]
}
```
Response Body
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data":  { 
    "id": "1",
    "name": "octmm",
    "exchange": "binance",
    "owner": "strategy-10",
    "strategyType": "VolumeMarketMaker",
    "symbols": [{
        "name": "test",
        "symbol": "ETH-BTC",
        "currency1": "ETH",
        "margin1": 0.2,
        "currency2": "BTC",
        "margin2": 0.02,
        "orderQty": 0.1,
        "qtyVar": 0.0,
        "priceVar": 0.0,
        "middlePrice": 0,
        "replaceDelay": 60.0,
        "delayVar": 10
      }
    ]
  }
}
```

### /config/strategy/custom/{uid}/item/create _[POST, Authentication]_
Request Body
```javascript
{
    "name": "octmm",
    "exchange": "binance",
    "owner": "strategy-10",
    "strategyType": "VolumeMarketMaker",
    "symbols": [{
        "name": "test",
        "symbol": "ETH-BTC",
        "currency1": "ETH",
        "margin1": 0.2,
        "currency2": "BTC",
        "margin2": 0.02,
        "orderQty": 0.1,
        "qtyVar": 0.0,
        "priceVar": 0.0,
        "middlePrice": 0,
        "replaceDelay": 60.0,
        "delayVar": 10
      }
    ]
}
```
Response Body
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data":  { 
    "1" : {
      "name": "octmm",
      "exchange": "binance",
      "owner": "strategy-10",
      "strategyType": "VolumeMarketMaker",
      "symbols": [{
          "name": "test",
          "symbol": "ETH-BTC",
          "currency1": "ETH",
          "margin1": 0.2,
          "currency2": "BTC",
          "margin2": 0.02,
          "orderQty": 0.1,
          "qtyVar": 0.0,
          "priceVar": 0.0,
          "middlePrice": 0,
          "replaceDelay": 60.0,
          "delayVar": 10
        }
      ]
    }
  }
}
```


### /config/strategy/custom/{uid}/item/delete _[POST, Authentication]_
Request Body
```javascript
{
  "id": "1"
}
```
Response Body
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data": {
    "1": {
      "name": "octmm",
      "exchange": "binance",
      "owner": "strategy-10",
      "strategyType": "VolumeMarketMaker",
      "symbols": [{
          "name": "test",
          "symbol": "ETH-BTC",
          "currency1": "ETH",
          "margin1": 0.2,
          "currency2": "BTC",
          "margin2": 0.02,
          "orderQty": 0.1,
          "qtyVar": 0.0,
          "priceVar": 0.0,
          "middlePrice": 0,
          "replaceDelay": 60.0,
          "delayVar": 10
        }
      ]
    }
  }
}
```

### /config/market/custom/{uid}/favorite/delete _[POST, Authentication]_
Request Body
```javascript
{
  "counterParty": "binance",
  "symbol": "ETH-BTC"
}
```

Response Body
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data": null
}
```

### /config/layout/professional/default _[GET]_
Response Body
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data":[
    {
      "position":1,
      "counterParty":"binance",
      "symbol":"ETH-BTC"
    },
    {
      "position":2,
      "counterParty":"huobi",
      "symbol":"EOS-USDT"
    },
    {
      "position":3,
      "counterParty":"okcoin",
      "symbol":"EOS-USDT"
    }
  ]
}
```

### /config/layout/professional/custom/{uid} _[GET, Authentication]_
Response Body
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data":[
    {
      "position":1,
      "counterParty":"binance",
      "symbol":"ETH-BTC"
    },
    {
      "position":2,
      "counterParty":"huobi",
      "symbol":"EOS-USDT"
    },
    {
      "position":3,
      "counterParty":"okcoin",
      "symbol":"EOS-USDT"
    }
  ]
}
```

### /config/layout/professional/custom/{uid}/update _[POST, Authentication]_
Request Body
```javascript
{
  "position":1,
  "counterParty":"binance",
  "symbol":"EOS-USDT"
}
```

Response Body
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data":null
}
```

### /config/layout/simple/default _[GET]_
Response Body
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data":[
    {
      "position":1,
      "counterParty":"binance",
      "symbol":"ETH-BTC"
    },
    {
      "position":2,
      "counterParty":"huobi",
      "symbol":"EOS-USDT"
    },
    
    // .....
    
    {
      "position":12,
      "counterParty":"okcoin",
      "symbol":"EOS-USDT"
    }
  ]
}
```

### /config/layout/simple/custom/{uid} _[GET, Authentication]_
Response Body
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data":[
    {
      "position":1,
      "counterParty":"binance",
      "symbol":"ETH-BTC"
    },
    {
      "position":2,
      "counterParty":"huobi",
      "symbol":"EOS-USDT"
    },
    
    // ...
    
    {
      "position":12,
      "counterParty":"okcoin",
      "symbol":"EOS-USDT"
    }
  ]
}
```

### /config/layout/simple/custom/{uid}/update _[POST, Authentication]_
Request Body
```javascript
{
  "position":1,
  "counterParty":"binance",
  "symbol":"EOS-USDT"
}
```

Response Body
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data":null
}
```



## Order API

### /order/create _[POST, Authentication]_

Request

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
    "processedTime":1530861306000
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
    "processedTime":1530861306000
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
    "processedTime":1530861306000
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
    "processedTime":1530861306000
  }
}
```

### /order/cancel-all _[POST, Authentication]_

Request:

```javascript
{
  "owner":"123",
  "selectedCounterParty":[
    "huobi",
    "binance"
  ]
}
```

  - selectedCounterParty: empty array will cancel all counterParties


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

### /order/active _[GET, Authentication]_

`Active Order:`
- EXECUTION_ACK
- EXECUTION_PARTIAL_FILLED
- EXECUTION_CANCELING

Request Parameters
- owner: 1234

Response body (order by ORDER createTime DESC)
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data":[
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
      "processedTime":1530861306000
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
      "executionType":"EXECUTION_PARTIAL_FILLED",
      "createTime":1530861306000,
      "processedTime":1530861306000
    }
  ]
}
```

### /order/inactive _[GET, Authentication]_

`Inactive Order:`
- EXECUTION_FILLED
- EXECUTION_CANCELED
- EXECUTION_PARTIAL_CANCELED

Request Parameters
- owner: 1234
- startTime: 1530861306000 (optional, default value= currentTimestamp - 24hour)
- endTime: 1530861306000 (optional, default value= currentTimestamp)
- pageIndex: 0 (optional, default value=`0`)
- pageSize: 10 (optional, default value=`10`)


Response body (order by ORDER createTime DESC)
```javascript
{
  "success":true,
  "errorCode":0,
  "errorMsg":"ok",
  "data":{
    "totalPage":10,
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
        "processedTime":1530861306000
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
        "processedTime":1530861306000
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
    "email": "abc@gmail.com",
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
  "userName": "abc",
  "password": "pwd123"
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

### /user/{uid}/info _[GET, Authentication]_

```javascript
{
  "success":true,
  "errorCode":0,
  "data":{
    "uid":"123",
    "googleAuthEnabled":false,
    "group":"group1",
    "userName":"test",
    "phone":"135555555",
    "email":"xxx@ggmail.com",
    "avatar":"https://cdn1.iconfinder.com/data/icons/ninja-things-1/1772/ninja-512.png",
    "estimatedValue":"10.0027 USDT",
    "availableValue":"82.0000",
    "netUnitValue":"10.0027 USDT",
    "onOrders":"12.000",
    "changePercentage":-0.0218
  }
}
```

### /user/{uid}/position _[GET, Authentication]_

```javascript
{
  "success":true,
  "errorCode":0,
  "data":[
    {
      "counterParty":"binance",
      "currency":"BTC",
      "totalAmount":"1000.0",
      "usedAmount":"12.001",
      "availableAmount": "999"
    },
    {
      "counterParty":"binance",
      "currency":"ETH",
      "totalAmount":"400.0",
      "usedAmount":"42.00",
      "availableAmount": "339"
    }
  ]
}
```


### /user/pwd/change _[POST, Authentication]_ (Not Released)

request

```javascript
{
  "uid": 1234
  "oldPassword": "old_password",
  "newPassword": "new_password"
}
```

### /user/phone/verify _[POST, Authentication]_ (Not Released)
request

```javascript
{
  "uid": 1234
  "phone": "123456789",
  "countryCode": "+86"
}
```

### /user/phone/bind _[POST, Authentication]_ (Not Released)

request

```javascript
{
  "uid": 1234
  "phone": "123456789",
  "countryCode": "+86",
  "verificationCode": "123456"
}
```

### /user/totp/new _[POST, Authentication]_ (Not Released)

response

```javascript
{
  "bind": false
  "hashKey": "asbcas123sdasd",
  "qrCodeText": "asbcas123sdasdasbcas123sdasd" // javascript generate QR code
}
```

### /user/totp/bind _[POST, Authentication]_ (Not Released)

response

```javascript
{
  "uid": 1234
  "authCode": "025187" // auth code from google authenticator
}
```

## API key & Secret key API

### /secret/{owner} _[GET, Authentication]_

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

### /secret/add _[POST, Authentication]_

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


### /secret/delete _[POST, Authentication]_

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

### /strategy/start [POST, Authentication]

request

```javascript
{
  "userid": "123",
  "strategy": {
      "id": "1",
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
    }
}
```

ok response
```javascript
{
  "success": true,
  "seq_no": 0,
  "error_code": 0,
  "data": {
    "strategy": {
      "id": "1",
      "status": "running"
    }
  }
}
```

| error_code | reason |
|:---:|---|
| 0 | OK |
| 500 | Unexpected Error (wrong owner) |


## stop strategy

### /strategy/stop _[POST, Authentication]_

request


```javascript
{
  "userid": "123",
  "strategy": { 
    "id":  "1", 
    "bCancel": true,
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
  }
}
```

ok response

```javascript
{
  "success": true,
  "seq_no": 0,
  "error_code": 0,
  "data": {
    "strategy": { 
      "id":  "1"，
      'status': "stopped"
    }
  }
}
```

| error_code | reason |
|:---:|---|
| 0 | OK |
| 500 | Unexpected Error (wrong owner) |

<<<<<<< HEAD
## leave strategy
### /strategy/stop _[POST, Authentication]_

request


```javascript
{
  "userid": "123",
  "strategy": { 
    "id":  "1", 
    "bCancel": false,
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
  }
}
```

| error_code | reason |
|:---:|---|
| 0 | OK |
| 500 | Unexpected Error (wrong owner) |


### /strategy/status/{uid} _[GET, Authentication]_


ok response

```javascript
{
  "success": true,
  "seq_no": 0,
  "error_code": 0,
  "data": {
    "status": [
      {
        "id": "1",
        "status": "running"
      },
      {
        "id": "2",
        "status": "stopped"
      }
    ]
  }
}
```

| error_code | reason |
|:---:|---|
| 0 | OK |
| 500 | Unexpected Error (wrong owner) |
