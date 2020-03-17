## Content Delivery > CDN > API v2.0ガイド

TOAST CDNで提供するPublic API v2.0について説明します。

## API共通情報

### ドメイン

| 名前        | ドメイン                             |
| --------------- | ------------------------------------- |
| CDN Public APIドメイン | https://api-gw.cloud.toast.com/tc-cdn |

### 事前準備

APIを使用するにはアプリキー(Appkey)とセキュリティキー(SecretKey)が必要です。
アプリキーとセキュリティキーは、コンソール右上の**URL & Appkey**メニューで確認できます。

### リクエスト共通情報

#### リクエストヘッダ

| 名前      | 説明                  |
| ------------- | ------------------------- |
| Authorization | コンソールで発行されたセキュリティキー(SecretKey) |

#### Pathパラメータ

すべてのAPIは、pathパラメータにappKeyを指定する必要があります。
* 例) /v2.0/appKeys/**{appKey}**/distributions

| 名前 | 説明              |
| ------ | --------------------- |
| appKey | コンソールで発行されたアプリキー(Appkey) | 

### レスポンス共通情報

#### ヘッダ

すべてのAPIリクエストに対して、**200 OK**でレスポンスします。詳細なレスポンス結果は、次の例のようなレスポンス本文のヘッダを参照してください。

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "Success"
    }
}
```


[フィールド]

| フィールド             | タイプ | 説明 |
| -------------------- | ------- | ------ |
| header               | Object  | ヘッダ領域  |
| header.isSuccessful  | Boolean | 成否 |
| header.resultCode | Integer | 結果コード |
| header.resultMessage | String | 結果メッセージ |

#### CDN状態コード

次はCDNサービスの状態を表す状態コードです。サービスの照会時にサービスの状態を確認できます。

| 値   | 説明               |
| ---------- | ------------------------ |
| OPENING    | サービス起動中     |
| OPEN       | サービス中          |
| MODIFYING  | 修正中            |
| RESUME     | 開始               |
| SUSPENDING | 停止進行中       |
| SUSPEND    | 停止               |
| CLOSING    | 使用終了中       |
| CLOSE      | 使用終了          |
| ERROR      | サービス作成中にエラーが発生 |


## サービスAPI

### サービス作成

#### リクエスト


[URI]

| メソッド | URI                                  |
| ---- | ------------------------------------ |
| POST | /v2.0/appKeys/{appKey}/distributions |


[リクエスト本文]

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

[フィールド]

| 名前                             | タイプ | 必須か | デフォルト値 | 有効範囲             | 説明                                                   |
| -------------------------------------- | ------- | --------- | ------ | --------------------------- | ------------------------------------------------------------ |
| distributions                          | List    | 必須  |        |                              | 作成するCDNのオブジェクトリスト                               |
| distributions[0].useOriginHttpProtocolDowngrade | Boolean  | 必須 |        | true/false         | オリジンサーバーがHTTPレスポンスのみ可能な場合、CDNサーバーからオリジンサーバーにリクエストする時、HTTPSリクエストからHTTPリクエストにダウングレードするための設定を使用するかどうか |
| distributions[0].forwardHostHeader     | String  | 必須  |        | ORIGIN_HOSTNAME/REQUEST_HOST_HEADER   | CDNサーバーがオリジンサーバーにコンテンツをリクエストする時、伝達するホストヘッダ設定("ORIGIN_HOSTNAME"：オリジンサーバーのホストネームで設定、"REQUEST_HOST_HEADER"：クライアントリクエストのホストヘッダで設定 |
| distributions[0].useOriginCacheControl | Boolean | 必須  |        | true/false                  | キャッシュ満了設定(true：オリジンサーバー設定を使用、false：ユーザー設定)   |
| distributions[0].referrerType          | String  | 必須   |        | BLACKLIST/WHITELIST         | リファラーアクセス管理("BLACKLIST"：ブラックリスト、"WHITELIST"：ホワイトリスト) |
| distributions[0].referrers             | List    | 任意  |        |                           | 正規表現の形式のリファラーヘッダリスト |
| distributions[0].description           | String  | 任意   |        | 最大255文字            | 説明                                                  |
| distributions[0].domainAlias           | List    | 任意  |        |                           | ドメインエイリアスリスト(個人または会社が所有しているドメインを使用) |
| distributions[0].defaultMaxAge         | Integer | 任意  | 0      | 0～2,147,483,647             | キャッシュ満了時間(秒)、デフォルト値0は604,800秒です。             |
| distributions[0].origins               | List    | 必須   |        |                             | オリジンサーバーオブジェクトリスト                               |
| distributions[0].origins[0].origin     | String  | 必須   |        | 最大255文字            | オリジンサーバー(domainまたはIP)                                     |
| distributions[0].origins[0].originPath | String  | 任意   |        | 最大8192文字           | オリジンサーバーの下層パス(/を含むパスで入力してください。)        |
| distributions[0].origins[0].httpPort   | Integer  | 任意  |        |[コンソール使用ガイド] > [[オリジンサーバー](./console-guide/#_2)の[表2]使用可能なオリジンサーバーポート番号]参照 | オリジンサーバーHTTPプロトコルポート(origins[0].httpPortとorigins[0].httpsPortのいずれか1つは必ず入力する必要があります。)  |
| distributions[0].origins[0].httpsPort   | Integer  | 任意  |        |[コンソール使用ガイド] > [[オリジンサーバー](./console-guide/#_2)の[表2]使用可能なオリジンサーバーポート番号]参照 | オリジンサーバーHTTPSプロトコルポート(origins[0].httpPortとorigins[0].httpsPortのいずれか1つは必ず入力する必要があります。)  |
| distributions[0].callback              | Object  | 任意   |        |                             | CDN作成処理結果の通知を受けるコールバックURL(コールバック設定は任意入力です。) |
| distributions[0].callback.httpMethod   | String  | 必須   |        | GET/POST/PUT                | コールバックのHTTP Method                                           |
| distributions[0].callback.url          | String  | 必須  |        | 最大1024文字             | コールバックURL                                                     |




#### レスポンス


[レスポンス本文]

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


[フィールド]

| フィールド                            | タイプ | 説明                                                  |
| -------------------------------------- | ------- | ------------------------------------------------------------ |
| header                                 | Object  | ヘッダ領域                                             |
| header.isSuccessful                    | Boolean | 成否                                              |
| header.resultCode                      | Integer | 結果コード                                             |
| header.resultMessage                   | String  | 結果メッセージ                                           |
| distributions                          | List    | 作成されたCDNオブジェクトリスト                            |
| distributions[0].domain                | String  | 作成されたドメイン(サービス)名                            |
| distributions[0].domainAlias           | List    | ドメインエイリアスリスト(個人または会社が所有しているドメインを使用)              |
| distributions[0].region                | String  | サービス地域("GLOBAL"：グローバル)            |
| distributions[0].description           | String  | 説明                                                  |
| distributions[0].status                | String  | CDN状態コード([表] CDN状態コード参照)                                 |
| distributions[0].defaultMaxAge         | Integer  | キャッシュ満了時間(秒)                                           |
| distributions[0].referrerType          | String  | リファラーアクセス管理("BLACKLIST"：ブラックリスト、"WHITELIST"：ホワイトリスト) |
| distributions[0].referrers             | List    | 正規表現形式のリファラーヘッダリスト                              |
| distributions[0].useOriginCacheControl | Boolean  | オリジンサーバー設定を使用するか(true：オリジンサーバー設定を使用、false：ユーザー設定) |
| distributions[0].origins               | List    | オリジンサーバーオブジェクトリスト                               |
| distributions[0].origins[0].origin     | String  | オリジンサーバー(domainまたはIP)                                      |
| distributions[0].origins[0].originPath | String  | オリジンサーバーの下層パス                                   |
| distributions[0].origins[0].httpPort   | Integer | オリジンサーバーHTTPプロトコルポート                                           |
| distributions[0].origins[0].httpsPort  | Integer | オリジンサーバーHTTPSプロトコルポート                                           |
| distributions[0].useOriginHttpProtocolDowngrade | Boolean | オリジンサーバーがHTTPレスポンスのみ可能な場合、CDNサーバーからオリジンサーバーにリクエストする時、HTTPSリクエストからHTTPリクエストにダウングレードするための設定を使用するか |
| distributions[0].forwardHostHeader     | String  | CDNサーバーがオリジンサーバーにコンテンツをリクエストする時、伝達するホストヘッダ設定("ORIGIN_HOSTNAME"：オリジンサーバーのホストネームで設定、"REQUEST_HOST_HEADER"：クライアントリクエストのホストヘッダで設定 |
| distributions[0].callback              | Object  | サービス作成処理結果の通知を受けるコールバック                 |
| distributions[0].callback.httpMethod   | String  | コールバックのHTTP Method                                           |
| distributions[0].callback.url          | String  | コールバックURL                                                     |



### サービス照会

#### リクエスト


[URI]

| メソッド | URI                                  |
| ---- | ------------------------------------ |
| GET  | /v2.0/appKeys/{appKey}/distributions |


[パラメータ]

| 名前 | タイプ | 必須か | 有効範囲 | 説明                  |
| ------ | ------ | --------- | ------------- | ---------------------------- |
| domain | String | 任意   | 最大255文字 | 照会するドメイン(サービス名)   |
| status | String | 任意   | CDN状態コード | CDN状態コード([表] CDN状態コード参照) |

[例]
```
curl -X GET "https://api-gw.cloud.toast.com/tc-cdn/v2.0/appKeys/{appKey}/distributions?domain={domain}" \
 -H "Authorization: {secretKey}" \
 -H "Content-Type: application/json"
