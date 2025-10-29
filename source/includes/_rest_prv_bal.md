## Balance

### Cash Account Balance 

> Cash Account Balance - Sample response 

```json
{
    "code": 0,
    "data": [
        {
            "asset":            "USDT",
            "totalBalance":    "1285.366663467",
            "availableBalance": "1285.366663467"
        },
        {
            "asset":            "BTC",
            "totalBalance":    "22.1308675",
            "availableBalance": "16.1308675"
        },
        {
            "asset":            "ETH",
            "totalBalance":     "0.6",
            "availableBalance": "0.6"
        }
    ]
}
```

#### HTTP Request

`GET <account-group>/api/pro/v1/cash/balance`

#### Signature

You should sign the message in header as specified in [**Authenticate a RESTful Request**](#sign-a-request) section.

#### Prehash String

`<timestamp>+balance`


#### Request Parameters

Name          |  Type     | Required | Value Range    | Description
------------- | --------- | -------- | ---------------| -----------
**asset**     | `String`  |   No     |valid asset code| this allow you query single asset balance, e.g. `BTC`
**showAll**   | `Boolean` |   No     |  true / false  | by default, the API will only respond with assets with non-zero balances. Set `showAll=true` to include all assets in the response. 


#### Response Content

 Name                | Type     | Description                        | Sample Response
-------------------- | -------- | ---------------------------------- | -------------------------
**asset**            | `String` | asset code                         | `"USDT"`
**totalBalance**     | `String` | total balance in string format     | `"1234.56"`
**availableBalance** | `String` | available balance in string format | `"234.56"`