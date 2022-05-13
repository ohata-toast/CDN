## Content Delivery > CDN > API v2.0 Guide

This document describes Public API v2.0 of NHN Cloud CDN.

## Common API Information

### Domain

| Name              | Domain                                   |
| --------------- | ------------------------------------- |
| CDN Public API Domain | https://kr1-cdn.api.nhncloudservice.com |

### Prerequisites

To use the API, you need Appkey and SecretKey.
The Appkey and SecretKey can be found in the **URL & Appkey** menu in the upper right corner of the console.

### Common Request Information

#### Request Header

| Name            | Description                        |
| ------------- | ------------------------- |
| Authorization | SecretKey issued from the console |

#### Path Parameter

In all APIs, the appKey must be specified in the path parameter.
* Example: /v2.0/appKeys/**{appKey}**/distributions

| Name     | Description                    |
| ------ | --------------------- |
| appKey | Appkey issued from the console |

### Common Response Information

#### Header

The API responds with **200 OK** to all API requests. For more details, see the header at the response body as below:

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "Success"
    }
}
```


[Field]

| Field                   | Type      | Description     |
| -------------------- | ------- | ------ |
| header               | Object  | Header area  |
| header.isSuccessful  | Boolean | Successful or not  |
| header.resultCode    | Integer | Result code  |
| header.resultMessage | String  | Result message |

#### CDN Status Codes

The following shows the status codes of CDN service, which are available at the query of service.

| Value         | Description                     |
| ---------- | ------------------------ |
| OPENING    | Starting service           |
| OPEN       | In service                |
| MODIFYING  | Under modification                  |
| RESUME     | Resumed                     |
| SUSPENDING | Under suspension             |
| SUSPEND    | Suspended                     |
| CLOSING    | Closing service             |
| CLOSE      | Closed                |
| ERROR      | Error occurred while creating service |


## Service API

### Create a Service

#### Request


[URI]

| Method  | URI                                  |
| ---- | ------------------------------------ |
| POST | /v2.0/appKeys/{appKey}/distributions |


[Request Body]

```json
{
    "distributions" : [
    {
      "useOriginHttpProtocolDowngrade": false,
      "forwardHostHeader": "ORIGIN_HOSTNAME",
      "domainAlias": ["alias.test.net"],
      "description" : "sample-cdn",
      "useOriginCacheControl" : false,
      "defaultMaxAge": 86400,
      "cacheKeyQueryParam": "INCLUDE_ALL",
      "referrerType" : "BLACKLIST",     
      "referrers" : ["cloud.nhn.com"],
      "isAllowWhenEmptyReferrer" : true,
      "origins" : [
        {
          "origin" : "static.origin.com",
          "originPath" : "/resources",
          "httpPort": 80,
          "httpsPort": 443
        }
      ],
      "rootPathAccessControl" : {
          "enable": true,
          "controlType": "REDIRECT",
          "redirectPath": "/default.png",
          "redirectStatusCode": 302
      },
      "callback": {
          "httpMethod": "GET",
          "url": "http://test.callback.com/cdn?=appKey={appKey}&status={status}&domain={domain}"
      }
    }
  ]
}
```

[Field]

| Name                                   | Type    | Required | Default | Valid Range                   | Description                                                         |
| -------------------------------------- | ------- | --------- | ------ | --------------------------- | ------------------------------------------------------------ |
| distributions                          | List    | Required      |        |                              | List of CDN objects to create                                   |
| distributions[0].useOriginHttpProtocolDowngrade | Boolean  | Required     | false       | true/false         | Whether to enable settings to downgrade a request from HTTPS to HTTP when the request is made to origin server from CDN server, if the origin server can respond only via HTTP |
| distributions[0].forwardHostHeader     | String  | Required      |   | ORIGIN_HOSTNAME<br/>REQUEST_HOST_HEADER   | Set the host header to be forwarded by the CDN server when requesting content to the origin server ("ORIGIN_HOSTNAME": Set to the host name of the origin server, "REQUEST_HOST_HEADER": Set to the host header of the client request)|
| distributions[0].useOriginCacheControl | Boolean | Required      |        | true/false                  | Cache expiration setting (true: Use the origin server setting, false: User-configured setting)   |
| distributions[0].referrerType          | String  | Required      |        | BLACKLIST/WHITELIST         | Referrer access management ("BLACKLIST": Blacklist, "WHITELIST": Whitelist) |
| distributions[0].referrers             | List    | Optional      |        |                           | List of regex referrer headers   |
| distributions[0].isAllowWhenEmptyReferrer | Boolean | Optional      | true      | true/false             | Whether to allow (true) or deny (false) access to content when there is no referer header             |
| distributions[0].description           | String  | Optional      |        | Up to 255 characters                  | Description                                                         |
| distributions[0].domainAlias           | List    | Optional      |        |                           | List of domain aliases (using domains owned by individuals or companies) |
| distributions[0].defaultMaxAge         | Integer | Optional      | 0      | 0~2,147,483,647             | Cache expiration time (seconds), the default value 0 is 604,800 seconds.             |
| distributions[0].cacheKeyQueryParam    | String  | Optional      | INCLUDE_ALL | INCLUDE_ALL/EXCLUDE_ALL | Set whether to include the request query string in the cache key ("INCLUDE_ALL": Include all, "EXCLUDE_ALL": Exclude all) |
| distributions[0].origins               | List    | Required      |        |                             | List of origin server objects                                      |
| distributions[0].origins[0].origin     | String  | Required      |        | Up to 255 characters                  | Origin server (domain or IP)                                     |
| distributions[0].origins[0].originPath | String  | Optional      |        | Up to 8192 characters                 | Sub-path of the origin server (Enter a path including /.)        |
| distributions[0].origins[0].httpPort   | Integer  | Optional      |        | See '[Table 2] Available Origin Server Port Numbers' of [Console User Guide > Origin Server](./console-guide/#origin-server) | HTTP protocol port of the origin server (one of origins[0].httpPort and origins[0].httpsPort must be entered.)  |
| distributions[0].origins[0].httpsPort  | Integer  | Optional      |        | See '[Table 2] Available Origin Server Port Numbers' of [Console User Guide > Origin Server](./console-guide/#origin-server) | HTTPS protocol port of the origin server (one of origins[0].httpPort and origins[0].httpsPort must be entered.) |
| distributions[0].rootPathAccessControl  | Object  | Optional      |        |                             | Set the access control for the CDN service root path | 
| distributions[0].rootPathAccessControl.enable | Boolean | Required      | true      | true/false             | Whether the access control for the root path is enabled (true) or disabled (false)          |
| distributions[0].rootPathAccessControl.controlType  | String  | Optional      |        | DENY, REDIRECT    | Required if enable is true. The access control method for the root path. ("DENY": deny access, "REDIRECT": redirect to the specified path) | 
| distributions[0].rootPathAccessControl.redirectPath | String | Optional      |       |       |   Required if controlType is "REDIRECT". The path to redirect requests for the root path to. (Enter a path including /.)        |
| distributions[0].rootPathAccessControl.redirectStatusCode | Integer | Optional      |       | 301, 302, 303, 307             |  Required when controlType is "REDIRECT". The HTTP response code sent when redirecting.          |
| distributions[0].callback              | Object  | Optional      |        |                             | Callback URL to receive the processing result of CDN creation (callback setting is optional.) |
| distributions[0].callback.httpMethod   | String  | Required      |        | GET/POST/PUT                | HTTP method of callback                                           |
| distributions[0].callback.url          | String  | Required      |        | Up to 1024 characters                 | Callback URL                                                     |

- The default value of forwardHostHeader is REQUEST_HOST_HEADER if domainAlias is set, or ORIGIN_HOSTNAME otherwise. 



#### Response


[Response Body]

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "distributions": [
        {
            "domain": "djwbjvqa.toastcdn.net",
            "domainAlias": [
                "alias.test1.net"
            ],
            "region": "GLOBAL",
            "description": "sample-cdn",
            "status": "OPENING",
            "defaultMaxAge": 0,
            "cacheKeyQueryParam": "INCLUDE_ALL",
            "referrerType": "BLACKLIST",
            "referrers": [
                "cloud.nhn.com"
            ],
            "isAllowWhenEmptyReferrer" : true,
            "useOriginCacheControl": true,
            "origins": [
                {
                    "origin": "static.origin.com",
                    "originPath": "/resources",
                    "httpPort": 80,
                    "httpsPort": 443
                }
            ],
            "forwardHostHeader": "ORIGIN_HOSTNAME",
            "useOriginHttpProtocolDowngrade": false,
            "rootPathAccessControl" : {
                "enable": true,
                "controlType": "REDIRECT",
                "redirectPath": "/default.png",
                "redirectStatusCode": 302
            },
            "callback": {
                "httpMethod": "GET",
                "url": "http://test.callback.com/cdn?=appKey={appKey}&status={status}&domain={domain}"
            }
        }
    ]
}
```


