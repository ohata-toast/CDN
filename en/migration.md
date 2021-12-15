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
        - Previously CDN service supported HTTP protocol transfer only, but new CDN service supports transfer of HTTP/HTTPS protocol for the **[ServiceID].toastcdn.net** domain.
        - **Origin Server Port** sets HTTP/HTTPS port number under operation of the origin server. If not set, Not Set shows and it is set with default port (HTTP:80, HTTPS:443).
            - Only allowed ports can be used for origin servers. For allowed ports, see **[Table2] Avalable Port Numbers for Origin Server** of [User Console Guide > Origin Server](./console-guide/#_2).
        - To support secured transfer (HTTPS) via CDN service domain (or domain alias), origin servers must support HTTPS response.
            - The CDN edge server checks certificate validity of an origin server to prevent 'man-in-the-middle (MITM)' attacks.
            - The origin server must have a certificate trusted by TOAST CDN or one by certificate authority (CA).
            - Regarding certificates or certificate authorities trusted by TOAST CDN, go to [Console User Guide > Origin Server](./console-guide/#_2) and see **[Table1] List of Trusted Certificates**.
        - If origin server is not available for the support of HTTPS response, enable the **Downgrade Request Original HTTP Protocol** setting.
            - Nevertheless, since the **Downgrade Request Original HTTP Protocol** setting has constraints, make sure to check details.
    4. **Downgrading Request Original HTTP Protocol**
        - When the origin server can support for HTTP response only, set **Enable** for the setting of Downgrade Request Original HTTP Protocol.
        - When the downgrade setting is **Enabled**, the origin request via HTTP protocol at CDN edge server must be made by downgrading the protocol to HTTP.
        - Nevertheless, since the setting has following constraints, make sure to check details.
            1. The entire site address cannot be downgraded for protocol. (e.g.: "www.toast.com", which is the entire site address of the origin server, cannot be downgraded.)
            2. No other methods than GET, HEAD and OPTIONS are supported.
            3. Following headers may be excluded when a downgrade is requested from a CDN to an original server.
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
    - To do this, use service domain address of **[ServiceID].toastcdn.net** for newly created CDN service, instead of service domain of **[ServiceID].cdn.toastcloud.com** for the existing domain.
3. Test the testing build on a local or development environment server to see if it works well.
    ![DefaultServiceDomain-TestEnvironment](https://static.toastoven.net/prod_cdn/v2/en/migration-test-test-build-before.png)
4. When test is completed, apply and deploy **[ServiceID].toastcdn.net**, the newly created CDN service domain, to the running service build.
    ![DefaultServiceDomain-TestEnvironment-Apply](https://static.toastoven.net/prod_cdn/v2/en/migration-test-test-build-after.png)

#### 3.2 When Serviced with Domain Alias Service
Domain alias service regards to using CDN service with owned domain, instead of **[ServiceID].cdn.toastcloud.com** which is the default domain address.

##### [Prerequisites] To support secured transfer (HTTPS) between client and CDN edge, execute [Issue Certificate] first.
1. To support secured transfer (HTTPS) in the sector between client and CDN edge server, certificate of owned domain must be deployed to CDN edge server. To issue and deploy certificates, go to the **Certificate Management** tab. See [Console User Guide > Certificate Management](./console-guide/#_5) to execute up to the **Issue and Deploy Certificate** phase.
2. When it is done up to certificate deployment, in the CDN service integration phase,**exclude CNAME record setting** and set domain alias setup only to newly created CDN service.
    - **CNAME record setting must be executed after testing is completed so that impact on service could be minimized.**

##### How to Test 1: By Altering Host Files on Local PC Environment
1. By commanding nslookup, check IP address of the CDN edge server set for record A of **[ServiceID].toastcdn.net**.
    - The nslookup command may exist in a different format or with different result format, depending on the operating system.
    Check IP address of record A. A multiple number of addresses may be registered like the example as below. If many numbers are queried, use only a random IP.

    ![nslookup-CheckIP](https://static.toastoven.net/prod_cdn/v2/migration-nslookup.png)

2. Enter confirmed edge IP address and domain for host file on local or development environment which is to be tested.
    ```
    xxx.xxx.xxx.xxx your-alias.domain.com
    ```
3. Alter the host file on local or development environment to see if it works fine.
   ![DomainAlias-AlterHostFile](https://static.toastoven.net/prod_cdn/v2/en/migration-test-alias-hosts-before.png)
4. When testing is done, delegate CNAME record of domain alias under operations to **[ServiceID].toastcdn.net**.
   ![DomainAlias-AlterHostFile-Apply](https://static.toastoven.net/prod_cdn/v2/en/migration-test-alias-hosts-after.png)


##### How to Test 2: By Creating New Domain Alias
1. If change of host file is unavailable (e.g. on mobile applications), check by creating a new domain alias for testing.  (e.g.: alias-for-test.domain.com)
2. As new domain alias also requires a certificate, first get a certificate and deploy it from certificate management. (See **[Prerequisites] To support secured transfer (HTTPS) between client and CDN edge, execute [Issue Certificate] first.** in the above.)
3. Create a test build by setting new domain alias.
4. Run service on a local or development environment to test if it works well.
   ![DomainAlias-TestBuild](https://static.toastoven.net/prod_cdn/v2/en/migration-test-alias-build-before.png)
5. When testing is done, delegate CNAME record of domain alias to **[ServiceID].toastcdn.net**.
   ![DomainAlias-TestBuild-Apply](https://static.toastoven.net/prod_cdn/v2/en/migration-test-alias-build-after.png)

### 4. Delete Previous CDN Service
1. When all migration tasks are completed, go to the **Statistics** tab to query statistics on previous CDN service.
2. When there is no more traffic inflow to the previous CDN service as confirmed by such statistics, delete the previous **[ServiceID].cdn.toastcloud.com** service. Statistical data are created in about 30-minute delays, so take sufficient time to check traffic inflow.
  ![DeleteAfterMigrationCompleted](https://static.toastoven.net/prod_cdn/v2/migration-old-delete.png)