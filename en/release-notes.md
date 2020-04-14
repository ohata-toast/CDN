## Content Delivery > CDN > Release Notes

### March 24, 2020

#### Feature Updates  
* CDN Service Regions: Provided for the GLOBAL region only.
	* Korea-only CDN service is to be closed. 
	* Please use the Global Service region that includes the Korea region.  
* Change of New CDN Service Domain 
	* When a new CDN service is created, [ServiceID].toastcdn.net is provided as service domain address
	* Previous [ServiceID].cdn.cloudtoast.com service domain cannot be issued anew; since [ServiceID].cdn.cloudtoast.com is valid until 10:00:00 KST of May 26, 2020, it must be migrated to a new CDN service. 
	* Regarding migration method, see [Migration Guide](./migration/).
* Support of HTTP/HTTPS Service Protocols 
	* [Service ID].toastcdn.net, which is issued for a new service, supports HTTP/HTTPS by default. 
* Added CDN Service Setup Option 
	* Setting HTTP/HTTPS Port at Origin Server: Service port can be set for each HTTP/HTTPS protocol at the origin server.
	* Downgrading HTTP Protocols: HTTPS request from CDN Server to the Origin Server can be downgraded to HTTP protocol. 
	* Forward Host Header: When a content is requested from CDN server to origin server, either host name or requested host header can be selected as host header.  
	* For more details, see [Console User Guide](./console-guide/).
