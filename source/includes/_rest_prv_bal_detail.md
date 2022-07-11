## Balance Snapshot And Update Detail

Here we provide rest API to get daily balance snapshot, and intraday balance and order fills update details. We recommend calling balance snapshot endpoint(`<cash/margin>/balance/snapshot`) to get balance at the beginning of the day, and get the sequence number `sn`; then start to query balance or order fills update from `<cash/margin>/balance/history` by setting parameter `sn` value to be `sn + 1`.

Please note we enforce rate limit 8 / minute. Data query for most recent 7 days is supported.

### Balance Snapshot

This API returns cash or margin balance snapshot infomation on daily basis.

#### HTTP Request

For cash
`GET  api/pro/data/v1/cash/balance/snapshot`

For margin
`GET  api/pro/data/v1/margin/balance/snapshot`

#### Signature

You should sign the message in header as specified in [**Authenticate a RESTful Request**](#signing-a-Request) section.

#### Prehash String

For cash
`<timestamp>+data/v1/cash/balance/snapshot`

For margin
`<timestamp>+data/v1/margin/balance/snapshot`

> Cash Account Balance Snapshot - Sample response

```json
{
    "meta": {
        "ac": "cash",
        "accountId": "cshfi7p9j312936d2hkjJpAahWyb4RCJ",
        "sn": 10870097597,
        "snapshotTime": 1617148800000
    },

    "snapshot": [
        {"asset": "AKRO", "totalBalance": "1858"},
        {"asset": "ALTBEAR", "totalBalance": "250"},
        {"asset": "ANKR", "totalBalance": "200"}
    ]
}
```

#### Request Parameters

Name        |  Type     | Required |           Value Range       | Description
------------| --------- | -------- |-----------------------------| -----------
**date**    | `String`  |   Yes    | `YYYY-mm-dd`                |  balance date

#### Response Content

 Name       | Type         | Description
----------- | -------------| ---------------------------------
**meta**    | `Json`       | `meta` info. See detail below
**balance** | `Json Array` | `balance` info. See detail below

`meta` field provides some basic info.

`meta` schema

 Name            | Type     | Description                    | Sample Response
-----------------| -------- | -------------------------------| -------------------------
**ac**           | `String` | account category               | `"cash" or "margin"`
**accountId**    | `String` | accountId                      |
**sn**           | `Long`   | sequence number                |
**balanceTime**  | `Long`   | balance snapshot time in milli seconds  |

`balance` field provides asset balance snapshot info.

`balance` schema

 Name              | Type     | Description                   | Sample Response
-------------------| -------- | ------------------------------| ----------------
**asset**          | `String` | asset code                    | `"USDT"`
**totalBalance**   | `String` | current asset total balance   | `"1234.56"`

#### Code Sample

Please refer to python code to [query balance snapshot](https://github.com/ascendex/ascendex-pro-api-demo/blob/master/python/query_balance_and_order_fills.py)


### Order and Balance Detail

This API is for intraday balance change detail from balance event and order fillss.

#### HTTP Request

For cash
`GET  api/pro/data/v1/cash/balance/history`

For margin
`GET  api/pro/data/v1/margin/balance/history`


#### Prehash String

For cash
`<timestamp>+data/v1/cash/balance/history`

For margin
`<timestamp>+data/v1/margin/balance/history`

> Cash Account Balance Detail - Sample response 

```json
{
    "meta": {
        "ac": "cash", 
        "accountId": "MPXFNEYEKFK93CREYX3LTCIDIJPCFNIX"
    },

    "order": [
        {
            "data": [
                {
                    "asset": "BTC",  
                    "curBalance": "0.0301", 
                    "dataType": "trade", 
                    "deltaQty": "0.0001"
                },
                {
                    "asset": "USDT",
                    "curBalance": "-1628.51466633",
                    "dataType": "trade",
                    "deltaQty": "-5.4175"
                },
                {
                    "asset": "USDT",
                    "curBalance": "-1628.51466633",
                    "dataType": "fee",
                    "deltaQty": "-0.00270875"
                }
            ],
            "liquidityInd": "RemovedLiquidity",
            "orderId": "r17873f13ab2U7684538613bbtcpuHLA",
            "orderType": "Limit",
            "side": "Buy",
            "sn": 16823297563,
            "transactTime": 1616852892564
        }
    ],

    "balance": [
        {
            "data": [
                {
                    "asset": "BTMX-S",
                    "curBalance": "40000.112249285",
                    "deltaQty": "0.000000007"
                }
            ],
            "eventType": "Btmxmining",
            "sn": 10888843672,
            "transactTime": 1617213942176
        },
        {
            "data": [
                {
                    "asset": "BTMX-S",
                    "curBalance": "40000.112249159",
                    "deltaQty": "0.000000029"
                }
            ],
            "eventType": "Btmxmining",
            "sn": 10870559479,
            "transactTime": 1617149141108
        }
    ]
}
```

#### Request Parameters

Name           |  Type     | Required |           Value Range         | Description
-------------- | --------- | -------- | ------------------------------| -----------
**sn**         | `Long`    |   Yes    |  start from snapshot `sn`     | Start sn 
**limit**      | `Int`     |   No     |  1 to 500                     | Number of records. max 500 


#### Response Content

 Name       | Type         | Description
----------- | -------------| ---------------------------------
**meta**    | `Json`       | `meta` info. See detail below
**order**   | `Json Array` | `order` info. See detail below
**balance** | `Json Array` | `balance` info. See detail below

##### Meta 
`meta` field provides some basic info.

`meta` schema

 Name                | Type     | Description      | Sample Response
-------------------- | -------- | -----------------| -------------------------
**ac**               | `String` | account category | `"cash", "margin", "futures`
**accountId**        | `String` | accountId        | 

##### Order

`order` field provides an array of asset balance detail from order fill event.

`order` schema

 Name            | Type        | Description                    | Sample Response
-----------------| ----------- | -------------------------------| -------------------------
**liquidityInd** | `String`    | liquidity indicator            | `RemovedLiquidity` for taker order, `AddedLiquidity` for maker order, or `NULL_VAL`
**orderId**      | `String`    | orderId                        | order Id
**orderType**    | `String`    | order type                     | `market`, `limit`
**side**         | `String`    | order side                     | `buy`, `sell`
**sn**           | `Long`      | sequence number                | unique and increasing sequence number
**transactTime** | `Long`      | transactTime in milli seconds  |
**data**         | `Json Array`| list of order info json objects| see detail below

order balance detail by asset

`data` schema

 Name                | Type     | Description                                           | Sample Response
-------------------- | -------- | ----------------------------------------------------- | ----------------
**asset**            | `String` | asset code                                            | `"USDT"`
**curBalance**       | `String` | asset balance after this transaction                  | `"1234.56"`
**dataType**         | `String` | `trade` for trading asset; `fee` for fee balance asset| `trade`, `fee`
**deltaQty**         | `String` | balance change in this transaction                    | `100`

##### Balance

`balance` field provides an array of asset balance detail due to balance event.

`balance` schema

 Name            | Type        | Description                      | Sample Response
-----------------| ----------- | ---------------------------------| -----------------------
**eventType**    | `String`    | balance event type               | `deposit`, `withdrawal`
**sn**           | `Long`      | sequence number                  |
**transactTime** | `Long`      | transactTime in milli seconds    |
**data**         | `Json Array`| list of balance info json objects| see detail below

`data` schema

 Name                | Type     | Description                                           | Sample Response
-------------------- | -------- | ----------------------------------------------------- | ----------------
**asset**            | `String` | asset code                                            | `"USDT"`
**curBalance**       | `String` | asset balance after this transaction                  | `"1234.56"`
**deltaQty**         | `String` | balance change in this transaction                    | `100`


#### Code Sample

Please refer to python code to [query order and balance detail](https://github.com/ascendex/ascendex-pro-api-demo/blob/master/python/query_balance_and_order_fills.py)
