## Migration Guide

### Overview
CDN service of the **[ServiceID].cdn.toastcloud.com** domain created before TOAST regular maintenance on March 24 of 2020 shall be provided **until 10:00:00 KST on May 26 of 2020**. 
Afterwards, the CDN service of the **[ServiceID].cdn.toastcloud.com** domain is to be closed. 

After service of the **[ServiceID].cdn.toastcloud.com** domain is closed, below tasks become unavailable. 
  - Downloading content from the CDN service domain.
  - Service operations on console and API. 

To continue to use CDN service, please execute migration in reference of the guide as below. 

## Order of Migration

### 1. Check Target of Migration for CDN Service 
1. Go to the **CDN Service** tab on the console page of TOAST CDN.  
2. If the service domain is **[ServiceID].cdn.toastcloud.com**, the CDN service requires migration.
    ![Migration-Target List](https://static.toastoven.net/prod_cdn/v2/migration-target-list.png)
3. Click **Migration** next to service name.  
4. Click **Migration** and a page for CDN service creation shows. The CDN service creation page has previous CDN setting information as default.   

### 2. Check Setting of New CDN Service and Create Service 

1. Check CDN service setting on the CDN service creation page. 
    For details regarding CDN service setting, see [Console User Guide](./console-guide/#cdn).

    ![Migration-Service Creation1](https://static.toastoven.net/prod_cdn/v2/migration-create-modal.png)
    ![Migration-Service Creation2](https://static.toastoven.net/prod_cdn/v2/migration-create-modal-options.png)    
    
    1. **Service Region** 
        - KOREA region is not supported.  
        - Only the GLOBAL service region, including Korea region, is provided.  
        - **China and Russia** regions are not included to Global service region. 

    2. **Domain Alias** 
        - To enable secured transfer (HTTPS) via domain set for domain alias, please get a certificate first. 
            - Before creating CDN, go to the **Certificate Management** tab and click **Issue New Certificates** to get a certificate first. Without a certificate issued, error may occur in certificate while using HTTPS protocol service.  
            - For more details on certificates, see [User Console Guide > Certificate Management](./console-guide/#_5).
        
    3. **Origin Server**
        - Previsouly CDN service supported HTTP protocol transfer only, but new CDN service supports transfer of HTTP/HTTPS protocol for the **[ServiceID].toastcdn.net** domain. 
        - **Origin Server Port원본 서버 포트** sets HTTP/HTTPS port number under operation of the origin server. If not set, Not Set shows and it is set with default port (HTTP:80, HTTPS:443). 
            - Only allowed ports can be used for origin servers. For allowed ports, see **[Table2] Avalable Port Numbers for Origin Server** of [User Console Guide > Origin Server](./console-guide/#_2).
        - To support secured transfer (HTTPS) via CDN service domain (or domain alias), origin servers must support HTTPS response. 
            - The CDN edge server checks certificate validity of an origin server to prevent 'man-in-the-middle(MITM)' attacks.  
            - The origin server must have a certificate credited by TOAST CDN or one by certificate authority.
            - Regarding certificates or certificate authorities credited by TOAST CDN, go to [Console User Guide > Origin Server](./console-guide/#_2) and see **[Table1] List of Credible Certificates**.
        - If origin server is not available for the support of HTTPS response, enable the **Downgrade Request Original HTTP Protocol** setting. 
            - Neveretheless, since the **Downgrade Request Original HTTP Protocol** setting has constraints, make sure to check details.  
    4. **Downgrading Request Original HTTP Protocol 원본 요청 HTTP 프로토콜 다운그레이드**
        - When the origin server can support for HTTP response only, set **Enable** for the setting of Downgrade Request Original HTTP Protocol. 원본 서버가 HTTP 응답만 지원 가능한 경우, 원본 요청 HTTP 프로토콜 다운그레이드 설정을 **사용**으로 설정합니다. 
        - When the downgrade setting is **Enabled**, the origin request via HTTP protocol at CDN edge server must be maded by downgrading the protocol to HTTP. CDN 에지(edge) 서버에서 원본 요청시 HTTPS 프로토콜 요청은 HTTP 프로토콜로 다운그레이드하여 요청합니다. 
        - Nevertheless, since the setting has following constraints, make sure to check details. 이 설정은 다음과 같은 제약 사항이 있으므로 주의하여 사용하시기 바랍니다. 
            1. The entire site address cannot be downgraded for protocol. (e.g.: "www.toast.com", which is the entire site address of the origin server, cannot be downgraded. 전체 사이트 주소는 프로토콜 다운그레이드가 불가합니다. (예: 원본 서버의 전체 사이트 주소인 "www.toast.com"은 다운그레이드가 불가합니다.)
            2. No other methods than GET, HEAD and OPTIONS are supported. 메서드 외 메서드는 지원되지 않습니다. 
            3. Following headers may be excluded when a downgrade is requested from a CDN to an original server. CDN 서버에서 원본 서버로 다운그레이드 요청시 다음의 헤더는 제외될 수 있습니다.
                - Origin, Referer, Cookie, Cookie2, sec-\*, proxy-\*

    5. **Forwarding Host Header** 
    Set the **Host** header value to be forwarded when CDN server requests origin server of original files. 
    If origin server is under the operation of name-based virtual host, the **Request Host Header** setting may be required. Select appropriate value of setting, depending on the operation type of an origin server.  
        - Origin Host Name: Set host name of origin server as the host header.  
        - Request Host Header: Set a client request as the host header. 

    6. **Cache** 
    Set the operation method and expiration time of CDN cache server. 
        - **Original Settings**: Apply cache control header (Cache-Control, Expires), as priority, provided by response from the origin server. If cache control header (Cache-Control, Expires) is invalid or unavailable for origin server response, cache during time as specified for cache expiration time (Seconds). Default is enabling the **Original Settings**.  
        - **User-configured**: Cache during specified cache expiration time (seconds). 
        - **Cache Expiration Time (seconds)**: Set TTL for how long to cache original files to CDN cache server. 

2. Click **OK** to create CDN service.   
3. New CDN service is created in the **[ServiceID].toastcdn.net** domain, along with the exclamation mark in blue. 
    ![ListofCreatedMigrationServices](https://static.toastoven.net/prod_cdn/v2/migration-new-create.png)

    
### 3. Apply Newly Created CDN Service for Testing and Operations 

Test and deploy newly created **[ServiceID].toastcdn.net** service before it is applied to an existing service. 


#### 3.1 When Serviced with Default Service Domain([ServiceID].cdn.toastcloud.com) 
1. Create a test build to test newly created CDN service. 
    - To do this, use service domain address of **[ServiceID].toastcdn.net** for newly created CDN service, instaed of service domain of **[ServiceID].cdn.toastcloud.com** for the existing domain. 
3. Test the testing build on a local or development environment server to see if it works well.
    ![DefaultServiceDomain-TestEnvironment](https://static.toastoven.net/prod_cdn/v2/migration-test-test-build-before.png)
4. When test is completed, apply and deploy **[ServiceID].toastcdn.net**, the newly created CDN service domain, to the running service build. 
    ![DefaultServiceDomain-TestEnvironment-Apply](https://static.toastoven.net/prod_cdn/v2/migration-test-test-build-after.png)

#### 3.2 When Serviced with Domain Alias Service 
Domain alias service regards to using CDN service with owned domain, instead of **[ServiceID].cdn.toastcloud.com** which is the default domain address.

##### [Prerequisites] To support secured transfer (HTTPS) between client and CDN edge, execute [Issue Certificate] first.
1. To support secured transfer (HTTPS) in the sector between client and CDN edge server, certificate of owned domain must be deployed to CDN edge server. To issue and deploy certificates, go to the **Certificate Management** tab. See [Console User Guide > Certificate Management](./console-guide/#_5) to execute up to the **Issue and Deploy Certificate** phase. 
2. When it is done up to certificate deployment, in the CDN service integration phase,**exclude CNAME record setting** and set domain alias setup only to newly created CDN service.  
    - **CNAME record setting must be executed after testing is completed so that impact on service could be minimized.**

##### How to Test 1: By altering host files on local pc environment 로컬 PC 환경의 hosts 파일을 변조하여 확인 하는 방법
1. nslookup 명령어을 통해 **[서비스ID].toastcdn.net**의 A 레코드에 설정된 CDN 에지(edge) 서버의 IP 주소를 확인합니다.  
    - nslookup 명령어는 운영체제에 따라 명령어가 다르거나 결과 형식이 다를 수 있습니다.
    A 레코드의 IP 주소를 확인 합니다. 아래 예시와 같이 복수 개가 등록되어 있을 수 있습니다. 복수 개가 조회되는 경우 임의의 하나 IP만 사용하면 됩니다.

    ![nslookup-IP확인](https://static.toastoven.net/prod_cdn/v2/migration-nslookup.png)

2. 테스트를 수행 할 로컬 또는 개발 환경의 hosts 파일에 확인한 에지 IP 주소와 도메인을 입력합니다.
    ```
    xxx.xxx.xxx.xxx your-alias.domain.com
    ```
3. 로컬 또는 개발환경에서 Hosts 파일을 변조하여 동작에 문제가 없는지 테스트합니다.
   ![도메인별칭-hosts파일변조](https://static.toastoven.net/prod_cdn/v2/migration-test-alias-hosts-before.png)
4. 테스트가 완료되면 운영 중인 도메인 별칭의 CNAME 레코드를 **[서비스ID].toastcdn.net** 으로 위임합니다. 
   ![도메인별칭-hosts파일변조-적용](https://static.toastoven.net/prod_cdn/v2/migration-test-alias-hosts-after.png)


##### How to Test 2: By creating a new domain alias 새로운 Domain Alias 생성하여 테스트 하는 방법 
1. hosts 파일 변경이 불가한 경우(모바일 애플리케이션 등)에는 새로운 도메인 별칭을 테스트 용도로 생성하여 확인합니다. (예: alias-for-test.doamin.com)
2. 임의의 새로운 도메인 별칭도 인증서 발급이 필요하므로 인증서 관리에서 먼저 인증서 발급과 배포를 진행하시기 바랍니다. (문서 상단의 **[사전 작업] 클라이언트와 CDN 에지(edge) 구간에 보안 전송(HTTPS)을 지원하려면 먼저 [인증서 발급] 작업을 진행합니다.** 내용을 참고합니다.)
3. 테스트용 빌드를 만들어 임의의 새로운 도메인 별칭으로 설정하여 테스트용 빌드를 생성합니다. 
4. 로컬 또는 개발환경에 서비스를 구동하여 서비스 동작에 문제가 없는지 테스트합니다.
   ![도메인별칭-테스트빌드](https://static.toastoven.net/prod_cdn/v2/migration-test-alias-build-before.png)
5. 테스트가 완료되면 도메인 별칭의 CNAME 레코드를 **[서비스ID].toastcdn.net** 으로 위임합니다. 
   ![도메인별칭-테스트빌드-적용](https://static.toastoven.net/prod_cdn/v2/migration-test-alias-build-after.png)

### 4. 기존 CDN 서비스 삭제 Delete Previous CDN Service 
1. 모든 마이그레이션 작업이 완료되면 기존 CDN 서비스를 **통계** 탭에서 통계를 조회합니다. 
2. 기존 CDN 서비스의 통계를 확인하여 더 이상 트래픽 유입이 없다면 기존 **[서비스ID].cdn.toastcloud.com** 서비스를 삭제합니다. 통계 데이터는 약 30분의 지연 후 생성되므로 충분한 시간을 두고 트래픽 유입을 확인합니다.
  ![마이그레이션완료후삭제](https://static.toastoven.net/prod_cdn/v2/migration-old-delete.png)