[Field]

| Field                                   | Type    | Description                                                         |
| -------------------------------------- | ------- | ------------------------------------------------------------ |
| header                                 | Object  | Header area                                                    |
| header.isSuccessful                    | Boolean | Successful or not                                                    |
| header.resultCode                      | Integer | Result code                                                    |
| header.resultMessage                   | String  | Result message                                                  |
| distributions                          | List    | List of created CDN objects                                   |
| distributions[0].domain                | String  | Name of the created domain (service)                                   |
| distributions[0].domainAlias           | List    | List of domain aliases (using domains owned by individuals or companies)              |
| distributions[0].region                | String  | Service region ("GLOBAL": Global service)            |
| distributions[0].description           | String  | Description                                                         |
| distributions[0].status                | String  | CDN status code (See [Table] CDN Status Code)                                 |
| distributions[0].defaultMaxAge         | Integer  | Cache expiration time (seconds)                                           |
| distributions[0].cacheKeyQueryParam    | String  | Set whether to include the request query string in the cache key ("INCLUDE_ALL": Include all, "EXCLUDE_ALL": Exclude all) |
| distributions[0].referrerType          | String  | Referrer access management ("BLACKLIST": Blacklist, "WHITELIST": Whitelist) |
| distributions[0].referrers             | List    | List of regex referrer headers                                  |
| distributions[0].isAllowWhenEmptyReferrer | Boolean | Whether to allow (true) or deny (false) access to content when there is no referer header |
| distributions[0].useOriginCacheControl | Boolean  | Whether to use origin server setting or not (true: Use the origin server setting, false: User-configured setting) |
| distributions[0].origins               | List    | List of origin server objects                                      |
| distributions[0].origins[0].origin     | String  | Origin server (domain or IP)                                      |
| distributions[0].origins[0].originPath | String  | Sub-path of origin server                                          |
| distributions[0].origins[0].httpPort   | Integer | HTTP protocol port of origin server                                               |
| distributions[0].origins[0].httpsPort  | Integer | HTTPS protocol port of origin server                                               |
| distributions[0].useOriginHttpProtocolDowngrade | Boolean | Whether to enable settings to downgrade a request from HTTPS to HTTP when the request is made to origin server from CDN server, if the origin server can respond only via HTTP |
| distributions[0].forwardHostHeader     | String  | Set the host header to be forwarded by the CDN server when requesting content to the origin server ("ORIGIN_HOSTNAME": Set to the host name of the origin server, "REQUEST_HOST_HEADER": Set to the host header of the client request) |
| distributions[0].rootPathAccessControl  | Object  | Set the access control for the CDN service root path | 
| distributions[0].rootPathAccessControl.enable | Boolean | Whether the access control for the root path is enabled (true) or disabled (false)          |
| distributions[0].rootPathAccessControl.controlType  | String  | Required if enable is true. The access control method for the root path. ("DENY": deny access, "REDIRECT": redirect to the specified path) | 
| distributions[0].rootPathAccessControl.redirectPath | String | Required if controlType is "REDIRECT". The path to redirect requests for the root path to. (Enter a path including /.)        |
| distributions[0].rootPathAccessControl.redirectStatusCode | Integer | Required when controlType is "REDIRECT". The HTTP response code sent when redirecting.          |
| distributions[0].callback              | Object  | Callback to receive service creation result                        |
| distributions[0].callback.httpMethod   | String  | HTTP method of callback                                           |
| distributions[0].callback.url          | String  | Callback URL                                                     |



