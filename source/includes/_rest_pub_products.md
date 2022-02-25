### List all Products 

> List all Products 

```
curl -X GET "https://ascendex.com/api/pro/v1/cash/products"
curl -X GET "https://ascendex.com/api/pro/v1/margin/products"
```

> Sample Response 

```json
{
    "code": 0,
    "data": [
        {
            "symbol":                "ASD/USDT",
            "baseAsset":             "ASD",
            "quoteAsset":            "USDT",
            "status":                "Normal",
            "minNotional":           "5",
            "maxNotional":           "100000",
            "marginTradable":         true,
            "commissionType":        "Quote",
            "commissionReserveRate": "0.001",
            "tickSize":              "0.000001",
            "lotSize":               "0.001"
        }
    ]
}
```

#### HTTP Request

`GET /api/pro/v1/{accountCategory}/products`

#### Response Content

You can obtain a list of all products traded on the exchange through this API.

The response contains the following general fields:

 Name         | Type     | Description                                                                                 
-------------- | -------- | --------------------- 
 `symbol`      | `String` | e.g. `"ASD/USDT"`
 `baseAsset`   | `String` | e.g. `"ASD"`
 `quoteAsset`  | `String` | e.g. `"USDT"`
 `status`      | `String` | `"Normal"`

The response also contains criteria for new order request. 

 Name                    | Type      | Description                                                                                 
------------------------ | --------- | --------------------- 
 `symbol`                | `String`  | Symbol like `BTC/USDT`
 `displayName`           | `String`  | Symbol's display Name
 `domain`                | `String`  | Symbol's Tradins Domain `USDS` / `ETH` / `BTC`
 `tradingStartTime`      | `Number`  | Trading Start Time
 `collapseDecimals`      | `String`  | Trading Start Time
 `minQty`                | `String`  | minimum quantity of an order
 `maxQty`                | `String`  | minimum quantity of an order
 `minNotional`           | `String`  | minimum notional of an order 
 `maxNotional`           | `String`  | maximum notional of an order 
 `statusCode`            | `String`  | Symbol's status code
 `statusMessage`         | `String`  | Symbol's status message
 `tickSize`              | `String`  | tick size of order price 
 `useTick`               | `Boolean` | 
 `lotSize`               | `String`  | lot size of order quantity 
 `useLot`                | `Boolean` | 
 `commissionType`        | `String`  | `"Base"`, `"Quote"`, `"Received"`
 `commissionReserveRate` | `String`  | e.g. `"0.001"`, see below.
 `qtyScale`              | `Number`  | Quantity Scale
 `notionalScale`         | `Number`  | Notional Scale


When placing orders, you should comply with all criteria above. More details can be found in the [Order Request Criteria](#order-request-criteria) section.


