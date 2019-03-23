# Interface
edited this page on 2019.03.21

## 接口列表
请求类型     |   请求方法   | 描述 | 接口示例
----------  | -------   | ----- | -------
GET/POST application/x-www-form-urlencoded |  [api/v1/queryCoinList](#查询币种列)  | 查询币种列 | [代码示例](#查询币种列接口示例)
POST application/json |  [api/v1/getBaseInfo](#获取兑换汇率基本信息)| 获取兑换汇率基本信息 | [代码示例](#获取兑换汇率基本信息接口示例)
POST application/json | [api/v2/accountExchange](#创建订单) | 创建订单 | [代码示例](#创建订单接口示例)
POST application/json | [api/v2/queryOrderState](#查询订单状态) | 查询订单状态 | [代码示例](#查询订单状态接口示例)


## 查询币种列
### api/v1/queryCoinList
### 说明
提供币种列表展示给用户，告诉用户哪些币可以进行兑换 
### 请求参数
参数名称  | 是否必须 | 数据类型 | 描述 | 默认值 | 取值范围
-------  | ------- | ------- | ---- | ----- | --------
supportType | 否 | String | 兑换类型 | | advanced
### 响应参数
````
{
      "data": [
           {
            "coinAllCode": "SwftCoin",
            "coinCode": "SWFTC",
            "coinImageUrl": "",
            "coinName": "速币",
            "contact": "0x0bb217E40F8a5Cb79Adf04E1aAb60E5abd0dfC1e",
            "isSupportAdvanced": "Y",
            "mainNetwork": "ETH",
            "noSupportCoin": "BCC,SAN,ICX,EET,ETDM,BCC,GZRO,DTO,UCTT"
        },
        {
            "coinAllCode": "Bitcoin",
            "coinCode": "BTC",
            "coinImageUrl": "/static/image/coins/bitcoin.png",
            "coinName": "比特币",
            "contact": "",
            "isSupportAdvanced": "Y",
            "mainNetwork": "",
            "noSupportCoin": "BCC,SAN,ICX,EET,ETDM,BCC,GZRO,DTO,UCTT"
        },
        {
            "coinAllCode": "Ether",
            "coinCode": "ETH",
            "coinImageUrl": "/static/image/coins/ether.png",
            "coinName": "以太币",
            "contact": "",
            "isSupportAdvanced": "Y",
            "mainNetwork": "",
            "noSupportCoin": "BCC,SAN,ICX,EET,ETDM,BCC,GZRO,DTO,UCTT"
        },
        {
            "coinAllCode": "EOS",
            "coinCode": "EOS",
            "coinImageUrl": "",
            "coinName": "基数链",
            "contact": "",
            "isSupportAdvanced": "Y",
            "mainNetwork": "",
            "noSupportCoin": "BCC,SAN,ICX,EET,ETDM,BCC,GZRO,DTO,UCTT"
        },
          ...
      ],
      "resCode": "800",
      "resMsg": "成功"
  }
````
### data说明
````
        {
            "coinAllCode": 币种全称,
            "coinCode": "币种编码",
            "coinImageUrl": 图片地址,
            "coinName": "币种名称",
            "contact": 合约地址,
            "isSupportAdvanced": 是否支持高级兑换,
            "mainNetwork": 所属主网,
            "noSupportCoin": 不支持兑换的币种
        }
````

## 获取兑换汇率基本信息
### api/v1/getBaseInfo
### 说明 
提供两个币种之间兑换的汇率，汇率更新频率为：4~6s
### 请求参数
参数名称  | 是否必须 | 数据类型 | 描述 | 默认值 | 取值范围
-------  | ------- | ------- | ---- | ----- | --------
depositCoinCode | 是 | String | 存币币种 | | 见查询币种接口
receiveCoinCode | 是 | String | 接收币币种 | | 见查询币种接口
### 响应参数
````
  {
    "data": {
        "depositMax": "1692690",
        "depositMin": "169269",
        "instantRate": "0.000000538438",
        "minerFee": "0.00004368",
        "receiveCoinFee": "0.009328"
    },
    "resCode": "800",
    "resMsg": "成功"
}
````
### data说明
````
  {
        "depositMax": 最高存储限额（精确到小数点后六位）,
        "depositMin": 最低存储限额（精确到小数点后六位）,
        "instantRate": 实时汇率（精确到小数点后十位）,
        "minerFee": 兑换手续费（精确到小数点后六位）,
        "receiveCoinFee": 发币手续费（精确到小数点后六位，兑换成功时才会收取此费用）
    }
````
## 创建订单
### api/v2/accountExchange
### 说明 
创建订单有ip频率限制，2s内下单数量不能超过5个
### 请求参数
参数名称  | 是否必须 | 数据类型 | 描述 | 默认值 | 取值范围
-------  | ------- | ------- | ---- | ----- | --------
depositCoinCode | 是 | String | 存币币种 | | 见查询币种接口
receiveCoinCode | 是 | String | 接收币币种 | | 见查询币种接口
depositCoinAmt | 是 | String | 存币数量 | | 用户创建订单后，需向订单地址上充值depositCoinAm
receiveCoinAmt | 是 | String | 接收币数量 | | receiveCoinAmt = depositCoinAmt * instantRate 汇率是通过getBaseInfo接口获得，该值用于记录当时下单时的市场价格，精度保留6位小数
receiveSwftAmt | 是 | String | 速币数量 | | 
destinationAddr | 是 | String | 收币地址 | | 接收币地址
soureFlag | 否 | String | 订单创建来源 | | 三方平台标识
developerId | 否 | String | 开发者订单ID | | 用于记录项目方的订单关联数据，项目方可用该字段来表示该订单归属于自己的某个用户或用于记录自己系统内的订单编号，或其他编号；在订单创建完成后，会回传该字段值（SWFT不支持通过该字段查询寻订单信息）
refundAddr | 是 | String | 退币地址 | | 兑换失败存入币退币地址
equipmentNo | 是 | String | 设备编号 | | 环境编号，这个可用于查询属于该编号的所有订单信息，请勿泄露
sourceType | 是 | String | 设备来源 | | ANDROID,IOS,H5
userNo | 否 | String | 用户手机号/email | | 未登录情况下，此值为空，登录后，存登录信息
sessionUuid | 否| String | 登录后的sessionUuid | | 未登录情况下，此值为空，登录后，存返回的sessionUuid
### 响应参数
````
  {
      "data": {
          "changeType": "advanced",
          "choiseFeeType": "3",
          "depositCoinAmt": "0.01",
          "depositCoinCode": "BTC",
          "depositCoinFeeAmt": "0.00001",
          "depositCoinFeeRate": "0.001",
          "depositCoinState": "wait_send",
          "destinationAddr": "0x364397e2fc9929f11ba0c03826ef282dd64a829f",
          "detailState": "wait_deposit_send",
          "developerId": "",
          "orderId": "60a31323-0507-446b-866c-973a91856e8c",
          "orderState": "wait_deposits",
          "platformAddr": "3HKL2rwot5YezHGdc5yAabZo74TJeTkVqb",
          "receiveCoinAmt": "0.33642",
          "receiveCoinCode": "ETH",
          "receiveSwftAmt": "18.06",
          "refundAddr": "18orDLFMp3fGoy5Uk93LDGTGbxWEm7b7FY",
          "refundCoinAmt": "",
          "refundCoinMinerFee": "",
          "refundDepositTxid": "",
          "refundSwftAmt": "",
          "swftCoinFeeRate": "0.0005",
          "swftCoinState": "",
          "swftReceiveAddr": "",
          "swftRefundAddr": "",
          "transactionId": ""
      },
      "resCode": "800",
      "resMsg": "成功"
  }
````
### data说明
````
    {
          "changeType": 兑换方式（simple,advanced）,
          "choiseFeeType": 手续费类型（3-原币模式，4-速币模式）,
          "depositCoinAmt": 存币数量,
          "depositCoinCode": 存币币种,
          "depositCoinFeeAmt": 存币手续费,
          "depositCoinFeeRate": 存币手续费率,
          "depositCoinState": 存币的存放状态,
          "destinationAddr": 目标币接收地址,
          "detailState": 订单状态（"(1)wait_deposit_send:等待存币发送
                              (2)timeout:超时；
                              (3)wait_exchange_push:等待交换信息推送；
                              (4)wait_exchange_return:等待交换信息返回；
                              (5.1)wait_receive_send:等待接收币种发送, wait_receive_confirm:等待接收币种确认, receive_complete:接收币种确认完成.
                              (5.2)wait_refund_send:等待退原币币种发送, wait_refund_confirm:等待退原币币种确认, refund_complete:退原币币种确认完成；"）,
          "developerId": 开发者id，其他平台传过来的订单号，透传返回给项目方,
          "orderId": 订单号,
          "orderState": 订单状态,
          "platformAddr": 存币地址,
          "receiveCoinAmt": 接收币数量,
          "receiveCoinCode": 接收币币种,
          "receiveSwftAmt": 速币数量,
          "refundAddr": 退币地址,
          "refundCoinAmt": 退币金额,
          "refundCoinMinerFee": 退币旷工费,
          "refundDepositTxid": 退币交易ID,
          "refundSwftAmt": 退币速币金额,
          "swftCoinFeeRate": 速币手续费率,
          "swftCoinState": 速币汇率,
          "swftReceiveAddr": 速币接收地址,
          "swftRefundAddr": 速币退币地址,
          "transactionId": 发币交易ID
      }
````

## 查询订单状态
### api/v2/queryOrderState
### 请求参数
参数名称  | 是否必须 | 数据类型 | 描述 | 默认值 | 取值范围
-------  | ------- | ------- | ---- | ----- | --------
orderId | 是 | String | 订单ID | | eg：1fc8499f-dd6d-4ff3-8b7f-7a0d74c59adc
equipmentNo | 是 | String | 设备编号 | | 环境编号，这个可用于查询属于该编号的所有订单信息，请勿泄露
sourceType | 是 | String | 设备来源 | | ANDROID,IOS,H5
userNo | 否 | String | 用户手机号/email | | 未登录情况下，此值为空，登录后，存登录信息
sessionUuid | 否| String | 登录后的sessionUuid | | 未登录情况下，此值为空，登录后，存返回的sessionUuid
### 响应参数
````  
    {
      "data": {
          "changeType": "advanced",
          "choiseFeeType": "3",
          "dealReceiveCoinAmt": "10.513615",
          "depositCoinAmt": "0.276",
          "depositCoinCode": "ETH",
          "depositCoinFeeAmt": "0.000276",
          "depositCoinFeeRate": "0.001",
          "depositCoinState": "already_confirm",
          "destinationAddr": "loveqyw12345",
          "detailState": "receive_complete",
          "orderId": "de752da3-0ff7-4682-8038-d8e1f20cad95",
          "orderState": "wait_send",
          "platformAddr": "0x6e76d9e78b6f4878fbe83e6985185173373f0b7e",
          "receiveCoinAmt": "10.403011",
          "receiveCoinCode": "EOS",
          "receiveSwftAmt": "14.06",
          "refundAddr": "0x189547cb7984711c2ab25b1543a784e65628a8f6",
          "refundCoinAmt": "",
          "refundCoinMinerFee": "",
          "refundDepositTxid": "",
          "refundSwftAmt": "",
          "swftCoinFeeRate": "0.0005",
          "swftCoinState": "",
          "swftReceiveAddr": "",
          "swftRefundAddr": "",
          "tradeState": "",
          "transactionId": "072a7915ac9de0e64d5131afde8f61e1cdf0cf517ffa3f8db2f2d26bf208b658"
      },
      "resCode": "800",
      "resMsg": "成功"
  }
  ````
### data说明
````
    {
          "changeType": 兑换方式（simple,advanced）,
          "choiseFeeType": 手续费类型（3-原币模式，4-速币模式）,
          "dealReceiveCoinAmt": "10.513615",
          "depositCoinAmt": 存币数量,
          "depositCoinCode": 存币币种,
          "depositCoinFeeAmt": 存币手续费,
          "depositCoinFeeRate": 存币手续费率,
          "depositCoinState": 存币的存放状态,
          "destinationAddr": 目标币接收地址,
          "detailState": 订单状态（"(1)wait_deposit_send:等待存币发送
                                   (2)timeout:超时；
                                   (3)wait_exchange_push:等待交换信息推送；
                                   (4)wait_exchange_return:等待交换信息返回；
                                   (5.1)wait_receive_send:等待接收币种发送, wait_receive_confirm:等待接收币种确认, receive_complete:接收币种确认完成.
                                   (5.2)wait_refund_send:等待退原币币种发送, wait_refund_confirm:等待退原币币种确认, refund_complete:退原币币种确认完成；"）,
          "orderId": 订单号,
          "orderState": 订单状态,
          "platformAddr": 存币地址,
          "receiveCoinAmt": 接收币数量,
          "receiveCoinCode": 接收币币种,
          "receiveSwftAmt": 速币数量,
          "refundAddr": 退币地址,
          "refundCoinAmt": 退币金额,
          "refundCoinMinerFee": 退币旷工费,
          "refundDepositTxid": 退币交易ID,
          "refundSwftAmt": 退币速币金额,
          "swftCoinFeeRate": 速币手续费率,
          "swftCoinState": 速币汇率,
          "swftReceiveAddr": 速币接收地址,
          "swftRefundAddr": 速币退币地址,
          "tradeState": "",
          "transactionId": 发币交易ID
      }
````

# 代码示例
##查询币种列接口示例

#### java代码示例：
````
  OkHttpClient client = new OkHttpClient();
  ​
  MediaType mediaType = MediaType.parse("application/x-www-form-urlencoded");
  RequestBody body = RequestBody.create(mediaType, "supportType=advanced");
  Request request = new Request.Builder()
    .url("https://transfer.swft.pro/api/v1/queryCoinList")
    .post(body)
    .addHeader("Content-Type", "application/x-www-form-urlencoded")
    .addHeader("cache-control", "no-cache")
    .build();
  ​
  Response response = client.newCall(request).execute();
````
#### C# 代码示例
````
  var client = new RestClient("https://transfer.swft.pro/api/v1/queryCoinList");
  var request = new RestRequest(Method.POST);
  request.AddHeader("cache-control", "no-cache");
  request.AddHeader("Content-Type", "application/x-www-form-urlencoded");
  request.AddParameter("undefined", "supportType=advanced&undefined=", ParameterType.RequestBody);
  IRestResponse response = client.Execute(request);
  ````
#### Objective-C 代码示例
````
  #import <Foundation/Foundation.h>
  ​
  NSDictionary *headers = @{ @"Content-Type": @"application/x-www-form-urlencoded",
                             @"cache-control": @"no-cache" };
  ​
  NSMutableData *postData = [[NSMutableData alloc] initWithData:[@"supportType=advanced" dataUsingEncoding:NSUTF8StringEncoding]];
  [postData appendData:[@"&undefined=undefined" dataUsingEncoding:NSUTF8StringEncoding]];
  ​
  NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"https://transfer.swft.pro/api/v1/queryCoinList"]
                                                         cachePolicy:NSURLRequestUseProtocolCachePolicy
                                                     timeoutInterval:10.0];
  [request setHTTPMethod:@"POST"];
  [request setAllHTTPHeaderFields:headers];
  [request setHTTPBody:postData];
  ​
  NSURLSession *session = [NSURLSession sharedSession];
  NSURLSessionDataTask *dataTask = [session dataTaskWithRequest:request
                                              completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                                                  if (error) {
                                                      NSLog(@"%@", error);
                                                  } else {
                                                      NSHTTPURLResponse *httpResponse = (NSHTTPURLResponse *) response;
                                                      NSLog(@"%@", httpResponse);
                                                  }
                                              }];
  [dataTask resume];
  
````
#### Nodejs代码示例
````
  var qs = require("querystring");
  var http = require("https");
  ​
  var options = {
    "method": "POST",
    "hostname": [
      "transfer",
      "swft",
      "pro"
    ],
    "path": [
      "api",
      "v1",
      "queryCoinList"
    ],
    "headers": {
      "Content-Type": "application/x-www-form-urlencoded",
      "cache-control": "no-cache"
    }
  };
  ​
  var req = http.request(options, function (res) {
    var chunks = [];
  ​
    res.on("data", function (chunk) {
      chunks.push(chunk);
    });
  ​
    res.on("end", function () {
      var body = Buffer.concat(chunks);
      console.log(body.toString());
    });
  });
  ​
  req.write(qs.stringify({ supportType: 'advanced', undefined: undefined }));
  req.end();
  ````
  ## 获取兑换汇率基本信息接口示例
 #### java代码示例：
 ````
    OkHttpClient client = new OkHttpClient();
    ​
    MediaType mediaType = MediaType.parse("application/json");
    RequestBody body = RequestBody.create(mediaType, "{\"depositCoinCode\":\"BTC\",\"receiveCoinCode\":\"ETH\"}");
    Request request = new Request.Builder()
      .url("https://transfer.swft.pro/api/v1/getBaseInfo")
      .post(body)
      .addHeader("Content-Type", "application/json")
      .addHeader("cache-control", "no-cache")
      .build();
    ​
    Response response = client.newCall(request).execute();
  ````
  #### C# 代码示例
  ````
    var client = new RestClient("https://transfer.swft.pro/api/v1/getBaseInfo");
    var request = new RestRequest(Method.POST);
    request.AddHeader("cache-control", "no-cache");
    request.AddHeader("Content-Type", "application/json");
    request.AddParameter("undefined", "{\"depositCoinCode\":\"BTC\",\"receiveCoinCode\":\"ETH\"}", ParameterType.RequestBody);
    IRestResponse response = client.Execute(request);
 ````
 #### Objective-C 代码示例
 ````
    #import <Foundation/Foundation.h>
    ​
    NSDictionary *headers = @{ @"Content-Type": @"application/json",
                               @"cache-control": @"no-cache" };
    NSDictionary *parameters = @{ @"depositCoinCode": @"BTC",
                                  @"receiveCoinCode": @"ETH" };
    ​
    NSData *postData = [NSJSONSerialization dataWithJSONObject:parameters options:0 error:nil];
    ​
    NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"https://transfer.swft.pro/api/v1/getBaseInfo"]
                                                           cachePolicy:NSURLRequestUseProtocolCachePolicy
                                                       timeoutInterval:10.0];
    [request setHTTPMethod:@"POST"];
    [request setAllHTTPHeaderFields:headers];
    [request setHTTPBody:postData];
    ​
    NSURLSession *session = [NSURLSession sharedSession];
    NSURLSessionDataTask *dataTask = [session dataTaskWithRequest:request
                                                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                                                    if (error) {
                                                        NSLog(@"%@", error);
                                                    } else {
                                                        NSHTTPURLResponse *httpResponse = (NSHTTPURLResponse *) response;
                                                        NSLog(@"%@", httpResponse);
                                                    }
                                                }];
    [dataTask resume];
    
