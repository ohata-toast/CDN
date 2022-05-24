
## Content Delivery > CDN > エラーコード

| errorCode | errorMessage | 説明 |
| --- | --- | --- |
| 0 | SUCCESS | 処理成功 |
| -1 | FAIL | 処理失敗 |
| -2 | ASSERT | 処理失敗 |
| 10000 | Domain does not exist. | 存在しないサービスです。 |
| 10001 | There is no path to purge. | 再配布するパスを入力してください。 |
| 10002 | The HTTP Method or url of Callback URL is not valid. | Callback URLのHTTP MethodまたはURLが入力されていません。 |
| 10003 | Callback url format is not valid. | Callback URLのURL形式が正しくありません。 |
| 10004 | Exist a creating service. Please retry after it is finished. | 要求した作業を実行できない状態です。以前に要求した作業が完了した後に再度お試しください。
| 10006 | Not found resource | 要求したリソースが見つからないか、無効な要求情報です。 |
| 10007 | Alreay exists domain alias. | すでに他のCDNサービスドメインに登録されているドメインエイリアス(Domain Alias)です。 |
| 10008 | Origin path must starts with a slash('/). | ソースパスは'/'から入力してください。|
| 10009 | Origin path must not end with a slash('/). | ソースパスの最後の文字に'/'を入力できません。 |
| 10010 | Origin domain is required. | 原本サーバーは必須入力事項です。 | 
| 10011 | Please enter origin domain without scheme. | 原本サーバーには http:// などのスキーマ(scheme)情報を入力できません。 |
| 10012 | Please enter the origin domain in public IP or domain format. | 原本サーバー形式が正しくありません。ドメインまたはIP形式で入力してください。 |
| 10013 | Domain alias is invalid. | Domain Alias形式が正しくありません。 |
| 10014 | Referrers is invalid. Enter referrers regular expression. | リファラー(referrer)形式が正しくありません。有効な正規表現かを確認してください。 |
| 10015 | Please enter use orgin 'Y' or 'N'. | キャッシュ満了設定の原本設定を使用するかは、'Y'または'N'を入力してください。  |
| 10016 | Please enter origin. | 原本サーバー設定がないか、正しくありません。 |
| 10017 | Invalid Purge Type. Please enter purge type 'ITEM' or 'ALL' | 再配布(Purge)タイプが正しくありません。タイプは'ITEM'または'ALL'を入力してください。 |
| 10018 | Invalid Origin Port. Please enter a number between 0 and 65,536. | 原本サーバーのポートが正しくありません。0～65,536の数字を入力してください。 |
| 10019 | Up to three domain aliases can be registered. | 最大3個のドメインエイリアスまで登録できます。 |
| 10020 | The requested domain alias has duplicated values. | すでに登録されているドメインエイリアスのため登録できません。 |
| 10021 | Already deleted distribution. | 削除されているCDNサービスです。 |
| 10023 | The port field cannot be set with either httpPort or httpsPort. | portとhttpPort、httpsPortは同時に入力できません。portを入力sるか、httpPortとhttpsPortを入力してください。  |
| 10024 | The origin server port is not set. | origin server portは必須入力値です。PortまたはhttpPort、httpsPort項目にオリジンサーバーポートを入力してください。 |
| 10025 | Invalid origin port. The httpPort equals httpsPort. | httpPortとhttpsPortは別の値を入力してください。 |
| 10027 | The description's maximum length is limited to 255 characters. | CDNサービスの説明は最大255文字まで入力できます。 |
| 10028 | The Root Path Control 'controlType' is not set. Please enter type 'DENY' or 'REDIRECT' | controlTypeは必須入力値です。 'DENY'または'REDIRECT'を入力してください。 |
| 10029 | The Root Path Control 'redirectPath' is not set. | controlTypeが'REDIRECT'の場合、redirectPathは必須入力値です。 |
| 10030 | 'redirectPath' must starts with a slash('/). | redirectPathは'/'から入力してください。 |
| 10031 | The Root Path Control 'redirectStatusCode' is not set. Please enter Code 301 or 302 or 303 or 307 | controlTypeが'REDIRECT'の場合、redirectStatusCodeは必須入力値です。301、302、303、307のいずれかを入力してください。 |
| 10032 | Invalid 'redirectStatusCode'. Please enter Code 301 or 302 or 303 or 307 | redirectStatusCodeの入力値が正しくありません。301、302、303、307のいずれかを入力してください。  |
| 10033 | Auth Token 'encryptKey' is not set. | encryptKeyは必須入力値です。 NHN Cloud CDNコンソールに表示される[Auth Token認証制御管理 > トークン暗号化キー]値を入力してください。 |
| 10034 | Auth Token 'durationSeconds' is not set. | durationSecondsは必須入力値です。トークン有効時間として設定する値を秒単位で入力してください。 |
| 10035 | Invalid 'sessionId'. Please enter smaller than 36 bytes for the length of character string. | sessionId文字列の長さは36バイトを超えられません。 |
| 10036 | There is no path to generate token. Please enter 'singlePath' or 'singleWildcardPath' or 'multipleWildcardPath'. | singlePath、singleWildcardPath、multipleWildcardPathのうち1つ以上の値が必ず存在する必要があります。 |
| 10037 | Auth token paths ('singlePath', 'singleWildcardPath', 'multipleWildcardPath') must start with a slash(/). | singlePath、singleWildcardPath、multipleWildcardPathは(/)で始まる必要があります。 |
| 10038 | Invalid character exists. Since ! and ~ are used as reserved characters, do not include them in path or session ID. | !, ~文字は予約文字として使用されるため、パスまたはセッションIDに含められません。 |
| 20001 | Failed to authenticate with AppKey. | AppKeyが有効ではありません。 |
| 20002 | Failed to authenticate with the secret key. | SecretKeyが有効ではありません。 |
| 30000 | Failed to process request. Please try again later. | 要求された処理に失敗しました。 |
| 30001 | Purge usage has exceeded. Please try again later. | 再配布使用量を越えました。 |
| 30002 | Data not created or does not exist. Please try again later. | データが作成されていないか、存在しません。 |
| 30003 | Request not processed in current service status. | 現在のサービス状態で処理できない要求です。 |
| 30004 | The status of the service is unknown. | 不明なサービス状態です。 |
| 30005 | Resource not found. | リソースが見つかりません。 |
| 80500 | An unknown error occurred during processing. Please contact the service provider. | 処理中に不明なエラーが発生しました。サービス提供者にお問い合わせください。
