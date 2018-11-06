## Websocket API

### connect to websocket

- public user(without login): only public data
```
wss://api.coin-weight.com/ws?uid=0
```

- login user: will automatically subscribe all his order update stream

```
wss://api.coin-weight.com/ws?uid={uid}&token={jwt}
```

```
example: 

wss://api.coin-weight.com/ws?uid=1&token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1aWQiOjEsImlzcyI6ImV4IiwiZXhwIjoxNTI5MTcxMDE0LCJncm91cCI6Imdyb3VwMSJ9.s-wIDiBExMGCCyl-srynv1TrRtRi74lbqy3zx0Iyb44
```

### Subscribe & UnSubscribe

Notice:
  - use `seq` to track the response for the request

- Subscribe Topic
  - Request Example: subscribe depth price
    ```javascript
    {
    "type":"SUB.DEPTH_PRICE",
    "seq":1,
    "data":[
        {
        "symbol":"BTC-USDT",
        "counterParty":"binance"
        },
        {
        "symbol":"BTC-USDT",
        "counterParty":"huobi"
        }
    ]
    }
    ```

  - Response
    ```javascript
    {
        "errorCode": 0,
        "errorMsg": "ok",
        "seq": 1
    }
    ```

- UnSubscribe Topic
  - Request Example: UnSubscribe depth price
    ```javascript
    {
    "type":"UNSUB.DEPTH_PRICE",
    "seq":1,
    "data":[
        {
        "symbol":"BTC-USDT",
        "counterParty":"binance"
        },
        {
        "symbol":"BTC-USDT",
        "counterParty":"huobi"
        }
    ]
    }
    ```
  - Response
    ```javascript
    {
        "errorCode": 0,
        "errorMsg": "ok",
        "seq": 2
    }
    ```

### 1. Depth Price

#### Sub request
```javascript
{
  "type":"SUB.DEPTH_PRICE",
  "seq":1,
  "data":[
    {
      "symbol":"BTC-USDT",
      "counterParty":"binance"
    },
    {
      "symbol":"BTC-USDT",
      "counterParty":"huobi"
    }
  ]
}
```

#### Depth price data
```javascript
{
  "type":"DEPTH_PRICE",
  "data":{
    "counterParty":"binance",
    "symbol":"BTC-USDT",
    "bids":[
      {
        "price":"6323.98000000",
        "quantity":"2.87027100"
      },
      {
        "price":"6322.99000000",
        "quantity":"1.78365300"
      },
      {
        "price":"6316.98000000",
        "quantity":"0.63262900"
      },
      {
        "price":"6316.61000000",
        "quantity":"0.37062700"
      },
      {
        "price":"6315.65000000",
        "quantity":"0.01000000"
      }
    ],
    "asks":[
      {
        "price":"6324.86000000",
        "quantity":"0.02246900"
      },
      {
        "price":"6325.00000000",
        "quantity":"3.13870000"
      },
      {
        "price":"6325.50000000",
        "quantity":"0.10000000"
      },
      {
        "price":"6325.81000000",
        "quantity":"0.24000000"
      },
      {
        "price":"6326.00000000",
        "quantity":"0.08772600"
      }
    ]
  }
}
```

### 2. Simple Price

#### Sub request
```javascript
{
  "type":"SUB.SIMPLE_PRICE",
  "seq":1,
  "data":[
    {
      "symbol":"BTC-USDT",
      "counterParty":"binance"
    },
    {
      "symbol":"BTC-USDT",
      "counterParty":"okcoin"
    },
    {
      "symbol":"BTC-USDT",
      "counterParty":"fcoin"
    },
    {
      "symbol":"BTC-USDT",
      "counterParty":"huobi"
    }
  ]
}
```

#### Simple price data
```javascript
{
  "type":"SIMPLE_PRICE",
  "data":[
    {
      "counterParty":"binance",
      "symbol":"BTC-USDT",
      "bid":{
        "price":"6323.98000000",
        "quantity":"2.87027100"
      },
      "ask":{
        "price":"6324.86000000",
        "quantity":"0.02246900"
      }
    },
    {
      "counterParty":"huobi",
      "symbol":"BTC-USDT",
      "bid":{
        "price":"6323.98000000",
        "quantity":"2.87027100"
      },
      "ask":{
        "price":"6324.86000000",
        "quantity":"0.02246900"
      }
    }
  ]
}
```

### 3. Market Info
#### Sub request
```javascript
{
  "type":"SUB.MARKET_INFO",
  "seq":1,
  "data":null
}
```

#### Market Info Data
```javascript
{
  "type":"MARKET_INFO",
  "data":[
    {
      "counterParty":"binance",
      "symbol":"BTC-USDT",
      "last":"1.111",
      "change":0.05,
      "open":"1.123",
      "high":"1.222",
      "low":"1.001",
      "volume":"12324123123.123"
    },
    {
      "counterParty":"huobi",
      "symbol":"BTC-USDT",
      "last":"1.121",
      "change":0.06,
      "open":"1.133",
      "high":"1.122",
      "low":"1.041",
      "volume":"999923.123"
    }
  ]
}
```

### **4. User Order Event**

#### Sub
`by default automatically subscribed when user login`

#### Order Event Data

`Active Order Event`
- EXECUTION_ACK
- EXECUTION_PARTIAL_FILLED
- EXECUTION_CANCELING

```javascript
{
  "type":"USER_ORDER",
  "data":{
    "status":"ACTIVE",
    "sequence":1534926182000,
    "order":{
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
}
```

`Inactive Order Event`
- EXECUTION_FILLED
- EXECUTION_CANCELED
- EXECUTION_PARTIAL_CANCELED

```javascript
{
  "type":"USER_ORDER",
  "data":{
    "status":"INACTIVE",
    "sequence":1534926182000,
    "order":{
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
      "executionType":"EXECUTION_FILLED",
      "createTime":1530861306000,
      "processedTime":1530861306000
    }
  }
}
```

#### Contract Position

```javascript
{
  "type":"CONTRACT_POSITION",
  "data":{
    "owner":"14",
    "counterParty":"bitmex",
    "symbol":"XBT-Z18",
    "lastPrice":"6484.34",
    "liquidationPrice":"100000000",
    "contractQuantity":-1,
    "simpleQuantity":"-0.000154", // quantity in BTC
    "timestamp":1541548096050
  }
}
```
