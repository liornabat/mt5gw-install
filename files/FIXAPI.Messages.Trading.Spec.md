# FIX Message Specification

App version **1.1.0+Branch.master.Sha.51ede29a0248690b3f628d449137b77eaabf2441**

The document defines message specifications for MetaTrader 5 FIX API by Brokeree Solutions.

## Supported messages

 * ExecutionReport (8)
 * OrderMassStatusRequest (AF)
 * PositionMaintenanceRequest (AL)
 * PositionMaintenanceReport (AM)
 * RequestForPositions (AN)
 * RequestForPositionsAck (AO)
 * PositionReport (AP)
 * CollateralReport (BA)
 * CollateralInquiry (BB)
 * CollateralInquiryAck (BG)
 * NewOrderSingle (D)
 * OrderCancelRequest (F)
 * OrderCancelReplaceRequest (G)


## Message specifications



### ExecutionReport (8)

Server → Client

The execution report message is used to:

* Confirm the receipt of an order (39=0)
* Confirm orders (39=2)
* Reject orders (39=8)
* Confirm order cancellation (39=4)
* Provide status for all open trades (39=I)

Tag | Field | Req'd | Data Type | Notes
--- | ---- | --- | --------- | -----------
**35** | **MsgType** ||| **8**
 11 | ClOrdID | Y | String | 
 37 | OrderID | N | UInt64 | 
 1 | Account | N | UInt64 | 
 40 | OrdType | N | OrderType | Valid values: <br/>1 = Market<br/>2 = Limit<br/>3 = Stop<br/>4 = StopLimit<br/>Z = CloseBy
 54 | Side | N | Side | Valid values: <br/>1 = Buy<br/>2 = Sell<br/>7 = Unknown
 38 | OrderQty | N | Double | 
 55 | Symbol | N | String | 
 44 | Price | N | Double | Market, limit or stop-limit order price
 99 | StopPx | N | Double | Stop order price or stop-limit trigger price
 59 | TimeInForce | N | TimeInForce | Valid values: <br/>0 = GoodTillDay<br/>1 = GoodTillCancel<br/>3 = ImmediateOrCancel<br/>4 = FillOrKill<br/>6 = GoodTillDate<br/>R = Return
 20101 | StopLossPx | N | Decimal | Order SL price
 20102 | TakeProfitPx | N | Decimal | Order TP price
 60 | TransactTime | N | DateTime | Deal/order time
 17 | ExecID | Y | UInt64 | Deal ID
 31 | LastPx | Y | Double | Deal price
 32 | LastQty | N | Double | Deal volume
 12 | Commission | N | Double | Deal commission
 77 | PositionEffect | N | PositionEffect | Whether this deal opens or closes a position<br/>Valid values: <br/>B = CloseBy<br/>C = Close<br/>D = Default<br/>O = Open<br/>X = CloseOpen
 2618 | PositionID | N | UInt64 | 
 20207 | Profit | N | Double | 
 6 | AvgPx | N | Double | 
 14 | CumQty | N | Double | 
 151 | LeavesQty | N | Double | 
 150 | ExecType | N | ExecutionType | 
 39 | OrdStatus | N | OrderState | Valid values: <br/>0 = New<br/>1 = PartiallyFilled<br/>2 = Filled<br/>4 = Cancelled<br/>6 = PendingCancel<br/>8 = Rejected<br/>A = PendingNew<br/>B = Undefined<br/>C = Expired<br/>E = PendingReplace
 58 | Text | N | String | 
 584 | MassStatusReqID | N | String | 




### OrderMassStatusRequest (AF)

Client → Server

Used to request the list of open pending orders

Tag | Field | Req'd | Data Type | Notes
--- | ---- | --- | --------- | -----------
**35** | **MsgType** ||| **AF**
 584 | MassStatusReqID | Y | String | 
 585 | MassStatusReqType | Y | OrderListScope | Valid values: <br/>1 = Symbol<br/>7 = All<br/>8 = Account
 1 | Account | C | UInt64 | 
 55 | Symbol | C | String | 




### PositionMaintenanceRequest (AL)

Client → Server

Used to update SL or TP values of positions

