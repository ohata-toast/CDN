## Content Delivery > CDN > API v2.0 Guide 

This document describes Public API v2.0 of TOAST CDN. 

## API V2.0 Support for Each Service Domain   
API V2.0 supports \*.toastcdn.net domain only.  
To use the \*.cdn.toastcloud.com domain, use v1.5 or lowr version of API. 

## Common API Information 

### Domain

| Name              | Domain                                   |
| --------------- | ------------------------------------- |
| CDN Public API Domain | https://api-gw.cloud.toast.com/tc-cdn |

### Prerequisites

Requires Appkey and Secretkey API, which are available in **URL & Appkey** on top right of the console.

### Common Request Information

#### Request Header

| Name            | Description                        |
| ------------- | ------------------------- |
| Authorization | SecretKey issued on a console |

#### Path Parameter 

In all APIs, the appKey must be specified in the path parameter.
* e.g.) /v2.0/appKeys/**{appKey}**/distributions

| Name     | Description                    |
| ------ | --------------------- |
| appKey | Appkey issued on console |

### Common Response Information 

#### Header 

Respond with **200 OK** to all API requests. For more details, see the header at the response body as below

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

Below shows the status codes of CDN service, which are available at the query of service.

| Value         | Description                     |
| ---------- | ------------------------ |
| OPENING    | Starting service            |
| OPEN       | In service                 |
| MODIFYING  | Under modification         |
| RESUME     | Resumed                     |
| SUSPENDING | Under suspension            |
| SUSPEND    | Suspended                   |
| CLOSING    | Closing service              |
| CLOSE      | Closed                 |
| ERROR      | Error occurred while creating service  |


## Service API