* Added Certificate Management Features
	* To use CDN service with your own domain, HTTPS protocol service is provided as part of certificate management features. With certificate management, certificates can be easily issued and automatically renewed before expired. 
	* For more details, see [Console User Guide > Certificate Management](./console-guide/#_5).
* API Support for (Old) Service Domain (*.cdn.toastcloud.com) and (New) Service Domain (*.toastcdn.net)
	* (Old) [ServiceID].cdn.toastcloud.com is available without changing the old API (lower than v1.5). However, newly added features are not available. 
	* (New) [ServiceID].toastcdn.net is available even without changing previous API (lower than v1.5). New features are added to API specifications that are higher than v1.5. 
* Purge Cache  Purging 캐시 재배포(purge)
	* Support of High-speed Cache Purge 고속 캐시 재배포 지원 : 캐시 재배포를 요청한 후 몇 초 이내 캐시 재배포가 완료됩니다. 고속 캐시 재배포를 이용해 변경된 콘텐츠를 바로 반영해 콘텐츠의 신뢰성을 높일 수 있습니다.
	* 특정 파일 타입의 캐시 재배포 요청 방식 변경: 캐시 재배포를 할 파일의 전체 URL 주소를 입력하도록 변경되었습니다.
		* 예) 기존: /images/toast.png -> 변경: http://[서비스ID].toastcdn.net/images/toast.png
	* 와일드카드 타입 캐시 재배포 기능 종료
	* 사용량 제한 정책 변경
		* 상세한 내용은 [사용자 콘솔 가이드 > CDN 캐시 재배포](./console-guide/#cdn-purge)를 참고합니다.
* 감시 설정 기능이 종료됩니다.

### February 26, 2019 

#### Feature Updates
* Fixed Purge Error at Particular CDN Service 특정 CDN 서비스 퍼지 오류 수정
	* 원본 서버의 원본 경로가 설정된 경우, 원본 경로를 포함한 퍼지 경로를 작성하지 않을 경우 퍼지가 정상적으로 수행되지 않는 오류를 수정하였습니다.
		* 퍼지 경로는 원본 서버의 원본 경로는 제외한 경로를 입력해야합니다.
	* 복수 개의 도메인 별칭을 등록한 경우 퍼지가 정상적으로 수행되지 않는 오류를 수정하였습니다.

* Domain Alias Restriction 도메인 별칭 제한
	* No more than 3 domain alises are allowed. 도메인 별칭이 최대 3개까지 입력할 수 있도록 제한되었습니다.


### January 15, 2019 

#### Feature Updates 
* CDN 서비스 부분 수정 API 추가 Added API for Partial CDN Modification 
	* Added API to modify only partial service settings 서비스 일부 설정만 변경할 수 있는 부분 수정 API를 추가하였습니다.

### August 28, 2018 

#### Feature Updates
* CDN 서비스 설정 유효성 체크 추가
	* 유효하지 않은 CDN 설정 정보를 미리 체크할 수 있도록 설정 정보에 대한 유효성 검사를 추가하였습니다.

### May 29, 2018 

#### Feature Updates
* Updates for CDN API 1.5v 업데이트
	* API 안정성을 강화하여 품질을 향상하였습니다.
	* 서비스 배포(변경) 작업이 완료되면 콜백을 통하여 작업의 성공여부와 서비스 상태를 전달합니다.
* 서비스 배포(변경) 진행 상태를 확인 할 수 있도록 대시보드에 배포 상태를 표시하도록 수정하였습니다.
	* API를 통한 서비스 배포(변경)를 하는 경우 v1.5 이상 버전의 API를 사용한 경우에만 콘솔에서 서비스 배포 상태를 확인할 수 있습니다.


### January 25, 2018 

#### Feature Updates
* CDN 서비스 삭제 API 추가 Added Delete CDN API 
* CDN 생성 및 수정에 대한 콜백 서비스 추가 Added callback service for Create and Modify CDN 
	* 콘솔 또는 API를 통해 CDN 서비스를 생성 또는 수정에 대한 콜백을 등록할 수 있습니다.
		* 서비스 생성 또는 수정이 완료된 후 등록된 콜백을 통해  생성 또는 수정된 CDN서비스의 정보를 전달해줍니다.

### July 20, 2017 

#### Feature Updates
* Deployed CDN API. For more details, see API Guide. 를 배포하였습니다. 자세한 내용은 API 가이드를 참고해주세요.  
	* CDN 생성, 수정, 조회 API가 추가 되었습니다.Added Create, Modify, and Query CDN APIs.
	* 재배포, 재배포 조회 API가 추가 되었습니다. Added Purge, and Query Purge APIs.

* 원본 서버 하위 경로 지원
	* 기존에는 도메인 또는 ip 형식의 원본서버만 설정할 수 있었으나 원본 서버의 하위 경로도 설정할 수 있습니다.

* 통계 기능 개선
	* TimeUnit(시간, 일, 주, 월)단위로 편리하게 통계를 확인할 수 있도록 UI를 개선하였습니다.
	* 검색 기간별 통계 단위가 조정되었습니다.
		* 검색 기간이 6시간 미만인 경우 5분 단위
		* 검색 기간이 1일 미만인 경우 1시간 단위
		* 검색 기간이 1일 초과인 경우 1일 단위
	* 통계는 3가지 종류의 통계를 제공하며, 통계 데이터와 실제 데이터 사이에는 지연이 발생될 수 있습니다.
		* 트래픽 사용량: 네트워크 대역폭 및 전송량을 확인할 수 있습니다.
		* HTTP 응답별 통계: HTTP 상태코드 별로 CDN 캐시 적중률(Cache hit raito)을 확인할 수 있습니다.
		* Top contents : 가장 많이 조회된 콘텐츠를 확인할 수 있습니다.

#### Bug Fixes
* Fixed bugs in UI Statistics통계 > service Name  서비스 이름 선택 UI 버그 수정
	* 서비스 설명에 대한 문구가 길어지면 서비스 이름 선택 UI가 일부만 노출되는 현상을 수정하였습니다.

### December 22, 2016 

#### Feature Updates
* 서비스 생성 시 실제 접속 가능한 시점에 OPEN 으로 상태 변경 하도록 수정 Updated to change status to 'OPEN' at an available access time when creating a service
* Supports CORS (Cross-Origin Resource Sharing) 지원

#### Bug Fixes
* Fixed inoperability of Global Purge 기능이 동작하지 않던 문제 수정
