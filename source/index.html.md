---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python
  - java

toc_footers:
  - <a href='https://ascendex.com'>ascendex.com</a>

includes:
  - rest_general
  - rest_auth
  - rest_pub
  - rest_pub_assets
  - rest_pub_products
  - rest_pub_ticker
  - rest_pub_bar
  - rest_pub_depth
  - rest_pub_trades
  - rest_act
  - rest_act_info
  - rest_act_fee
  - rest_risk_limit_info
  - rest_exchange_info
  - rest_prv_bal
  - rest_prv_bal_detail
  - rest_prv_wal
  - rest_prv_wal_deposit
  - rest_prv_wal_withdraw
  - rest_prv_wal_txes
  - rest_prv_order
  - rest_prv_order_new
  - rest_prv_order_cancel
  - rest_prv_order_cancel_all
  - rest_prv_order_new_batch
  - rest_prv_order_cancel_batch
  - rest_prv_order_query
  - rest_prv_order_open
  - rest_prv_order_hist_curr
  - rest_prv_order_hist_deprecated
  - rest_prv_order_hist_v2
  - ws_general
  - ws_keep_alive
  - ws_auth
  - ws_sub
  - ws_sub_level1
  - ws_sub_level2
  - ws_sub_trades
  - ws_sub_bar
  - ws_sub_order
  - ws_req
  - ws_req_level2
  - ws_req_order_place
  - ws_req_order_cancel
  - ws_req_order_cancel_all
  - ws_req_order_open
  - ws_req_trades
  - ws_req_margin_risk
  - expr
  - ap_enum
  - ap_enum_blockchain
  - ap_enum_error_code

header_navigators:
  - <a href="https://ascendex.github.io/ascendex-pro-api/" class="current">Cash/Margin APIs</a>
  - <a href="https://ascendex.github.io/ascendex-futures-pro-api-v2/">Futures APIs</a>

search: true
---


# AscendEx Pro API Documentation

AscendEx Pro API is the latest release of APIs allowing our users to access the exchange programmatically. It is a major revision 
of the older releases. The AscendEx team re-implemented the entire backend system in support for the AscendEx Pro API. It is designed
to be fast, flexible, stable, and comprehensive. 

## What's New

* Dynamic subscription/unsubscription to private and public data channels via WebSocket. 
* Synchronized/Asynchronized API calls. When placing/cancelling orders, you may use synchronized method 
  to get the order result in a single API call. You may also use asynchronized method to achieve minimum latency. 
* Simplified API schemas. For instance, we simplified the cancel order logic, now you can track the entire order
  life cycle with only one indentifier (`orderId`). 
* More detailed error message.


## Market Making Incentive Program

AscendEX offers a Market Making Incentive Program for professional liquidity providers. Key benefits of this program include:

* Favorable fee structure.
* Monthly bonus pending satisfying KPI.
* Direct Market Access and Co-location service.

Users with good maker strategies and significant trading volume are welcome to participate in this long-term program. If your account has a trading volume of more than 150,000,000 USDT in the last 30 days on any exchange, please send the following information via email to institution@ascendex.com, with the subject "Market Maker Incentive Application":

* One AscendEX account ID.
* A brief explanation of your market making method (NO detail is needed), as well as estimation of maker orders' percentage.



## SDKs and Client Libraries

### Official SDK

**CCXT** is our authorized SDK provider and you may access the AscendEX API through CCXT. For more information, please visit: [https://ccxt.trade](https://ccxt.trade)

### Demo Code

We provide comprehensive demos (currently available in python). We provide two types of demo codes:

* Short, self contained demo scripts that you can plugin to you larger system. 
* Large, complex demo scripts to show you how to design a trading strategy using APIs from this document.

See [https://github.com/ascendex/ascendex-pro-api-demo](https://github.com/ascendex/ascendex-pro-api-demo) for more details.


## Got Questions?

Join our official telegram channel: [https://t.me/AscendEX_Official_API](https://t.me/AscendEX_Official_API)


## Release Note

**2022-02-28**

* Added the [Limit Info API](#risk-limit-info) to get ban info and risk limit info.

**2022-02-25**

* Update the [List all Products API](#list-all-products) to different endpoints by account type: for cash `/api/pro/v1/cash/products`, for margin `/api/pro/v1/margin/products`.

**2021-12-02**

* Update the [Balance Snapshot API](#balance-snapshot) and [Order and Balance Detail API](#order-and-balance-detail) to different endpoints by account type: `api/pro/data/v1/cash/balance/snapshot` for cash and `api/pro/data/v1/margin/balance/snapshot` for margin balance;`api/pro/data/v1/cash/balance/history` for cash and `api/pro/data/v1/margin/balance/history` for margin balance or order fill detail.
* Redirect the [Order Hist v2 API](#list-history-orders-v2) to new endpoint `api/pro/data/v2/order/hist` (No group in the endpoint anymore), and change prehash string to `data/v2/order/hist`.  Original way through `<group>/api/pro/v2/order/hist` is still supported, but will be removed in the future.

**2021-11-22**

* Replaced the ticker API `GET api/pro/v1/ticker` with `GET api/pro/v1/spot/ticker` to include only spot symbols. The old API will still be available but will be removed in the future.
* Added the [VIP fee schedule API](#vip-fee-schedule) and the [Fee Schedule by Symbol API](#fee-schedule-by-symbol).

**2021-07-16**

* Introduced the experimental [Balance Snapshot](#balance-snapshot) and [Order and Balance Detail](#order-and-balance-detail) api.

**2021-06-15**

* Introduced the [List all Assets (version v2)](#list-all-assets) api. The version v1 api will remain available. However, you are highly recommended to upgrade. 

**2021-06-08**

* Added [balance transfer between sub account](#balance-transfer-for-subaccount). 

**2020-08-10**

* We have deprecated [list history orders API](#list-history-orders-deprecated) and replace it with [list history orders v2 API](#list-history-orders-v2). 

**2020-08-06**

* Added `expireTime` and `allowedIps` to [account info](#account-info)

**2020-07-17**

* Added API to [query deposit addresses](#query-deposit-addresses).
* Added API to [query wallet transaction history](#query-wallet-transaction-history)

**2019-12-26**

* Added execution instruction to order messages (place new order, list open/historical orders). This field indicates if the order is Post-Only (`Post`) or forced liquidation (`Liquidation`). It is named `execInst` 
  in RESTful responses and `ei` in websocket messages. 
