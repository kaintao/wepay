---
title: '下载平台证书 API'
linkTitle: '下载平台证书'
date: '2019-09-09'
weight: 4
description: >
  由于证书有效期限制和交易安全的原因, 微信支付会不定期的更换平台证书。微信支付提供了一系列接口, 帮助商户后台系统实现平滑的证书更换。
type: 'docs'
---

{{% alert title="更换指引:" color="warning" %}}

- 建议开发者使用中控服务器(即统一管理和分发, 注意证书的保密和安全性）统一下载和管理微信支付平台证书。其他业务逻辑服务器通过该中控服务器进行报文的验签和解密。
- 在微信支付更换平台证书之前, 待更换的证书会提前 24 小时加入商户的平台证书列表。中控服务器需要定时查询商户的平台证书列表, 并及时下载新的平台证书。
- 在微信支付更换平台证书期间, 商户收到的应答请求和回调通知中会同时存在不同的证书序列号, 商户要能正确处理这种情况。
- 获取平台证书的接口频率限制规则: 单个商户号 1000 次/s (查单接口为 600 次/s)。

{{% /alert %}}

{{% alert title="最佳实践:" color="warning" %}}

在中控服务器上调用；\
定时调用, 间隔应小于 12 小时；\
与本地证书序列表对比, 如果发现有新增证书序列号, 则需要新换的证书。老证书会在 1 天内失效, 应及时清理；\
获取到证书后, 分发到各业务接口服务器。

{{% /alert %}}

## 接口说明

**适用对象**: `电商平台`\
**请求 URL**: https://api.mch.weixin.qq.com/v3/certificates\
**请求方式**: GET\
**接口规则**: https://wechatpay-api.gitbook.io/wechatpay-api-v3

`path` 指该参数需在请求 URL 传参\
`query` 指该参数需在请求 JSON 传参

## 请求参数

无请求参数

## 返回参数

| 参数名 | 变量                | 类型         | 必填 | 描述         |
| ------ | ------------------- | ------------ | ---- | ------------ |
| 序列号 | serial_no           | string(32)   | 是   | 证书的序列号 |
| 证书   | encrypt_certificate | string(4096) | 是   | 证书内容     |

异常返回

| 参数名     | 变量    | 类型        | 必填 | 描述                                                     |
| ---------- | ------- | ----------- | ---- | -------------------------------------------------------- |
| 返回状态码 | code    | string(32)  | 是   | 错误码, 枚举值见错误码列表<br>示例值: INVALID_REQUEST    |
| 返回信息   | message | string(256) | 否   | 返回信息, 如非空, 为错误原因<br>示例值: 参数格式校验错误 |

### 返回示例

正常返回
异常返回

```json
{
  "data": [
    {
      "serial_no": "",
      "encrypt_certificate": ""
    },
    {
      "serial_no": "",
      "encrypt_certificate": ""
    }
  ]
}
```

## 错误码<sub>[公共错误码](https://wechatpay-api.gitbook.io/wechatpay-api-v3/wei-xin-zhi-fu-api-v3-jie-kou-gui-fan#http-zhuang-tai-ma)</sub>

| 状态码 | 错误码                  | 描述                                   | 解决方案                                                                                                                                                               |
| ------ | ----------------------- | -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 500    | SYSTEMERROR             | 系统错误                               | 系统异常, 请使用相同参数稍后重新调用                                                                                                                                   |
| 400    | PARAM_ERROR             | 参数错误                               | 请使用正确的参数重新调用                                                                                                                                               |
| 400    | RESOURCE_ALREADY_EXISTS | 存在流程进行中的申请单, 请检查是否重入 | 可通过[查询申请状态](https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/ecommerce/applyments/chapter3_2.shtml)查看此申请单的申请状态, 或更换 out_request_no 提交新的申请单 |
| 403    | NO_AUTH                 | 商户权限异常                           | 请确认是否已经开通相关权限                                                                                                                                             |
| 429    | RATE_LIMITED            | 频率限制                               | 请降低调用频率                                                                                                                                                         |
| 404    | RESOURCE_NOT_EXISTS     | 申请单不存在                           | 确认入参, 传入正确的申请单编号                                                                                                                                         |