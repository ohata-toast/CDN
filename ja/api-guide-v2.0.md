## Content Delivery > CDN > API v2.0ガイド

NHN Cloud CDNで提供するPublic API v2.0について説明します。

## API共通情報

### ドメイン

| 名前        | ドメイン                             |
| --------------- | ------------------------------------- |
| CDN Public APIドメイン | https://kr1-cdn.api.nhncloudservice.com |

### 事前準備

APIを使用するにはアプリキー(Appkey)とセキュリティキー(SecretKey)が必要です。
アプリキーとセキュリティキーは、コンソール右上の**URL & Appkey**メニューで確認できます。

### リクエスト共通情報

#### リクエストヘッダ

| 名前      | 説明                  |
| ------------- | ------------------------- |
| Authorization | コンソールで発行されたセキュリティキー(SecretKey) |

#### Pathパラメータ

すべてのAPIは、pathパラメータにアプリキーを指定する必要があります。
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

#### 証明書発行ステータスコード

以下はドメインの証明書発行ステータスを表すステータスコードで、証明書の照会時に発行ステータスを確認できます。

| 値    | 説明                |
| ---------- | ------------------------ |
| PENDING_NEW        | 証明書新規発行が要求され処理待機中 |
| PENDING_CANCEL     | 証明書の発行がキャンセル要求され、ドメイン検証キャンセル処理待機中 |
| PENDING_DELETE     | 発行された証明書が削除要求され、処理待機中 |
| PENDING_EXPIRE     | 発行された証明書が期限切れになり、期限切れ処理待機中 |
| VALIDATED          | ドメイン検証完了                |
| DEPLOYED           | 証明書の配布完了                |
| WAITING_VALIDATION | ドメイン検証待機中             |
| CANCELED           | ドメイン検証キャンセル完了            |
| DELETED            | ドメイン証明書の削除完了          |
| EXPIRED            | ドメイン証明書の期限切れ               |

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
      "cacheType": "BYPASS",    
      "defaultMaxAge": 86400,
      "cacheKeyQueryParam": "INCLUDE_ALL",
      "referrerType" : "BLACKLIST",      
      "referrers" : ["cloud.nhn.com"],
      "isAllowWhenEmptyReferrer" : true,
      "isAllowPost" : true,
      "isAllowPut" : false,
      "isAllowPatch" : true,
      "isAllowDelete" : false,
      "useLargeFileOptimization" : false,
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

[フィールド]