```

#### レスポンス


[レスポンス本文]

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


[フィールド]

| フィールド                            | タイプ | 説明                                                  |
| -------------------------------------- | ------- | ------------------------------------------------------------ |
| header                                 | Object  | ヘッダ領域                                             |
| header.isSuccessful                    | Boolean | 成否                                              |
| header.resultCode                      | Integer | 結果コード                                             |
| header.resultMessage                   | String  | 結果メッセージ                                           |
| distributions                          | List    | 作成されたCDNオブジェクトリスト                              |
| distributions[0].domain                | String  | ドメイン名(サービス名)                                     |
| distributions[0].domainAlias           | List  | ドメインエイリアスリスト(個人または会社が所有しているドメインを使用)                                                  |
| distributions[0].region                | String  | サービス地域("GLOBAL"：グローバル)             |
| distributions[0].status                | String  | CDN状態コード([表] CDN状態コード参照)                                 |
| distributions[0].defaultMaxAge         | Integer  | キャッシュ満了時間(秒)                                           |
| distributions[0].referrerType          | String  | リファラーアクセス管理("BLACKLIST"：ブラックリスト、"WHITELIST"：ホワイトリスト) |
| distributions[0].referrers             | List    | 正規表現形式のリファラーヘッダリスト                             |
| distributions[0].useOriginCacheControl | Boolean | オリジンサーバー設定を使用するか(true：オリジンサーバー設定を使用、false：ユーザー設定) |
| distributions[0].origins               | List    | オリジンサーバーオブジェクトリスト                               |
| distributions[0].origins[0].origin     | String  | オリジンサーバー(domainまたはIP)                                      |
| distributions[0].origins[0].originPath | String  | オリジンサーバーの下層パス                                   |
| distributions[0].origins[0].httpPort   | Integer | オリジンサーバーHTTPプロトコルポート                              |
| distributions[0].origins[0].httpsPort  | Integer | オリジンサーバーHTTPSプロトコルポート                             |
| distributions[0].forwardHostHeader     | String  | サービス配布処理結果を受け取るコールバック                    |
| distributions[0].useOriginHttpProtocolDowngrade | Boolean | オリジンサーバーがHTTPレスポンスのみ可能な場合、CDNサーバーからオリジンサーバーにリクエストする時、HTTPSリクエストからHTTPリクエストにダウングレードするための設定を使用するか |
| distributions[0].forwardHostHeader     | String  | CDNサーバーがオリジンサーバーにコンテンツをリクエストする時、伝達するホストヘッダ設定("ORIGIN_HOSTNAME"：オリジンサーバーのホストネームで設定、"REQUEST_HOST_HEADER"：クライアントリクエストのホストヘッダで設定 |
| distributions[0].callback              | Object  | サービス配布処理結果の通知を受けるコールバック                 |
| distributions[0].callback.httpMethod   | String  | コールバックのHTTP Method                                           |
| distributions[0].callback.url          | String  | コールバックURL                                                     |


### サービス修正

#### リクエスト


[URI]

| メソッド | URI                                  |
| ---- | ------------------------------------ |
| PUT  | /v2.0/appKeys/{appKey}/distributions |


[リクエスト本文]

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


[フィールド]

| 名前           | タイプ | 必須か | デフォルト値 | 有効範囲                                             | 説明                                                  |
| --------------------- | ------- | --------- | ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| domain                | String  | 必須   |        | 最大255文字                                             | 修正するドメイン(サービス名)                                   |
| useOriginCacheControl | Boolean | 必須  |        | true/false                                                        | キャッシュ満了設定(true：オリジンサーバー設定を使用、false：ユーザー設定)      |
| referrerType          | String  | 必須   |        | BLACKLIST/WHITELIST                                          | リファラーアクセス管理("BLACKLIST"：ブラックリスト、"WHITELIST"：ホワイトリスト) |
| referrers             | List    | 任意  |        |                                                              | 正規表現形式のリファラーヘッダリスト |
| description           | String  | 任意   |        | 最大255文字                                             | 説明                                                  |
| domainAlias           | List    | 任意  |        | 最大255文字                                               | ドメインエイリアス(個人または会社が所有しているドメインを使用) |
| defaultMaxAge         | Integer | 任意  | 0      | 0 ～ 2,147,483,647                                            | キャッシュ満了時間(秒)、デフォルト値0は604,800秒です。              |
| origins               | List    | 必須   |        |                                                              | オリジンサーバー                                             |
| origins[0].origin     | String  | 必須   |        | 最大255文字                                             | オリジンサーバー(domainまたはIP)                                      |
| origins[0].originPath | String  | 任意   |        | 最大8192文字                                            | オリジンサーバーの下層パス                                   |
| origins[0].httpPort   | Integer  | 任意  |        |[コンソール使用ガイド] > [[オリジンサーバー](./console-guide/#_2)の[表2]使用可能なオリジンサーバーポート番号]参照 | オリジンサーバーHTTPプロトコルポート(origins[0].httpPortとorigins[0].httpsPortのいずれか1つは必ず入力する必要があります。)  |
| origins[0].httpsPort  | Integer  | 任意  |        |[コンソール使用ガイド] > [[オリジンサーバー](./console-guide/#_2)の[表2]使用可能なオリジンサーバーポート番号]参照 | オリジンサーバーHTTPSプロトコルポート(origins[0].httpPortとorigins[0].httpsPortのいずれか1つは必ず入力する必要があります。) |
| useOriginHttpProtocolDowngrade | Boolean  | 必須 | true/false       |          | オリジンサーバーがHTTPレスポンスのみ可能な場合、CDNサーバーからオリジンサーバーにリクエストする時、HTTPSリクエストからHTTPリクエストにダウングレードするための設定を使用するか |
| forwardHostHeader     | String  | 必須  |        | ORIGIN_HOSTNAME, REQUEST_HOST_HEADER   | CDNサーバーがオリジンサーバーにコンテンツをリクエストする時、伝達するホストヘッダ設定("ORIGIN_HOSTNAME"：オリジンサーバーのホストネームで設定、"REQUEST_HOST_HEADER"：クライアントリクエストのホストヘッダで設定 |
| callback              | Object  | 任意   |        | CDNサービス配布結果の通知を受けるコールバックURL(コールバック設定は任意入力です。) |                                                              |
| callback.httpMethod   | String  | 必須   |        | GET/POST/PUT                                                 | コールバックのHTTP Method                                           |
| callback.url          | String  | 必須  |        | 最大1024文字                                              | コールバックURL                                                     |


#### レスポンス


[レスポンス本文]

```json
{
    "header" : {
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS",
        "isSuccessful" :  true
    }
}
```


[フィールド]

| フィールド             | タイプ | 説明 |
| -------------------- | ------- | ------ |
| header               | Object  | ヘッダ領域  |
| header.isSuccessful  | Boolean | 成否 |
| header.resultCode | Integer | 結果コード |
| header.resultMessage | String | 結果メッセージ |

### サービス削除

#### リクエスト


[URI]

| メソッド | URI                                  |
| ------ | ------------------------------------ |
| DELETE | /v2.0/appKeys/{appKey}/distributions |


[リクエスト本文]

```json
{
    "domains" : [
        "lhcsxuo0.toastcdn.net"
    ]
}
```


[フィールド]

| 名前 | タイプ | 必須か | デフォルト値 | 有効範囲 | 説明             |
| ------- | ------ | ----- | ---- | ----- | --------------------- |
| domains | String | 必須 |      |       | 削除するドメイン、複数ドメイン入力可 |

**\* 複数のドメイン入力時、該当サービスはすべて終了します。**

#### レスポンス


[レスポンス本文]

```json
{
    "header" : {
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS",
        "isSuccessful" :  true
    }
}
```


[フィールド]

| フィールド             | タイプ | 説明 |
| -------------------- | ------- | ------ |
| header               | Object  | ヘッダ領域  |
| header.isSuccessful  | Boolean | 成否 |
| header.resultCode | Integer | 結果コード |
| header.resultMessage | String | 結果メッセージ |


## キャッシュ再配布API

### キャッシュ再配布(Purge) -  ITEM(特定ファイルタイプ)

#### リクエスト

[URI]

| メソッド | URI                           |
| ---- | ----------------------------- |
| POST | /v2.0/appKeys/{appKey}/purge/item |


[リクエスト本文]

```json
{
	"domain": "sample.toastcdn.net",
	"purgeList":["http://sample.toastcdn.net/img_01.png",
  "http://sample.toastcdn.net/img_02.png"]
}
```


[フィールド]

| 名前 | タイプ | 必須か | デフォルト値 | 有効範囲      | 説明                                                  |
| --------- | ------ | --------- | ------ | --------------------- | ------------------------------------------------------------ |
| domain    | String | 必須   |        | 最大255文字      | 再配布するドメイン(サービス)名                          |
| purgeList | List | 必須  |        |                       | 再配布対象URLリスト |

#### レスポンス

[レスポンス本文]

```json
{
    "header" : {
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS",
        "isSuccessful" :  true
    }
}
```


[フィールド]

| フィールド            | タイプ | 説明 |
| -------------------- | ------- | --------- |
| header               | Object  | ヘッダ領域  |
| header.isSuccessful  | Boolean | 成否  |
| header.resultCode    | Integer | 結果コード  |
| header.resultMessage | String  | 結果メッセージ |

### キャッシュ再配布(Purge) -  ALL(全体ファイルタイプ)

#### リクエスト

[URI]

| メソッド | URI                           |
| ---- | ----------------------------- |
| POST | /v2.0/appKeys/{appKey}/purge/all |


[リクエスト本文]

```json
{
	"domain": "sample.toastcdn.net"
}
```


[フィールド]

| 名前 | タイプ | 必須か | デフォルト値 | 有効範囲      | 説明                                                  |
| --------- | ------ | --------- | ------ | --------------------- | ------------------------------------------------------------ |
| domain    | String | 必須   |        | 最大255文字      | 再配布するドメイン(サービス)名                          |

#### レスポンス

[レスポンス本文]

```json
{
    "header" : {
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS",
        "isSuccessful" :  true
    }
}
```


[フィールド]

| フィールド            | タイプ | 説明 |
| -------------------- | ------- | --------- |
| header               | Object  | ヘッダ領域  |
| header.isSuccessful  | Boolean | 成否  |
| header.resultCode    | Integer | 結果コード  |
| header.resultMessage | String  | 結果メッセージ |

- CDNサービスを新規で作成した後、約1時間はキャッシュ再配布リクエストが失敗する場合もあります。その後も失敗が続く場合はサポートへお問い合わせください。
- パージAPI使用量制限ポリシーがあります。詳細な内容は[コンソール使用ガイド] > [[CDNキャッシュ再配布](./console-guide/#cdn_4)のキャッシュ再配布使用量制限]を確認してください。

### キャッシュ再配布(Purge)照会
- API v2.0を通してキャッシュを再配布する時、高速キャッシュ再配布が実行され、リクエスト後、数秒以内に完了するため、キャッシュ再配布状態を照会するAPIが別途提供されません。

## コールバックレスポンス
CDNサービスにコールバック機能が設定されている場合、作成/修正/一時停止/再開/削除の変更完了時、コールバックURLに下記のようなレスポンス値を伝達します。

[レスポンス本文]
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

[フィールド]

| フィールド                            | タイプ | 説明                                                  |
| -------------------------------------- | ------- | ------------------------------------------------------------ |
| header                                 | Object  | ヘッダ領域                                             |
| header.isSuccessful                    | Boolean | 成否                                              |
| header.resultCode                      | Integer | 結果コード                                             |
| header.resultMessage                   | String  | 結果メッセージ                                           |
| distribution                          | Object    | 変更作業が完了したCDNオブジェクト                               |
| distribution.appKey                   | String    | アプリケーションキー                              |
| distribution.domain                | String  | ドメイン名(サービス名)                                     |
| distribution.domainAlias           | List  | ドメインエイリアスリスト(個人または会社が所有しているドメインを使用)                                 |
| distribution.region                | String  | サービス地域("GLOBAL"：グローバル)             |
| distribution.status                | String  | CDNステータスコード([表] CDNステータスコード参照)                                 |
| distribution.defaultMaxAge         | Integer  | キャッシュ満了時間(秒)                                           |
| distribution.referrerType          | String  | リファラーアクセス管理("BLACKLIST"：ブラックリスト、"WHITELIST"：ホワイトリスト) |
| distribution.referrers             | List    | 正規表現形式のリファラーヘッダリスト                             |
| distribution.useOriginCacheControl | Boolean | オリジンサーバー設定を使用するか(true：オリジンサーバー設定を使用、false：ユーザーを設定) |
| distribution.createTime            | DateTime | 作成日時                                     |
| distribution.deleteTime            | DateTime | 削除日時                                     |
| distribution.origins               | List    | オリジンサーバーオブジェクトリスト                                  |
| distribution.origins[0].origin     | String  | オリジンサーバー(domainまたはIP)                                      |
| distribution.origins[0].originPath | String  | オリジンサーバー下層パス                                      |
| distribution.origins[0].httpPort   | Integer | オリジンサーバーHTTPプロトコルポート                                           |
| distribution.origins[0].httpsPort  | Integer | オリジンサーバーHTTPSプロトコルポート                                           |
| distribution.forwardHostHeader     | String  | サービス配布処理結果を受け取るコールバック                    |
| distribution.useOriginHttpProtocolDowngrade | Boolean | オリジンサーバーがHTTPレスポンスのみ可能な場合、CDNサーバーからオリジンサーバーにリクエストする時、HTTPSリクエストからHTTPリクエストにダウングレードするための設定を使用するか |
| distribution.forwardHostHeader     | String  | CDNサーバーがオリジンサーバーにコンテンツをリクエストする時に伝達するホストヘッダ設定("ORIGIN_HOSTNAME"：オリジンサーバーのホストネームで設定、"REQUEST_HOST_HEADER"：クライアントリクエストのホストヘッダで設定 |
| distribution.callback              | Object  | サービス配布処理結果を受け取るコールバック                    |
| distribution.callback.httpMethod   | String  | コールバックのHTTP Method                                           |
| distribution.callback.url          | String  | コールバックURL                                                     |
