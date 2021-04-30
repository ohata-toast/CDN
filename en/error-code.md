## Content Delivery > CDN > Error Codes

| errorCode | errorMessage | Description |
| --- | --- | --- |
| 0 | SUCCESS | Successful |
| -1 | FAIL | Failed |
| -2 | ASSERT | Failed |
| 10000 | Domain does not exist. | Service does not exist. |
| 10001 | There is no path to purge. | Enter path to purge. |
| 10002 | The HTTP Method or url of Callback URL is not valid. | HTTP method or url of callback URL is not entered. |
| 10003 | Callback url format is not valid. | Callback url format is not valid. |
| 10004 | Exist a creating service. Please retry after it is finished. | Cannot execute requested task. Try again after previous request is completed. |
| 10006 | Not found resource | Resource is not found as requested, or the request is invalid. |
| 10007 | Alreay exists domain alias. | Domain Alias is already registered in another CDN service domain. |
| 10008 | Origin path must starts with a slash('/). | Start with '/' to enter origin path. |
| 10009 | Origin path must not end with a slash('/). | '/' is not allowed as the last character of the origin path. |
| 10010 | Origin domain is requried. | Origin server is required. |
| 10011 | Please enter origin domain without scheme. | Origin server cannot have scheme information, such as http://. |
| 10012 | Please enter the origin domain in public IP or domain format. | The origin server format is not valid: enter in domain or IP. |
| 10013 | Domain alias is invalid. | The Domain Alias format is not valid. |
| 10014 | Referrers is invalid. Enter referrers regular expression. | The referrer format is not valid: check if it is valid regular expression. |
| 10015 | Please enter use orgin 'Y' or 'N'. | Enter 'Y' or 'N'  regarding whether to use origins for cache expiration setting. |
| 10016 | Please enter origin. | Origin server setting is not available or invalid. |
| 10017 | Invalid Purge Type. Please enter purge type 'ITEM' or 'ALL' | The purge type is not valid: enter 'ITEM' or 'ALL'. |
| 10018 | Invalid Origin Port. Please enter a number between 0 and 65,536. | The origin server port is not valid: enter a number between 0 and 64, or 536. |
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
| 20001 | Failed to authenticate with AppKey. | Appkey is not valid. |
| 20002 | Failed to authenticate with the secret key. | SecretKey is not valid. |
| 30000 | Failed to process request. Please try again later. | Failed to process request. |
| 30001 | Purge usage has exceeded. Please try again later. | Purge volume has been exceeded. |
| 30002 | Data not created or does not exist. Please try again later. | Data has not been created or is not available. |
| 30003 | Request not processed in current service status. | Cannot process the request under the current service status. |
| 30004 | The status of the service is unknown. | The service status is unknown. |
| 30005 | Resource not found. | Resource cannot be found. |
| 80500 | An unknown error occurred during processing. Please contact the service provider. | Unknown error has occurred while processed. Contact service provider. |