### Query a Service

#### Request


[URI]

| Method  | URI                                  |
| ---- | ------------------------------------ |
| GET  | /v2.0/appKeys/{appKey}/distributions |


[Parameter]

| Name   | Type   | Required | Valid Range     | Description                         |
| ------ | ------ | --------- | ------------- | ---------------------------- |
| domain | String | Optional      | Up to 255 characters    | Domain to query (service name)   |
| status | String | Optional      | CDN Status Codes | CDN status code (See [Table] CDN Status Code) |

[Example]
```
curl -X GET "https://kr1-cdn.api.nhncloudservice.com/v2.0/appKeys/{appKey}/distributions?domain={domain}" \
 -H "Authorization: {secretKey}" \
 -H "Content-Type: application/json"
```

#### Response


[Response Body]

```json
{
    "header" : {
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS",
      "isSuccessful" :  true
    },
    "domain" :  "lhcsxuo0.toastcdn.net",
    "domainAlias" :  ["test.domain.com"],
    "region" :  "GLOBAL",
    "status" : "OPEN",
    "defaultMaxAge" : 86400,
    "cacheKeyQueryParam": "INCLUDE_ALL",
    "status" :  "OPENING",
    "referrerType" :  "BLACKLIST",
    "referrers" :  ["test.com"],    
    "useOriginCacheControl" :  false,
    "origins" : [
        {
            "origin" :  "static.resource.com",
            "httpPort" :  80,
            "httpsPort" : 443
        }
    ],
    "forwardHostHeader": "ORIGIN_HOSTNAME",
    "useOriginHttpProtocolDowngrade": false,
    "rootPathAccessControl" : {
        "enable": true,
        "controlType": "REDIRECT",
        "redirectPath": "/default.png",
        "redirectStatusCode": 302
    },
    "callback": {
        "httpMethod": "GET",
        "url": "http://test.callback.com/cdn?=appKey={appKey}&status={status}&domain={domain}"
    }
}
```