````
 ​
####  Nodejs代码示例
````
    var http = require("https");
    ​
    var options = {
      "method": "POST",
      "hostname": [
        "transfer",
        "swft",
        "pro"
      ],
      "path": [
        "api",
        "v1",
        "getBaseInfo"
      ],
      "headers": {
        "Content-Type": "application/json",
        "cache-control": "no-cache"
      }
    };
    ​
    var req = http.request(options, function (res) {
      var chunks = [];
    ​
      res.on("data", function (chunk) {
        chunks.push(chunk);
      });
    ​
      res.on("end", function () {
        var body = Buffer.concat(chunks);
        console.log(body.toString());
      });
    });
    ​
    req.write(JSON.stringify({ depositCoinCode: 'BTC', receiveCoinCode: 'ETH' }));
    req.end();
````
## 创建订单接口示例
####java代码示例：
````
  OkHttpClient client = new OkHttpClient();
  ​
  MediaType mediaType = MediaType.parse("application/json");
  RequestBody body = RequestBody.create(mediaType, "{\"equipmentNo\":\"Zsda352943090468518\",\"sessionUuid\":\"\",\"sourceType\":\"ANDROID\",\"userNo\":\"\",\"orderId\":\"de752da3-0ff7-4682-8038-d8e1f20cad95\",\"depositCoinCode\":\"BTC\",\"receiveCoinCode\":\"ETH\",\"depositCoinAmt\":\"0.01\",\"receiveCoinAmt\":\"0.336585\",\"receiveSwftAmt\":\"18.06\",\"destinationAddr\":\"0x364397e2fc9929f11ba0c03826ef282dd64a829f\",\"refundAddr\":\"18orDLFMp3fGoy5Uk93LDGTGbxWEm7b7FY\",\"sourceFlag\":\"LendChain\",\"developerId\":\"\"}");
  Request request = new Request.Builder()
    .url("https://transfer.swft.pro/api/v2/accountExchange")
    .post(body)
    .addHeader("Content-Type", "application/json")
    .addHeader("cache-control", "no-cache")
    .build();
  ​
  Response response = client.newCall(request).execute();
