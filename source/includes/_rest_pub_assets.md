### List all Assets

> List all Assets

```
curl -X GET "https://ascendex.com/api/pro/v2/assets"
```

> Sample Response 

```json
{
    "code": 0,
    "data": [
        {
            "assetCode":      "USDT",
            "assetName":      "Tether",
            "precisionScale":  9,
            "nativeScale":     4,
            "blockChain": [
                {
                    "chainName":        "Omni",
                    "withdrawFee":      "30.0",
                    "allowDeposit":      true,
                    "allowWithdraw":     true,
                    "minDepositAmt":    "0.0",
                    "minWithdrawal":    "50.0",
                    "numConfirmations":  3
                },
                {
                    "chainName":        "ERC20",
                    "withdrawFee":      "10.0",
                    "allowDeposit":      true,
                    "allowWithdraw":     true,
                    "minDepositAmt":    "0.0",
                    "minWithdrawal":    "20.0",
                    "numConfirmations":  12
                }
            ]
        }
    ]
}
```

#### HTTP Request

`GET /api/pro/v2/assets`

You can obtain a list of all assets listed on the exchange through this API.

#### Response Content

 Name               | Type     | Description                                                                                 
------------------- | -------- | --------------------- 
 `assetCode`        | `String` | asset code. e.g. `"BTC"`
 `assetname`        | `String` | full name of the asset, e.g. `"Bitcoin"`
 `precisionScale`   | `Int`    | scale used in internal position keeping.
 `nativeScale`      | `Int`    | scale used in deposit/withdraw transaction from/to chain. 
 `blockChain`       | `List`   | block chain specific details


Blockchain specific details

 Name               | Type      | Description                                                                                 
------------------- | --------- | --------------------- 
 `chainName`        | `String`  | name of the blockchain
 `withdrawFee`      | `String`  | fee charged for each withdrawal request. e.g. "0.01"
 `allowDepoist`     | `Boolean` | allow deposit
 `allowWithdraw`    | `Boolean` | allow withdrawal
 `minDepositAmt`    | `String`  | minimum amount required for the deposit request e.g. "0.0"
 `minWithdrawalAmt` | `String`  | minimum amount required for the withdrawal request e.g. "50"
 `numConfirmations` | `Int`     | number of confirmations needed for the exchange to recoganize a deposit