| 名前                             | タイプ | 必須か | デフォルト値 | 有効範囲             | 説明                                                   |
| -------------------------------------- | ------- | --------- | ------ | --------------------------- | ------------------------------------------------------------ |
| distributions                          | List    | 必須  |        |                              | 作成するCDNのオブジェクトリスト                               |
| distributions[0].useOriginHttpProtocolDowngrade | Boolean  | 必須 |        | true/false         | オリジンサーバーがHTTPレスポンスのみ可能な場合、CDNサーバーからオリジンサーバーにリクエストする時、HTTPSリクエストからHTTPリクエストにダウングレードするための設定を使用するかどうか |
| distributions[0].forwardHostHeader     | String  | 必須  |        | ORIGIN_HOSTNAME<br/>REQUEST_HOST_HEADER   | CDNサーバーがオリジンサーバーにコンテンツをリクエストする時、伝達するホストヘッダ設定("ORIGIN_HOSTNAME"：オリジンサーバーのホスト名で設定、"REQUEST_HOST_HEADER"：クライアントリクエストのホストヘッダで設定 |
| distributions[0].useOriginCacheControl | Boolean | 任意    |        | true/false                  | キャッシュ期限設定(true：オリジンサーバー設定を使用、false：ユーザー設定を使用)。 useOriginCacheControlまたはcacheTypeのいずれかを必ず入力する必要があります。   |
| distributions[0].cacheType             | String  | 選択    |        | BYPASS, NO_STORE            | キャッシュタイプ設定。 useOriginCacheControlまたはcacheTypeのいずれかを必ず入力する必要があります。    |
| distributions[0].referrerType          | String  | 必須   |        | BLACKLIST/WHITELIST         | リファラーアクセス管理("BLACKLIST"：ブラックリスト、"WHITELIST"：ホワイトリスト) |
| distributions[0].referrers             | List    | 任意  |        |                           | 正規表現形式のリファラーヘッダリスト |
| distributions[0].isAllowWhenEmptyReferrer | Boolean | 任意      | true      | true/false             | リファラー ヘッダがない場合、コンテンツアクセス許可(true)/拒否(false)             |
| distributions[0].isAllowPost           | Boolean | 選択    | false      | true/false             | POSTメソッド許可(true)/拒否(false)             |
| distributions[0].isAllowPut            | Boolean | 選択    | false      | true/false             | PUTメソッド許可(true)/拒否(false)            |
| distributions[0].isAllowPatch          | Boolean | 選択    | false      | true/false             | PATCHメソッド許可(true)/拒否(false)            |
| distributions[0].isAllowDelete         | Boolean | 選択    | false      | true/false             | DELETEメソッド許可(true)/拒否(false)            |
| distributions[0].useLargeFileOptimization | Boolean | 選択    | false      | true/false             | 大容量ファイル最適化設定使用有無    |
| distributions[0].description           | String  | 任意   |        | 最大255文字            | 説明                                                  |
| distributions[0].domainAlias           | List    | 任意  |        |                           | ドメインエイリアスリスト(個人または会社が所有しているドメインを使用) |
| distributions[0].defaultMaxAge         | Integer | 任意  | 0      | 0～2,147,483,647             | キャッシュ満了時間(秒)、デフォルト値0は604,800秒です。             |
| distributions[0].cacheKeyQueryParam    | String  | 任意    | INCLUDE_ALL | INCLUDE_ALL/EXCLUDE_ALL | キャッシュキーにリクエストクエリ文字列を含めるかの設定("INCLUDE_ALL"：全て含める、"EXCLUDE_ALL"：全て含めない) |
| distributions[0].origins               | List    | 必須   |        |                             | オリジンサーバーオブジェクトリスト                               |
| distributions[0].origins[0].origin     | String  | 必須   |        | 最大255文字            | オリジンサーバー(ドメインまたはIP)                                     |
| distributions[0].origins[0].originPath | String  | 任意   |        | 最大8192文字           | オリジンサーバーの下層パス(/を含むパスで入力してください。)        |
| distributions[0].origins[0].httpPort   | Integer  | 任意     |        | [コンソール使用ガイド > オリジンサーバー](./console-guide/#_2)の「[表2]使用可能なオリジンサーバーポート番号」参照 | オリジンサーバーHTTPプロトコルポート(origins[0].httpPortとorigins[0].httpsPortのいずれか1つは必ず入力する必要があります。)  |
| distributions[0].origins[0].httpsPort  | Integer  | 任意     |        | [コンソール使用ガイド > オリジンサーバー](./console-guide/#_2)の「[表2]使用可能なオリジンサーバーポート番号」参照 | オリジンサーバーHTTPSプロトコルポート(origins[0].httpPortとorigins[0].httpsPortのいずれか1つは必ず入力する必要があります。)  |
| distributions[0].rootPathAccessControl  | Object  | 任意     |        |                             | CDNサービスのルートパスに対するアクセス制御設定 | 
| distributions[0].rootPathAccessControl.enable | Boolean | 必須     | true      | true/false             | ルートパスのアクセス制御使用(true)/未使用(false)          |
| distributions[0].rootPathAccessControl.controlType  | String  | 任意     |        | DENY, REDIRECT    | enableがtrueの場合は必須入力。ルートパスへのアクセス制御方式("DENY"：アクセス拒否、"REDIRECT"：指定したパスへリダイレクトリダイレクト) | 
| distributions[0].rootPathAccessControl.redirectPath | String | 任意     |       |       |   controlTypeが"REDIRECT"の場合は必須入力。ルートパスへのリクエストをリダイレクトするパス(/を含めたパスで入力してください。)        |
| distributions[0].rootPathAccessControl.redirectStatusCode | Integer | 任意     |       | 301, 302, 303, 307             |  controlTypeが"REDIRECT"の場合は必須入力。 リダイレクトする時に伝達されるHTTPレスポンスコード         |
| distributions[0].callback              | Object  | 任意   |        |                             | CDN作成処理結果の通知を受けるコールバックURL(コールバック設定は任意入力です。) |
| distributions[0].callback.httpMethod   | String  | 必須   |        | GET/POST/PUT                | コールバックのHTTPメソッド                                           |
| distributions[0].callback.url          | String  | 必須  |        | 最大1024文字             | コールバックURL                                                     |

- forwardHostHeaderのデフォルト値は、domainAliasを設定した場合はREQUEST_HOST_HEADERで、未設定の場合はORIGIN_HOSTNAMEです。


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
            "cacheKeyQueryParam": "INCLUDE_ALL",
            "referrerType": "BLACKLIST",
            "referrers": [
                "cloud.nhn.com"
            ],
            "isAllowWhenEmptyReferrer" : true,
            "isAllowPost" : true,
            "isAllowPut" : false,
            "isAllowPatch" : true,
            "isAllowDelete" : false,
            "useLargeFileOptimization" : false,
            "useOriginCacheControl": true,
            "cacheType": "BYPASS",
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
| distributions[0].cacheKeyQueryParam    | String  | キャッシュキーにリクエストクエリ文字列を含めるかの設定("INCLUDE_ALL"：全て含める、"EXCLUDE_ALL"：全て含めない) |
| distributions[0].referrerType          | String  | リファラーアクセス管理("BLACKLIST"：ブラックリスト、"WHITELIST"：ホワイトリスト) |
| distributions[0].referrers             | List    | 正規表現形式のリファラーヘッダリスト                              |
| distributions[0].isAllowWhenEmptyReferrer | Boolean | リファラーヘッダがない場合のコンテンツアクセス許可(true)/拒否(false) |
| distributions[0].isAllowPost | Boolean | POSTメソッド許可(true)/拒否(false)            |
| distributions[0].isAllowPut | Boolean | PUTメソッド許可(true)/拒否(false)            |
| distributions[0].isAllowPatch | Boolean | PATCHメソッド許可(true)/拒否(false)            |
| distributions[0].isAllowDelete | Boolean | DELETEメソッド許可(true)/拒否(false)            |
| distributions[0].useLargeFileOptimization | Boolean | 大容量ファイル最適化設定使用有無    |
| distributions[0].useOriginCacheControl | Boolean  | オリジンサーバー設定を使用するか(true：オリジンサーバー設定を使用、 false：ユーザー設定を使用) |
| distributions[0].cacheType             | String  | キャッシュタイプ設定                                        |
| distributions[0].origins               | List    | オリジンサーバーオブジェクトリスト                               |
| distributions[0].origins[0].origin     | String  | オリジンサーバー(ドメインまたはIP)                                      |
| distributions[0].origins[0].originPath | String  | オリジンサーバーの下層パス                                   |
| distributions[0].origins[0].httpPort   | Integer | オリジンサーバーHTTPプロトコルポート                                           |
| distributions[0].origins[0].httpsPort  | Integer | オリジンサーバーHTTPSプロトコルポート                                           |
| distributions[0].useOriginHttpProtocolDowngrade | Boolean | オリジンサーバーがHTTPレスポンスのみ可能な場合、CDNサーバーからオリジンサーバーにリクエストする時、HTTPSリクエストからHTTPリクエストにダウングレードするための設定を使用するか |
| distributions[0].forwardHostHeader     | String  | CDNサーバーがオリジンサーバーにコンテンツをリクエストする時、伝達するホストヘッダ設定("ORIGIN_HOSTNAME"：オリジンサーバーのホスト名で設定、"REQUEST_HOST_HEADER"：クライアントリクエストのホストヘッダで設定 |
| distributions[0].rootPathAccessControl  | Object  | CDNサービスのルートパスに対するアクセス制御設定 | 
| distributions[0].rootPathAccessControl.enable | Boolean | ルートパスに対するアクセス制御使用(true)/未使用(false)          |
| distributions[0].rootPathAccessControl.controlType  | String  | enableがtrueの場合は必須入力。ルートパスに対するアクセス制御方式("DENY"：アクセス拒否、"REDIRECT"：指定したパスにリダイレクト) | 
| distributions[0].rootPathAccessControl.redirectPath | String | controlTypeが"REDIRECT"の場合は必須入力。ルートパスに対するリクエストをリダイレクトするパス(/を含めたパスで 入力してください。)        |
| distributions[0].rootPathAccessControl.redirectStatusCode | Integer | controlTypeが"REDIRECT"の場合は必須入力。 リダイレクトする時に伝達されるHTTPレスポンスコード         |
| distributions[0].callback              | Object  | サービス作成処理結果の通知を受けるコールバック                 |
| distributions[0].callback.httpMethod   | String  | コールバックのHTTPメソッド                                           |
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
curl -X GET "https://kr1-cdn.api.nhncloudservice.com/v2.0/appKeys/{appKey}/distributions?domain={domain}" \
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
    "cacheKeyQueryParam": "INCLUDE_ALL",
    "status" :  "OPENING",
    "referrerType" :  "BLACKLIST",
    "referrers" :  ["test.com"],
    "isAllowWhenEmptyReferrer" : true,
    "isAllowPost" : true,
    "isAllowPut" : false,
    "isAllowPatch" : true,
    "isAllowDelete" : false,
    "useLargeFileOptimization" : false,
    "useOriginCacheControl" :  false,
    "cacheType": "NO_STORE",
    "origins" : [
        {
            "origin" :  "static.resource.com",
            "httpPort" :  80,
            "httpsPort" : 443
        }
    ],
    "forwardHostHeader": "ORIGIN_HOSTNAME",
    "useOriginHttpProtocolDowngrade": false,
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
| distributions[0].cacheKeyQueryParam    | String  | キャッシュキーにリクエストクエリ文字列を含めるかの設定("INCLUDE_ALL"：全て含める、"EXCLUDE_ALL"：全て含めない) |
| distributions[0].referrerType          | String  | リファラーアクセス管理("BLACKLIST"：ブラックリスト、"WHITELIST"：ホワイトリスト) |
| distributions[0].referrers             | List    | 正規表現形式のリファラーヘッダリスト                             |
| distributions[0].isAllowWhenEmptyReferrer | Boolean | リファラーヘッダがない場合のコンテンツアクセス許可(true)/拒否(false) |
| distributions[0].isAllowPost          | Boolean | POSTメソッド許可(true)/拒否(false)            |
| distributions[0].isAllowPut           | Boolean | PUTメソッド許可(true)/拒否(false)            |
| distributions[0].isAllowPatch         | Boolean | PATCHメソッド許可(true)/拒否(false)            |
| distributions[0].isAllowDelete        | Boolean | DELETEメソッド許可(true)/拒否(false)            |
| distributions[0].useLargeFileOptimization | Boolean | 大容量ファイル最適化設定使用有無    |
| distributions[0].useOriginCacheControl | Boolean | オリジンサーバー設定を使用するか(true：オリジンサーバー設定を使用、 false：ユーザー設定を使用) |
| distributions[0].cacheType             | String  | キャッシュタイプ設定                                        |
| distributions[0].origins               | List    | オリジンサーバーオブジェクトリスト                               |
| distributions[0].origins[0].origin     | String  | オリジンサーバー(ドメインまたはIP)                                      |
| distributions[0].origins[0].originPath | String  | オリジンサーバーの下層パス                                   |
| distributions[0].origins[0].httpPort   | Integer | オリジンサーバーHTTPプロトコルポート                              |
| distributions[0].origins[0].httpsPort  | Integer | オリジンサーバーHTTPSプロトコルポート                             |
| distributions[0].forwardHostHeader     | String  | サービス配布処理結果を受け取るコールバック                    |
| distributions[0].useOriginHttpProtocolDowngrade | Boolean | オリジンサーバーがHTTPレスポンスのみ可能な場合、CDNサーバーからオリジンサーバーにリクエストする時、HTTPSリクエストからHTTPリクエストにダウングレードするための設定を使用するか |
| distributions[0].forwardHostHeader     | String  | CDNサーバーがオリジンサーバーにコンテンツをリクエストする時、伝達するホストヘッダ設定("ORIGIN_HOSTNAME"：オリジンサーバーのホスト名で設定、"REQUEST_HOST_HEADER"：クライアントリクエストのホストヘッダで設定 |
| distributions[0].rootPathAccessControl  | Object  | CDNサービスのルートパスに対するアクセス制御設定 | 
| distributions[0].rootPathAccessControl.enable | Boolean | ルートパスに対するアクセス制御使用(true)/未使用(false)          |
| distributions[0].rootPathAccessControl.controlType  | String  | enableがtrueの場合は必須入力。ルートパスに対するアクセス制御方式("DENY"：アクセス拒否、"REDIRECT"：指定したパスにリダイレクト) | 
| distributions[0].rootPathAccessControl.redirectPath | String | controlTypeが"REDIRECT"の場合は必須入力。ルートパスに対するリクエストをリダイレクトするパス(/を含めたパスで 入力してください。)        |
| distributions[0].rootPathAccessControl.redirectStatusCode | Integer | controlTypeが"REDIRECT"の場合は必須入力。 リダイレクトする時に伝達されるHTTPレスポンスコード         |
| distributions[0].callback              | Object  | サービス配布処理結果の通知を受けるコールバック                 |
| distributions[0].callback.httpMethod   | String  | コールバックのHTTPメソッド                                           |
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
      "cacheType": "BYPASS",
      "defaultMaxAge": 86400,
      "cacheKeyQueryParam": "INCLUDE_ALL",
      "referrerType" : "BLACKLIST",
      "referrers" : ["test.com"],
      "isAllowWhenEmptyReferrer" : true,
      "isAllowPost" : true,
      "isAllowPut" : false,
      "isAllowPatch" : true,
      "isAllowDelete" : false,
      "useLargeFileOptimization" : true,
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


[フィールド]

| 名前           | タイプ | 必須か | デフォルト値 | 有効範囲                                             | 説明                                                  |
| --------------------- | ------- | --------- | ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| domain                | String  | 必須   |        | 最大255文字                                             | 修正するドメイン(サービス名)                                   |
| useOriginCacheControl | Boolean | 選択    |        | true/false                                                        | キャッシュ期限設定(true：オリジンサーバー設定を使用、 false：ユーザー設定を使用). useOriginCacheControlまたはcacheTypeのいずれかを必ず入力する必要があります。      |
| cacheType             | String  | 選択    |        | BYPASS, NO_STORE            | キャッシュタイプ設定。 useOriginCacheControlまたはcacheTypeのいずれかを必ず入力する必要があります。                                          |
| referrerType          | String  | 必須   |        | BLACKLIST/WHITELIST                                          | リファラーアクセス管理("BLACKLIST"：ブラックリスト、"WHITELIST"：ホワイトリスト) |
| referrers             | List    | 任意  |        |                                                              | 正規表現形式のリファラーヘッダリスト |
| isAllowWhenEmptyReferrer | Boolean | 任意      | true      | true/false             | リファラー ヘッダがない場合、コンテンツアクセス許可(true)/拒否(false)             |
| isAllowPost           | Boolean | 選択    | false      | true/false             | POSTメソッド許可(true)/拒否(false)            |
| isAllowPut            | Boolean | 選択    | false      | true/false             | PUTメソッド許可(true)/拒否(false)            |
| isAllowPatch          | Boolean | 選択    | false      | true/false             | PATCHメソッド許可(true)/拒否(false)            |
| isAllowDelete         | Boolean | 選択    | false      | true/false             | DELETEメソッド許可(true)/拒否(false)            |
| useLargeFileOptimization | Boolean | 選択 | false      | true/false             | 大容量ファイル最適化設定使用有無    |
