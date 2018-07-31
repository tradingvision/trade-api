## this documents list all strategy types supported right now

### 1. SingleMarketMaker
- default configuration:

```javascript
{
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
```


### 2. VolumeMarketMaker
- default configuration:
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


### 3. arbitrage
- default configuration:
```javascript
{
	"name": "octmm",
	"strategyType": "Arbitrage",
	"owner": "strategy-11",
  "exchange": "binance",
	"symbols": [{
		"name": "test",
		"symbol": "ETH-BTC",
		"expiredSeconds": 2,
		"margins": [{
			"currency1": "ETH",
			"margin1": 0.1,
      "currency2": "BTC",
			"margin2": 0.01,
			"counterParty": "binance"
		}, {
			"currency1": "ETH",
			"margin1": 0.1,
      "currency2": "BTC",
			"margin2": 0.01,
			"counterParty": "bigone"
		}],
		"exchange1": "binance",
    "exchange2": "bigone",
    "buySpread": -0.001,
    "sellSpread": 0.001,
		"maxOrderQty": 0.1
	}]
} 
```


### 4. triangular
- default configuration:
```javascript
{
    "bCancel": true, 
    "exchange": "binance", 
    "name": "test3", 
    "owner": "strategy-11", 
    "strategyType": "Triangular", 
    "symbols": [
        {
            "counterParty": "binance", 
            "currency1": "ETH", 
            "currency2": "BTC", 
            "currency3": "ONT", 
            "expiredSeconds": 5, 
            "margin1": "0.1", 
            "margin2": "0.01", 
            "margin3": "6", 
            "maxOrderSize": 0.01, 
            "name": "test", 
            "parentIndex": 2, 
            "symbol1": "ETH-BTC", 
            "symbol2": "ONT-ETH", 
            "symbol3": "ONT-BTC", 
            "thresholdPct": 0.01
        }
    ]
}

```


### 5. autospreadingpct
- default configuration:
```javascript
{
    "bCancel": true, 
    "exchange": "binance", 
    "name": "test4", 
    "owner": "strategy-11", 
    "strategyType": "AutoSpreadingPct", 
    "symbols": [
        {
            "makerCcy1": "ETH", 
            "makerCcy2": "BTC", 
            "makerExchange": "binance", 
            "makerMargin1": "0.1", 
            "makerMargin2": "0.06", 
            "maxOrderSize": "0.02", 
            "name": "UNNAME", 
            "parentIndex": 3, 
            "priceVar": "0", 
            "quantityVar": "0", 
            "side": "Ask", 
            "spreadPct": "0.01", 
            "symbol": "ETH-BTC", 
            "watchedExchange": "binance"
        }
    ]
}
```

### 5. autospreadingvalue
- default configuration:
```javascript
{
    "bCancel": true, 
    "exchange": "binance", 
    "name": "test4", 
    "owner": "strategy-11", 
    "strategyType": "AutoSpreadingValue", 
    "symbols": [
        {
            "makerCcy1": "ETH", 
            "makerCcy2": "BTC", 
            "maxOrderSize": "0.015", 
            "name": "UNNAME", 
            "parentIndex": 3, 
            "priceVar": "0", 
            "quantityVar": "0", 
            "side": "Ask", 
            "spreadValue": "0.0001", 
            "symbol": "ETH-BTC", 
            "watchedExchange": "binance",
            "makerExchange": "huobi", 
            "margins": [{
              "currency1": "ETH",
              "margin1": 0.1,
              "currency2": "BTC",
              "margin2": 0.06,
              "counterParty": "binance"
            }, {
              "currency1": "ETH",
              "margin1": 0.1,
              "currency2": "BTC",
              "margin2": 0.06,
              "counterParty": "huobi"
            }]
        }
    ]
}
```


### 6. autospreadingvquote
- default configuration:
```javascript
{
    "bCancel": true, 
    "exchange": "binance", 
    "name": "test4", 
    "owner": "strategy-11", 
    "strategyType": "AutoSpreadingValue", 
    "symbols": [
        {
            "makerCcy1": "ETH", 
            "makerCcy2": "BTC", 
            "maxOrderSize": "0.015", 
            "name": "UNNAME", 
            "parentIndex": 3, 
            "priceVar": "0", 
            "quantityVar": "0", 
            "side": "Ask", 
            "spreadValue": "0.0001", 
            "symbol": "ETH-BTC", 
            "watchedExchange": "binance",
            "makerExchange": "huobi", 
            "quoteExchange": "huobi",
            "margins": [{
              "currency1": "ETH",
              "margin1": 0.1,
              "currency2": "BTC",
              "margin2": 0.06,
              "counterParty": "binance"
            }, {
              "currency1": "ETH",
              "margin1": 0.1,
              "currency2": "BTC",
              "margin2": 0.06,
              "counterParty": "huobi"
            }]
        }
    ]
}
```


### 7. SingleMarketMaker
- default configuration:
```javascript

{
    "exchange": "binance", 
    "name": "test4", 
    "owner": "strategy-11", 
    "strategyType": "SpeedOrder", 
    "symbols": [
        {
            "name": "speedOrder",
            "symbol": "ETH-BTC",
            "side": "Bid",
            "currency1": "ETH",
            "margin1": 0.1,
            "currency2": "BTC",
            "margin2": 0.01,
            "basePrice": 0.07,
            "priceStep": 0.0001,
            "priceVar": 0.0001,
            "orderNum": 2,
            "orderQty": 0.1,
            "qtyVar": 0.02
          }
    ]
}
```