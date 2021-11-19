### Fee Schedule 

> Fee Schedule - Sample response for general info::

```json
{'code': 0,

 'data': {'domain': 'futures',

          'genericFee': {'largeCap': {'maker': 0.00085, 'taker': 0.00085},

                         'smallCap': {'maker': 0.001, 'taker': 0.001}},

          'userUID': 'U0866943712',

          'vipLevel': 0
        }
}
```
> Fee Schedule - Sample response for each symbol::

```json
{'code': 0,

 'data': {'domain': 'generic',

          'userUID': 'U0866943712',

          'vipLevel': 0,

          'productFee': [{'fee': {'maker': 0.0001, 'taker': 0.0001},

                          'symbol': 'ZEC/BTC'},

                         {'fee': {'maker': 0.0001, 'taker': 0.0001},

                          'symbol': 'ETC/USDT'},

                         {'fee': {'maker': 0.002, 'taker': 0.002},

                          'symbol': 'ONG/USDT'},

                         {'fee': {'maker': 0.002, 'taker': 0.002},

                          ‘symbol': 'IBVOL/USDT'} …]

        }
}
```

**HTTP Request(general)** 

`GET <account-group>/api/pro/v1/<generic/futures>/fee/info`

**Signature**

You should sign the message in header as specified in [**Authenticate a RESTful Request**](#sign-a-request) section.

**prehash string** 

`<timestamp>+fee/info`


**HTTP Request(each symbol)** 

`GET <account-group>/api/pro/v1/<generic/futures>/fee`

**Signature**

You should sign the message in header as specified in [**Authenticate a RESTful Request**](#sign-a-request) section.

**prehash string** 

`<timestamp>+fee`


