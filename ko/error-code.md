## Content Delivery > CDN > 오류 코드

| errorCode | errorMessage | 설명 |
| --- | --- | --- |
| 0 | SUCCESS | 처리 성공 |
| -1 | FAIL | 처리 실패 |
| -2 | ASSERT | 처리 실패 |
| 10000 | Domain does not exist. | 존재하지 않는 서비스입니다. |
| 10001 | There is no path to purge. | 재배포할 경로를 입력해 주세요. |
| 10002 | The HTTP Method or url of Callback URL is not valid. | Callback URL의 HTTP Method 또는 URL이 입력되지 않았습니다. |
| 10003 | Callback url format is not valid. | Callback URL의 URL 형식이 올바르지 않습니다. |
| 10004 | Exist a creating service. Please retry after it is finished. | 요청한 작업을 수행할 수 없는 상태입니다. 이전에 요청한 작업이 완료된 후 다시 시도해 주세요. |
| 10006 | Not found resource | 요청한 리소스를 찾을 수 없거나 잘못된 요청 정보입니다. |
| 10007 | Alreay exists domain alias. | 이미 다른 CDN 서비스 도메인에 등록된 Domain Alias(도메인 별칭)입니다. |
| 10008 | Origin path must starts with a slash('/). | 원본 경로는 '/'부터 입력해주세요. |
| 10009 | Origin path must not end with a slash('/). | 원본 경로의 마지막 문자로 '/'를 입력할 수 없습니다. |
| 10010 | Origin domain is requried. | 원본 서버는 필수 입력 사항입니다. |
| 10011 | Please enter origin domain without scheme. | 원본 서버에는 http://와 같은 스키마(scheme) 정보를 입력할 수 없습니다. |
| 10012 | Please enter the origin domain in public IP or domain format. | 원본 서버 형식이 올바르지 않습니다. 도메인 또는 IP 형식으로 입력해 주세요. |
| 10013 | Domain alias is invalid. | Domain Alias 형식이 올바르지 않습니다. |
| 10014 | Referrers is invalid. Enter referrers regular expression. | 리퍼러(referrer) 형식이 올바르지 않습니다. 유효한 정규 표현식인지 확인해 주세요. |
| 10015 | Please enter use orgin 'Y' or 'N'. | 캐시 만료 설정의 원본 설정 사용 여부는 'Y' 또는 'N' 으로 입력해 주세요. |
| 10016 | Please enter origin. | 원본 서버 설정이 없거나 올바르지 않습니다. |
| 10017 | Invalid Purge Type. Please enter purge type 'ITEM' or 'ALL' | 재배포(purge) 타입이 올바르지 않습니다. 타입은 'ITEM' 또는 'ALL'로 입력해 주세요. |
| 10018 | Invalid Origin Port. Please enter a number between 0 and 65,536. | 원본 서버의 포트가 올바르지 않습니다. 0~65,536 사이의 숫자로 입력해주세요. |
| 10019 | Up to three domain aliases can be registered. | 최대 3개의 도메인 별칭까지 등록이 가능합니다. |
| 10020 | The requested domain alias has duplicated values. | 이미 등록된 도메인 별칭으로, 신규로 등록이 불가합니다. |
| 10021 | Already deleted distribution. | 이미 삭제된 CDN 서비스입니다. |
| 10023 | The port field cannot be set with either httpPort or httpsPort. | port와 httpPort, httpsPort는 동시에 입력할 수 없습니다. port를 입력하거나 혹은 httpPort와 httpsPort를 입력해주세요.  |
| 10024 | The origin server port is not set. | origin server port는 필수 입력값입니다. port 혹은 httpPort 혹은 httpsPort 항목에 원본 서버 포트를 입력해주세요. |
| 10025 | Invalid origin port. The httpPort equals httpsPort. | httpPort와 httpsPort 입력값을 다르게 입력해주세요. |
| 10027 | The description's maximum length is limited to 255 characters. | CDN 서비스의 설명은 최대 255자까지 입력 가능합니다. |
| 10028 | The Root Path Control 'controlType' is not set. Please enter type 'DENY' or 'REDIRECT' | controlType은 필수 입력값입니다. 'DENY' 또는 'REDIRECT'를 입력해주세요. |
| 10029 | The Root Path Control 'redirectPath' is not set. | controlType이 'REDIRECT'일 경우, redirectPath는 필수 입력값입니다. |
| 10030 | 'redirectPath' must starts with a slash('/). | redirectPath는 '/'부터 입력해주세요. |
| 10031 | The Root Path Control 'redirectStatusCode' is not set. Please enter Code 301 or 302 or 303 or 307 | controlType이 'REDIRECT'일 경우, redirectStatusCode는 필수 입력값입니다. 301, 302, 303, 307 중에 입력해주세요. |
| 10032 | Invalid 'redirectStatusCode'. Please enter Code 301 or 302 or 303 or 307 | redirectStatusCode 입력값이 올바르지 않습니다. 301, 302, 303, 307 중에 입력해주세요.  |
| 20001 | Failed to authenticate with AppKey. | AppKey가 유효하지 않습니다. |
| 20002 | Failed to authenticate with the secret key. | SecretKey가 유효하지 않습니다. |
| 30000 | Failed to process request. Please try again later. | 요청 처리에 실패했습니다. |
| 30001 | Purge usage has exceeded. Please try again later. | 재배포 사용량을 초과했습니다. |
| 30002 | Data not created or does not exist. Please try again later. | 데이터가 생성되지 않았거나 존재하지 않습니다. |
| 30003 | Request not processed in current service status. | 현재 서비스 상태에서 처리할 수 없는 요청입니다. |
| 30004 | The status of the service is unknown. | 알 수 없는 서비스의 상태입니다. |
| 30005 | Resource not found. | 리소스를 찾을 수 없습니다. |
| 80500 | An unknown error occurred during processing. Please contact the service provider. | 처리 중 알 수 없는 오류가 발생했습니다. 서비스 제공자에게 문의해 주세요. |