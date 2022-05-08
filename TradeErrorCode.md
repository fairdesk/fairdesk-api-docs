
## Core Trading ERROR CODE

| CODE | NAME                                   | DESCRIPTION                                                                      | 
|------|----------------------------------------|----------------------------------------------------------------------------------|
| 1000 | NO_SUCH_ACCOUNT                        | no such account.                                                                 |
| 1002 | ACCOUNT_SETTLED_CURRENCY_NOT_MATCH     | account settled currency do not match.                                           |
| 1004 | MIN_LEVERAGE_RATIO                     | leverage is too small                                                            |
| 1005 | MAX_LEVERAGE_RATIO                     | leverage is too large.                                                           |
| 1006 | NEW_LEVERAGE_EQUAL_OLD                 | leverage is equal old leverage.                                                  |
| 1007 | MARGIN_NOT_ENOUGH                      | margin not enough.                                                               |
| 1008 | BALANCE_INSUFFICIENT                   | balance insufficient.                                                            |
| 1009 | RECHARGE_AMOUNT_ILLEGAL                | the amount of recharge is illegal.                                               |
| 1010 | BALANCE_CHANGE_AMOUNT_ILLEGAL          | the amount of balance change is illegal.                                         |
| 1011 | INVALID_MARGIN_CHANGE_AMOUNT           | invalid margin change amount.                                                    |
| 1012 | ISOLATED_LEVERAGE_REJECT_WITH_POSITION | leverage reduction is not supported in Isolated Margin Mode with open positions. |
| 1013 | BALANCE_VERSION_NOT_EQUAL              | balance version not equal.                                                       |
| 2000 | INVALID_SYMBOL_ID                      | invalid symbol id.                                                               |
| 2004 | DUPLICATED_ASSET_NAME                  | duplicated asset name                                                            |
| 2005 | DUPLICATED_ASSET_ID                    | duplicated asset id                                                              |
| 2006 | INVALID_ASSET_ID                       | invalid asset id.                                                                |
| 4000 | NO_SUCH_ORDER                          | no such order.                                                                   |
| 4001 | PRICE_MISSING                          | price is missing.                                                                |
| 4002 | QTY_MISSING                            | qty is missing.                                                                  |
| 4003 | INVALID_QTY                            | qty is invalid.                                                                  |
| 4004 | INVALID_CURRENCY_ID                    | invalid currency id.                                                             |
| 4005 | UNABLE_TO_FILL                         | unable to fill.                                                                  |
| 4006 | ORDER_WOULD_IMMEDIATELY_TRIGGER        | order would immediately trigger.                                                 |
| 4007 | REDUCE_ONLY_REJECT                     | reduce only reject.                                                              |
| 4008 | POSITION_NOT_SUFFICIENT                | position is not sufficient.                                                      |
| 4009 | REDUCE_ONLY_BUY_ERROR                  | reduce only rejected, position more than 0 while buy.                            |
| 4010 | REDUCE_ONLY_SELL_ERROR                 | reduce only rejected, position less than 0 while sell.                           |
| 4011 | MAX_OPEN_ORDER_LIMIT                   | max open order limit.                                                            |
| 4012 | MAX_CONDITIONAL_ORDER_LIMIT            | max conditional order limit.                                                     |
| 4013 | INVALID_CONDITIONAL_ORDER              | conditional order is invalid.                                                    |
| 4014 | CLI_ORD_ID_DUPLICATE                   | clientOrderId duplicated.                                                        |
| 4015 | REDUCE_ONLY_ILLEGAL                    | reduce only reject, order type not supported.                                    |
| 4020 | CANCEL_REJECTED                        | unknown order sent.                                                              |
| 4021 | UNKNOWN_UPDATE_TYPE                    | unknown update type.                                                             |
| 4030 | REDUCE_ONLY_MARGIN_CHECK_FAILED        | reduceOnly order Failed. please check your existing position and open orders.    |
| 4040 | UNABLE_TO_PARSE_ORDER                  | unable to parse order.                                                           |
| 4050 | MARKET_ORDER_REJECT                    | the counter party's best price does not meet the PERCENT_PRICE filter limit.     |
| 4051 | ORDER_PRICE_RANGE_REJECT               | order price not in the proper range.                                             |
| 4060 | TRIGGER_PRICE_MISSING                  | trigger price is missing.                                                        |
| 4070 | NO_OPEN_ORDER                          | no open orders to close                                                          |
| 4071 | ORDER_ID_DUPLICATE                     | order id duplicated.                                                             |
| 5000 | INVALID_POSITION_SIDE                  | positionSide is not valid.                                                       |
| 5001 | POSITION_SIDE_NOT_MATCH                | positionSide does not match with userSetting.                                    |
| 5002 | ADD_ISOLATED_MARGIN_NO_POSITION_REJECT | isolated position quantity should more than 0 when add margin.                   |
| 5003 | NO_BALANCE_FOR_ADD_MARGIN              | No balance for add position margin.                                              |
| 5004 | POSITION_MARGIN_CAN_NOT_DECREASE       | Position margin can not decrease.                                                |

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
