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


### Margin Account Balance 

> Margin Account Balance - Sample response 

```json
{
    "code": 0,
    "data": [
        {
            "asset":            "USDT",
            "totalBalance":     "11200",
            "availableBalance": "11200",
            "borrowed":         "0",
            "interest":         "0"
        },
        {
            "asset":            "ETH",
            "totalBalance":     "20",
            "availableBalance": "20",
            "borrowed":         "0",
            "interest":         "0"
        }
    ]
}
```

#### HTTP Request

`GET <account-group>/api/pro/v1/margin/balance`

#### Signature

You should sign the message in header as specified in [**Authenticate a RESTful Request**](#signing-a-Request) section.

#### Prehash String

`<timestamp>+balance`

#### Request Parameters 

Name          |  Type     | Required | Value Range    | Description
------------- | --------- | -------- | -------------- | -----------
**asset**     | `String`  |   No     |valid asset code| this allow you query single asset balance, e.g. `BTC`
**showAll**   | `Boolean` |   No     |  true / false  | by default, the API will only respond with assets with non-zero balances. Set `showAll=true` to include all assets in the response. 


#### Response Content

 Name                | Type     | Description       | Sample Response
-------------------- | -------- | ----------------- | -------------------------
**asset**            | `String` | asset code        | `"USDT"`
**totalBalance**     | `String` | total balance     | `"1234.56"`
**availableBalance** | `String` | available balance | `"234.56"`
**borrowed**         | `String` | borrowed amount   | `"0"`
**interest**         | `String` | interest owed     | `"0"`


### Margin Risk Profile

> Margin Account Risk Profile - Sample response 

```json
{
    "code": 0,
    "data": {
        "accountMaxLeverage":     "10",
        "availableBalanceInUSDT": "17715.8175",
        "totalBalanceInUSDT":     "17715.8175",
        "totalBorrowedInUSDT":    "0",
        "totalInterestInUSDT":    "0",
        "netBalanceInUSDT":       "17715.8175",
        "pointsBalance":          "0",
        "currentLeverage":        "1",
        "cushion":                "-1"
    }
}
```

#### HTTP Request

`POST <account-group>/api/pro/v1/margin/risk`

#### Signature

You should sign the message in header as specified in [**Authenticate a RESTful Request**](#signing-a-Request) section.

#### Prehash String

`<timestamp>+margin/risk`


### Balance Transfer

> Transfer from Cash To Margin - Sample request and response

> Request

```json
{
    "amount": "11.0",
    "asset": "USDT",
    "fromAccount": "cash",
    "toAccount": "margin"
}
 ```

> Response

```json
{
    "code": 0,
}
```

This api allows balance transfer between different accounts of the same user.

#### HTTP Request

`POST <account-group>/api/pro/v1/transfer`

#### Signature

You should sign the message in header as specified in [**Authenticate a RESTful Request**](#sign-request) section.

#### Prehash String

`<timestamp>+transfer`

#### Request Parameters 

Name           |  Type     | Required | Value Range               | Description
-------------- | --------- | -------- | ------------------------- | -----------
**amount**     | `String`  |   Yes    | Positive numerical string | Asset amount to transfer.
**asset**      | `String`  |   Yes    | Valid asset code          | 
**fromAccount**| `String`  |   Yes    | `cash`/`margin`/`futures` |
**toAccount**  | `String`  |   Yes    | `cash`/`margin`/`futures` |


#### Response Content

Response `code` value 0 indicate successful transfer. 


### Balance Transfer for Subaccount

> Request

```json
{
    "userFrom": "parent-account-userId",  // support userId or username, pls use "parentUser" for parent account username
    "userTo": "sub-account-userId",  // support userId or username, pls use "parentUser" for parent account username
    "acFrom": "cash",
    "acTo": "cash",
    "asset": "USDT",
    "amount": "40",
    "mode": "ack" // ack/accept. use accept to hold and wait ack from OMS
}
 ```

> Response

```json
{
   "code":0,
   "info":{
      "acFrom":"cash",
      "acTo":"cash",
      "amount":"40.0000",
      "asset":"USDT",
      "userFrom":"parent-account-userId",
      "userTo":"sub-account-userId"
   }
}
```

This api allows balance transfer between the parent and sub accounts and between two sub accounts. You can only call this API from the parent account.

#### HTTP Request

`POST <account-group>/api/pro/v2/subuser/subuser-transfer`

#### Signature

You should sign the message in header as specified in [**Authenticate a RESTful Request**](#sign-request) section.

#### Prehash String

`<timestamp>+subuser-transfer`

#### Request Parameters 

Name           |  Type     | Required | Value Range               | Description
-------------- | --------- | -------- | ------------------------- | -----------
**amount**     | `String`  |   Yes    | Positive numerical string | Asset amount to transfer.
 **asset**     | `String`  |   Yes    | Valid asset code          | 
**userFrom**   | `String`  |   Yes    | userId or username        | use `parentUser` as parent account username
 **userTo**    | `String`  |   Yes    | userId or username        | use `parentUser` as parent account username
 **acFrom**    | `String`  |   Yes    | `cash`/`margin`/`futures` |
  **acTo**     | `String`  |   Yes    | `cash`/`margin`/`futures` |
  **mode**     | `String`  |   No     | empty/`ack`/`accept`      | use `accept` to hold-and-wait for confirmation from the exchange backend.


#### Response Content

Response `code` value 0 indicates successful transfer and `info` echos the request parameters. 

### Balance Transfer history for Subaccount

> Request

```json
{
    "subUserId": null,
    "asset": null,
    "startTime": null,
    "endTime": null,
    "page": 1,
    "pageSize": 10
}
 ```

> Response

```json
{
    "code": 0,
    "data": {
        "page": 1,
        "pageSize": 10,
        "totalSize": 6,
        "limit": 10,
        "data": [
            {
                "time": 1620125922493,
                "userFrom": "ParentUser",
                "userTo": "sub-account-username",
                "acFrom": "futures",
                "acTo": "cash",
                "asset": "USDT",
                "amount": "11",
                "status": "SUCCESS"
            }
            ...
        ]
    }
}
```

This api allows to fetch balance transfer history among different sub users based on given filtering conditions. You can only call this API from the parent account.

#### HTTP Request

`POST <account-group>/api/pro/v2/subuser/subuser-transfer-hist`

#### Signature

You should sign the message in header as specified in [**Authenticate a RESTful Request**](#sign-request) section.

#### Prehash String

`<timestamp>+subuser-transfer-hist`

#### Request Parameters 

Name           |  Type     | Required | Value Range               | Description
-------------- | --------- | -------- | ------------------------- | -----------
 **asset**     | `String`  |    No    | Valid asset code          | asset to query
**subUserId**  | `String`  |    No    |   userId                  | user id to query
**startTime**  |  `Long`   |    No    | timestamp in milliseconds | start time to query
 **endTime**   |  `Long`   |    No    | timestamp in milliseconds | end time to query
  **page**     |   `Int`   |   Yes    | number of page            | number of page
**pageSize**   |   `Int`   |   Yes    |     1-10                  | record size in one page


#### Response Content

Response `code` value 0 indicates successful query and `data` shows query result. 