Tag | Field | Req'd | Data Type | Notes
--- | ---- | --- | --------- | -----------
**35** | **MsgType** ||| **AL**
 710 | PosReqID | Y | String | Request ID
 2618 | PositionID | Y | UInt64 | Position ID as defined in MT5
 1 | Account | Y | UInt64 | Account holding the position
 55 | Symbol | Y | String | Position symbol
 20101 | StopLossPx | C | Decimal | Either TP or SL value is required
 20102 | TakeProfitPx | C | Decimal | Either TP or SL value is required
 709 | PosTransType | Y |  | Ignored but required by FIX4.4
 712 | PosMaintAction | Y |  | Ignored but required by FIX4.4
 715 | ClearingBusinessDate | Y |  | Ignored but required by FIX4.4
 581 | AccountType | Y |  | Ignored but required by FIX4.4
 60 | TransactTime | Y |  | Ignored but required by FIX4.4




### PositionMaintenanceReport (AM)

Server → Client

Server sends this message for accepted or rejected PositionMaintenanceRequest messages

Tag | Field | Req'd | Data Type | Notes
--- | ---- | --- | --------- | -----------
**35** | **MsgType** ||| **AM**
 709 | PosTransType | Y | Int32 | Always 3 (`POSITION_ADJUSTMENT`)
 709 | PosTransType | Y | Int32 | Always 2 (`REPLACE`)
 715 | ClearingBusinessDate | Y | DateTime | Always the message date
 60 | TransactTime | Y | DateTime | Always the message datetime
 581 | AccountType | Y | Int32 | Always 1 (`ACCOUNT_IS_CARRIED_ON_CUSTOMER_SIDE_OF_BOOKS`)
 721 | PosMaintRptID | Y | String | 
 710 | PosReqID | Y | String | 
 729 | PosReqStatus | Y | PosRequestStatus | 
 1 | Account | Y | UInt64 | 
 55 | Symbol | Y | String | 
 58 | Text | Y | String | 
 712 | PosMaintAction | Y |  | Ignored but required by FIX4.4
 713 | OrigPosReqRefID | Y |  | Ignored but required by FIX4.4
 722 | PosMaintStatus | Y |  | Ignored but required by FIX4.4




### RequestForPositions (AN)

Client → Server

Used to request list of open positions per account

Tag | Field | Req'd | Data Type | Notes
--- | ---- | --- | --------- | -----------
**35** | **MsgType** ||| **AN**
 1 | Account | Y | UInt64 | 
 710 | PosReqID | Y | String | 
 724 | PosReqType | Y |  | Ignored but required by FIX4.4
 581 | AccountType | Y |  | Ignored but required by FIX4.4
 715 | ClearingBusinessDate | Y |  | Ignored but required by FIX4.4
 60 | TransactTime | Y |  | Ignored but required by FIX4.4




### RequestForPositionsAck (AO)

Server → Client

Server sends this message in response to RequestForPositions messages

Tag | Field | Req'd | Data Type | Notes
--- | ---- | --- | --------- | -----------
**35** | **MsgType** ||| **AO**
 721 | PosMaintRptID | Y | String | 
 710 | PosReqID | Y | String | 
 1 | Account | Y | UInt64 | 
 728 | PosReqResult | N | PosListRequestResult | Valid values: <br/>0 = Valid<br/>1 = InvalidRequest<br/>2 = NoPositions
 729 | PosReqStatus | N | PosListRequestStatus | Valid values: <br/>0 = Completed<br/>1 = CompletedWarnings<br/>2 = Rejected
 727 | TotalNumPosReports | N | Int32 | 
 58 | Text | N | String | Will contain reason in case of error
 728 | PosReqResult | N | Int32 | Always 0 (COMPLETED)
 581 | AccountType | N | Int32 | Always 1 (`ACCOUNT_IS_CARRIED_ON_CUSTOMER_SIDE_OF_BOOKS`)




### PositionReport (AP)

Server → Client

Will be sent in response to RequestForPositions and after any events that add, modify or delete positions

