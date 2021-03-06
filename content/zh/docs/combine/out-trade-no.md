---
title: '合单查询订单API'
linkTitle: '查询订单'
weight: 3
description: >
  通过此接口查询订单状态
type: 'docs'
---

电商平台通过合单查询订单 API 查询订单状态, 完成下一步的业务逻辑。

{{% alert title="注意" color="warning" %}}

需要调用查询接口的情况:

1. 当商户后台. 网络. 服务器等出现异常, 商户系统最终未接收到支付通知。
2. 调用支付接口后, 返回系统错误或未知交易状态情况。
3. 调用刷卡支付 API, 返回 USERPAYING 的状态。
4. 调用关单或撤销接口 API 之前, 需确认支付状态。

{{% /alert %}}

## 接口说明

- **适用对象:** `电商平台` `服务商` `直连商户`
- **请求 URL:** https://api.mch.weixin.qq.com/v3/combine-transactions/out-trade-no/{combine_out_trade_no}
- **请求方式:** `GET`
- **接口规则:** https://wechatpay-api.gitbook.io/wechatpay-api-v3

## 请求参数

- `path` 指该参数需在请求 URL 传参
- `query` 指该参数需在请求 JSON 传参

| 参数                 | 类型       | 必填 | 参数名/描述/示例值                                |
| -------------------- | ---------- | ---- | ------------------------------------------------- |
| combine_out_trade_no | string(32) | 是   | `合单商户订单号` `path` 合单支付总订单号[^q-cotn] |
|                      |            |      | P20150806125346                                   |

[^q-cotn]: 要求 32 个字符内, 只能是数字、大小写字母\_-|\*@ , 且在同一个商户号下唯一 。

#### 请求示例

`GET` https://api.mch.weixin.qq.com/v3/combine-transactions/out-trade-no/P20150806125346

## 返回参数

| 参数                                 | 类型       | 必填 | 参数名/描述/示例值                         |
| ------------------------------------ | ---------- | ---- | ------------------------------------------ |
| combine_appid                        | string(32) | 是   | `合单商户 appid`合单发起方的 appid。       |
|                                      |            |      | wxd678efh567hg6787                         |
| combine_mchid                        | string(32) | 是   | `合单商户号` 合单发起方商户号。            |
|                                      |            |      | 1900000109                                 |
| combine_out_trade_no                 | string(32) | 是   | `合单商户订单号` 合单支付总订单号[^r-cotn] |
|                                      |            |      | P20150806125346                            |
| :arrow_right_hook:scene_info         | object     | 否   | `场景信息` 支付场景信息描述                |
| :arrow_right_hook:sub_orders         | array      | 是   | `子单信息` 最多支持子单条数: 50            |
| :arrow_right_hook:combine_payer_info | object     | 否   | `支付者` 见请求示例                        |

[^r-cotn]: 要求 32 个字符内, 只能是数字. 大小写字母\_-|\*@ , 且在同一个商户号下唯一。

场景信息(scene_info)

