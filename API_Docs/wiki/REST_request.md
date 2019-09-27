##  请求说明

### 访问地址

**注意：<br>**

**请使用中国大陆以外的服务器访问火币 API；<br>**

- Global 现货/杠杆：
行情： https://api.huobi.pro/market
交易： https://api.huobi.pro/v1

- HBDM 合约：
行情/交易： https://api.hbdm.com

- 限频规则：现货 / 杠杆(api.huobi.pro)：10秒100次，合约(api.hbdm.com)：限制频率为10秒50次。（单个APIKEY维度限制，建议行情API访问也要加上签名，否则限频会更严格）

- POST请求头信息中必须声明 Content-Type:application/json。
- GET请求头信息中必须声明 Content-Type:application/x-www-form-urlencoded。(汉语用户建议设置 Accept-Language:zh-cn)
- 所有请求参数请按照 API 说明进行参数封装。
- 将封装好参数的 API 请求通过 POST 或 GET 的方式提交到服务器。
- 火币网处理请求，并返回相应的 JSON 格式结果。
- 请使用 https 请求。
- 查询资产详情方法调用顺序：查询当前用户的所有账户->查询指定账户的余额 
- 支持所有Huobi Global站上交易中的交易对，上新币保持与网站同步。