Tag | Field | Req'd | Data Type | Notes
--- | ---- | --- | --------- | -----------
**35** | **MsgType** ||| **AP**
 710 | PosReqID | C | String | Present if report is a response to RequestForPositions
 727 | TotalNumPosReports | C | Int32 | 
 721 | PosMaintRptID | Y | String | 
 715 | ClearingBusinessDate | Y | DateTime | 
 1 | Account | Y | UInt64 | 
 55 | Symbol | Y | String | 
 702 | NoPositions | Y | Int32 | Always 1
 705 | ShortQty | C | Decimal | ShortQty is present if position side is SELL<br/>LongQty is present if position side is BUY
 704 | LongQty | C | Decimal | ShortQty is present if position side is SELL<br/>LongQty is present if position side is BUY
 20223 | OpenTime | Y | DateTime | 
 779 | LastUpdateTime | Y | DateTime | 
 730 | SettlPrice | Y | Decimal | Position open price
 20224 | PriceCurrent | N | Decimal | Current position close price
 20101 | StopLossPx | N | Decimal | 
 20102 | TakeProfitPx | N | Decimal | 
 20207 | Profit | N | Decimal | Current position profit
 20208 | Storage | N | Decimal | Swap value
 2618 | PositionID | Y | UInt64 | 
 728 | PosReqResult | Y |  | Ignored but required by FIX4.4
 581 | AccountType | Y |  | Ignored but required by FIX4.4
 731 | SettlPriceType | Y |  | Ignored but required by FIX4.4
 734 | PriorSettlPrice | Y |  | Ignored but required by FIX4.4




### CollateralReport (BA)

Server → Client

Will be sent in response to CollateralInquiry and after any events that modify the accounts balance, margin, etc

