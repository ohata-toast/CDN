## Content Delivery > CDN > Console User Guide

This document describes how CDN service is configured and applied on TOAST CDN console. 

## Creating CDN 

Go to **Contents Delivery > CDN** and to **CDN Service** and click **Create**, and the **Creating CDN** window pops up.  
CDN service domain is autoatically created in the **[ServiceID].toastcdn.net** format. To use your own domain, enable **Domain Alias**. 
It takes up to 2 hours to complete deployment after service is requested for creation. Service becomes available after it is completely deployed. 

### Basic Information 
Basic information is set. 
![Creating CDN- Basic Information](https://static.toastoven.net/prod_cdn/v2/console-cdn-create-default.png)

- **Service Region**
  GLOBAL service is provided via CDN edge servers located around the globe. 
  Note, however, **China and Russia** are excluded from service regions. 

- **Description**
  More description is added on CDN service.  

- **Domain Alias**
  The default service domain address of TOAST CDN is provided in the **[ServiceID].toastcdn.net** format. 
  To use CDN service with your own domain, enable **Domain Alias**. 
  To use HTTPS protocol with your own domain, get a certificate issued from **Certificate Management** and set domain alias.
  After domain alias is set, register CNAME record at DNS provider of domain like follows. Please consult your DNS provider regarding DNS settings. 
    - Record Type: **CNAME**
    - Record Name: **[Registered domain for domain alias]**
    - Record Value (Rdata): **[ServiceID].toastcdn.net**
    - TTL: Randomly selected 

- **Callback**
 It takes hours to create and change CDN service (e.g. edit, suspend/restart, and delete)
 After task is completed, enable the callback setting to receive change status via callback URL and CDN setting information. See [API Guide](./api-guide-v2.0/#_23) to find information sent to callback. 
    1. Enter **HTTP Method** and **Callback URL**.
    2. To receive results on the change of CDN via query parameter, include the following path variable to **Callback URL**.  
         e.g.: http://callback.url?appKey={appKey}&status={status}&isSuccessful={isSuccessful})

| Path Variables | Description | 예시 전달 값 |
| ------------- | --- | ------- |
| {appKey} | Appkey of CDN Service | Appkey issued on console |
| {domain} | Name of CDN Service | [Service ID].cdn.toastcloud.com or [ServiceID].toastcdn.net |
| {status} | Current status of CDN | OPEN, SUSPEND, CLOSE, ERROR | 
| {isSuccessful} | Whether service change is successful (API v1.0 is not supported.) | "true" or "false" |

### Origin Server
Set server providing original files to be deployd to CDN. 
![Creating CDN- Basic Information](https://static.toastoven.net/prod_cdn/v2/console-cdn-create-origin.png)

- **Origin Server**
  The origin server provides original files to be deployd for CDN service. You may use IPv4 or the entire domain address (FQDN: Fully Qualified Domain Name) type for the origin server. Since IP address is highly likely to be changed, it is recommended to set with domain.  
  If there is no operating origin server, use an instance of TOAST Compute or object storage of TOAST Storage. To support secured transfer (HTTPS) via CDN domain, the origin server must support HTTPS response. This means a certificate credited by TOAST CDN must be installed at the origin server. Refer to the following table for credible certificates. 
  If the origin server cannot support HTTPS response, use the **Downgrading HTTP Protocols Requesting Originals** setting. 
  Nevertheless, since **Downgrading HTTP Protocols Requesting Originals** has constraints, it is recommended that the origin server support HTTPS protocol.   

**[Table 1] List of Credible Certificates**

| Common Name| Expiry Date |SHA-1 Fingerprint |
|---|---|---|
|SecureTrust CA|1.Jan.30|8782c6c304353bcfd29692d2593e7d44d934ff11|
|Entrust.net Certification Authority (2048)|24.Jul.29|503006091d97d4f5ae39f7cbe7927d7d652d3431|
|DigiCert Global Root CA|10.Nov.31|a8985d3a65e5e5c4b2d7d66d40c6dd2fb19c5436|
||30.Sep.23|36b12b49f9819ed74c9ebc380fc6568f5dacb2f7|
|QuoVadis Root CA 2 G3|13.Jan.42|093c61f38b8bdc7d55df7538020500e125f5c836|
|thawte Primary Root CA|17.Jul.36|91c6d6ee3e8ac86384e548c299295c756c817b81|
|Go Daddy Root Certificate Authority - G2|1.Jan.38|47beabc922eae80e78783462a79f45c254fde68b|
|GeoTrust Primary Certification Authority|17.Jul.36|323c118e1bf7b8b65254e2e2100dd6029037f096|
|VeriSign Class 3 Public Primary Certification Authority - G4|19.Jan.38|22d5d8df8f0231d18df79db7cf8a2d64c93f6c3a|
|Entrust Root Certification Authority|28.Nov.26|b31eb1b740e36c8402dadc37d44df5d4674952f9|
||29.May.29|5f3b8cf2f810b37d78b4ceec1919c37334b9c774|
|AffirmTrust Commercial|31.Dec.30|f9b5b632455f9cbeec575f80dce96e2cc7b278b7|
|Amazon Root CA 4|26.May.40|f6108407d6f8bb67980cc2e244c2ebae1cef63be|
|Certum CA|11.Jun.27|6252dc40f71143a22fde9ef7348e064251b18118|
|DST Root CA X3|30.Sep.21|dac9024f54d8f6df94935fb1732638ca6ad77c13|
|TC TrustCenter Class 2 CA II|1.Jan.26|ae5083ed7cf45cbc8f61c621fe685d794221156e|
|SwissSign Gold CA - G2|25.Oct.36|d8c5388ab7301b1b6ed47ae645253a6f9f1a2761|
|USERTrust ECC Certification Authority|19.Jan.38|d1cbca5db2d52a7f693b674de5f05a1d0c957df0|
|QuoVadis Root CA 2|25.Nov.31|ca3afbcf1240364b44b216208880483919937cf7|
|COMODO ECC Certification Authority|19.Jan.38|9f744e9f2b4dbaec0f312c50b6563b8e2d93c311|
|USERTrust RSA Certification Authority|19.Jan.38|2b8f1b57330dbba2d07a6c51f70ee90ddab9ad8e|
|ISRG Root X1|4.Jun.35|cabd2a79a1076a31f21d253635cb039d4329a5e8|
|DigiCert High Assurance EV Root CA|10.Nov.31|5fb7ee0633e259dbad0c4c9ae6d38f1a61c7dc25|
|VeriSign Class 3 Public Primary Certification Authority - G5|17.Jul.36|4eb6d578499b1ccf5f581ead56be3d9b6744a5e5|
|GlobalSign|15.Dec.21|75e0abb6138512271c04f85fddde38e4b7242efe|
|QuoVadis Root CA 3|25.Nov.31|1f4914f7d874951dddae02c0befd3a2d82755185|
|GlobalSign|18.Mar.29|d69b561148f01c77c54578c10926df5b856976ad|
|Starfield Services Root Certificate Authority - G2|1.Jan.38|925a8f8d2c6d04e0665f596aff22d863e8256f3f|
|Baltimore CyberTrust Root|13.May.25|d4de20d05e66fc53fe1a50882c78db2852cae474|
|AAA Certificate Services|1.Jan.29|d1eb23a46d17d68fd92564c2f1f1601764d8e349|
|Amazon Root CA 3|26.May.40|0d44dd8c3c8c1a1a58756481e90f2e2affb3d26e|
|VeriSign Class 3 Public Primary Certification Authority - G3|17.Jul.36|132d0d45534b6997cdb2d5c339e25576609b5cc6|
|GlobalSign Root CA|28.Jan.28|b1bc968bd4f49d622aa89a81f2150152a41d829c|
|Actalis Authentication Root CA|22.Sep.30|f373b387065a28848af2f34ace192bddc78e9cac|
|AffirmTrust Networking|31.Dec.30|293621028b20ed02f566c532d1d6ed909f45002f|
|AffirmTrust Premium|31.Dec.40|d8a6332ce0036fb185f6634f7d6a066526322827|
|QuoVadis Root Certification Authority|18.Mar.21|de3f40bd5093d39b6c60f6dabc076201008976c9|
||6.Jun.37|feb8c432dcf9769aceae3dd8908ffd288665647d|
|GeoTrust Primary Certification Authority - G3|2.Dec.37|039eedb80be7a03c6953893b20d2d9323a4c2afd|
|thawte Primary Root CA - G2|19.Jan.38|aadbbc22238fc401a127bb38ddf41ddb089ef012|
|VeriSign Universal Root Certification Authority|2.Dec.37|3679ca35668772304d30a5fb873b0fa77bb70d54|
|Cybertrust Global Root|15.Dec.21|5f43e5b1bff8788cac1cc7ca4a9ac6222bcc34c6|
|Global Chambersign Root|1.Oct.37|339b6b1450249b557a01877284d9e02fc3d2d8e9|
|SwissSign Silver CA - G2|25.Oct.36|9baae59f56ee21cb435abe2593dfa7f040d11dcb|
|Amazon Root CA 1|17.Jan.38|8da7f965ec5efc37910f1c6e59fdc1cc6a6ede16|
|Entrust Root Certification Authority - G2|8.Dec.30|8cf427fd790c3ad166068de81e57efbb932272d4|
|Amazon Root CA 2|26.May.40|5a8cef45d7a69859767a8c8b4496b578cf474b1a|
|DigiCert Assured ID Root CA|10.Nov.31|0563b8630d62d75abbc8ab1e4bdfb5a899b24d43|
||30.Jun.34|2796bae63f1801e277261ba0d77770028f20eee4|
|COMODO Certification Authority|1.Jan.30|6631bf9ef74f9eb6c9d5a60cba6abed1f7bdef7b|
|AddTrust External CA Root|30.May.20|02faf3e291435468607857694df5e45b68851868|
|COMODO RSA Certification Authority|19.Jan.38|afe5d244a8d1194230ff479fe2f897bbcd7a8cb4|
|thawte Primary Root CA - G3|2.Dec.37|f18b538d1be903b6a6f056435b171589caf36bf2|
|DigiCert Global Root G3|15.Jan.38|7e04de896a3e666d00e687d33ffad93be83d349e|
|GeoTrust Global CA|21.May.22|de28f4a4ffe5b92fa3c503d1a349a7f9962a8212|
|DigiCert Global Root G2|15.Jan.38|df3c24f9bfd666761b268073fe06d1cc8d4f82a4|

- **Origin Server Port**  
  An origin server must be operated by a web-protocol support service. Service port numbers can be set for HTTP/HTTPS protocols under operations.  
  Either HTTP or HTTPS must be entered for the origin server port, and if not set, a port is set by default with HTTP:80 or HTTPS:443.
  Only limited number of ports are available as the origina port. Refer to the following table for available port numbers. 

**[Table 2] Available Origin Server Port Numbers**

|Port Number|
|---|
|72, 488, 1080, 1443, 7070|
|8000-9001|
|11080-11110|
|80-89|
|591, 1088, 2080, 7612|
|12900-12949|
|443, 777, 1111, 7001, 7777|
|9901-9908|
|45002|

- **Original Path**  
  Set the lower paths of an original file. Content may be requested without the original path. 
  
> **[Example] When the original path is set with /files/images** 
>
> - URL of Original File: http://your.origin.com/**files/images**/logo.png 
> - URL of CDN Service: http://[ServiceID].toastcdn.net/logo.png
> - Origin paths may be missing from URL of CDN Service when it is requested. 

- **Downgrading HTTP Protocols Requesting Originals**  
  The CDN edge server requests origin server of the original files via service protocol (HTTP/HTTPS) of client's original request.    
  That is, when a client requests via HTTPS but if the origin server does not support HTTPS response, original files do not come as response.   
  If the origin server operates HTTPS protocols only, enable the **Downgrading HTTP Protocols Requesting Originals** setting and make a request from CDN edge server to origin server by downgrading HTTPS to HTTP protocol.    
  In short, the CDN edge server section of client is communicated via HTTPS, while the origin server section of CDN edge server is communicated via HTTP.   
  Note the following constraints when downgrading HTTP protocols requesting originals:   
> **[Caution] Contraints for Downgrading HTTP Protocols Requesting Originals**
> 1. Protocol downgrade is not applied to a whole website address. For instance, **www.toast.com**, which is the entire site address of the origin server, cannot be downgraded.   
> 2. No other methods than GET, HEAD, or OPTIONS, are supported. 
> 3. When a downgrade is requested from CDN to an origin server, following headers may be excluded:
>    Origin, Referer, Cookie, Cookie2, sec-\*, proxy-\*

- **Forward Host Header**
  Set **Host** header value to be sent along with a request of CDN server for original files to origin server. 
  If the origin server is run as name-based virtual host, **Request Host Header** setting may be required. Select an appropriate value depending on the opearting type of an origin server.  
  - **Original Host Name**: Set the host name of origin server as the host header.
    - **Request Host Header**: Set as the host header of client request. 

### Cache

CDN cache operations and expiration time can be set.  
![Creating CDN-Cache](https://static.toastoven.net/prod_cdn/v2/console-cdn-create-cache.png)

- **Setting Cache Expiration**
  Cache can be configured from the response header of cache control at the origin server. 
    - **Enable Original Setting**: Apply the cache control header first, as provided by the origin server's response. If cache control header is not valid or unavailable, it is cached during specified cache expiration time (seconds). **Enable Original Setting** is default. 
    - **Enable User-Configuration**: Cached during specified cache expiration time (seconds). 

> **[Note] Default and Validity of Cache Expiration Time**
> Default cache expiration time is 0. With 0 as default, the cache expiration time is 604,800 (seconds)=1 week.
> Cache expiration time is available from 0 as default to 2,147,483,647(seconds).

### Referrer Header Access Management 
Content access management is set with the referrer request header. 
![Creating CDN Service - Cache](https://static.toastoven.net/prod_cdn/v2/console-cdn-create-cache.png)

The referrer request header includes the webpage address of previous links of the currently requested page. It helps to find the paths a request comes from. With referrer header access management, only particular request headers can be configured to access user content. Enter in regex, and break the lines to enter many.

- **Blacklist Type**:
    * Appropriate to restrict the access of particular referrer request headers only. 
    * Content access is restricted if a referrer request header matches regex setup. If not, content access is allowed. 
- **Whitelist Type**:
    * Appropriate to allow the access of particular referrer request headers only. 
    * Content access is allowed if a referrer request header matches regex setup. If not, content access is not allowed. 

> **[Caution]**
> If referrer request header is not available, access control does not work. 
>
> **[Example]**
>
> * Typ: Whitelist
> * Regex:`^https://[a-zA-Z0-9._-]*\.toast\.com/.*`
> Content access is allowed only when resources are requested from lower paths of a toast.com sub-domain. 
>
> **[Note] Regex Escape Characters**
> Some characters are used as special characters for regex. 
> For instance, a period (`.`) indicates agreement with all characters for regex. 
> To understand a special character as a general one, add backlash before it.(e.g.: `\.`).
> Regex special characters include `^, ., [, ], $, (, ), |, *, +, ?, {, }, and \`.
> To control many referrers, enter in consecutive lines. 
> To set many referrers with APIs, delimit with \n tokens. 


## Settings

### Modify CDN Service Setting 
CDN service setting can be modified, except the name and region of service domain. 
![Enable Modifying CDN Service](https://static.toastoven.net/prod_cdn/v2/console-cdn-modify1.png)

1. Select a CDN service to modify from the list. 
2. Click **Modify** from the **Setting** at the bottom of the page. 

Then, items that are modifiable are activated like below.  
![Check Modifying CDN Service서비스수정확인](https://static.toastoven.net/prod_cdn/v2/console-cdn-modify2.png)

* Modify the setting.  
* Click **OK** to complete with changes. 
* To edit other settings, except description and callack setting, it may take more time than usual since changes must be applied throughout the whole CDN server.  

**Modification takes dozens of minutes, but change of domain alias may take a few hours.**

> **[Note] Deployment Status while Modifying CDN and Service Status** 
> If service is under modification, CDN runs in the existing service setting.
> If it fails to modify, it is rolled back to the existing setting information and the deployment status shows red circle on CDN list.  Modification fails if there is error in setting information, or internal error occurs.  

### Suspend and Resume CDN
CDN service can be suspended or resumed. 


1. Select a CDN service to suspend. 
2. Click **Suspend**. 
![CDN Service- Suspend](https://static.toastoven.net/prod_cdn/v2/console-cdn-pause.png)
3. A warning guide will show for CDN services that are integrated with certificate. To prevent certificate expiration, CDN service must be resumed before a start day of certificate renewal. 
![CDN Service-Suspend시정지](https://static.toastoven.net/prod_cdn/v2/console-cdn-restart.png)
4. To resume suspended CDN service, select a CDN service to resume. 
5. Click **Resume**.




> **[Note] Delays of Suspension and Resumption일시 정지와 재시작 동작의 지연**
> Suspension and resumption of CDN service operates by changing DNS records of CDN domain. CDN 서비스 일시정지와 재시작은 CDN 서비스 도메인의 DNS 레코드를 변경하여 동작됩니다. 
> Accordingly, even if 따라서 캐시 DNS 서버에서 TTL 동안 캐시되어 있거나 DNS 전파에 따라 일시정지/재시작이 완료 되어도 즉시 일시정지/재시작이 동작되지 않을 수 있습니다.

> **[Caution] Suspending CDN Service Integrated with Issued Certificate 발급된 인증서가 연동된 CDN서비스의 일시정지**
> To suspend a CDN service integrated with certificate, the certificate cannot be renewed. 인증서가 연동된 CDN 서비스를 일시정지 하는 경우, 인증서 갱신이 불가합니다. 
> Please resume CDN before **Start Day of Certificate Renewal** from **Certificate Management** > Certificate List. 의 **인증서 갱신 시작일** 이전에 CDN 서비스를 재시작하시기 바랍니다. 
> A certificate is allowed to be renewed for 5 days after start day of renewal, and a suspension during the period may cause the certificate to be expired. 인증서 갱신 시작일로 부터 5일 동안은 인증서 갱신 기간이므로 해당 기간에 일시정지를 하면 인증서가 만료될 수 있으므로 유의하시기 바랍니다.


### Delete CDN
CDN service can be deleted. Once deleted, however, a service cannot be recovered. 

1. Select a CDN service to delete. 
2. Click **Delete**. 
![CDN Service-Delete](https://static.toastoven.net/prod_cdn/v2/console-cdn-delete.png)
3. 인증서가 연동된 CDN 서비스에는 인증서 만료 경고 안내가 표시됩니다. 인증서가 만료되지 않게 하려면 서비스 중인 다른 CDN 서비스에 인증서를 연동하시기 바랍니다. A warning guide will show for CDN services that are integrated with certificate. To prevent certificate expiration, please integrate certificate to another working CDN service. 

> **[Note] Deleting CDN 서비스 삭제 소요 시간**
> It may take a few hours (up to 3 hours) to delete CDN service. 

> **[Caution] Deleting CDN Service Integrated with Issued Certificate발급된 인증서가 연동된 CDN 서비스의 삭제**
> When a CDN service integrated with certificate is deleted, the certificate cannot be renewed. 인증서가 연동된 CDN 서비스를 삭제하면, 인증서를 갱신할 수 없습니다. 
> **인증서 관리**의 인증서 목록에서 **인증서 갱신 시작일** 이전에 서비스 중인 다른 CDN 서비스로 연동하시기 바랍니다.Please integrate certificate to another working CDN before **Start Day of Certificate Renewal** from **Certificate Management** > Certificate List.
> 인증서 갱신 시작일로부터 5일 동안은 인증서 갱신 기간이므로 해당 기간에 삭제하면 인증서가 만료될 수 있으므로 유의하시기 바랍니다.A certificate is allowed to be renewed for 5 days after start day of renewal, and deletion during the period may cause the certificate to be expired.


## CDN 캐시 재배포(purge) CDN Cache Purge 
CDN 캐시 서버는 캐시 설정에 따라 지정된 만료 시간 동안 원본 서버의 파일을 캐시합니다. 파일을 캐시하면 원본 파일이 변경되어도 캐시가 만료되기 전까지는 변경전 원본 파일을 유지합니다. 
변경된 원본 파일로 콘텐츠가 즉시 업데이트되려면 **캐시 재배포**를 요청해야 합니다.
캐시 재배포를 하면 요청한 콘텐츠의 오래된 캐시 데이터를 삭제하고 원본 서버에서 새 원본 파일을 다시 캐시합니다. 

1. 변경하려는 서비스를 CDN 서비스 목록에서 선택합니다. Select a service to change from the list of CDN services. 
2. **캐시 재배포** 탭을 클릭합니다. Click **Purge Cache**. 
![CDN캐시재배포](https://static.toastoven.net/prod_cdn/v2/console-cdn-purge.png)

3. 캐시 재배포 타입을 선택합니다. Select a purge type. 
  - CDN 서비스 도메인에 따라 지원되는 캐시 재배포 타입과 요청 양식이 다르므로 유의하시기 바랍니다. 
  - **[ServiceID].toastcdn.net** 서비스 도메인의 재배포 타입과 요청 양식
    * 특정 파일: 재배포할 콘텐츠의 URL을 입력합니다. 요청한 URL만 캐시가 재배포되므로 도메인 별칭으로 여러 서비스 도메인 주소가 있다면 각 URL 주소로 요청해야 합니다.
      * 예) 기본 서비스 도메인 주소: http://[서비스ID].toastcdn.net/path/to/file1.jpg
      * 예) 도메인 별칭 도메인 주소: http://customer.domain.com/path/to/file1.jpg
    * 전체 파일: 캐시 파일을 모두 삭제합니다. 원본 서버에 과도한 트래픽이 유입될 수 있으므로 주의하시기 바랍니다. 
  - **[ServiceID].cdn.toastcloud.com** 서비스 도메인의 재배포 타입과 요청 양식
    * 특정 파일: 재배포할 콘텐츠의 경로를 입력합니다.
      * 예) /path/to/file1.jpg
    * 와일드 카드: 파일 이름과 경로 이름에 와일드 카드 문자를 이용할 수 있습니다. 와일드 카드는 **\*.cdn.toastcloud.com** 서비스만 제공됩니다.
      * \*: 임의의 문자열
      * ?: 1개의 문자
      * \: 이스케이프(escape) 문자
          * 예) /images/games/\*.jpg
          * /\_/sports/\_.jpg
          * /images/sports/ac?e/\*.jpg
    * 전체 파일: 캐시 파일을 모두 삭제합니다. 원본 서버에 과도한 트래픽이 유입될 수 있으므로 주의하시기 바랍니다. 
4. 선택한 캐시 재배포 타입에 맞게 재배포할 파일을 지정합니다.
5. **캐시 재배포** 버튼을 클릭해 재배포를 요청합니다.

캐시 재배포는 사용량 제한이 있으므로 아래의 표를 참고하시고 사용량이 초과되지 않도록 유의하시기 바랍니다.

|Category |[ServiceID].cdn.toastcloud.com|[ServiceID].toastcdn.net |
|---|---|---|
| 제한 단위 | 서비스 도메인별 | 프로젝트별(Appkey) |
| 특정 파일 | 1시간당 요청 가능: 60회, 요청당 경로(path) 수 제한: 1,000개 | 1초당 요청 가능: 1회, 요청당 URL 수 제한: 200 URL |
| 와일드 카드 | 1시간당 요청 가능: 60회, 요청당 경로(path) 수 제한: 10개 | 미지원 |
| 전체 파일 타입 | 1시간당 요청 가능: 5회 | 5분당 요청 가능: 1회 |

> **[Caution] 도메인 별칭을 여러 개 사용 중인 서비스ID].cdn.toastcloud.com 서비스의 캐시 재배포**
> 도메인 별칭이 여러 개 등록된 CDN 서비스는 요청한 캐시 재배포 경로에 대해 각 도메인 별칭별로 작업이 진행됩니다. 
> 
> (Example)
> `custom1.domain-alias.com` 과 `custom2.domain-alias.com` 이 도메인 별칭으로 설정된 CDN 서비스가 있다고 가정합니다.
> 위 CDN 서비스에 대해 **특정 파일** 타입, `/images/photo.png`로 캐시 재배포를 요청하면 아래 2개의 경로로 캐시 재배포가 수행됩니다. 
>
> - custom1.domain-alias.com/images/photo.png
> - custom2.domain-alias.com/images/photo.png 
>
> (등록된 도메인 별칭 수 X 요청한 캐시 재배포 경로 수)만큼 캐시 재배포 경로가 추가되므로 유의해 주세요. 
> 만일, 전체 캐시 재배포 경로 수가 캐시 재배포 사용량 제한의 요청당 최대 캐시 재배포 경로 수를 초과하면 요청당 최대 캐시 재배포 경로 수만큼씩 나눠 요청됩니다. 
> 나눠서 진행된 요청 수만큼 캐시 재배포 제한 사용량은 증가하므로 사용량 초과에 유의해주세요.
>
> **[Caution] [ServiceID].toastcdn.net 서비스를 생성한 후 캐시 재배포 실패 오류**
> CDN 서비스를 생성한 후 약 1시간 이내에는 캐시 재배포 요청에 실패할 수 있습니다. 이후에도 계속 실패하면  고객 센터로 문의해주시기 바랍니다.

## Managing Certificates
소유한 도메인으로 콘텐츠를 보안 전송(HTTPS)하려면 CDN 서버에 소유한 도메인의 인증서를 배포해야 합니다. 인증서가 없으면 클라이언트(브라우저)와 CDN 에지 서버 간 보안 통신(HTTPS)을 할 수 없어 인증서 오류가 발생합니다.
TOAST CDN의 인증서 관리는 다음과 같은 기능을 제공합니다.

- 단일 도메인 타입의 인증서 발급
- 전 세계 거점의 CDN 서버에 인증서 배포(중국과 러시아 지역은 제외)
- 인증서 만료 전 자동 갱신

### 신규 인증서 발급 Issue New Certificates
**인증서 관리** 탭에서 인증서를 발급할 수 있습니다.
![CDN신규인증서발급](https://static.toastoven.net/prod_cdn/v2/console-certificate-create.png)

1. **인증서 관리** 탭의 **신규 인증서 발급** 버튼을 클릭합니다.
2. 발급할 인증서의 도메인을 전체 도메인 주소(FQDN, fully qualified domain name)형식으로 입력합니다.
3. 인증서 발급 안내 내용을 확인하고 **확인** 버튼을 클릭합니다.
4. 신규 발급 인증서를 요청하면 **인증서 관리** 탭의 인증서 도메인이 표시됩니다. 인증서 상태가 **도메인 검증** 상태로 변경되면 이후 도메인 검증 작업을 진행하시기 바랍니다. 

> **[Caution] Checkpoints before Getting Certificates인증서 발급 전 확인 사항**
> 1. 소유한 도메인만 인증서를 발급할 수 있으므로 먼저 도메인을 구매하신 후 진행하시기 바랍니다. 
> 2. 다른 인증 기관(CA, certificate authority)에서 발급한 인증서는 이용할 수 없습니다. 
> 3. 단일 도메인의 인증서 발급만 가능합니다. 와일드 카드, 멀티 도메인 등의 인증서는 지원하지 않습니다.
> 4. 인증서 발급은 프로젝트당 5개로 제한됩니다. 한도 조정이 필요한 경우 TOAST 고객 센터로 문의하시기 바랍니다.
> 5. 신규 인증서 발급 요청 후 도메인 검증 단계는 몇 십분(최대 1~2시간) 후 변경될 수 있습니다. 인증서 상태가 도메인 검증 상태로 변경되면 TOAST 프로젝트 멤버를 대상으로 이메일 발송됩니다. 만일 시스템 오류로 이메일이 발송되지 않는다면 콘솔에서 상태를 확인하시기 바랍니다. 

### 도메인 검증 Validate Domain
신규 인증서 발급을 요청한 후 인증서 상태가 '도메인 검증'이 되면 도메인을 검증하시기 바랍니다.
도메인 검증 방법은 콘솔에서 도메인을 선택하여 확인하거나, 프로젝트 멤버에게 전송된 도메인 검증 가이드 메일의 내용을 참고하시기 바랍니다.

![CDN도메인검증](https://static.toastoven.net/prod_cdn/v2/console-certificate-domain-validation.png)

도메인 검증은 발급 요청한 인증서 도메인의 실제 소유자인지 확인하는 단계 입니다. 도메인 검증을 진행하지 않으면 인증서를 발급할 수 없습니다.
도메인 소유자인지 확인하기 위해 도메인 검증 방식으로 도메인의 제어 권한을 확인합니다. 
도메인 검증 방식에는 **DNS TXT 레코드 추가** 또는 **HTTP 페이지 추가** 방식이 있으며 **두 가지 방식 중 하나만 진행**하면 됩니다.

![CDN도메인검증](https://static.toastoven.net/prod_cdn/v2/console-certificate-domain-validation2.png)

#### DNS TXT 레코드 추가 방식 Adding DNS TXT Records 
도메인의 DNS 제어 권한을 확인해 도메인을 검증합니다. 

1. 도메인의 DNS 서비스 제공 업체의 DNS 관리 페이지에서 TXT 레코드를 추가합니다. 
   DNS 설정 방법은 DNS 서비스 제공 업체에 따라 다를 수 있습니다. 관련 설정은 해당 서비스 업체로 문의하시기 바랍니다.
  - Record Type: **TXT**
  - TTL: **60**. 60으로 설정할 수 없다면 되도록 작게 설정하시기 바랍니다.
  - Record Name: **_acme-challenge.[발급 요청한 인증서 도메인].**  콘솔 또는 발송된 이메일 가이드의 **레코드 이름**을 작성합니다.
  - Record Value: **임의의 문자열** (콘솔 또는 발송된 이메일 가이드의 **레코드값**을 작성합니다.)

2. nslookup 명령어로 추가한 TXT 레코드가 질의되는지 확인합니다. DNS 전파 시간에 따라 질의되기까지 시간이 소요될 수 있습니다.
    `nslookup -type=TXT _acme-challenge.[발급 요청한 인증서 도메인].`

다음 화면은 TOAST DNS+ 서비스에서 설정한 예시입니다. DNS 서비스 제공 업체에 따라 설정 방법은 다를 수 있습니다.
![CDN도메인검증](https://static.toastoven.net/prod_cdn/v2/console-certificate-domain-validation-dns.png)


#### HTTP 페이지 추가 방식 Adding HTTP Pages
도메인이 연결된 웹 서버에 HTTP 페이지를 추가해 도메인을 검증합니다. 

1. 웹 서버의 **http://[발급 요청한 인증서 도메인]/.well-known/acme-challenge/[임의의 문자열]** 경로에 HTTP 페이지를 추가합니다. 
2. HTTP 페이지의 본문에 콘솔 또는 발송된 이메일 가이드의 **페이지 콘텐츠(토큰) **값으로 설정합니다. 
3. 웹 브라우저에서  **http://[발급 요청한 인증서 도메인]/.well-known/acme-challenge/[임의의 문자열]** URL로 접속하면  **페이지 콘텐츠(토큰)** 값이 화면에 표시되는지 확인합니다. 

> **[Caution] Cautions for Validating Domain 도메인 검증 주의 사항**
> 1. 도메인 검증은 인증서 발급 요청일로부터 **5일 이내**에 진행해야 합니다. **기간 내 진행하지 않으면 인증서 발급은 자동으로 취소**됩니다.
> 2. 도메인 검증 작업 완료 후 검증에 성공하면 몇 시간 내 인증서 발급 및 배포 작업이 진행됩니다. 하루 이상 진행되지 않으면 도메인 검증 작업 내용이 올바른지 확인합니다. 문제가 없는데도 진행되지 않으면 TOAST 고객 센터로 문의해 주시기 바랍니다.
> 3. 도메인 검증 방식 중 HTTP 페이지 추가 방식은 HTTP 서버가 기본 포트 80 포트로 운영 중일 때만 가능합니다. 포트를 변경할 수 없다면 DNS TXT 레코드 추가 방식을 이용하시기 바랍니다.

### 인증서 발급 및 배포 Issue and Deploy Certificates
도메인 검증을 통과하면 몇 시간 내 인증서 발급 및 배포 작업이 진행됩니다. 
콘솔의 인증서 상태가  **인증서 발급 및 배포** 단계로 표시되며, TOAST 프로젝트 멤버 대상으로 알림 메일이 발송됩니다. 
이 단계에서는 별도로 작업할 내용은 없습니다.

>  **[참고] 인증서 발급과 배포 단계의 작업 시간**
> 인증서 발급 및 배포 작업은 최대 9시간 이상 걸릴 수 있습니다. 

### CDN 서비스 연동 Integrate with CDN Service
발급된 인증서를 이용하려면 CDN 서비스와 연동해야 합니다. 
이 작업을 진행하지 않거나 작업 내용을 유지하지 않으면 발급된 인증서가 만료될 수 있으므로 주의하시기 바랍니다. 

1. **CNAME 레코드 설정**: 인증서 도메인의 DNS 서비스 제공 업체의 DNS 관리에서 다음의 CNAME 레코드를 추가합니다. 
    - Record Type:  **CNAME**
    - TTL: Random. 자주 변경해야 한다면 작게 설정하시기를 권장합니다. 레코드 변경 시 캐시 DNS 서버에 TTL 시간 동안 캐시될 수 있습니다.
    - Record Name:  **[Certificate Domain].** (예시: test.alias.com.com.)
    - Record Value:  **[CDN Service Domain to be Integrated]** (예시: xxxxxxxx.toastcdn.net)
다음 화면은 TOAST DNS+ 서비스에서 설정한 예시입니다. DNS 서비스 제공 업체에 따라 설정 방법은 다를 수 있습니다.
![CDN서비스연동-CNAME위임](https://static.toastoven.net/prod_cdn/v2/console-certificate-service-cname.png)

2. **Domain Alias Setting**: Add domain alias setting for the CDN to use certificate. 인증서를 이용할 CDN 서비스에 도메인 별칭 설정을 추가합니다. 
    -  Select CDN to be integrated from **CDN Service** and click **Modify**.  버튼을 클릭합니다. 도메인 별칭에 인증서 도메인을 추가한 후  **확인** 버튼을 클릭합니다.
![CDN서비스연동-도메인별칭](https://static.toastoven.net/prod_cdn/v2/console-certificate-service-alias.png)


>  **[Caution] Caution for Expiration 인증서 만료 주의 사항**
> Certificates provided by TOAST CDN are automatically renewed before expired. 에서 제공하는 인증서는 인증서 만료 전 자동으로 인증서를 갱신합니다. 
> For auto-renewal, user's certificate must be integrated with CDN service. 자동으로 인증서를 갱신하려면 반드시 사용 중인 인증서가 CDN 서비스와 연동돼 있어야 합니다. 
> Otherwise, certificates may not be renewed during specific period and expired. CDN 서비스와 연동돼 있지 않으면 인증서 갱신 기간에 갱신되지 않아 인증서가 만료될 수 있습니다.
> 인증서 갱신은 **인증서 관리**의 목록에 표시된 인증서 갱신 시작일로부터 **5일 이내** 진행됩니다. 
> 인증서가 만료되지 않도록 다음의 설정 사항을 항상 유지하시기 바랍니다.
> 
> 1. 인증서의 도메인은 CNAME 레코드로 연동할 CDN 서비스 도메인 주소로 위임해야 합니다. 
> 2. 연동할 CDN 서비스의 도메인 별칭에 인증서 도메인이 설정되어 있어야 합니다.
> 3. 인증서가 연동된 CDN 서비스를 일시 정지하면 인증서를 갱신할 수 없습니다. 인증서 갱신 시작일 이전에 재시작하거나 다른 운영 중인 CDN 서비스에 인증서를 연동하시기 바랍니다.
> 4. 인증서가 연동된 CDN 서비스를 삭제하면 인증서를 갱신할 수 없습니다. 삭제하기 전에 운영 중인 다른 CDN 서비스에 인증서를 연동하시기 바랍니다. 

CDN 서비스 연동 작업이 완료되면 인증서 상태가 '정상'으로 표시됩니다.
![CDN인증서정상상태](https://static.toastoven.net/prod_cdn/v2/console-certificate-active.png)

## Statistics

Check statistics on the network transfer volume, HTTP status code, most downloaded content, and more.  네트워크 전송량, HTTP 상태 코드별 통계 및 다운로드가 가장 많은 콘텐츠의 순위 통계를 확인할 수 있습니다.
7일 이내 통계 데이터는 정확하지 않으므로 참고용으로만 이용하시기 바랍니다. 정확한 통계 데이터는 7일 이후에 확인하시기 바랍니다. 

1. Click **Statistics** from **Contents Delivery > CDN**.
![cdn_08_201812](https://static.toastoven.net/prod_cdn/cdn_08_201812.png)
2. 통계를 확인하려면 CDN 서비스를 선택합니다. Select CDN to check statistics. 
3. 검색 기간을 입력합니다. Enter search period. 
4. 검색 기간 내 데이터 주기는 선택한 기간에 따라 자동으로 선택됩니다. Data cycle within a search period is automatically selected depending on the period. 
5. **검색** 버튼을 클릭합니다. Click **Search**. 