````
#### C# 代码示例
````
  var client = new RestClient("https://transfer.swft.pro/api/v2/accountExchange");
  var request = new RestRequest(Method.POST);
  request.AddHeader("cache-control", "no-cache");
  request.AddHeader("Content-Type", "application/json");
  request.AddParameter("undefined", "{\"equipmentNo\":\"352943090468518\",\"sessionUuid\":\"\",\"sourceType\":\"ANDROID\",\"userNo\":\"\",\"orderId\":\"de752da3-0ff7-4682-8038-d8e1f20cad95\",\"depositCoinCode\":\"BTC\",\"receiveCoinCode\":\"ETH\",\"depositCoinAmt\":\"0.01\",\"receiveCoinAmt\":\"0.336585\",\"receiveSwftAmt\":\"18.06\",\"destinationAddr\":\"0x364397e2fc9929f11ba0c03826ef282dd64a829f\",\"refundAddr\":\"18orDLFMp3fGoy5Uk93LDGTGbxWEm7b7FY\",\"sourceFlag\":\"LendChain\",\"developerId\":\"\"}", ParameterType.RequestBody);
  IRestResponse response = client.Execute(request);
  ````
#### Objective-C 代码示例
````
  #import <Foundation/Foundation.h>
  ​
  NSDictionary *headers = @{ @"Content-Type": @"application/json",
                             @"cache-control": @"no-cache" };
  NSDictionary *parameters = @{ @"equipmentNo": @"Zsda352943090468518",
                                @"sessionUuid": @"",
                                @"sourceType": @"ANDROID",
                                @"userNo": @"",
                                @"orderId": @"de752da3-0ff7-4682-8038-d8e1f20cad95",
                                @"depositCoinCode": @"BTC",
                                @"receiveCoinCode": @"ETH",
                                @"depositCoinAmt": @"0.01",
                                @"receiveCoinAmt": @"0.336585",
                                @"receiveSwftAmt": @"18.06",
                                @"destinationAddr": @"0x364397e2fc9929f11ba0c03826ef282dd64a829f",
                                @"refundAddr": @"18orDLFMp3fGoy5Uk93LDGTGbxWEm7b7FY",
                                @"sourceFlag": @"LendChain",
                                @"developerId": @"" };
  ​
  NSData *postData = [NSJSONSerialization dataWithJSONObject:parameters options:0 error:nil];
  ​
  NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"https://transfer.swft.pro/api/v2/accountExchange"]
                                                         cachePolicy:NSURLRequestUseProtocolCachePolicy
                                                     timeoutInterval:10.0];
  [request setHTTPMethod:@"POST"];
  [request setAllHTTPHeaderFields:headers];
  [request setHTTPBody:postData];
  ​
  NSURLSession *session = [NSURLSession sharedSession];
  NSURLSessionDataTask *dataTask = [session dataTaskWithRequest:request
                                              completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                                                  if (error) {
                                                      NSLog(@"%@", error);
                                                  } else {
                                                      NSHTTPURLResponse *httpResponse = (NSHTTPURLResponse *) response;
                                                      NSLog(@"%@", httpResponse);
                                                  }
                                              }];
  [dataTask resume];