Please refer to [MetaQuotes documentation](https://support.metaquotes.net/en/docs/mt5/api/reference_trading/user_account/imtaccount)

Tag | Field | Req'd | Data Type | Notes
--- | ---- | --- | --------- | -----------
**35** | **MsgType** ||| **BA**
 910 | CollStatus | Y | Int32 | Always 3 (Assigned)
 908 | CollRptID | N | String | 
 909 | CollInquiryID | N | String | Present if message is a response to CollateralInquiry
 1 | Account | Y | UInt64 | 
 53 | Quantity | Y | Decimal | 
 15 | Currency | Y | String | 
 20202 | Credit | N | Decimal | 
 20203 | Margin | N | Decimal | 
 20204 | MarginFree | N | Decimal | 
 20205 | MarginLevel | N | Decimal | 
 20206 | MarginLeverage | N | UInt32 | 
 20207 | Profit | N | Decimal | 
 20208 | Storage | N | Decimal | 
 12 | Commission | N | Decimal | 
 20210 | Floating | N | Decimal | 
 20211 | Equity | N | Decimal | 
 20212 | SOActivation | N | EnSoActivation | 
 20213 | SOTime | N | DateTime | 
 20214 | SOLevel | N | Decimal | 
 20215 | SOEquity | N | Decimal | 
 20216 | SOMargin | N | Decimal | 
 20217 | BlockedCommission | N | Decimal | 
 20218 | BlockedProfit | N | Decimal | 
 20219 | MarginInitial | N | Decimal | 
 20220 | MarginMaintenance | N | Decimal | 
 20221 | Assets | N | Decimal | 
 20222 | Liabilities | N | Decimal | 
 58 | Text | N | String | 




### CollateralInquiry (BB)

Client → Server

Account status request, will return CollInquiryStatus with CollInquiryStatus = ACCEPTED or REJECTED as a response.
The account status will be sent as CollateralReport

Tag | Field | Req'd | Data Type | Notes
--- | ---- | --- | --------- | -----------
**35** | **MsgType** ||| **BB**
 909 | CollInquiryID | Y | String | Inquiry id, will be contained in ack and report
 1 | Account | Y | UInt64 | Account ID to receive the report for




### CollateralInquiryAck (BG)

Server → Client

Sent in response to CollateralInquiry

Tag | Field | Req'd | Data Type | Notes
--- | ---- | --- | --------- | -----------
**35** | **MsgType** ||| **BG**
 945 | CollInquiryStatus | Y | AckStatus | Status of request<br/>Valid values: <br/>0 = Accepted<br/>4 = Rejected
 909 | CollInquiryID | Y | String | Original CollInquiryID
 58 | Text | N | String | If rejected, will contain the reason




### NewOrderSingle (D)

Client → Server

Client → Server
Used to

* Open new positions (77=O)
* Close existing positions (77=C)
* Place new pending orders (40=2/3)


Tag | Field | Req'd | Data Type | Notes
--- | ---- | --- | --------- | -----------
**35** | **MsgType** ||| **D**
 11 | ClOrdID | Y | String | Unique Client ID for the order
 1 | Account | Y | UInt64 | MT5 account login
 54 | Side | Y | Side | Buy/Sell<br/>Valid values: <br/>1 = Buy<br/>2 = Sell<br/>7 = Unknown
 40 | OrdType | Y | OrderType | Required for open/place request.<br/>Ignored for close requests.<br/>Valid values: <br/>1 = Market<br/>2 = Limit<br/>3 = Stop<br/>4 = StopLimit<br/>Z = CloseBy
 38 | OrderQty | Y | Decimal | 
 55 | Symbol | Y | String | 
 59 | TimeInForce | C | TimeInForce | Valid values: <br/>0 = GoodTillDay<br/>1 = GoodTillCancel<br/>3 = ImmediateOrCancel<br/>4 = FillOrKill<br/>6 = GoodTillDate<br/>R = Return
 44 | Price | C | Decimal | 
 99 | StopPx | C | Decimal | 
 20101 | StopLossPx | N | Decimal | 
 20102 | TakeProfitPx | N | Decimal | 
 77 | PositionEffect | N | PositionEffect | Valid values: <br/>B = CloseBy<br/>C = Close<br/>D = Default<br/>O = Open<br/>X = CloseOpen
 2618 | PositionID | N | UInt64 | 
 126 | ExpireTime | N | DateTime | 
 60 | TransactTime | Y |  | Ignored but required by FIX4.4




### OrderCancelRequest (F)

Client → Server

Used to request order cancellations.

Tag | Field | Req'd | Data Type | Notes
--- | ---- | --- | --------- | -----------
**35** | **MsgType** ||| **F**
 41 | OrigClOrdID | Y | String | Either OrigClOrdId or OrderId must be correct to cancel an order.<br/><br/>Since this field is required by FIX 4.4 specifications, it must always be present in the message.
 11 | ClOrdID | Y | String | The ID of the cancellation request.<br/><br/>In case of cancellation rejection, Order Cancel Reject will contain the value of this field.<br/><br/>In case of success, Execution Report will contain the value of this field.
 37 | OrderID | N | UInt64 | Either OrigClOrdId or OrderId must be correct to cancel an order.
 54 | Side | Y |  | Ignored but required by FIX4.4
 60 | TransactTime | Y |  | Ignored but required by FIX4.4




### OrderCancelReplaceRequest (G)

Client → Server

Used to modify open pending orders: prices, sl, tp

Tag | Field | Req'd | Data Type | Notes
--- | ---- | --- | --------- | -----------
**35** | **MsgType** ||| **G**
 41 | OrigClOrdID | Y | String | Either OrigClOrdId or OrderId must be correct to cancel an order.<br/><br/>Since this field is required by FIX 4.4 specifications, it must always be present in the message.
 11 | ClOrdID | Y | String | The ID of the modification request. In case of modification rejection, Order Cancel Reject will contain the value of this field.<br/><br/>Please note that the ClOrdId of the original order will change to this field's value.
 1 | Account | Y | UInt64 | 
 37 | OrderID | N | UInt64 | Either OrigClOrdId or OrderId must be correct to cancel an order.
 55 | Symbol | Y | String | 
 44 | Price | N | Decimal | 
 99 | StopPx | N | Decimal | 
 20101 | StopLossPx | N | Decimal | 
 20102 | TakeProfitPx | N | Decimal | 
 59 | TimeInForce | N | TimeInForce | Valid values: <br/>0 = GoodTillDay<br/>1 = GoodTillCancel<br/>3 = ImmediateOrCancel<br/>4 = FillOrKill<br/>6 = GoodTillDate<br/>R = Return
 126 | ExpireTime | N | DateTime | 
 54 | Side | Y |  | Ignored but required by FIX4.4
 60 | TransactTime | Y |  | Ignored but required by FIX4.4
 40 | OrdType | Y |  | Ignored but required by FIX4.4


