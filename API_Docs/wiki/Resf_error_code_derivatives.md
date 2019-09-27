## 错误代码列表

|错误代码| 错误描述|
|-------|---------------|
|403|无效身份|
|1000|系统异常|
|1001|系统未准备就绪|
|1002|查询异常|
|1003|操作redis异常|
|1010|用户不存在|
|1011|用户会话不存在|
|1012|用户账户不存在|
|1013|合约品种不存在|
|1014|合约不存在|
|1015|指数价格不存在|
|1016|对手价不存在|
|1017|查询订单不存在|
|1030|输入错误|
|1031|非法的报单来源|
|1032|访问次数超出限制|
|1033|合约周期字段值错误|
|1034|报单价格类型字段值错误|
|1035|报单方向字段值错误|
|1036|报单开平字段值错误|
|1037|杠杆倍数不符合要求|
|1038|报单价格不符合最小变动价|
|1039|报单价格超出限制|
|1040|报单数量不合法|
|1041|报单数量超出限制|
|1042|超出多头持仓限制|
|1043|超出多头持仓限制|
|1044|超出平台持仓限制|
|1045|杠杆倍数与所持有仓位的杠杆不符合|
|1046|持仓未初始化|
|1047|可用保证金不足|
|1048|持仓量不足|
|1050|客户报单号重复|
|1051|没有可撤订单|
|1052|超出批量数目限制|
|1053|无法获取合约的最新价格区间|
|1054|无法获取合约的最新价|
|1055|平仓时权益不足|
|1056|结算中无法下单和撤单|
|1057|暂停交易中无法下单和撤单|
|1058|停牌中无法下单和撤单|
|1059|交割中无法下单和撤单|
|1060|此合约在非交易状态中，无法下单和撤单|
|1061|订单不存在，无法撤单|
|1062|撤单中，无法重复撤单|
|1063|订单已成交，无法撤单|
|1064|报单主键冲突|
|1065|客户报单号不是整数|
|1066|字段不能为空|
|1067|字段不合法|
|1068|导出错误|
|1069|报单价格不合法|
|1071|订单已撤，无法撤单|
|1100|用户没有开仓权限|
|1101|用户没有平仓权限|
|1102|用户没有入金权限|
|1103|用户没有出金权限|
|1104|合约交易权限,当前禁止交易|
|1105|合约交易权限,当前只能平仓|
|1200|登录错误|
|1220|用户尚未开通合约交易|
|1221|开户资金不足|
|1222|开户天数不足|
|1223|开户VIP等级不足|
|1224|开户国家限制|
|1225|开户不成功|
|1250|无法获取HT\_token|
|1251|BTC折合资产无法获取|
|1252|现货资产无法获取|
|1077|交割结算中，当前品种资金查询失败|
|1078|交割结算中，部分品种资金查询失败|
|1079|交割结算中，当前品种持仓查询失败|
|1080|交割结算中，部分品种持仓查询失败|

## 币币和合约划转接口错误代码列表


### err-code列表

err-code | err-msg(中文） | err-msg(Englis)|补充说明
------ | ------- | -----|-------------
|base-msg|||其他错误，具体的err-msg, 请参照对应的错误消息列表。
|base-currency-error|币种无效|The currency is invalid|
|frequent-invoke|操作过于频繁，请稍后重试。（如果超过1分钟10次，系统返回该error-code）|the operation is too frequent. Please try again later|如果请求次数超过1分钟10次，系统返回该error-code
|banned-by-blacklist|黑名单限制|Blacklist restriction|
|dw-insufficient-balance|可划转余额不足，最大可划转 {0}。（币币账户的余额不足。）|Insufficient balance. You can only transfer {0} at most.|币币账户的余额不足。
|dw-account-transfer-unavailable|转账暂时不可用|account transfer unavailable|该接口暂时不可用
|dw-account-transfer-error|由于其他服务不可用导致的划转失败|account transfer error|
|dw-account-transfer-failed|划转失败。请稍后重试或联系客服 |Failed to transfer. Please try again later.|由于系统异常导致的划转失败
|dw-account-transfer-failed-account-abnormality|账户异常，划转失败。请稍后重试或联系客服|Account abnormality, failed to transfer。Please try again later.|

### base-msg对应的err-msg列表
err-code | err-msg(中文） | err-msg(Englis)|补充说明
------ | ------- | -----|-------------
|base-msg|用户没有入金权限|Unable to transfer in currently. Please contact customer service.|
|base-msg|用户没有出金权限|Unable to transfer out currently. Please contact customer service.|
|base-msg|合约状态异常，无法出入金|Abnormal contracts status. Can’t transfer.|
|base-msg|子账号没有入金权限，请联系客服|Sub-account doesn't own the permissions to transfer in. Please contact customer service.|
|base-msg|子账号没有出金权限，请联系客服|Sub-account doesn't own the permissions to transfer out. Please contact customer service.|
|base-msg|子账号没有划转权限，请登录主账号授权|The sub-account does not have transfer permissions. Please login main account to authorize.|
|base-msg|可划转余额不足|Insufficient amount available.|合约账户的余额不足
|base-msg|单笔转出的数量不能低于{0}{1}|The single transfer-out amount must be no less than {0}{1}.|
|base-msg|单笔转出的数量不能高于{0}{1}|The single transfer-out amount must be no more than {0}{1}.|
|base-msg|单笔转入的数量不能低于{0}{1}|The single transfer-in amount must be no less than {0}{1}.|
|base-msg|单笔转入的数量不能高于{0}{1}|The single transfer-in amount must be no more than {0}{1}.|
|base-msg|您当日累计转出量超过{0}{1}，暂无法转出|Your accumulative transfer-out amount is over the daily maximum, {0}{1}. You can't transfer out for the time being.|
|base-msg|您当日累计转入量超过{0}{1}，暂无法转入|Your accumulative transfer-in amount is over the daily maximum, {0}{1}. You can't transfer in for the time being.|
|base-msg|您当日累计净转出量超过{0}{1}，暂无法转出|Your accumulative net transfer-out amount is over the daily maximum, {0}{1}. You can't transfer out for the time being.|
|base-msg|您当日累计净转入量超过{0}{1}，暂无法转入|Your accumulative net transfer-in amount is over the daily maximum, {0}{1}. You can't transfer in for the time being.|
|base-msg|超过平台当日累计最大转出量限制，暂无法转出|The platform's accumulative transfer-out amount is over the daily maximum. You can't transfer out for the time being.|
|base-msg|超过平台当日累计最大转入量限制，暂无法转入|The platform's accumulative transfer-in amount is over the daily maximum. You can't transfer in for the time being.|
|base-msg|超过平台当日累计最大净转出量限制，暂无法转出|The platform's accumulative net transfer-out amount is over the daily maximum. You can't transfer out for the time being.|
|base-msg|超过平台当日累计最大净转入量限制，暂无法转入|The platform's accumulative net transfer-in amount is over the daily maximum. You can't transfer in for the time being.|
|base-msg|划转失败，请稍后重试或联系客服|Transfer failed. Please try again later or contact customer service.|
|base-msg|服务异常，划转失败，请稍后再试|Abnormal service, transfer failed. Please try again later.|
|base-msg|您尚未开通合约交易，无访问权限|You don’t have access permission as you have not opened contracts trading.|
|base-msg|合约品种不存在|This contract type doesn't exist.|没有相应币种的合约