
## Contract Trading ERROR CODE




| CODE                                    | NAME       | DESCRIPTION                                                                      | 
|-----------------------------------------|------------|----------------------------------------------------------------------------------|
| NO_SUCH_ACCOUNT                         | 1000       | no such account.                                                                 | 
| DUPLICATE_ACCOUNT                       | 1001       | user account duplicate.                                                          | 
| ACCOUNT_SETTLED_CURRENCY_NOT_MATCH      | 1002       | account settled currency do not match.                                           | 
| ACCOUNT_IN_LIQUIDATION                  | 1003       | account in liquidation, not permitted.                                           | 
| MIN_LEVERAGE_RATIO                      | 1004       | leverage is too small                                                            | 
| MAX_LEVERAGE_RATIO                      | 1005       | leverage is too large.                                                           | 
| NEW_LEVERAGE_EQUAL_OLD                  | 1006       | leverage is equal old leverage.                                                  | 
| MARGIN_NOT_ENOUGH                       | 1007       | margin not enough.                                                               | 
| BALANCE_INSUFFICIENT                    | 1008       | balance insufficient.                                                            | 
| RECHARGE_AMOUNT_ILLEGAL                 | 1009       | the amount of recharge is illegal.                                               | 
| BALANCE_CHANGE_AMOUNT_ILLEGAL           | 1010       | the amount of balance change is illegal.                                         | 
| INVALID_MARGIN_CHANGE_AMOUNT            | 1011       | invalid margin change amount.                                                    | 
| ISOLATED_LEVERAGE_REJECT_WITH_POSITION  | 1012       | leverage reduction is not supported in Isolated Margin Mode with open positions. | 
| BALANCE_VERSION_NOT_EQUAL               | 1013       | balance version not equal.                                                       | 
| ACCOUNT_CAN_NOT_WITHDRAW                | 1014       | account can not withdraw.                                                        | 
| ACCOUNT_CAN_NOT_TRADE                   | 1015       | account can not trade.                                                           | 
| ACCOUNT_CAN_ONLY_REDUCE                 | 1016       | account can only place reduce only order.                                        | 
| INVALID_FEE_RATE                        | 1017       | invalid fee rate.                                                                | 
| INVALID_MAKER_FEE_RATE                  | 1018       | invalid maker fee rate.                                                          | 
| INVALID_TAKER_FEE_RATE                  | 1019       | invalid taker fee rate.                                                          | 
| UNABLE_UPDATE_POSITION_MODE             | 1020       | unable to update account position mode with open order or positions              | 
| INVALID_SYMBOL_ID                       | 2000       | invalid symbol id.                                                               | 
| DUPLICATE_SYMBOL_ID                     | 2001       | duplicate symbol id.                                                             | 
| DUPLICATE_SYMBOL_NAME                   | 2002       | duplicate symbol name.                                                           | 
| INVALID_NOTIONAL_LIMIT_COEF             | 2003       | invalid notional limit coef                                                      | 
| DUPLICATED_ASSET_NAME                   | 2004       | duplicated asset name                                                            | 
| DUPLICATED_ASSET_ID                     | 2005       | duplicated asset id                                                              | 
| INVALID_ASSET_ID                        | 2006       | invalid asset id.                                                                | 
| ASSET_NOT_FOUND                         | 2007       | asset is not found                                                               | 
| NO_SUCH_ORDER                           | 4000       | no such order.                                                                   | 
| PRICE_MISSING                           | 4001       | price is missing.                                                                | 
| QTY_MISSING                             | 4002       | qty is missing.                                                                  | 
| INVALID_QTY                             | 4003       | qty is invalid.                                                                  | 
| INVALID_CURRENCY_ID                     | 4004       | invalid currency id.                                                             | 
| UNABLE_TO_FILL                          | 4005       | unable to fill.                                                                  | 
| ORDER_WOULD_IMMEDIATELY_TRIGGER         | 4006       | order would immediately trigger.                                                 | 
| REDUCE_ONLY_REJECT                      | 4007       | reduce only reject.                                                              | 
| POSITION_NOT_SUFFICIENT                 | 4008       | position is not sufficient.                                                      | 
| REDUCE_ONLY_BUY_ERROR                   | 4009       | reduce only rejected, position more than 0 while buy.                            | 
| REDUCE_ONLY_SELL_ERROR                  | 4010       | reduce only rejected, position less than 0 while sell.                           | 
| MAX_OPEN_ORDER_LIMIT                    | 4011       | max open order limit.                                                            | 
| MAX_CONDITIONAL_ORDER_LIMIT             | 4012       | max conditional order limit.                                                     | 
| INVALID_CONDITIONAL_ORDER               | 4013       | conditional order is invalid.                                                    | 
| CLI_ORD_ID_DUPLICATE                    | 4014       | clientOrderId is existed.                                                        | 
| REDUCE_ONLY_ILLEGAL                     | 4015       | reduce only reject, order type not supported.                                    | 
| CANCEL_REJECTED                         | 4020       | unknown order sent.                                                              | 
| UNKNOWN_UPDATE_TYPE                     | 4021       | unknown update type.                                                             | 
| REDUCE_ONLY_MARGIN_CHECK_FAILED         | 4030       | reduceOnly order Failed. please check your existing position and open orders.    | 
| UNABLE_TO_PARSE_ORDER                   | 4040       | unable to parse order.                                                           | 
| MARKET_ORDER_REJECT                     | 4050       | the counter party's best price does not meet the PERCENT_PRICE filter limit.     | 
| ORDER_PRICE_RANGE_REJECT                | 4051       | order price not in the proper range.                                             | 
| TRIGGER_PRICE_MISSING                   | 4060       | trigger price is missing.                                                        | 
| NO_OPEN_ORDER                           | 4070       | no open orders.                                                                  | 
| ORDER_ID_DUPLICATE                      | 4071       | order id is existed.                                                             | 
| ACCOUNT_TEMP_LOCKED                     | 4072       | account temporary locked.                                                        | 
| INVALID_POSITION_SIDE                   | 5000       | positionSide is not valid.                                                       | 
| POSITION_SIDE_NOT_MATCH                 | 5001       | positionSide does not match with userSetting.                                    | 
| ADD_ISOLATED_MARGIN_NO_POSITION_REJECT  | 5002       | isolated position quantity should more than 0 when add margin.                   | 
| NO_BALANCE_FOR_ADD_MARGIN               | 5003       | No balance for add position margin.                                              | 
| POSITION_MARGIN_CAN_NOT_DECREASE        | 5004       | Position margin can not decrease.                                                | 
| INVALID_POSITION_MARGIN_MODE            | 5008       | invalid position margin mode                                                     | 
| SNAPSHOT_REJECT                         | 5005       | Snapshot will not be executed on master instance.                                | 
| SNAPSHOT_TRIGGER_TIME_REJECT            | 5006       | Snapshot will not be executed on this trigger time.                              | 

