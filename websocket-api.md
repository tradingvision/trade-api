## Websocket API

### connect to websocket

- public user(without login): only public data
```
wss://api.coin-weight.com/ws?uid=0
```

- login user: will automactically subscribe all his order update stream

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
    {
        "type": "SUB",
        "seq": 1,
        "data": {
            "symbol": "BTC-USDT",
            "counterParty": "binance"
        }
    }
    ```
  - Response
    ```
    {
        "errorCode": 0,
        "errorMsg": "ok",
        "seq": 1
    }
    ```

- UnSubscribe Topic
  - Request
    ```
    {
        "type": "UNSUB",
        "seq": 2,
        "data": {
            "symbol": "BTC-USDT",
            "counterParty": "binance"
        }
    }
    ```
  - Response
    ```
    {
        "errorCode": 0,
        "errorMsg": "ok",
        "seq": 2
    }
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