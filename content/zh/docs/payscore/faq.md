---
title: '常见QA'
linkTitle: '常见QA'
weight: 22
description: >
  常见QA
type: 'docs'
---

|                                 |                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 微信支付分常见业务问题详见:     | https://developers.weixin.qq.com/community/pay/doc/00020c26e9c738be02c86bfbd5b808?blockType=8                                                                                                                                                                                                                                                                                                                   |
| 微信支付分常见技术问题详见:     | https://developers.weixin.qq.com/community/pay/doc/0004060fa7c65855d698166145b808?blockType=8                                                                                                                                                                                                                                                                                                                   |
| 关于申请退款、对账单下载的问题: | 支付分接口没有单独的申请退款、对账单下载接口，商户可以调用普通支付的接口来完成这些操作，接口文档可以查看 API 列表->申请退款、查询退款、退款结果通知、下载对账单，其中申请退款需要使用【支付成功回调通知】接口回调的 transaction_id 作为条件，transaction_id 等于普通支付接口中的 transaction_id                                                                                                                 |
| 微信支付分对账单:               | 微信支付分对账单案例下载点击下载                                                                                                                                                                                                                                                                                                                                                                                |
| 关于订单风险金额问题:           | 1、订单风险金额不宜比服务订单结算总金额高过多: 订单风险金额越高，可获得权益的用户将越少，用户可享受权益的通过率将越低。<br>2、订单风险金额不宜比服务订单结算总金额低过多: 对于用户是否能享受权益，是基于订单风险金额来评估的，因此，订单风险金额预估比服务订单结算总金额越低，收款成功率可能会越低。<br>因此，尽量准确、合理预估订单风险金额，以保证通过率和收款成功率<br>3、完结金额可大于、小于或等于风险金额 |