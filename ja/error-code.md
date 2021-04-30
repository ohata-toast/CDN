
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
| 10010 | Origin domain is requried. | 原本サーバーは必須入力事項です。 | 
| 10011 | Please enter origin domain without scheme. | 原本サーバーにはhttp://などのスキーマ(scheme)情報を入力できません。 | 
| 10012 | Please enter the origin domain in public IP or domain format. | 原本サーバー形式が正しくありません。ドメインまたはIP形式で入力してください。 | 
| 10013 | Domain alias is invalid. | Domain Alias形式が正しくありません。 | 
| 10014 | Referrers is invalid. Enter referrers regular expression. | リファラー(referrer)形式が正しくありません。有効な正規表現かを確認してください。 |
| 10015 | Please enter use orgin 'Y' or 'N'. | キャッシュ満了設定の原本設定を使用するかは、'Y'または'N'を入力してください。  |
| 10016 | Please enter origin. | 原本サーバー設定がないか、正しくありません。 |
| 10017 | Invalid Purge Type. Please enter purge type 'ITEM' or 'ALL' | 再配布(Purge)タイプが正しくありません。タイプは'ITEM'または'ALL'を入力してください。 |
| 10018 | Invalid Origin Port. Please enter a number between 0 and 65,536. | 原本サーバーのポートが正しくありません。0～65,536の数字を入力してください。 |
| 10019 | Up to three domain aliases can be registered. | 최대 3개의 도메인 별칭까지 등록이 가능합니다. |
| 10020 | The requested domain alias has duplicated values. | 이미 등록되어있는 도메인 별칭으로, 신규로 등록이 불가합니다. |
| 10021 | Already deleted distribution. | 이미 삭제된 CDN 서비스 입니다. |
| 10023 | The port field cannot be set with either httpPort or httpsPort. | port와 httpPort, httpsPort는 동시에 입력할 수 없습니다. port를 입력하거나 혹은 httpPort와 httpsPort를 입력해주세요.  |
| 10024 | The origin server port is not set. | origin server port는 필수 입력값입니다. port 혹은 httpPort 혹은 httpsPort 항목에 원본 서버 포트를 입력해주세요. |
| 10025 | Invalid origin port. The httpPort equals httpsPort. | httpPort와 httpsPort 입력값을 다르게 입력해주세요. |
| 10027 | The description's maximum length is limited to 255 characters. | CDN 서비스의 설명은 최대 255자까지 입력 가능합니다. |
| 10028 | The Root Path Control 'controlType' is not set. Please enter type 'DENY' or 'REDIRECT' | controlType은 필수 입력값 입니다. 'DENY' 또는 'REDIRECT'를 입력해주세요. |
| 10029 | The Root Path Control 'redirectPath' is not set. | controlType이 'REDIRECT'일 경우, redirectPath는 필수 입력값 입니다. |
| 10030 | 'redirectPath' must starts with a slash('/). | redirectPath는 '/'부터 입력해주세요. |
| 10031 | The Root Path Control 'redirectStatusCode' is not set. Please enter Code 301 or 302 or 303 or 307 | controlType이 'REDIRECT'일 경우, redirectStatusCode는 필수 입력값 입니다. 301, 302, 303, 307 중에 입력해주세요. |
| 10032 | Invalid 'redirectStatusCode'. Please enter Code 301 or 302 or 303 or 307 | redirectStatusCode 입력값이 올바르지 않습니다. 301, 302, 303, 307 중에 입력해주세요.  |
| 20001 | Failed to authenticate with AppKey. | AppKeyが有効ではありません。 |
| 20002 | Failed to authenticate with the secret key. | SecretKeyが有効ではありません。 |
| 30000 | Failed to process request. Please try again later. | 要求された処理に失敗しました。 |
| 30001 | Purge usage has exceeded. Please try again later. | 再配布使用量を越えました。 |
| 30002 | Data not created or does not exist. Please try again later. | データが作成されていないか、存在しません。 |
| 30003 | Request not processed in current service status. | 現在のサービス状態で処理できない要求です。 |
| 30004 | The status of the service is unknown. | 不明なサービス状態です。 |
| 30005 | Resource not found. | リソースが見つかりません。 |
| 80500 | An unknown error occurred during processing. Please contact the service provider. | 処理中に不明なエラーが発生しました。サービス提供者にお問い合わせください。
