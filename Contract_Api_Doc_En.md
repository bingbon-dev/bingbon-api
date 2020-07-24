Bingbon Exchange Contract Official API documentation
==================================================
[Bingbon][]Developer documentation([English Docs][])。

<!-- TOC -->

- [Introduction](#Introduction)
- [Contract Business API Reference](#Contract Business API Reference)
    - [Contract Market API](#Contract Market API)
        - [1. Get All the Information of Contract Trading Pairs](#1-Get All the Information of Contract Trading Pairs)
        - [2. Get Trading Depths of Contract Trading Pairs](#2-Get Trading Depths of Contract Trading Pairs)
        - [3. Get Trading Information of Cryptocurrency Pairs](#3-Get Trading Information of Cryptocurrency Pairs)
        - [4. Get K-Line Information of Cryptocurrency Pairs](#4-Get K-Line Information of Cryptocurrency Pairs)

<!-- /TOC -->

# Introduction

Welcome to Use [Bingbon][] Development Document

This document provides an introduction to the usage of related APIs, such as contract trading and market enquiry.
The market API provides a public market data interface for the market, such as obtaining all Cryptocurrency Pairs and their corresponding in-depth information.



## Request Interaction

Root URL for REST access：`https://api.bingbon.com`


### Request

All requests are based on the HTTPS protocol, and the Content-Type in the request header information needs to be uniformly set to:'application/json'.


**Request Interaction Description**

1、Request Parameters: encapsulate parameters according to the interface request parameters.

2、Submit Request Parameters: submit the encapsulated request parameters to the server through POST/GET/DELETE, etc.

3、Server Response: The server first performs parameter security verification on the user request data, and returns the response data to the user in JSON format according to the business logic after the verification

4、Data Processing: processing the server response data.

**Success**

HTTP status code 200 indicates a successful response and may contain content. If the response contains content, it will be displayed in the corresponding return content.


**Common HTTP Error Codes**

* 4XX error codes are used to indicate wrong request content, behavior, format.

* 5XX error codes are used to indicate problems on the Bingbon service.

* 400 Bad Request – Invalid Request Format

* 401 Unauthorized – Invalid API Key

* 403 Forbidden – You do not have access to the requested resource

* 404 Not Found Error

* 429 Too Many Requests - Return code is used when breaking a request rate limit.

* 418 return code is used when an IP has been auto-banned for continuing to send requests after receiving 429 codes.

* 500 Internal Server Error – We had a problem with our server 

* 504 return code means that the API server has submitted a request to the business core but failed to get a response. It should be noted that the 504 return code does not mean that the request failed, but is unknown. It may have been executed, or it may fail, and further confirmation is required.


* If it fails, the response body will contain an error description


* Every interface may throw exceptions


## Standard Specification

### Timestamp

Unless otherwise specified, all timestamps in the API are returned in microseconds.

The timestamp of the request must be within 30 seconds of the API service time, otherwise the request will be considered expired and rejected. If there is a large deviation between the local server time and the API server time, we recommend that you update the HTTP header by querying the API server time.

### Example

1587091154123

### Numbers

In order to maintain the integrity of precision across platforms, decimal numbers are returned as strings. It is recommended that you also convert numbers to strings when making a request to avoid truncation and precision errors.

Integers (such as transaction number and sequence) are used without quotation marks.

### Rate Limit

If the requests are too frequent, the system will automatically limit the requests.


##### REST API

* Market Interface：We restrict the call of the public interface by IP, up to 10 requests per 1 second.

* Account and Transaction Interface: We restrict the call of the private interface by user ID, up to 10 requests per 1 second.

* The special restrictions of some interfaces are indicated on the specific interface


# Contract Business API Reference

## Contract Market API

### 1. Get All the Information of Contract Trading Pairs

**HTTP Request**
```http
    GET api/v1/market/symbols
    
    example： https://api.bingbon.com/api/v1/market/symbols
```

**Response**
```javascript
    {
    "code":0,
    "timestamp":1594209784093,
    "data":{
        "result":[
            {
                "base_currency":"BTC",
                "quote_currency":"USDT",
                "last_price":9317.24,
                "base_volume":14145.34,
                "quote_volume":131795454.1,
                "bid":9318.56,
                "ask":9315.55,
                "high":9329.69,
                "low":9204.33,
                "product_type":"Futures",
                "open_interest":0,
                "index_price":9317.24,
                "creation_timestamp":0,
                "expiry_timestamp":0,
                "fundint_rate":0.00012,
                "next_funding_rate_timestamp":1594224000000,
                "contract_type":"Vanilla",
                "contract_price":9317.24,
                "contract_price_currency":"USDT"
            }]
        }
    }
```



**Return Value Description**  


|Return Field | Description |
| ---------- |:-------:|
| code       | For error messages, 0 means normal|
| msg        |  Error message description |
| base_currency | Base Cryptocurrency name of Cryptocurrency Pairs |
| quote_currency     | Denominated Cryptocurrency name of Cryptocurrency Pairs |
| last_price       | Latest price of Cryptocurrency Pairs |
| base_volume       | Calculate trading volume in base Cryptocurrency within 24 hours |
| quote_volume  | Calculate trading volume in quoted Cryptocurrency within 24 hours |
| bid   | Highest bid price |
| ask   | Lowest selling price |
| high  | Today‘s highest price |
| low   | Today's lowest price |
| product_type      | Product type. The default type is Futures |
| open_interest     | Position size |
| index_price  | Index price, is equivalent to the latest price of the currency pair|
| creation_timestamp   | Start time. Reserved field.Not used yet |
| expiry_timestamp     | Expiration time. Reserved field.Not used yet |
| high  | Today's highest price |
| low   | Today's lowest price |
| funding_rate      | Funding rate |
| next_funding_rate_timestamp           | Timestamp for the next funding rate |
| contract_type       | ContractType.Vanilla -- Vanilla Contract, Inverse -- Inverse Contract|
| contract_price   | Contract Price.is now equal to the latest price|
| contract_price_currency      | Denominated currency name of Contract |

### 2. Get Trading Depths of Contract Trading Pairs

    Get the list of requests for the depth of the market.

**HTTP Request**
```http
    GET api/v1/market/orderbook/{symbol}?depth=10
```

**Request Parameters**  

| Parameter name | Type | Mandatory | Description |
| ------------- |------------------|--------|--------|
| symbol | String | YES | Trading pair symbol , like BTCUSDT |
| depth | Integer | YES | Depth Level.The default level is 10 |

**Response**
```javascript
    {
        "code":0,
        "timestamp":1594211453394,
        "data":{
            "ticker_id":"BTCUSDT",
            "timestamp":1594211453394,
            "bids":[[
                9295,
                1.7052
            ],
            [
                9294.9,
                0.8564
            ]...]
            "asks":[[
                9297,
                0.1613
            ],
            [
                9297.4,
                0.7363
            ]...]
        }
    }
```
**Return Value Description**  

|Return Field| Description|  
| ------------- |------------|
| code   | For error messages, 0 means normal |
| msg    | Error message description |
| asks   | Sell side depth, whose value is a two-dimensional array, the first in the array is the price of the Cryptocurrency Pairs, and the second is the quantity |
| bids   | Buy side depth, its value is a two-dimensional array, the first in the array is the price of the currency pair, and the second is the quantity |
| ticker_id      |  Cryptocurrency Pairs  | String |
| timestamp      |  Timestamp | Long |

### 3. Get Trading Information of Cryptocurrency Pairs

**HTTP Request**
```http
    GET api/v1/market/trade/{symbool}
    
    example： https://api.bingbon.com/api/v1/market/trade/BTCUSDT?maxCount=20
```

**Parameters**
| Parameter name | Type | Mandatory | Description |
| ------------- |------------------|--------|--------|
| symbol | String | YES | Trading pair symbol , like BTCUSDT |
| maxCount | Integer | YES | The nubmer of lateset trading data.The default number is 10，The maximum number is 50 |

**Response**
```javascript
    {
    "code": 0,
    "timestamp": 1594720385045,
    "data": {
        "result": [
            {
                "tradeId": 567868,
                "tradePrice": 9199.32000000000000000000,
                "direction": 0,
                "volume": 0.01087036,
                "createTime": 1594717863000
            }
        ]
    }
}
```


**Return Value Description**  

|Return Field|Description|  
| ------------- |------------|
| code   | For error messages, 0 means normal |
| msg    | Error message description |
| tradeId | Trade ID |
| tradePrice   | Trade Price |
| direction    | Direction.：0-buy  1-sell |
| volume  | Order volume |
| createTime   | Closing Time |

### 4. Get K-Line Information of Cryptocurrency Pairs

**HTTP Request**
```http
    GET api/v1/market/kline/{symbool}
    
    example： https://api.bingbon.com/api/v1/market/kline/BTCUSDT?period={period}&pagingSize={pagingSize}&startId={startId}
```

**Parameters**
| Parameter name | Type | Mandatory | Description |
| ------------- |------------------|--------|--------|
| symbol | String | YES | Trading pair symbol , like BTCUSDT |
| period | String | YES | K-line type：1min, 5min, 15min, 30min, 60min, 4hour, 1day, 1sec |
| pagingSize | Integer | YES | Number per page |
| startId | String | NO | Paging start id.The result of statdate is less than startId, like: 2018-09-20T00:00:00.000+0800. If it is polling, the first polling doesn't transmit pagingSize and startId. the last statDate is returned by startId after the second polling |

**Response**
```javascript
    {
    "code": 0,
    "timestamp": 1594715787766,
    "data": {
        "klineInfosVo": [
            {
                "statDate": "2020-07-14T00:00:00.000+0800",
                "open": 9290.35,
                "close": 9208.31,
                "low": 9149.09,
                "high": 9321.50,
                "amount": 170260728.99,
                "volume": 97688273.13,
                "count": 376599,
            }
        ],
        "period": "1day",
        "name": "BTCUSDT"
    }
}
```


**Return Value Description**  

|Return Field|Field Description|  
| ------------- |------------|
| code   | For error messages, 0 means normal |
| msg    | Error message description |
| statDate | Time dimension of statistics, the format is 2018-09-20T00:00:00.000+0800，yyyy-MM-dd'T'HH:mm:ss.SSSZ |
| open       | open |
| close       | close |
| low  | low |
| high   | high |
| amount   | Trading amount in quoted Cryptocurrency |
| volume   | Trading volume of  base Cryptocurrency |
| count    | Trade count |

**Remark**

         
    For more return error codes, please see the error code description on the homepage

    
[Bingbon]: https://bingbon.pro
[English Docs]: https://bingbon.pro
[Unix Epoch]: https://en.wikipedia.org/wiki/Unix_time
