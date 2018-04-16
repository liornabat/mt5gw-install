# FIX Message Specification

App version **1.1.0+Branch.master.Sha.51ede29a0248690b3f628d449137b77eaabf2441**

The document defines message specifications for MetaTrader 5 FIX API by Brokeree Solutions.

## Supported messages

 * MarketDataRequest (V)
 * MarketDataSnapshotFullRefresh (W)
 * SecurityListRequest (x)
 * SecurityList (y)
 * MarketDataRequestReject (Y)


## Message specifications



### MarketDataRequest (V)

Client → Server



Tag | Field | Req'd | Data Type | Notes
--- | ---- | --- | --------- | -----------
**35** | **MsgType** ||| **V**
 262 | MDReqID | Y | String | Request ID
 263 | SubscriptionRequestType | Y | Int32 | Always 1 = `SNAPSHOT_PLUS_UPDATES`
 55 | Symbol | N | String | Symbols to subscribe to
 267 | NoMDEntryTypes | Y | Int32 | Ignored but required by FIX4.4 spec
 264 | MarketDepth | Y |  | Ignored but required by FIX4.4




### MarketDataSnapshotFullRefresh (W)

Server → Client

Sent for symbols the client subscribed to with MarketDataRequest (V)

Tag | Field | Req'd | Data Type | Notes
--- | ---- | --- | --------- | -----------
**35** | **MsgType** ||| **W**
 262 | MDReqID | N | String | 
 55 | Symbol | Y | String | 
 269 | MDEntryType | Y | MarketDataEntryType | Valid values: <br/>0 = Bid<br/>1 = Offer<br/>2 = Last
 270 | MDEntryPx | Y | Decimal | 
 273 | MDEntryTime | N | DateTime | 
 268 | NoMDEntries | Y | Int32 | Number of entries
 => 269 | MDEntryType | Y | MarketDataEntryType | Valid values: <br/>0 = Bid<br/>1 = Offer<br/>2 = Last
 => 270 | MDEntryPx | Y | Decimal | 
 => 273 | MDEntryTime | N | DateTime | 




### SecurityListRequest (x)

Client → Server

Used to request security list.

Tag | Field | Req'd | Data Type | Notes
--- | ---- | --- | --------- | -----------
**35** | **MsgType** ||| **x**
 321 | SecurityRequestType | Y | Int32 | Always 4 (`ALL_SECURITIES`)
 320 | SecurityReqID | Y | String | 
 559 | SecurityListRequestType | Y |  | Ignored but required by FIX4.4




### SecurityList (y)

Server → Client

Sent in response to Security List Request (x)

Tag | Field | Req'd | Data Type | Notes
--- | ---- | --- | --------- | -----------
**35** | **MsgType** ||| **y**
 320 | SecurityReqID | Y | String | 
 322 | SecurityResponseID | Y | String | 
 560 | SecurityRequestResult | Y | Result | Valid values: <br/>0 = ValidRequest<br/>1 = InvalidOrUnsupportedRequest<br/>4 = InstrumentDataTemporarilyUnavailable
 146 | NoRelatedSym | N | Int32 | 
 55 | Symbol | Y | String | 
 146 | NoRelatedSym | N | Int32 | 
 => 55 | Symbol | Y | String | 




### MarketDataRequestReject (Y)

Server → Client

The Market Data Request Reject (Y) is used when the broker cannot honor the Market Data Request (V) , due to business or technical reasons.

Tag | Field | Req'd | Data Type | Notes
--- | ---- | --- | --------- | -----------
**35** | **MsgType** ||| **Y**
 262 | MDReqID | Y | String | Original request ID
 58 | Text | N | String | Will contain the reason for the rejection