### Create

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
      "referrerType" : "BLACKLIST",      
      "referrers" : ["cloud.toast.com"],
      "origins" : [
        {
          "origin" : "static.origin.com",
          "originPath" : "/resources",       
          "httpPort": 80,
          "httpsPort": 443
        }
      ],
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
| distributions[0].useOriginHttpProtocolDowngrade | Boolean  | Required     | false       | true/false         | Whether to enable settings to downgrade a request from HTTPS to HTTP when the request is made to origin server from CDN server, if the origin server can respond only via HTTP|
| distributions[0].forwardHostHeader     | String  | Required      | When domain alias setting is enabled: REQUEST_HOST_HEADER, When domain alias setting is enabled: ORIGIN_HOSTNAME       | ORIGIN_HOSTNAME/REQUEST_HOST_HEADER   | Setting host header to be delivered when CDN server requests content to origin server ("ORIGIN_HOSTNAME": Set as host name for origin server, "REQUEST_HOST_HEADER": Set as host header for client requests)| 
| distributions[0].useOriginCacheControl | Boolean | Required      |        | true/false                  | Cache expiration setting (true: Original setting, false: User-configured)   |
| distributions[0].referrerType          | String  | Required      |        | BLACKLIST/WHITELIST         | Referrer access management ("BLACKLIST": Blacklist, "WHITELIST": Whitelist) |
| distributions[0].referrers             | List    | Optional      |        |                           | List of regex referrer headers   |
| distributions[0].description           | String  | Optional      |        | Up to 255 characters                  | Description                                                         |
| distributions[0].domainAlias           | List    | Optional      |        |                           | List of domain aliases (Use personal or company-owned domain) |
| distributions[0].defaultMaxAge         | Integer | Optional      | 0      | 0~2,147,483,647             | Cache expiration time (seconds); default is 0 with 604,800 seconds.             |
| distributions[0].origins               | List    | Required      |        |                             | List of origin server objects                                      |
| distributions[0].origins[0].origin     | String  | Required      |        | Up to 255 characters                  | Origin server (domain or IP)                                     |
| distributions[0].origins[0].originPath | String  | Optional      |        | Up to 8192 characters                 | Lower paths of origin server (path must include /.)        |
| distributions[0].origins[0].httpPort   | Integer  | Optional      |        | See '[Table 2] Available Origin Server Port Number' of [Console User Guide > Origin Server](./console-guide/#_2) | HTTP protocol port of origin server(must enter either origins[0].httpPort or origins[0].httpsPort.)  |
| distributions[0].origins[0].httpsPort  | Integer  | Optional      |        | See '[Table 2] Available Origin Server Port Number' of[Console User Guide > Origin Server](./console-guide/#_2) | HTTPS protocol port of origin server (must enter either origins[0].httpPort or origins[0].httpsPort.) |
| distributions[0].callback              | Object  | Optional      |        |                             | Callback URL to receive processing result of CDN creation (callback setting is optional.) |
| distributions[0].callback.httpMethod   | String  | Required      |        | GET/POST/PUT                | HTTP method of callback                                           |
| distributions[0].callback.url          | String  | Required      |        | Up to 1024 characters                 | Callback URL                                                     |




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
            "referrerType": "BLACKLIST",
            "referrers": [
                "cloud.toast.com"
            ],
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
| header                                 | Object  | Header area                                                   |
| header.isSuccessful                    | Boolean | Successful or not                                                    |
| header.resultCode                      | Integer | Result code                                                    |
| header.resultMessage                   | String  | Result message                                                  |
| distributions                          | List    | List of created CDN objects                                    |
| distributions[0].domain                | String  | Name of created domain (service)                               |
| distributions[0].domainAlias           | List    | List of domain aliases (use personal or company-owned domain)              |
| distributions[0].region                | String  | Service region ("GLOBAL": Global service)            |
| distributions[0].description           | String  | Description                                                     |
| distributions[0].status                | String  | CDN status code (see [Table] CDN Status Code)                                 |
| distributions[0].defaultMaxAge         | Integer | Cache expiration time (seconds)                                           |
| distributions[0].referrerType          | String  | Referrer access management ("BLACKLIST": Blacklist, "WHITELIST": Whitelist) |
| distributions[0].referrers             | List    | List of regex referrer headers                                   |
| distributions[0].useOriginCacheControl | Boolean  | Whether to use origin server setting or not (true: Enable origin server setting, false: User-configured setting) |
| distributions[0].origins               | List    | List of origin server objects                                      |
| distributions[0].origins[0].origin     | String  | Origin server (domain or IP)                                      |
| distributions[0].origins[0].originPath | String  | Lower paths of origin server                                           |
| distributions[0].origins[0].httpPort   | Integer | HTTP protocol port of origin server                                               |
| distributions[0].origins[0].httpsPort  | Integer | HTTPS protocol port of origin server                                              |
| distributions[0].useOriginHttpProtocolDowngrade | Boolean | 원본 서버가 HTTP 응답만 가능한 경우, CDN 서버에서 원본 서버로 요청 시 HTTPS 요청을 HTTP 요청으로 다운그레이드하기 위한 설정 사용 여부 |
| distributions[0].forwardHostHeader     | String  | CDN 서버가 원본 서버로 콘텐츠 요청 시 전달 할 호스트 헤더 설정("ORIGIN_HOSTNAME": 원본 서버의 호스트 이름으로 설정, "REQUEST_HOST_HEADER": 클라이언트 요청의 호스트헤더로 설정 |
| distributions[0].callback              | Object  | Callback to receive 서비스 생성 처리 결과를 통보받을 콜백                        |
| distributions[0].callback.httpMethod   | String  | HTTP method of callback                                           |
| distributions[0].callback.url          | String  | Callbak URL                                                     |



### Get

#### Request


[URI]

| Method  | URI                                  |
| ---- | ------------------------------------ |
| GET  | /v2.0/appKeys/{appKey}/distributions |


[Parameter]

| Name   | Type   | Required | Valid Range     | Description                         |
| ------ | ------ | --------- | ------------- | ---------------------------- |
| domain | String | Optional      | Up to 255 characters    | Domain to get (service name)   |
| status | String | Optional      | CDN status code | CDN Status Code (see [Table] CDN Status Code) |

[Example]
```
curl -X GET "https://api-gw.cloud.toast.com/tc-cdn/v2.0/appKeys/{appKey}/distributions?domain={domain}" \
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
    "status" :  "OPENING",
    "referrerType" :  "BLACKLIST",
    "referrers" :  ["test.com"],    
    "useOriginCacheControl" :  false,
    "origins" : [
        {
            "origin" :  "static.toastoven.net",
            "httpPort" :  80,
            "httpsPort" : 443
        }
    ],
    "forwardHostHeader": "ORIGIN_HOSTNAME",
    "useOriginHttpProtocolDowngrade": false,    
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
| header.isSuccessful                    | Boolean | Successful or not                                                   |
| header.resultCode                      | Integer | Result code                                                   |
| header.resultMessage                   | String  | Result message                                                  |
| distributions                          | List    | 생성된 CDN 오브젝트 목록                                     |
| distributions[0].domain                | String  | 도메인 이름(서비스 이름)                                     |
| distributions[0].domainAlias           | List  | 도메인 별칭 목록(개인 혹은 회사가 소유한 도메인 사용)                                                  |
| distributions[0].region                | String  | 서비스 지역("GLOBAL": 글로벌)             |
| distributions[0].status                | String  | CDN 상태 코드([표] CDN 상태 코드 참고)                                 |
| distributions[0].defaultMaxAge         | Integer  | 캐시 만료 시간(초)                                           |
| distributions[0].referrerType          | String  | 리퍼러 접근 관리("BLACKLIST": 블랙리스트, "WHITELIST": 화이트리스트) |
| distributions[0].referrers             | List    | 정규 표현식 형태의 리퍼러 헤더 목록                                 |
| distributions[0].useOriginCacheControl | Boolean | 원본 서버 설정 사용 여부(true: 원본 서버 설정 사용, false: 사용자 설정) |
| distributions[0].origins               | List    | 원본 서버 오브젝트 목록                                      |
| distributions[0].origins[0].origin     | String  | 원본 서버(도메인 또는 IP)                                      |
| distributions[0].origins[0].originPath | String  | 원본 서버 하위 경로                                          |
| distributions[0].origins[0].httpPort   | Integer | 원본 서버 HTTP 프로토콜 포트                                  |
| distributions[0].origins[0].httpsPort  | Integer | 원본 서버 HTTPS 프로토콜 포트                                 |
| distributions[0].forwardHostHeader     | String  | 서비스 배포 처리 결과를 통보받을 콜백                        |
| distributions[0].useOriginHttpProtocolDowngrade | Boolean | 원본 서버가 HTTP 응답만 가능한 경우, CDN 서버에서 원본 서버로 요청 시 HTTPS 요청을 HTTP 요청으로 다운그레이드하기 위한 설정 사용 여부 |
| distributions[0].forwardHostHeader     | String  | CDN 서버가 원본 서버로 콘텐츠 요청 시 전달 할 호스트 헤더 설정("ORIGIN_HOSTNAME": 원본 서버의 호스트 이름으로 설정, "REQUEST_HOST_HEADER": 클라이언트 요청의 호스트헤더로 설정 |
| distributions[0].callback              | Object  | 서비스 배포 처리 결과를 통보받을 콜백                        |
| distributions[0].callback.httpMethod   | String  | 콜백의 HTTP 메서드                                           |
| distributions[0].callback.url          | String  | 콜백 URL                                                     |


### 서비스 수정

#### 요청


[URI]

| 메서드  | URI                                  |
| ---- | ------------------------------------ |
| PUT  | /v2.0/appKeys/{appKey}/distributions |


[요청 본문]

```json
{
    "distributions" : [
    {
      "domain" : "sample.toastcdn.net",
      "useOriginCacheControl" : false,
      "defaultMaxAge": 86400,
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
      "callback": {
          "httpMethod": "GET",
          "url": "http://test.callback.com/cdn?=appKey={appKey}&status={status}&domain={domain}"
      },
      "description" : "change contents"        
    }
  ]
}
```


[필드]

| 이름                  | 타입    | 필수 여부 | 기본값 | 유효 범위                                                    | 설명                                                         |
| --------------------- | ------- | --------- | ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| domain                | String  | 필수      |        | 최대 255자                                                   | 수정할 도메인(서비스 이름)                                   |
| useOriginCacheControl | Boolean | 필수      |        | true/false                                                        | 캐시 만료 설정(true: 원본 서버 설정 사용, false: 사용자 설정)      |
| referrerType          | String  | 필수      |        | BLACKLIST/WHITELIST                                          | 리퍼러 접근 관리("BLACKLIST": 블랙리스트, "WHITELIST": 화이트리스트) |
| referrers             | List    | 선택      |        |                                                              | 정규 표현식 형태의 리퍼러 헤더 목록 |
| description           | String  | 선택      |        | 최대 255자                                                   | 설명                                                         |
| domainAlias           | List    | 선택      |        | 최대 255자                                                   | 도메인 별칭(개인 혹은 회사가 소유한 도메인 사용) |
| defaultMaxAge         | Integer | 선택      | 0      | 0~2,147,483,647                                            | 캐시 만료 시간(초), 기본값 0은 604,800초입니다.              |
| origins               | List    | 필수      |        |                                                              | 원본 서버                                                    |
| origins[0].origin     | String  | 필수      |        | 최대 255자                                                   | 원본 서버(도메인 또는 IP)                                      |
| origins[0].originPath | String  | 선택      |        | 최대 8192자                                                  | 원본 서버 하위 경로                                          |
| origins[0].httpPort   | Integer  | 선택      |        |[콘솔 사용 가이드 > 원본 서버](./console-guide/#_2)의 '[표 2] 사용 가능한 원본 서버 포트 번호' 참고| 원본 서버 HTTP 프로토콜 포트(origins[0].httpPort와 origins[0].httpsPort 중 하나는 반드시 입력해야 합니다.)  |
| origins[0].httpsPort  | Integer  | 선택      |        |[콘솔 사용 가이드 > 원본 서버](./console-guide/#_2)의 '[표 2] 사용 가능한 원본 서버 포트 번호' 참고 | 원본 서버 HTTPS 프로토콜 포트(origins[0].httpPort와 origins[0].httpsPort 중 하나는 반드시 입력해야 합니다.) |
| useOriginHttpProtocolDowngrade | Boolean  | 필수     | false       | true/false         | 원본 서버가 HTTP 응답만 가능한 경우, CDN 서버에서 원본 서버로 요청 시 HTTPS 요청을 HTTP 요청으로 다운그레이드하기 위한 설정 사용 여부 |
| forwardHostHeader     | String  | 필수      | 도메인 별칭 설정시: REQUEST_HOST_HEADER, 도메인 별칭 미설정시: ORIGIN_HOSTNAME       | ORIGIN_HOSTNAME/REQUEST_HOST_HEADER   | CDN 서버가 원본 서버로 콘텐츠 요청 시 전달할 호스트 헤더 설정("ORIGIN_HOSTNAME": 원본 서버의 호스트 이름으로 설정, "REQUEST_HOST_HEADER": 클라이언트 요청의 호스트 헤더로 설정)|
| useOrigin             | String  | 필수      |        | Y/N                                                          | 캐시 만료 설정(Y: 원본 설정 사용, "N":사용자 설정 사용)      |
| callback              | Object  | 선택      |        | CDN 서비스 배포 결과를 통보받을 콜백 URL(콜백 설정은 선택 입력입니다.) |                                                              |
| callback.httpMethod   | String  | 필수      |        | GET/POST/PUT                                                 | 콜백의 HTTP 메서드                                           |
| callback.url          | String  | 필수      |        | 최대 1024자                                                  | 콜백 URL                                                     |

- forwardHostHeader의 기본값은 domainAlias를 설정한 경우 REQUEST_HOST_HEADER이고, 미설정하면 ORIGIN_HOSTNAME입니다. 

#### 응답


[응답 본문]

```json
{
    "header" : {
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS",
        "isSuccessful" :  true
    }
}
```


[필드]

| 필드                   | 타입      | 설명     |
| -------------------- | ------- | ------ |
| header               | Object  | 헤더 영역  |
| header.isSuccessful  | Boolean | 성공 여부  |
| header.resultCode    | Integer | 결과 코드  |
| header.resultMessage | String  | 결과 메시지 |

### 서비스 삭제

#### 요청


[URI]

| 메서드    | URI                                  |
| ------ | ------------------------------------ |
| DELETE | /v2.0/appKeys/{appKey}/distributions |


[요청 본문]

```json
{
    "domains" : [
        "lhcsxuo0.toastcdn.net"
    ]
}
```


[필드]

| 이름      | 타입     | 필수 여부 | 기본값  | 유효 범위 | 설명                    |
| ------- | ------ | ----- | ---- | ----- | --------------------- |
| domains | String | 필수    |      |       | 삭제할 도메인, 여러 도메인 입력 가능 |

**\* 여러 도메인 입력 시, 해당하는 서비스는 모두 종료됩니다.**

#### 응답


[응답 본문]

```json
{
    "header" : {
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS",
        "isSuccessful" :  true
    }
}
```


[필드]

| 필드                   | 타입      | 설명     |
| -------------------- | ------- | ------ |
| header               | Object  | 헤더 영역  |
| header.isSuccessful  | Boolean | 성공 여부  |
| header.resultCode    | Integer | 결과 코드  |
| header.resultMessage | String  | 결과 메시지 |


## 캐시 재배포 API

### 캐시 재배포(Purge) -  ITEM(특정 파일 타입)

#### 요청

[URI]

| 메서드  | URI                           |
| ---- | ----------------------------- |
| POST | /v2.0/appKeys/{appKey}/purge/item |


[요청 본문]

```json
{
	"domain": "sample.toastcdn.net",
	"purgeList":["http://sample.toastcdn.net/img_01.png",
  "http://sample.toastcdn.net/img_02.png"]
}
```


[필드]

| 이름      | 타입   | 필수 여부 | 기본값 | 유효 범위             | 설명                                                         |
| --------- | ------ | --------- | ------ | --------------------- | ------------------------------------------------------------ |
| domain    | String | 필수      |        | 최대 255자            | 재배포할 도메인(서비스) 이름                                 |
| purgeList | List | 필수      |        |                       | 재배포 대상 URL 목록 |

#### 응답

[응답 본문]

```json
{
    "header" : {
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS",
        "isSuccessful" :  true
    }
}
```


[필드]

| 필드                   | 타입      | 설명        |
| -------------------- | ------- | --------- |
| header               | Object  | 헤더 영역     |
| header.isSuccessful  | Boolean | 성공 여부     |
| header.resultCode    | Integer | 결과 코드     |
| header.resultMessage | String  | 결과 메시지    |

### 캐시 재배포(Purge) -  ALL(전체 파일 타입)

#### 요청

[URI]

| 메서드  | URI                           |
| ---- | ----------------------------- |
| POST | /v2.0/appKeys/{appKey}/purge/all |


[요청 본문]

```json
{
	"domain": "sample.toastcdn.net"
}
```


[필드]

| 이름      | 타입   | 필수 여부 | 기본값 | 유효 범위             | 설명                                                         |
| --------- | ------ | --------- | ------ | --------------------- | ------------------------------------------------------------ |
| domain    | String | 필수      |        | 최대 255자            | 재배포할 도메인(서비스) 이름                                 |

#### 응답

[응답 본문]

```json
{
    "header" : {
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS",
        "isSuccessful" :  true
    }
}
```


[필드]

| 필드                   | 타입      | 설명        |
| -------------------- | ------- | --------- |
| header               | Object  | 헤더 영역     |
| header.isSuccessful  | Boolean | 성공 여부     |
| header.resultCode    | Integer | 결과 코드     |
| header.resultMessage | String  | 결과 메시지    |

- CDN 서비스를 신규로 생성한 후 약 1시간 이내에는 캐시 재배포 요청이 실패할 수 있습니다. 이후에도 실패가 계속되면 고객 센터로 문의해 주시기 바랍니다.
- 퍼지 API 사용량 제한 정책이 있습니다. 자세한 내용은 [콘솔 사용 가이드 > CDN 캐시 재배포](./console-guide/#cdn_4)의 캐시 재배포 사용량 제한] 내용을 확인해주세요.

### 캐시 재배포(Purge) 조회
- API v2.0을 통한 캐시 재배포 시, 고속 캐시 재배포가 수행되어 요청 후 수 초 이내에 완료되므로 캐시 재배포 상태를 조회하는 API가 별도로 제공되지 않습니다.

## 콜백 응답
CDN 서비스에 콜백 기능이 설정된 경우, 생성, 수정, 일시 정지, 재개, 삭제 변경이 완료되면 설정된 콜백 URL을 호출합니다.
콜백 호출시 요청 본문에는 다음과 같은 CDN 서비스 설정 정보가 포함됩니다.

[응답 본문]
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
      "status" :  "OPENING",
      "referrerType" :  "BLACKLIST",
      "referrers" :  ["test.com"],    
      "useOriginCacheControl" :  false,
      "createTime" : 1498613094692,
      "deleteTime": 1498613094692,
      "origins" : [
          {
              "origin" :  "static.toastoven.net",
              "httpPort" :  80,
              "httpsPort" : 443
          }
      ],
      "forwardHostHeader": "ORIGIN_HOSTNAME",
      "useOriginHttpProtocolDowngrade": false,    
      "callback": {
          "httpMethod": "GET",
          "url": "http"
      }
  }
}
```

[필드]

| 필드                                   | 타입    | 설명                                                         |
| -------------------------------------- | ------- | ------------------------------------------------------------ |
| header                                 | Object  | 헤더 영역                                                    |
| header.isSuccessful                    | Boolean | 성공 여부                                                    |
| header.resultCode                      | Integer | 결과 코드                                                    |
| header.resultMessage                   | String  | 결과 메시지                                                  |
| distribution                          | Object    | 변경 작업이 완료된 CDN 오브젝트                                   |
| distribution.appKey                   | String    | 앱키                                  |
| distribution.domain                | String  | 도메인 이름(서비스 이름)                                     |
| distribution.domainAlias           | List  | 도메인 별칭 목록(개인 혹은 회사가 소유한 도메인 사용)                                 |
| distribution.region                | String  | 서비스 지역("GLOBAL": 글로벌)             |
| distribution.status                | String  | CDN 상태 코드([표] CDN 상태 코드 참고)                                 |
| distribution.defaultMaxAge         | Integer  | 캐시 만료 시간(초)                                           |
| distribution.referrerType          | String  | 리퍼러 접근 관리("BLACKLIST": 블랙리스트, "WHITELIST": 화이트리스트) |
| distribution.referrers             | List    | 정규 표현식 형태의 리퍼러 헤더 목록                                 |
| distribution.useOriginCacheControl | Boolean | 원본 서버 설정 사용 여부(true: 원본 서버 설정 사용, false: 사용자 설정) |
| distribution.createTime            | DateTime | 생성 일시                                         |
| distribution.deleteTime            | DateTime | 삭제 일시                                         |
| distribution.origins               | List    | 원본 서버 오브젝트 목록                                      |
| distribution.origins[0].origin     | String  | 원본 서버(도메인 또는 IP)                                      |
| distribution.origins[0].originPath | String  | 원본 서버 하위 경로                                          |
| distribution.origins[0].httpPort   | Integer | 원본 서버 HTTP 프로토콜 포트                                               |
| distribution.origins[0].httpsPort  | Integer | 원본 서버 HTTPS 프로토콜 포트                                               |
| distribution.forwardHostHeader     | String  | 서비스 배포 처리 결과를 통보받을 콜백                        |
| distribution.useOriginHttpProtocolDowngrade | Boolean | 원본 서버가 HTTP 응답만 가능한 경우, CDN 서버에서 원본 서버로 요청 시 HTTPS 요청을 HTTP 요청으로 다운그레이드하기 위한 설정 사용 여부 |
| distribution.forwardHostHeader     | String  | CDN 서버가 원본 서버로 콘텐츠 요청 시 전달 할 호스트 헤더 설정("ORIGIN_HOSTNAME": 원본 서버의 호스트 이름으로 설정, "REQUEST_HOST_HEADER": 클라이언트 요청의 호스트헤더로 설정 |
| distribution.callback              | Object  | 서비스 배포 처리 결과를 통보받을 콜백                        |
| distribution.callback.httpMethod   | String  | 콜백의 HTTP 메서드                                           |
| distribution.callback.url          | String  | 콜백 URL                                                     |
