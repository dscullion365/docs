## Balance Snapshot And Update (**Experimential API**)

Here we provide experimental API to get daily balance snapshot and intraday balance and order fills update details. We recommend calling balance snapshot to get balance at the beginning of the day, and get the sequence number sn; then start to query balance or order fills update from sn + 1.

Please note we enforcerate limit 8 / minute.


### Balance Snapshot

This API returns balance snapshot on daily basis.

#### HTTP Request

`GET  api/pro/v1/data/balance/snapshot`

#### Signature

You should sign the message in header as specified in [**Authenticate a RESTful Request**](#signing-a-Request) section.

#### Prehash String

`<timestamp>+balance/snapshot`

> Cash Account Balance Snapshot - Sample response 

```json
{
    "meta": {
        "ac": "cash",
        "accountId": "MUXFNEYUJJJ93CRYXT4LTCIDIJPCFNIX",
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
**account** | `String`  |   Yes    | `cash`, `margin`, `futures` |  account type
**date**    | `String`  |   Yes    | `YYYY-mm-dd`                |  balance date


#### Response Content

Response for cash/margin and futures are different (same `meta` field).

##### Cash/Margin
 Name       | Type         | Description
----------- | -------------| ---------------------------------
**meta**    | `Json`       | `meta` info. See detail below
**snapshot**| `Json Array` | `snapshot` info. See detail below

`meta` field provides some basic info.

`meta` schema

 Name            | Type     | Description                    | Sample Response
-----------------| -------- | -------------------------------| -------------------------
**ac**           | `String` | account category               | `"cash", "margin", "futures`
**accountId**    | `String` | accountId                      | 
**sn**           | `Long`   | sequence number                |
**snapshotTime** | `Long`   | snapshotTime in milli seconds  |

`snapshot` field provides asset balance snapshot info.

`snapshot` schema

 Name              | Type     | Description                   | Sample Response
-------------------| -------- | ------------------------------| ----------------
**asset**          | `String` | asset code                    | `"USDT"`
**totalBalance**   | `String` | current asset total balance   | `"1234.56"`


##### Futures

 Name                | Type         | Description
-------------------- | -------------| ---------------------------------
**meta**             | `Json`       | `meta` info. See detail below
**collateralBalance**| `Json Array` | `collateral balance` info. See detail below
**contractBalance**  | `Json Array` | `contract balance` info. See detail below

`meta` field provides some basic info.

`meta` schema

 Name            | Type     | Description                    | Sample Response
-----------------| -------- | -------------------------------| -------------------------
**ac**           | `String` | account category               | `"cash", "margin", "futures`
**accountId**    | `String` | accountId                      | 
**sn**           | `Long`   | sequence number                |
**snapshotTime** | `Long`   | snapshotTime in milli seconds  |

`collateralBalance` field provides array of ‘asset’ and ‘totalBalance’ for collateral balance.

`collateralBalance` schema

 Name              | Type     | Description                   | Sample Response
-------------------| -------- | ------------------------------| ----------------
**asset**          | `String` | asset code                    | `"USDT"`
**totalBalance**   | `String` | current asset total balance   | `"1234.56"`

`collateralBalance` field provides array of ‘asset’ and ‘totalBalance’ for collateral balance.

`collateralBalance` schema

 Name                  | Type     | Description               |Sample Response
-----------------------| -------- | --------------------------| ----------------
**contract**           | `String` | contract name             | `"USDT"`
**futuresAssetBalance**| `String` | current contract position | `"1234.56"`
**isolatedMargin**     | `String` | Isolated margin           | `"134.56"`
**refCostBalance**     | `String` | Reference cost            | `"34.56"`

#### Code Sample

Please refer to python code to [query balance snapshot](https://github.com/ascendex/ascendex-pro-api-demo/blob/master/python/query_balance_and_order_fills.py)


### Order and Balance Detail

This API is for intraday balance change detail from balance event and order fillss.

#### HTTP Request

`GET  api/pro/v1/data/balance/history`

> Cash Account Balance - Sample response 

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

#### Signature

You should sign the message in header as specified in [**Authenticate a RESTful Request**](#sign-a-request) section.

#### Prehash String

`<timestamp>+balance/history`

> Futures Account Balance - Sample response 

```json
{
    "collateralBalance": [
        {
            "asset": "USDT",
            "totalBalance": "554.896891579"
        }
    ],
    "contractBalance": [
        {
            "contract": "BTC-PERP",
            "futuresAssetBalance": "0",
            "isolatedMargin": "0",
            "refCostBalance": "0"
        },
        {
            "contract": "ETH-PERP",
            "futuresAssetBalance": "0",
            "isolatedMargin": "0",
            "refCostBalance": "0"
        }
    ],
    "meta": {
        "ac": "futures",
        "accountId": "futTv9jxEyfQNLT1FSZdohpF8W9RKGjr",
        "sn": 10870105241,
        "snapshotTime": 1617148800000
    }
}
```

#### Request Parameters

Name           |  Type     | Required |           Value Range         | Description
-------------- | --------- | -------- | ------------------------------| -----------
**account**    | `String`  |   Yes    |  `cash`, `margin`, `futures`  | 
**startOffset**| `Long`    |   Yes    |  true / false                 | Start offset 
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
**orderId**      | `String`    | orderId                        | `market`, `limit`
**orderType**    | `String`    | order type                     | `buy`, `sell`
**sn**           | `Long`      | sequence number                |
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