[Field]

| Field                                   | Type    | Description                                                         |
| -------------------------------------- | ------- | ------------------------------------------------------------ |
| header                                 | Object  | Header area                                                    |
| header.isSuccessful                    | Boolean | Successful or not                                                    |
| header.resultCode                      | Integer | Result code                                                    |
| header.resultMessage                   | String  | Result message                                                  |
| distributions                          | List    | List of created CDN objects                                     |
| distributions[0].domain                | String  | Domain name (service name)                                     |
| distributions[0].domainAlias           | List  | List of domain aliases (using domains owned by individuals or companies)                                                  |
| distributions[0].region                | String  | Service region ("GLOBAL": Global service)             |
| distributions[0].status                | String  | CDN status code (See [Table] CDN Status Code)                                 |
| distributions[0].defaultMaxAge         | Integer  | Cache expiration time (seconds)                                           |
| distributions[0].cacheKeyQueryParam    | String  | Set whether to include the request query string in the cache key ("INCLUDE_ALL": Include all, "EXCLUDE_ALL": Exclude all) |
| distributions[0].referrerType          | String  | Referrer access management ("BLACKLIST": Blacklist, "WHITELIST": Whitelist) |
| distributions[0].referrers             | List    | List of regex referrer headers                                 |
| distributions[0].isAllowWhenEmptyReferrer | Boolean | Whether to allow (true) or deny (false) access to content when there is no referer header |
| distributions[0].useOriginCacheControl | Boolean | Whether to use origin server setting or not (true: Use the origin server setting, false: User-configured setting) |
| distributions[0].origins               | List    | List of origin server objects                                      |
| distributions[0].origins[0].origin     | String  | Origin server (domain or IP)                                      |
| distributions[0].origins[0].originPath | String  | Sub-path of origin server                                          |
| distributions[0].origins[0].httpPort   | Integer | HTTP protocol port of origin server                                  |
| distributions[0].origins[0].httpsPort  | Integer | HTTPS protocol port of origin server                                 |
| distributions[0].forwardHostHeader     | String  | Callback to receive service deployment result                        |
| distributions[0].useOriginHttpProtocolDowngrade | Boolean | Whether to enable settings to downgrade a request from HTTPS to HTTP when the request is made to origin server from CDN server, if the origin server can respond only via HTTP |
| distributions[0].forwardHostHeader     | String  | Set the host header to be forwarded by the CDN server when requesting content to the origin server ("ORIGIN_HOSTNAME": Set to the host name of the origin server, "REQUEST_HOST_HEADER": Set to the host header of the client request) |
| distributions[0].rootPathAccessControl  | Object  | Set the access control for the CDN service root path | 
| distributions[0].rootPathAccessControl.enable | Boolean | Whether the access control for the root path is enabled (true) or disabled (false)          |
| distributions[0].rootPathAccessControl.controlType  | String  | Required if enable is true. The access control method for the root path. ("DENY": deny access, "REDIRECT": redirect to the specified path) | 
| distributions[0].rootPathAccessControl.redirectPath | String | Required if controlType is "REDIRECT". The path to redirect requests for the root path to. (Enter a path including /.)        |
| distributions[0].rootPathAccessControl.redirectStatusCode | Integer | Required when controlType is "REDIRECT". The HTTP response code sent when redirecting.          |
| distributions[0].callback              | Object  | Callback to receive service deployment result                        |
| distributions[0].callback.httpMethod   | String  | HTTP method of callback                                           |
| distributions[0].callback.url          | String  | Callback URL                                                     |


