# Interface
edited this page on 2019.03.21

## Interface
Interface type   |   request method   | description | example
----------  | -------   | ----- | -------
GET/POST application/x-www-form-urlencoded |  [api/v1/queryCoinList](#Token List)  | Token List | [example](#Token List example)
POST application/json |  [api/v1/getBaseInfo](#Obtain exchange rate basic information)| Obtain exchange rate basic information | [example](#Obtain exchange rate basic information example)
POST application/json | [api/v2/accountExchange](#Create Order) | Create Order | [example](#Create Order example)
POST application/json | [api/v2/queryOrderState](#Check order status) | Check order status | [example](#Check order status example)


## Token List
### api/v1/queryCoinList
###  introduction
Provide token types list to display users, tell user which tokens can be traded.
### request parameter
Parameter name  | essential or not	 | data type | description | default | range
-------  | ------- | ------- | ---- | ----- | --------
supportType | no | String | supportType | | advanced
### respond parameter
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
      "resMsg": "success"
  }
````
### data introduction
````
        {
            "coinAllCode": Coin Fullname,
            "coinCode": Coin Code,
            "coinImageUrl": Coin LOGO,
            "coinName": Coin Name,
            "contact": The sole name specified on the mainnent, contract address or contract name,
            "isSupportAdvanced": Whether supports cross-chain,
            "mainNetwork": Which networks due the tokens belong to (Mainnet's Name),
            "noSupportCoin": Which networks due the tokens belong to (Mainnet's Name)
        }
````

## Obtain exchange rate basic information
### api/v1/getBaseInfo
###  introduction 
Provide the exchange rate between two tokens, the exchange rate update rate is 5-10s​
### request parameter
Parameter name  | essential or not	 | data type | description | default | range
-------  | ------- | ------- | ---- | ----- | --------
depositCoinCode | yes | String | Deposit Coin Code | | See Coin Code Table
receiveCoinCode | yes | String | Receive Coin Code | | See Coin Code Table
### respond parameter
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
    "resMsg": "success"
}
````
### data introduction
````
  {
        "depositMax": Max Deposit Amount,
        "depositMin": Min Deposit Amount,
        "instantRate": Instant Rate,
        "minerFee": Service Fee,
        "receiveCoinFee": Transfer Fee Amount
    }
````
## Create Order
### api/v2/accountExchange
###  introduction 
Create order that has ip frequency limits, can't have more than 5 attempts within 2 second
### request parameter
Parameter name  | essential or not	 | data type | description | default | range
-------  | ------- | ------- | ---- | ----- | --------
depositCoinCode | yes | String | Deposit Coin Code  | | See Coin Code Table
receiveCoinCode | yes | String | Receive Coin Code  | | See Coin Code Table
depositCoinAmt | yes | String | Deposit Coin Amount   | | 
receiveCoinAmt | yes | String | Receive Coin Amount | | receiveCoinAmt = depositCoinAmt * instantRate 
receiveSwftAmt | yes | String | Receive SWFTC Amount | | 
destinationAddr | yes | String | Destination Address | | Destination Address
soureFlag | no | String | Order creation source | | Used to mark which platform creates orders, needs to consult with SWFT Blockchain for setup
developerId | no | String | Developer order id | | Developer id, order numbers passed on by other platforms，项目方可用该字段来表示该订单归属于自己的某个用户或用于记录自己系统内的订单编号，或其他编号；在订单创建完成后，会回传该字段值（SWFT不支持通过该字段查询寻订单信息）
refundAddr | yes | String | Refund Address | | 
equipmentNo | yes | String | Device id | | Unique id for device.
sourceType | yes | String | Device Type | | ANDROID,IOS,H5
userNo | no | String | user's phone/email | | user's phone /email
sessionUuid | no| String | Login SessionUuid | | Login SessionUuid
### respond parameter
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
      "resMsg": "success"
  }
````
### data introduction
````
    {
          "changeType": Transfer method（simple,advanced）,
          "choiseFeeType": Service Fee Type（3-origin coin，4-swftc）,
          "depositCoinAmt": Deposit Coin Amount ,
          "depositCoinCode": Deposit Coin Code ,
          "depositCoinFeeAmt": Deposit Coin Fee Amount,
          "depositCoinFeeRate": Deposit Coin Fee Rate,
          "depositCoinState": Deposit Coin Status,
          "destinationAddr": Deposit Address,
          "detailState": Detailed Status（"(1)wait_deposit_send: wait for depositing coin；
                              (2)wait_fee_type: wait for determine fee type；
                              (3)timeout: timeout；
                              (4)wait_exchange_push: wait for pushing to exchange；
                              (5)wait_exchange_return: wait for exchange return；
                              (6.1)wait_receive_send: wait for received coin , wait_receive_confirm: wait for received coin confirm, receive_complete: complete.
                              (6.2.1)wait_refund_send: wait for refund wait_refund_confirm: wait for refund confirm, refund_complete: refund complete；"）,
          "developerId": Developer order id,
          "orderId": Order id,
          "orderState": Order Status,
          "platformAddr": Deposit Address,
          "receiveCoinAmt": Receive Coin Amount,
          "receiveCoinCode": Receive Coin Code ,
          "receiveSwftAmt": Receive SWFTC Amount,
          "refundAddr": Origin Coin Refund Address,
          "refundCoinAmt": Refund Coin Amount,
          "refundCoinMinerFee": Miner Fee,
          "refundDepositTxid": Unsuccessful transfer id,
          "refundSwftAmt": Refund Swft Amount,
          "swftCoinFeeRate": Deposit Coin Fee Rate,
          "swftCoinState": Swft Coin State,
          "swftReceiveAddr": Swft Receive Address,
          "swftRefundAddr": Swft Refund Address,
          "transactionId": swftRefundAddr
      }
````

## Check order status
### api/v2/queryOrderState
### request parameter
Parameter name  | essential or not	 | data type | description | default | range
-------  | ------- | ------- | ---- | ----- | --------
orderId | yes | String | swftRefundAddr | | eg：1fc8499f-dd6d-4ff3-8b7f-7a0d74c59adc
equipmentNo | yes | String | Device id | | Unique id for device.
sourceType | yes | String | Device Type | | ANDROID,IOS,H5
userNo | no | String | user's phone/email | | user's phone /email
sessionUuid | no| String | Login SessionUuid | | Login SessionUuid
### respond parameter
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
      "resMsg": "success"
  }
  ````
### data introduction
````
    {
          "changeType": Transfer method（simple,advanced）,
          "choiseFeeType": Service Fee Type（3-origin coin，4-swftc）,
          "depositCoinAmt": Deposit Coin Amount ,
          "depositCoinCode": Deposit Coin Code ,
          "depositCoinFeeAmt": Deposit Coin Fee Amount,
          "depositCoinFeeRate": Deposit Coin Fee Rate,
          "depositCoinState": Deposit Coin Status,
          "destinationAddr": Deposit Address,
          "detailState": Detailed Status（"(1)wait_deposit_send: wait for depositing coin；
                       (2)wait_fee_type: wait for determine fee type；
                       (3)timeout: timeout；
                       (4)wait_exchange_push: wait for pushing to exchange；
                       (5)wait_exchange_return: wait for exchange return；
                       (6.1)wait_receive_send: wait for received coin , wait_receive_confirm: wait for received coin confirm, receive_complete: complete.                                 
          "orderId": Order id,
          "orderState": Order Status,
          "platformAddr": Deposit Address,
          "receiveCoinAmt": Receive Coin Amount,
          "receiveCoinCode": Receive Coin Code ,
          "receiveSwftAmt": Receive SWFTC Amount,
          "refundAddr": Origin Coin Refund Address,
          "refundCoinAmt": Refund Coin Amount,
          "refundCoinMinerFee": Miner Fee,
          "refundDepositTxid": Unsuccessful transfer id,
          "refundSwftAmt": Refund Swft Amount,
          "swftCoinFeeRate": Deposit Coin Fee Rate,
          "swftCoinState": Swft Coin State,
          "swftReceiveAddr": Swft Receive Address,
          "swftRefundAddr": Swft Refund Address,
          "tradeState": ,
          "transactionId": swftRefundAddr
      }
````

# example
##Token List example

#### java example：
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
#### C# example
````
  var client = new RestClient("https://transfer.swft.pro/api/v1/queryCoinList");
  var request = new RestRequest(Method.POST);
  request.AddHeader("cache-control", "no-cache");
  request.AddHeader("Content-Type", "application/x-www-form-urlencoded");
  request.AddParameter("undefined", "supportType=advanced&undefined=", ParameterType.RequestBody);
  IRestResponse response = client.Execute(request);
  ````
#### Objective-C example
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
#### Nodejs example
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
  ## Obtain exchange rate basic information example
 #### java example：
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
  #### C# example
  ````
    var client = new RestClient("https://transfer.swft.pro/api/v1/getBaseInfo");
    var request = new RestRequest(Method.POST);
    request.AddHeader("cache-control", "no-cache");
    request.AddHeader("Content-Type", "application/json");
    request.AddParameter("undefined", "{\"depositCoinCode\":\"BTC\",\"receiveCoinCode\":\"ETH\"}", ParameterType.RequestBody);
    IRestResponse response = client.Execute(request);
 ````
 #### Objective-C example
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
####  Nodejs example
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
## Create Order example
####java example：
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
#### C# example
````
  var client = new RestClient("https://transfer.swft.pro/api/v2/accountExchange");
  var request = new RestRequest(Method.POST);
  request.AddHeader("cache-control", "no-cache");
  request.AddHeader("Content-Type", "application/json");
  request.AddParameter("undefined", "{\"equipmentNo\":\"352943090468518\",\"sessionUuid\":\"\",\"sourceType\":\"ANDROID\",\"userNo\":\"\",\"orderId\":\"de752da3-0ff7-4682-8038-d8e1f20cad95\",\"depositCoinCode\":\"BTC\",\"receiveCoinCode\":\"ETH\",\"depositCoinAmt\":\"0.01\",\"receiveCoinAmt\":\"0.336585\",\"receiveSwftAmt\":\"18.06\",\"destinationAddr\":\"0x364397e2fc9929f11ba0c03826ef282dd64a829f\",\"refundAddr\":\"18orDLFMp3fGoy5Uk93LDGTGbxWEm7b7FY\",\"sourceFlag\":\"LendChain\",\"developerId\":\"\"}", ParameterType.RequestBody);
  IRestResponse response = client.Execute(request);
  ````
#### Objective-C example
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
#### Nodejs example
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
 ##Check order status example
 #### java example：
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
 #### C# example
 ````
   var client = new RestClient("https://transfer.swft.pro/api/v2/queryOrderState");
   var request = new RestRequest(Method.POST);
   request.AddHeader("cache-control", "no-cache");
   request.AddHeader("Content-Type", "application/json");
   request.AddParameter("undefined", "{\n    \"equipmentNo\": \"Zasdf352943090468518\",\n   \n    \"sourceType\": \"ANDROID\",\n\n    \"orderId\": \"de752da3-0ff7-4682-8038-d8e1f20cad95\"\n}", ParameterType.RequestBody);
   IRestResponse response = client.Execute(request);
   ````
 ####Objective-C example
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
 #### Nodejs example
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