## Subscribe to Data Channels

### How to Subscribe

> Use `wscat` from Node.js to connect to websocket data.

```bash
# # Install wscat from Node.js if you haven't
# npm install -g wscat  
npm install -g wscat

# Connect to websocket
wscat -c wss://bakkt.exchange.test.com /0/api/pro/v1/stream -x '{"op":"sub", "ch": "depth:BTC/USDT"}'
```

> You can also setup authorized session

```bash
@ToDo
```

You can **subscribe/unsubscribe** to one or multiple data channels.

* If the subscription is successful, you will receive at least one ack message confirming the request is successful and you will start receiving data streams. 
* If the subscription is unsuccessful, you will receive one ack message with text explaining why the subscription failed. 

#### Request Body Schema 

The standard messages to subscribe to / unsubscribe from data channels is an JSON object with fields:

 Name  | Type               | Description                                                                                    
-------| ------------------ | ---------------------------------------------------------------------------------------------- 
 `op`  | `String`           | `sub` to subscribe and `unsub` to unsubscribe
 `id`  | `Optional[String]` | user specified UUID, if provided, the server will echo back this value in the response message 
 `ch`  | `String`           | name of the data channel with optional arguments, see below for details                        


> Subscribe to *bbo* stream for symbol `BTC/USDT`

```json
{ "op": "sub", "id": "abcd1234", "ch": "bbo:BTC/USDT" }
```

> Subscribe to *ref-px* stream for symbol `BTC`

```json
{ "op": "sub", "id": "abcd1234", "ch": "ref-px:BTC" }
```

> Subscribe to *trade* stream for a list of symbols

```json
{ "op": "sub", "id": "abcd1234", "ch": "trades:BTC/USDT,ETH/USDT,BTC/USDT" }
```

> Unsubscribes from the *depth* stream for all symbols (method 1)

```json
{ "op": "unsub", "id": "abcd1234", "ch": "depth:*" }
```

> Unsubscribes from the *depth* stream for all symbols (methond 2)

```json
{ "op": "unsub", "id": "abcd1234", "ch": "depth" }
```

> Unsubscribes from the 1 minute *bar* streams for all symbols (method 1)

```json
{ "op": "unsub", "id": "abcd1234", "ch": "bar:1:*" }
```

> Unsubscribes from the 1 minute *bar* streams for all symbols (method 2)

```json
{ "op": "unsub", "id": "abcd1234", "ch": "bar:1" }
```

> Unsubscribes from *bar* streams of all frequencies for `BTC/USDT`

```json
{ "op": "unsub", "id": "abcd1234", "ch": "bar:*:BTC/USDT" }
```

> Response for sub multiple symbols in one single message

```json
{"m":"sub","id":"abc23g","ch":"summary:BTC/USDT,BTC/USDT","code":0}
```

> Response for unsub multiple symbols in one single message

```json
{ "m": "unsub", "id": "abcd1234", "ch": "bar:*:BTC/USDT" }
```

#### Customize Channel content with `ch`

You can customize the channel content by setting `ch` according to the table below:

 Type    | Value                        | Description                                      
-------- | ---------------------------- | ------------------------------------------------ 
 public  | `depth:<symbol>`             | Updates to order book levels. 
 public  | `bbo:<symbol>`               | Price and size at best bid and ask levels on the order book.
 public  | `trades:<symbol>`            | Market trade data 
 public  | `bar:<interval>:<symbol>`    | Bar data containing O/C/H/L prices and volume for specific time intervals
 public  | `ref-px:<symbol>`            | Reference prices
 Private | `order:<account>`            | Order Update Stream: "cash", "margin", or actual accountId for `account.                              

 *Symbol* in *ref-px* is single asset symbol(e.g. `BTC`), not trading pair symbol (e.g. `BTC/USDT`), which is different from other channels.

 #### Unsubscribe with Wildcard Character `*`

Using the wildcard character `*`, you can unsubscribe from multiple channels with the same channel name.
 
#### Subscribe to single or multiple symbols

Subscribe to a single symbol (e.g. `BTC/USDT`), or multiple symbols (up to 10) separated by ",", e.g. `"BTC/USDT,ETH/USDT"`.

#### Sub/Unsub response with multiple symbols

When sub or unsub from multiple symbols, we may ack symbol by symbol, or ack in one single message.

You can subscribe/unsubscribe one channel per subscription message. You can subscribe to multiple data channels by sending multiple 
subscription messages. However, the exchange limits the total number of data channels per client (**NOT per session**) according to 
the following rules:

@ToDo rule: maximum number of channels 