### Modify a Service

#### Request


[URI]

| Method  | URI                                  |
| ---- | ------------------------------------ |
| PUT  | /v2.0/appKeys/{appKey}/distributions |


[Request Body]

```json
{
    "distributions" : [
    {
      "domain" : "sample.toastcdn.net",
      "useOriginCacheControl" : false,
      "defaultMaxAge": 86400,
      "cacheKeyQueryParam": "INCLUDE_ALL",
      "referrerType" : "BLACKLIST",
      "referrers" : ["test.com"],
      "origins" : [
          {
              "origin" : "static.resource.com",
              "httpPort" : 80,
              "httpsPort" : 443,
              "originPath" : "/latest/resources"
          }
      ],
      "useOriginHttpProtocolDowngrade": false,
      "forwardHostHeader": "ORIGIN_HOSTNAME",
      "rootPathAccessControl" : {
          "enable": true,
          "controlType": "REDIRECT",
          "redirectPath": "/default.png",
          "redirectStatusCode": 302
      },
      "callback": {
          "httpMethod": "GET",
          "url": "http://test.callback.com/cdn?=appKey={appKey}&status={status}&domain={domain}"
      },
      "description" : "change contents"
    }
  ]
}
```


[Field]

| Name                  | Type    | Required | Default | Valid Range                                                    | Description                                                         |
| --------------------- | ------- | --------- | ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| domain                | String  | Required      |        | Up to 255 characters                                                   | Domain (service name) to modify                                   |
| useOriginCacheControl | Boolean | Required      |        | true/false                                                        | Cache expiration setting (true: Use the origin server setting, false: User-configured setting)      |
| referrerType          | String  | Required      |        | BLACKLIST/WHITELIST                                          | Referrer access management ("BLACKLIST": Blacklist, "WHITELIST": Whitelist) |
| referrers             | List    | Optional      |        |                                                              | List of regex referrer headers |
| isAllowWhenEmptyReferrer | Boolean | Optional      | true      | true/false             | Whether to allow (true) or deny (false) access to content when there is no referer header             |
| description           | String  | Optional      |        | Up to 255 characters                                                   | Description                                                         |
| domainAlias           | List    | Optional      |        | Up to 255 characters                                                   | Domain alias (using a domain owned by individuals or companies) |
| defaultMaxAge         | Integer | Optional      | 0      | 0~2,147,483,647                                            | Cache expiration time (seconds), the default value 0 is 604,800 seconds.              |
| cacheKeyQueryParam    | String  | Optional      | INCLUDE_ALL | INCLUDE_ALL/EXCLUDE_ALL                               | Set whether to include the request query string in the cache key ("INCLUDE_ALL": Include all, "EXCLUDE_ALL": Exclude all) |
| origins               | List    | Required      |        |                                                              | Origin server                                                    |
| origins[0].origin     | String  | Required      |        | Up to 255 characters                                                   | Origin server (domain or IP)                                      |
| origins[0].originPath | String  | Optional      |        | Up to 8192 characters                                                  | Sub-path of origin server                                          |
| origins[0].httpPort   | Integer  | Optional      |        |See '[Table 2] Available Origin Server Port Numbers' of [Console User Guide > Origin Server](./console-guide/#origin-server)| HTTP protocol port of the origin server (one of origins[0].httpPort and origins[0].httpsPort must be entered.)  |
| origins[0].httpsPort  | Integer  | Optional      |        |See '[Table 2] Available Origin Server Port Numbers' of [Console User Guide > Origin Server](./console-guide/#origin-server) | HTTPS protocol port of the origin server (one of origins[0].httpPort and origins[0].httpsPort must be entered.) |
| useOriginHttpProtocolDowngrade | Boolean  | Required     | false       | true/false         | Whether to enable settings to downgrade a request from HTTPS to HTTP when the request is made to origin server from CDN server, if the origin server can respond only via HTTP |
| forwardHostHeader     | String  | Required      |        | ORIGIN_HOSTNAME<br/>REQUEST_HOST_HEADER   | Set the host header to be forwarded by the CDN server when requesting content to the origin server ("ORIGIN_HOSTNAME": Set to the host name of the origin server, "REQUEST_HOST_HEADER": Set to the host header of the client request)|
| useOrigin             | String  | Required      |        | Y/N                                                          | Cache expiration setting (Y: Use the original setting, "N": Use the user-configured setting)      |
| rootPathAccessControl  | Object  | Optional |  |  | Set the access control for the CDN service root path | 
| rootPathAccessControl.enable | Boolean | Required | false | true/false | Whether the access control for the root path is enabled (true) or disabled (false)          |
| rootPathAccessControl.controlType  | String  | Optional |  | DENY, REDIRECT | Required if enable is true. The access control method for the root path. ("DENY": deny access, "REDIRECT": redirect to the specified path) | 
| rootPathAccessControl.redirectPath | String | Optional |  | | Required if controlType is "REDIRECT". The path to redirect requests for the root path to. (Enter a path including /.)        |
| rootPathAccessControl.redirectStatusCode | Integer | Optional | | 301, 302, 303, 307 |Required when controlType is "REDIRECT". The HTTP response code sent when redirecting.          |
| callback              | Object  | Optional      |        | Callback URL to receive CDN service deployment result (Callback setting is optional.) |                                                              |
| callback.httpMethod   | String  | Required      |        | GET/POST/PUT                                                 | HTTP method of callback                                           |
| callback.url          | String  | Required      |        | Up to 1024 characters                                                  | Callback URL                                                     |

- The default value of forwardHostHeader is REQUEST_HOST_HEADER if domainAlias is set, or ORIGIN_HOSTNAME otherwise. 

#### Response


[Response Body]

```json
{
    "header" : {
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS",
        "isSuccessful" :  true
    }
}
```


[Field]

| Field                   | Type      | Description     |
| -------------------- | ------- | ------ |
| header               | Object  | Header area  |
| header.isSuccessful  | Boolean | Successful or not  |
| header.resultCode    | Integer | Result code  |
| header.resultMessage | String  | Result message |

### Delete a Service

#### Request


[URI]

| Method    | URI                                  |
| ------ | ------------------------------------ |
| DELETE | /v2.0/appKeys/{appKey}/distributions |


[Request Body]

```json
{
    "domains" : [
        "lhcsxuo0.toastcdn.net"
    ]
}
```


[Field]

| Name      | Type     | Required | Default  | Valid Range | Description                    |
| ------- | ------ | ----- | ---- | ----- | --------------------- |
| domains | String | Required    |      |       | Domains to delete; multiple domains allowed |

**\* When multiple domains are provided as input, all corresponding services are closed.**

#### Response


[Response Body]

```json
{
    "header" : {
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS",
        "isSuccessful" :  true
    }
}
```


[Field]

| Field                   | Type      | Description     |
| -------------------- | ------- | ------ |
| header               | Object  | Header area  |
| header.isSuccessful  | Boolean | Successful or not  |
| header.resultCode    | Integer | Result code  |
| header.resultMessage | String  | Result message |


## Auth Token API

### Create an Auth Token

#### Request

[URI]

| Method  | URI                           |
| ---- | ----------------------------- |
| POST | /v2.0/appKeys/{appKey}/auth-token |


[Request Body]

```json
{
  "encryptKey" : "AUTH_TOKEN_ENCRYPT_KEY",
  "durationSeconds": 3600,
  "singlePath": "/sample.png",
  "singleWildcardPath": "/dir/*",
  "multipleWildcardPath": ["/dir/*", "/dir2/*"],
  "sessionId": "sampleSessionId"
}
```


[Field]

| Name      | Type   | Required | Default | Valid Range             | Description                                                         |
| --------- | ------ | --------- | ------ | --------------------- | ------------------------------------------------------------ |
| encryptKey    | String | Required   |        |             | Access Control for Auth Token Authentication > Token Encryption Key displayed on NHN Cloud CDN console  |
| durationSeconds | Integer | Required |        | 0~2,147,483,647 | Duration, in seconds, for which the generated token is valid |
| singlePath      | String | Optional |        |             | Single path to access using the generated token |
| singleWildcardPath | String | Optional |     |             | Single wildcard path to access using generated token |
| multipleWildcardPath | String | Optional |   |             | Multiple wildcard paths to access using the generated token |
| sessionId |           String | Optional |    |  String length is up to 36 bytes           | Generates token including sessionId for a single access request |

* At least one of singlePath, singleWildcardPath, or multipleWildcardPath must exist.
* For details on creating and using tokens, refer to [Console User Guide > Access Control for Auth Token Authentication > 2. Create a Token](./console-guide/#access-control-for-auth-token-authentication).


#### Response

[Response Body]

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "authToken": {
        "singlePathToken": "exp=1652247396~id=fjdklfjklsdfjklsdjflksdjfkls~hmac=c743fcdb2c35c7c97455c18f6d354eef89743f556d3b82df3861ef9cb67eec94",
        "singleWildcardPathToken": "exp=1652247396~acl=%2fdir%2f*~id=fjdklfjklsdfjklsdjflksdjfkls~hmac=160acb24795daf63a7b0628420f8d7f4a37f014c01b73ad388ee5efaca17d663",
        "multipleWildcardPathToken": "exp=1652247396~acl=%2fdir%2f*~id=fjdklfjklsdfjklsdjflksdjfkls~hmac=160acb24795daf63a7b0628420f8d7f4a37f014c01b73ad388ee5efaca17d663"
    }
}
```


[Field]

| Field                   | Type      | Description        |
| -------------------- | ------- | --------- |
| header               | Object  | Header area     |
| header.isSuccessful  | Boolean | Successful or not     |
| header.resultCode    | Integer | Result code     |
| header.resultMessage | String  | Result message    |
| authToken             | Object    | Created Auth Token object |
| authToken.singlePathToken | String    | Auth token generated to allow access to a single path                                 |
| authToken.singleWildcardPathToken | String    | Auth token generated to allow access to a single wildcard path                 |
| authToken.multipleWildcardPathToken | String  | Auth token generated to access multiple wildcard paths             |



## Purge Cache API

### Purge Cache -  ITEM (particular file type)

#### Request

[URI]

| Method  | URI                           |
| ---- | ----------------------------- |
| POST | /v2.0/appKeys/{appKey}/purge/item |


[Request Body]

```json
{
	"domain": "sample.toastcdn.net",
	"purgeList":["http://sample.toastcdn.net/img_01.png",
  "http://sample.toastcdn.net/img_02.png"]
}
```


[Field]

| Name      | Type   | Required | Default | Valid Range             | Description                                                         |
| --------- | ------ | --------- | ------ | --------------------- | ------------------------------------------------------------ |
| domain    | String | Required      |        | Up to 255 characters            | Domain (service) name to be purged                                 |
| purgeList | List | Required      |        |                       | List of purge target URLs |

#### Response

[Response Body]

```json
{
    "header" : {
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS",
        "isSuccessful" :  true
    }
}
```


[Field]

| Field                   | Type      | Description        |
| -------------------- | ------- | --------- |
| header               | Object  | Header area     |
| header.isSuccessful  | Boolean | Successful or not     |
| header.resultCode    | Integer | Result code     |
| header.resultMessage | String  | Result message    |

### Purge Cache -  ALL (All file types)

#### Request

[URI]

| Method  | URI                           |
| ---- | ----------------------------- |
| POST | /v2.0/appKeys/{appKey}/purge/all |


[Request Body]

```json
{
	"domain": "sample.toastcdn.net"
}
```


[Field]

| Name      | Type   | Required | Default | Valid Range             | Description                                                         |
| --------- | ------ | --------- | ------ | --------------------- | ------------------------------------------------------------ |
| domain    | String | Required      |        | Up to 255 characters            | Domain (service) name to be purged                                 |

#### Response

[Response Body]

```json
{
    "header" : {
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS",
        "isSuccessful" :  true
    }
}
```


[Field]

| Field                   | Type      | Description        |
| -------------------- | ------- | --------- |
| header               | Object  | Header area     |
| header.isSuccessful  | Boolean | Successful or not     |
| header.resultCode    | Integer | Result code     |
| header.resultMessage | String  | Result message    |

- Cache purge requests may fail within an hour after the CDN service is newly created. If the failure continues, contact Customer Center.
- A usage limit policy exists for Purge APIs. For more details, see the cache purge usage limit of [Console User Guide > Purging CDN Cache](./console-guide/#purging-cdn-cache).

### Query Cache Purge
- In case of purging cache through API v2.0, high-speed cache purge is performed and completed within a few seconds after request, so an API to query cache purge status is not provided separately.

## Callback Response
If the callback function is set on the CDN service, the configured callback URL is called when creation, modification, pause, resume, and deletion changes are completed.
When a callback is called, the request body includes CDN service setting information as follows:

[Response Body]
```json
{
  "header" : {
    "resultCode" :  0,
    "resultMessage" :  "SUCCESS",
    "isSuccessful" :  true
  },
  "distribution":{
      "appKey": "wXDdIjJRcZDtY9F7",
      "domain" :  "lhcsxuo0.toastcdn.net",
      "domainAlias" :  ["test.domain.com"],
      "region" :  "GLOBAL",
      "status" : "OPEN",
      "defaultMaxAge" : 86400,
      "cacheKeyQueryParam": "INCLUDE_ALL",
      "status" :  "OPENING",
      "referrerType" :  "BLACKLIST",
      "referrers" :  ["test.com"],    
      "useOriginCacheControl" :  false,
      "createTime" : 1498613094692,
      "deleteTime": 1498613094692,
      "origins" : [
          {
              "origin" :  "static.resource.com",
              "httpPort" :  80,
              "httpsPort" : 443
          }
      ],
      "forwardHostHeader": "ORIGIN_HOSTNAME",
      "useOriginHttpProtocolDowngrade": false,
      "rootPathAccessControl" : {
          "enable": true,
          "controlType": "REDIRECT",
          "redirectPath": "/default.png",
          "redirectStatusCode": 302
      },
      "callback": {
          "httpMethod": "GET",
          "url": "http"
      }
  }
}
```

[Field]

| Field                                   | Type    | Description                                                         |
| -------------------------------------- | ------- | ------------------------------------------------------------ |
| header                                 | Object  | Header area                                                    |
| header.isSuccessful                    | Boolean | Successful or not                                                    |
| header.resultCode                      | Integer | Result code                                                    |
| header.resultMessage                   | String  | Result message                                                  |
| distribution                          | Object    | CDN objects completed with changes                                   |
| distribution.appKey                   | String    | Appkey                                  |
| distribution.domain                | String  | Domain name (service name)                                     |
| distribution.domainAlias           | List  | List of domain aliases (using domains owned by individuals or companies)                                 |
| distribution.region                | String  | Service region ("GLOBAL": Global service)             |
| distribution.status                | String  | CDN status code (See [Table] CDN Status Code)                                 |
| distribution.defaultMaxAge         | Integer  | Cache expiration time (seconds)                                           |
| distribution.cacheKeyQueryParam    | String  | Set whether to include the request query string in the cache key ("INCLUDE_ALL": Include all, "EXCLUDE_ALL": Exclude all) |
| distribution.referrerType          | String  | Referrer access management ("BLACKLIST": Blacklist, "WHITELIST": Whitelist) |
| distribution.referrers             | List    | List of regex referrer headers                                 |
| distribution.useOriginCacheControl | Boolean | Whether to use origin server setting or not (true: Use the origin server setting, false: User-configured setting) |
| distribution.createTime            | DateTime | Date and time of creation                                         |
| distribution.deleteTime            | DateTime | Date and time of deletion                                         |
| distribution.origins               | List    | List of origin server objects                                      |
| distribution.origins[0].origin     | String  | Origin server (domain or IP)                                      |
| distribution.origins[0].originPath | String  | Sub-path of origin server                                          |
| distribution.origins[0].httpPort   | Integer | HTTP protocol port of origin server                                               |
| distribution.origins[0].httpsPort  | Integer | HTTPS protocol port of origin server                                               |
| distribution.useOriginHttpProtocolDowngrade | Boolean | Whether to enable settings to downgrade a request from HTTPS to HTTP when the request is made to origin server from CDN server, if the origin server can respond only via HTTP |
| distribution.forwardHostHeader     | String  | Set the host header to be forwarded by the CDN server when requesting content to the origin server ("ORIGIN_HOSTNAME": Set to the host name of the origin server, "REQUEST_HOST_HEADER": Set to the host header of the client request) |
| distribution.rootPathAccessControl  | Object  | Set the access control for the CDN service root path | 
| distribution.rootPathAccessControl.enable | Boolean | Whether the access control for the root path is enabled (true) or disabled (false)          |
| distribution.rootPathAccessControl.controlType  | String  | Required if enable is true. The access control method for the root path. ("DENY": deny access, "REDIRECT": redirect to the specified path) | 
| distribution.rootPathAccessControl.redirectPath | String | Required if controlType is "REDIRECT". The path to redirect requests for the root path to. (Enter a path including /.)        |
| distribution.rootPathAccessControl.redirectStatusCode | Integer | Required when controlType is "REDIRECT". The HTTP response code sent when redirecting.         |
| distribution.callback              | Object  | Callback to receive service deployment result                        |
| distribution.callback.httpMethod   | String  | HTTP method of callback                                           |
| distribution.callback.url          | String  | Callback URL                                                     |
