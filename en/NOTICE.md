# NOTICE

## 公共输入

参数名称  | 是否必须 | 数据类型 | 描述 | 默认值 | 取值范围
-------  | ------- | ------- | ---- | ----- | --------
depositCoinCode | 是 | String | 存币币种 | | 见查询币种接口
receiveCoinCode | 是 | String | 接收币币种 | | 见查询币种接口
depositCoinAmt | 是 | String | 存币数量 | | 用户创建订单后，需向订单地址上充值depositCoinAm
receiveCoinAmt | 是 | String | 接收币数量 | | receiveCoinAmt = depositCoinAmt * instantRate 汇率是通过getBaseInfo接口获得，该值用于记录当时下单时的市场价格，精度保留6位小数
receiveSwftAmt | 是 | String | 速币数量 | | 
destinationAddr | 是 | String | 收币地址 | | 接收币地址
soureFlag | 否 | String | 订单创建来源 | | 三方平台标识
developerId | 否 | String | 开发者订单ID | | 三方平台传入的订单号
refundAddr | 是 | String | 退币地址 | | 兑换失败存入币退币地址
```

## 更新文档

```
git pull
gitbook build
rm -rf _book_cache
cp -r _book _book_cache
```