| 参数      | 类型       | 必填 | 参数名/描述/示例值                                                           |
| --------- | ---------- | ---- | ---------------------------------------------------------------------------- |
| device_id | string(16) | 否   | `商户端设备号` 终端设备号(门店号或收银设备 ID） 。特殊规则:长度最小 7 个字节 |
|           |            |      | POS1:1                                                                       |

子单信息(sub_orders)

| 参数                     | 类型        | 必填 | 参数名/描述/示例值                                                                                                                                |
| ------------------------ | ----------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| mchid                    | string(32)  | 是   | `子单商户号` 子单发起方商户号, 必须与发起方 Appid 有绑定关系。                                                                                    |
|                          |             |      | 1900000109                                                                                                                                        |
| trade_type               | string(16)  | 是   | `交易类型` 枚举值: NATIVE:扫码支付 JSAPI:公众号支付 APP:APP 支付 MWEB:H5 支付                                                                     |
|                          |             |      | JSAPI                                                                                                                                             |
| trade_state              | string(32)  | 是   | `交易状态` 枚举值: SUCCESS:支付成功 REFUND:转入退款 NOTPAY:未支付 CLOSED:已关闭 USERPAYING:用户支付中 PAYERROR:支付失败(其他原因, 如银行返回失败) |
|                          |             |      | SUCCESS                                                                                                                                           |
| bank_type                | string(16)  | 否   | `付款银行` 银行类型, 采用字符串类型的银行标识。                                                                                                   |
|                          |             |      | CMC                                                                                                                                               |
| attach                   | string(128) | 是   | `附加信息` 附加数据, 在查询 API 和支付通知中原样返回, 可作为自定义参数使用。                                                                      |
|                          |             |      | 深圳分店                                                                                                                                          |
| success_time             | string(32)  | 是   | `支付完成时间`[^success_time]                                                                                                                     |
|                          |             |      | 2015-05-20T13:29:35.120+08:00                                                                                                                     |
| transaction_id           | string(32)  | 是   | `微信订单号` 微信支付订单号。                                                                                                                     |
|                          |             |      | 1009660380201506130728806387                                                                                                                      |
| out_trade_no             | string(32)  | 是   | `子单商户订单号` 商户系统内部订单号                                                                                                               |
|                          |             |      | 20150806125346                                                                                                                                    |
| sub_mchid                | string(32)  | 是   | `二级商户号` 二级商户商户号, 由微信支付生成并下发。                                                                                               |
|                          |             |      | 1900000109                                                                                                                                        |
| :arrow_right_hook:amount | object      | 是   | `订单金额` 订单金额信息                                                                                                                           |

[^success_time]: 订单支付时间, 遵循 rfc3339 标准格式, 格式为 YYYY-MM-DDTHH:mm:ss:sss+TIMEZONE, YYYY-MM-DD 表示年月日, T 出现在字符串中, 表示 time 元素的开头, HH:mm:ss:sss 表示时分秒毫秒, TIMEZONE 表示时区(+08:00 表示东八区时间, 领先 UTC 8 小时, 即北京时间）。例如:2015-05-20T13:29:35+08:00 表示, 北京时间 2015 年 5 月 20 日 13 点 29 分 35 秒。

订单金额(amount)

| 参数           | 类型      | 必填 | 参数名/描述/示例值                                                          |
| -------------- | --------- | ---- | --------------------------------------------------------------------------- |
| total_amount   | int64     | 是   | `标价金额` 子单金额, 单位为分。                                             |
|                |           |      | 100                                                                         |
| currency       | string(8) | 否   | `标价币种` 符合 ISO 4217 标准的三位字母代码, 人民币:CNY。                   |
|                |           |      | CNY                                                                         |
| payer_amount   | int64     | 是   | `现金支付金额` 订单现金支付金额。                                           |
|                |           |      | 10                                                                          |
| payer_currency | string(8) | 否   | `现金支付币种` 货币类型, 符合 ISO 4217 标准的三位字母代码, 默认人民币:CNY。 |
|                |           |      | CNY                                                                         |

支付者(combine_payer_info)

| 参数   | 类型        | 必填 | 参数名/描述/示例值                                                                 |
| ------ | ----------- | ---- | ---------------------------------------------------------------------------------- |
| openid | string(128) | 是   | `用户标识` 使用合单 appid 获取的对应用户 openid。是用户在商户 appid 下的唯一标识。 |
|        |             |      | oUpF8uMuAJO_M2pxb1Q9zNjWeS6o                                                       |

#### 返回示例

正常示例

```json
{
  "combine_appid ": "wxd678efh567hg6787",
  "combine_mchid": "1230000109",
  "combine_payer_info": {
    "openid": "oUpF8uMuAJO_M2pxb1Q9zNjWeS6o"
  },
  "sub_orders": [
    {
      "mchid": "1900000109",
      "trade_type": "JSAPI",
      "trade_state": "SUCCESS",
      "bank_type": "CMC",
      "attach": "深圳分店",
      "success_time": "2015-05-20T13:29:35.120+08:00",
      "amount": {
        "total_amount": 10,
        "payer_amount": 10,
        "currency": "CNY",
        "payer_currency": "CNY"
      },
      "transaction_id": "1009660380201506130728806387",
      "out_trade_no": "20150806125346",
      "sub_mchid": "1230000109"
    }
  ],
  "scene_info": {
    "device_id": "POS1:1"
  },
  "combine_out_trade_no": "1217752501201407033233368018"
}
```

## 错误码<sub>公共错误码</sub>

| 状态码 | 错误码                | 描述/解决方案                                                                     |     |     |
| ------ | --------------------- | --------------------------------------------------------------------------------- | --- | --- |
| 202    | USERPAYING            | 用户支付中, 需要输入密码                                                          |     |     |
|        |                       | 等待 5 秒, 然后调用被扫订单结果查询 API, 查询当前订单的不同状态, 决定下一步的操作 |     |     |
| 403    | TRADE_ERROR           | 交易错误                                                                          |     |     |
|        |                       | 因业务原因交易失败, 请查看接口返回的详细信息                                      |     |     |
| 500    | SYSTEMERROR           | 系统错误                                                                          |     |     |
|        |                       | 系统异常, 请用相同参数重新调用                                                    |     |     |
| 401    | SIGN_ERROR            | 签名错误                                                                          |     |     |
|        |                       | 请检查签名参数和方法是否都符合签名算法要求                                        |     |     |
| 403    | RULELIMIT             | 业务规则限制                                                                      |     |     |
|        |                       | 因业务规则限制请求频率, 请查看接口返回的详细信息                                  |     |     |
| 400    | PARAM_ERROR           | 参数错误                                                                          |     |     |
|        |                       | 请根据接口返回的详细信息检查请求参数                                              |     |     |
| 403    | OUT_TRADE_NO_USED     | 商户订单号重复                                                                    |     |     |
|        |                       | 请核实商户订单号是否重复提交                                                      |     |     |
| 404    | ORDERNOTEXIST         | 订单不存在                                                                        |     |     |
|        |                       | 请检查订单是否发起过交易                                                          |     |     |
| 400    | ORDER_CLOSED          | 订单已关闭                                                                        |     |     |
|        |                       | 当前订单已关闭, 请重新下单                                                        |     |     |
| 500    | OPENID_MISMATCH       | openid 和 appid 不匹配                                                            |     |     |
|        |                       | 请确认 openid 和 appid 是否匹配                                                   |     |     |
| 403    | NOTENOUGH             | 余额不足                                                                          |     |     |
|        |                       | 用户帐号余额不足, 请用户充值或更换支付卡后再支付                                  |     |     |
| 403    | NOAUTH                | 商户无权限                                                                        |     |     |
|        |                       | 请商户前往申请此接口相关权限                                                      |     |     |
| 400    | MCH_NOT_EXISTS        | 商户号不存在                                                                      |     |     |
|        |                       | 请检查商户号是否正确                                                              |     |     |
| 500    | INVALID_TRANSACTIONID | 订单号非法                                                                        |     |     |
|        |                       | 请检查微信支付订单号是否正确                                                      |     |     |
| 400    | INVALID_REQUEST       | 无效请求                                                                          |     |     |
|        |                       | 请根据接口返回的详细信息检查                                                      |     |     |
| 429    | FREQUENCY_LIMITED     | 频率超限                                                                          |     |     |
|        |                       | 请降低请求接口频率                                                                |     |     |
| 500    | BANKERROR             | 银行系统异常                                                                      |     |     |
|        |                       | 银行系统异常, 请用相同参数重新调用                                                |     |     |
| 400    | APPID_MCHID_NOT_MATCH | appid 和 mch_id 不匹配                                                            |     |     |
|        |                       | 请确认 appid 和 mch_id 是否匹配                                                   |     |     |
| 403    | ACCOUNTERROR          | 账号异常                                                                          |     |     |
|        |                       | 用户账号异常, 无需更多操作                                                        |     |     |