````
  ​
#### Nodejs代码示例
````
  var http = require("https");
  ​
  var options = {
    "method": "POST",
    "hostname": [
      "transfer",
      "swft",
      "pro"
    ],
    "path": [
      "api",
      "v2",
      "accountExchange"
    ],
    "headers": {
      "Content-Type": "application/json",
      "cache-control": "no-cache"
    }
  };
  ​
  var req = http.request(options, function (res) {
    var chunks = [];
  ​
    res.on("data", function (chunk) {
      chunks.push(chunk);
    });
  ​
    res.on("end", function () {
      var body = Buffer.concat(chunks);
      console.log(body.toString());
    });
  });
  ​
  req.write(JSON.stringify({ equipmentNo: 'Zsda3529430s90468518',
    sessionUuid: '',
    sourceType: 'ANDROID',
    userNo: '',
    orderId: 'de752da3-0ff7-4682-8038-d8e1f20cad95',
    depositCoinCode: 'BTC',
    receiveCoinCode: 'ETH',
    depositCoinAmt: '0.01',
    receiveCoinAmt: '0.336585',
    receiveSwftAmt: '18.06',
    destinationAddr: '0x364397e2fc9929f11ba0c03826ef282dd64a829f',
    refundAddr: '18orDLFMp3fGoy5Uk93LDGTGbxWEm7b7FY',
    sourceFlag: 'LendChain',
    developerId: '' }));
  req.end();
 ````
 ##查询订单状态接口示例
 #### java代码示例：
 ````
   OkHttpClient client = new OkHttpClient();
   ​
   MediaType mediaType = MediaType.parse("application/json");
   RequestBody body = RequestBody.create(mediaType, "{\n    \"equipmentNo\": \"Zasdf352943090468518\",\n   \n    \"sourceType\": \"ANDROID\",\n\n    \"orderId\": \"de752da3-0ff7-4682-8038-d8e1f20cad95\"\n}");
   Request request = new Request.Builder()
     .url("https://transfer.swft.pro/api/v2/queryOrderState")
     .post(body)
     .addHeader("Content-Type", "application/json")
     .addHeader("cache-control", "no-cache")
     .build();
   ​
   Response response = client.newCall(request).execute();
   ````
 #### C# 代码示例
 ````
   var client = new RestClient("https://transfer.swft.pro/api/v2/queryOrderState");
   var request = new RestRequest(Method.POST);
   request.AddHeader("cache-control", "no-cache");
   request.AddHeader("Content-Type", "application/json");
   request.AddParameter("undefined", "{\n    \"equipmentNo\": \"Zasdf352943090468518\",\n   \n    \"sourceType\": \"ANDROID\",\n\n    \"orderId\": \"de752da3-0ff7-4682-8038-d8e1f20cad95\"\n}", ParameterType.RequestBody);
   IRestResponse response = client.Execute(request);
   ````
 ####Objective-C 代码示例
 ````
   #import <Foundation/Foundation.h>
   ​
   NSDictionary *headers = @{ @"Content-Type": @"application/json",
                              @"cache-control": @"no-cache" };
   NSDictionary *parameters = @{ @"equipmentNo": @"Zasdf352943090468518",
                                 @"sourceType": @"ANDROID",
                                 @"orderId": @"de752da3-0ff7-4682-8038-d8e1f20cad95" };
   ​
   NSData *postData = [NSJSONSerialization dataWithJSONObject:parameters options:0 error:nil];
   ​
   NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"https://transfer.swft.pro/api/v2/queryOrderState"]
                                                          cachePolicy:NSURLRequestUseProtocolCachePolicy
                                                      timeoutInterval:10.0];
   [request setHTTPMethod:@"POST"];
   [request setAllHTTPHeaderFields:headers];
   [request setHTTPBody:postData];
   ​
   NSURLSession *session = [NSURLSession sharedSession];
   NSURLSessionDataTask *dataTask = [session dataTaskWithRequest:request
                                               completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                                                   if (error) {
                                                       NSLog(@"%@", error);
                                                   } else {
                                                       NSHTTPURLResponse *httpResponse = (NSHTTPURLResponse *) response;
                                                       NSLog(@"%@", httpResponse);
                                                   }
                                               }];
   [dataTask resume];
   ````
 #### Nodejs代码示例
 ````
   var http = require("https");
   ​
   var options = {
     "method": "POST",
     "hostname": [
       "transfer",
       "swft",
       "pro"
     ],
     "path": [
       "api",
       "v2",
       "queryOrderState"
     ],
     "headers": {
       "Content-Type": "application/json",
       "cache-control": "no-cache"
     }
   };
   ​
   var req = http.request(options, function (res) {
     var chunks = [];
   ​
     res.on("data", function (chunk) {
       chunks.push(chunk);
     });
   ​
     res.on("end", function () {
       var body = Buffer.concat(chunks);
       console.log(body.toString());
     });
   });
   ​
   req.write(JSON.stringify({ equipmentNo: 'Zasdf352943090468518',
     sourceType: 'ANDROID',
     orderId: 'de752da3-0ff7-4682-8038-d8e1f20cad95' }));
   req.end();
   ````