## Data parse and processing ERROR 

| CODE | NAME                                   | DESCRIPTION                                                                      |
|------|----------------------------------------|----------------------------------------------------------------------------------|
| 100  | TIMEOUT                                | Request time out.                                                                |
| 200  | PRICE_LESS_THAN_ZERO                   | Price less than 0.                                                               |
| 201  | PRICE_LESS_THAN_MIN_PRICE              | Price less than min price.                                                       |
| 202  | PRICE_GREATER_THAN_MAX_PRICE           | Price greater than max price.                                                    |
| 203  | PRICE_NOT_INCREASED_BY_TICK_SIZE       | Price not increased by tick size.                                                |
| 204  | QTY_LESS_THAN_ZERO                     | Quantity less than zero.                                                         |
| 205  | QTY_GREATER_THAN_MAX_QTY               | Quantity greater than max quantity.                                              |
| 206  | QTY_LESS_THAN_MIN_QTY                  | Quantity less than min quantity.                                                 |
| 207  | QTY_NOT_INCREASED_BY_STEP_SIZE         | Qty not increased by step size.                                                  |
| 208  | TRIGGER_PRICE_LESS_THAN_ZERO           | Trigger price less than zero.                                                    |
| 209  | TRIGGER_PRICE_GREATER_THAN_MAX_PRICE   | Trigger price greater than max price.                                            |
| 210  | PRICE_TRIGGER_TYPE_IS_NULL_OR_ILLEGAL  | Price trigger type is null or illegal.                                           |
| 211  | NOT_SUPPORT_ORDER_TYPE                 | not support order type.                                                          |
| 212  | INVALID_CL_ORD_ID_LEN                  | client order length is not valid.                                                |
| 213  | INVALID_CL_ORD_ID                      | client order id is not valid.                                                    |
| 214  | CL_ORD_ID_ILLEGAL_CHARS                | Illegal characters found in a parameter.                                         |
| 216  | MANDATORY_PARAM_EMPTY_OR_MALFORMED     | Mandatory parameter {%s} was not sent, was empty/null, or malformed.             |
| 217  | PARAM_NOT_REQUIRED                     | Parameter '%s' not required.                                                     |
| 218  | INVALID_SIDE                           | Invalid side.                                                                    |
| 219  | OPTIONAL_PARAMS_BAD_COMBO              | Combination of optional parameters '%s' invalid.                                 |
| 220  | INVALID_PRICE_RANGE                    | price range not valid                                                            |
| 221  | QTY_TOO_LARGE                          | Qty too large.                                                                   |
| 250  | INVALID_CURRENCY                       | invalid currency.                                                                |
| 300  | INVALID_SYMBOL                         | invalid symbol.                                                                  |
| 301  | SYMBOL_CAN_NOT_TRADING                 | symbol can not trading now.                                                      |
| 302  | SYMBOL_ORDER_CAN_NOT_CANCEL            | symbol order can not cancel now.                                                 |
| 400  | INVALID_ACCOUNT                        | invalid account.                                                                 |
| 401  | ACCOUNT_CAN_NOT_TRADE                  | account could not trade.                                                         |
| 402  | INVALID_LEVERAGE                       | invalid leverage.                                                                |
| 403  | INVALID_USER_ID                        | invalid user id.                                                                 |
| 405  | INVALID_WITHDRAW_BALANCE               | invalid withdraw balance.                                                        |
| 408  | ACCOUNT_MARGIN_NOT_ENOUGH              | account margin not enough.                                                       |
| 501  | WALLET_INSUFFICIENT_BALANCE            | wallet insufficient balance                                                      |
