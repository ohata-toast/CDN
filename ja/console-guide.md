## Content Delivery > CDN > コンソール使用ガイド

この文書はTOAST CDNコンソールでCDNサービスを構成して利用する方法を説明します。

## CDNサービス作成

**Contents Delivery > CDNのCDNサービス**タブで**作成**ボタンをクリックすると、**CDNサービス作成**ダイアログボックスが表示されます。
CDNサービスドメインは**[サービスID].toastcdn.net**の形式で自動作成されます。所有しているドメインをサービスドメインで利用するには、**ドメインエイリアス**(Domain Alias)機能を利用できます。
作成をリクエストした後、サービスの配布が完了するまで最大2時間かかります。配布が完了した後、サービスを利用できます。

### 基本情報 
基本情報を設定します。
![CDNサービス作成-基本情報](https://static.toastoven.net/prod_cdn/v2/ja1/console-cdn-create-default_ja_20200518.png)

- **サービス地域**
  GLOBALサービス地域は、全世界の拠点にあるCDNエッジサーバーを通してCDNサービスを提供します。
 ただし、**中国とロシア**はサービス地域から除外されます。

- **説明**
  CDNサービスの説明を追加します。

- **ドメインエイリアス**
  TOSAT CDNは基本的に**[サービスID].toastcdn.net**形式のサービスドメインアドレスを提供しています。
 基本サービスドメインアドレスではない所有ドメインでCDNサービスを利用するには**ドメインエイリアス**で設定してください。
 所有しているドメインでHTTPSプロトコルサービスを利用するには、まず**証明書管理**タブで証明書を発行した後、ドメインエイリアスを設定してください。
 ドメインエイリアスを設定した後は、ドメインのDNSサービス提供業者で、次のようにCNAMEレコードを登録する必要があります。DNS設定関連はDNSサービス提供業者にお問い合わせください。
    - レコードタイプ：**CNAME**
    - レコード名：**[ドメインエイリアスに登録したドメイン]**
    - レコード値(Rdata)：**[サービスID].toastcdn.net**
    - TTL：任意の値

- **コールバック**
  CDNサービス作成と変更作業(修正、一時停止/再開、削除)は数時間かかります。
 作業が完了した後、設定したコールバックURLに変更状態とCDN設定情報を受け取るにはコールバックを設定してください。コールバックで伝達される情報は[APIガイド文書](./api-guide-v2.0/#_23)を参照してください。
    1. **HTTP Method**と**コールバックURL**を入力します。
    2. Query ParameterにCDNサービス変更作業の結果を受け取るには、**コールバックURL**に次のパス(path)変数を含めて入力してください。 
         例：http://callback.url?appKey={appKey}&status={status}&isSuccessful={isSuccessful})

| パス(path)変数 | 説明 | サンプル伝達値 |
| ------------- | --- | ------- |
| {appKey} | CDNサービスのアプリキー | コンソールで発行したアプリキー |
| {domain} | CDNサービス名 | [サービスID].toastcdn.net |
| {status} | 現在のCDNサービスの状態 | OPEN、SUSPEND、CLOSE、ERROR |
| {isSuccessful} | サービス変更作業が成功したか(API v1.0はサポートしません) | "true"または"false" |

### オリジンサーバー
CDNサービスへ配布する原本ファイルを提供するサーバーを設定します。
![CDNサービス作成-基本情報](https://static.toastoven.net/prod_cdn/v2/ja1/console-cdn-create-origin_ja_20200518.png)

- **オリジンサーバー**
オリジンサーバーはCDNサービスへ配布する原本ファイルを提供するサーバーです。オリジンサーバーはIPv4または完全修飾ドメイン名(FQDN、fully qualified domain name)形式で入力できます。IPアドレスは変更される可能性が高いため、ドメインで設定することを推奨します。 
 運用中のオリジンサーバーがない場合は、TOAST Computeサービスのインスタンスを使用するか、TOAST StorageサービスのObject Stroageを利用してください。
  CDNサービスドメインでHTTPS転送をサポートするには、オリジンサーバーがHTTPSレスポンスをサポートする必要があります。 
これはオリジンサーバーにTOAST CDNが信頼する証明書のインストールが必要であることを意味します。 
   信頼する証明書は次の表を参照してください。  
 もしオリジンサーバーがHTTPSレスポンスをサポートできない場合、**原本リクエストHTTPプロトコルダウングレード**設定を利用してください。 
 ただし、**オリジンリクエストHTTPプロトコルのダウングレード**は制約があるため、オリジンサーバーがHTTPSプロトコルをサポートすることを推奨します。 

**[表1]信頼する証明書リスト**

| Common name| Expire Date |SHA-1 Fingerprint |
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
  
- **オリジンサーバーポート**  
 オリジンサーバーはウェブプロトコルをサポートするサービスで運用する必要があります。運用中のHTTP/HTTPSプロトコルのサービスポート番号を設定できます。 
 オリジンサーバーのポートはHTTPまたはHTTPSポートのいずれか1つを入力する必要があり、設定していないポートはポートHTTP:80、HTTPS:443に設定されます。 
 オリジンサーバーのポートは制限されたポートのみ設定できます。設定できるポート番号は次の表を参照してください。  

**[表2]使用可能なオリジンサーバーポート番号**

|ポート番号|
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

- **ソースパス**  
 ソースパスは原本ファイルのパスのうち、下層パスを設定します。コンテンツをリクエストする時、ソースパスを省略してリクエストできます。

> **[例]ソースパスを /files/imagesに設定した場合** 
>
> - 原本ファイルURL：http://your.origin.com/**files/images**/logo.png 
> - CDNサービスURL：http://[サービスID].toastcdn.net/logo.png
> - CDNサービスURLでソースパス(/files/images)を省略してリクエストできます。 

- **オリジンリクエストHTTPプロトコルのダウングレード**  
  CDNエッジ(edge)サーバーは、オリジンサーバーに原本ファイルをリクエストする時、クライアントのオリジンリクエスト(request)のサービスプロトコル(HTTP/HTTPS)でリクエストします。  
 すなわち、クライアントからHTTPSでリクエストし、 オリジンサーバーがHTTPSレスポンスをサポートしていない場合、 CDNエッジサーバーからオリジンサーバーにリクエストする時、HTTPSプロトコルでリクエストするため、原本ファイルを受け取れません。
オリジンサーバーがHTTPプロトコルのみサポートしている場合、**オリジンサーバーHTTPプロトコルダウングレード**設定を使用してCDNエッジサーバーからオリジンサーバーにリクエストする時、HTTPSプロトコルをHTTPプロトコルへダウングレードしてリクエストできます。
  つまり、クライアントからCDNエッジサーバー間は暗号化通信(HTTPS)で通信し、CDNエッジサーバーからオリジンサーバー間は非暗号化通信(HTTP)で通信します。
 原本リクエストHTTPプロトコルをダウングレードする時は、次のような制約があります。
> **[注意]オリジンリクエストHTTPプロトコルのダウングレードの制約事項**
> 1. サイトアドレス全体はプロトコルのダウングレードができません。例えばオリジンサーバーのサイトアドレス全体**www.toast.com**はダウングレードできません。
> 2. GET、HEADおよびOPTIONSメソッド以外のメソッドはサポートされません。 
> 3. CDNサーバーからオリジンサーバーにダウングレードをリクエストする時、次のヘッダーは除外される場合があります。
>    Origin, Referer, Cookie, Cookie2, sec-\*, proxy-\*

- **Forward Host Header**
  CDNサーバーがオリジンサーバーへ原本ファイルをリクエストする時に送信する**Host**ヘッダー値を設定します。
 オリジンサーバーが名前ベースのバーチャルホストで運用中の場合、**リクエストホストヘッダー**設定が必要な場合もあります。オリジンサーバーの運用形態に従って適切な設定値を選択してください。
    - **オリジンサーバーホスト名**：オリジンサーバーのホスト名をHostヘッダーに設定します。 
    - **リクエストホストヘッダ**：クライアントリクエストのHostヘッダーに設定します。


### キャッシュ
CDNキャッシュ動作設定とキャッシュ時間を設定できます。
![CDNサービス作成-キャッシュ](https://static.toastoven.net/prod_cdn/v2/ja1/console-cdn-create-cache_ja_20200518.png)

- **キャッシュ時間設定**
 オリジンサーバーのキャッシュ制御に関するレスポンスヘッダーを通してキャッシュを設定できます。 
    - **オリジン設定使用**：オリジンサーバーのレスポンスで提供したキャッシュ制御ヘッダー(Cache-Control、Expires)を優先して適用します。もしオリジンサーバーのレスポンスでキャッシュ制御ヘッダー(Cache-Control、Expires)が有効ではないか、存在しない場合、キャッシュ時間(秒)に指定した時間キャッシュされます。**オリジン設定を使用**オプションがデフォルト値です。
    - **ユーザー設定使用**：キャッシュ時間(秒)に指定した時間、キャッシュされます。

> **[参考]キャッシュ時間のデフォルト値と有効範囲**
> キャッシュ時間のデフォルト値は0です。デフォルト値を0に設定すると、キャッシュ時間は604,800(単位/秒)=1週間です。
> キャッシュ時間は、デフォルト値の0から2,147,483,647(単位/秒)まで入力できます。

### リファラー(referer)ヘッダアクセス管理
リファラーリクエストヘッダーにコンテンツのアクセス管理を設定します。
![CDNサービス作成-キャッシュ](https://static.toastoven.net/prod_cdn/v2/ja1/console-cdn-create-cache_ja_20200518.png)

リファラーリクエストヘッダーは、現在リクエストされたページリンクより前のウェブページアドレスを含みます。リファラーリクエストヘッダーにより、どの経路でリクエストが流入したかを把握できます。リファラーヘッダアクセス管理では、特定のリファラーリクエストヘッダーのみユーザーコンテンツにアクセスできるよう設定できます。
正規表現形式で入力できます。複数入力する場合は改行して入力します。

- **ブラックリスト(blacklist)タイプ**：
    * 特定のリファラーリクエストヘッダーのみアクセスを制限する時に適しています。
* リファラーリクエストヘッダーの値が設定した正規表現式にマッチする文字列の場合、コンテンツへのアクセスが制限されます。マッチしない文字列の場合、コンテンツへのアクセスが許可されます。
- **ホワイトリスト(whitelist)タイプ**：
    * 特定のリファラーリクエストヘッダーのみアクセスを許可する時に適しています。
    * リファラーリクエストヘッダーの値が正規表現式にマッチする文字列の場合、コンテンツへのアクセスが許可されます。マッチしない文字列の場合、コンテンツへのアクセスが制限されます。

- **リファラー ヘッダがない場合、アクセス許可**
  リファラー(referer) リクエストヘッダがない場合に、コンテンツアクセスを許可するかどうかを選択します。
    - **許可**：リファラーリクエストヘッダがない場合、コンテンツアクセスを許可し、リファラーアクセス制御が動作しません。
    - **拒否**：リファラーリクエストヘッダがない場合、コンテンツアクセスが拒否され、登録された リファラーについてのみアクセスを許可します。

> [例]
>
> * タイプ：ホワイトリスト(Whitelist)
> * 正規表現：`^https://[a-zA-Z0-9._-]*\.toast\.com/.*`
> 任意のtoast.comサブドメインの下層パスからリソースをリクエストした場合にのみ、コンテンツアクセスを許可します。
>
> **[参考]正規表現式のエスケープ文字**
> 一部の文字は、正規表現で特殊文字として扱われます。
> 例えば、正規表現でドット(`.`)は、すべての文字にマッチする特殊文字です。
> 特殊文字としての意味ではなく、通常の文字として解釈する必要がある場合は、エスケープ文字バックスラッシュ(`\`)を特殊文字の前に追加してください。(例： `\.`)
> 正規表現の特殊文字には`^ . [ ] $ ( ) | * + ? { } \`などがあります。
> 複数のリファラーを制御する時は、次の行に連続して入力します。
> APIで複数のリファラーを設定する時は\nトークンで区切って入力します。

### 리퍼러(referer) 헤더 접근 관리
리퍼러 요청 헤더로 콘텐츠의 접근 관리를 설정합니다.
![CDN서비스생성-캐시](https://static.toastoven.net/prod_cdn/v2/console-cdn-create-cache2.png)

리퍼러 요청 헤더는 현재 요청된 페이지의 링크 이전의 웹 페이지 주소를 포함합니다. 리퍼러 요청 헤더로 어떤 경로에서 요청이 유입되었는지 알 수 있습니다. 리퍼러 헤더 접근 관리는 특정 리퍼러 요청 헤더만 사용자 콘텐츠에 접근할 수 있도록 설정할 수 있습니다.
정규 표현식 형태로 입력할 수 있으며, 여러 개를 입력할 때는 줄바꿈을 한 뒤 입력합니다.

- **접근 제어 방식**
  - **블랙리스트(blacklist) 타입**:
      * 특정 리퍼러 요청 헤더만 접근을 제한할 때 적합합니다.
      * 리퍼러 요청 헤더값이 설정한 정규 표현식에 매칭되는 문자열이면 콘텐츠 접근이 제한됩니다. 매칭되지 않는 문자열이면 콘텐츠 접근이 허용됩니다.
  - **화이트리스트(whitelist) 타입**:
      * 특정 리퍼러 요청 헤더만 접근을 허용할 때 적합합니다.
      * 리퍼러 요청 헤더값이 정규 표현식에 매칭되는 문자열이면 콘텐츠 접근이 허용됩니다. 매칭되지 않는 문자열이면 콘텐츠 접근이 제한됩니다.

- **리퍼러 헤더가 없는 경우 접근 허용**
  리퍼러(referer) 요청 헤더가 없는 경우 콘텐츠 접근 허용 여부를 선택합니다.
    - **허용**: 리퍼러 요청 헤더가 없는 경우 콘텐츠 접근을 허용하며 리퍼러 접근 제어가 동작하지 않습니다.
    - **거부**: 리퍼러 요청 헤더가 없는 경우 콘텐츠 접근을 거부하고 등록된 리퍼러에 대해서만 접근을 허용합니다.
  
> **[예시]**
>
> * 타입: 화이트리스트(whitelist)
> * 정규 표현식: `^https://[a-zA-Z0-9._-]*\.toast\.com/.*`
> 임의의 toast.com 서브 도메인의 하위 경로에서 리소스를 요청한 경우에만 콘텐츠 접근을 허용합니다.
>
> **[참고] 정규 표현식의 이스케이프 문자**
> 일부 문자는 정규 표현식에서 특수 문자로 사용됩니다. 
> 점(`.`)을 예로 들자면, 정규 표현식에서 점(`.`)은 모든 문자와 일치함을 나타내는 특수 문자입니다.
> 특수 문자로의 의미가 아닌 일반 문자 그대로 해석해야 한다면 이스케이프 문자 백슬래시(`\`)를 특수 문자 앞에 추가하면 됩니다(예: `\.`).
> 정규 표현식의 특수 문자에는 `^ . [ ] $ ( ) | * + ? { } \` 등이 있습니다.
> 여러 개의 리퍼러를 제어할 때는 다음 줄에 연속해 입력합니다.
> API를 이용해 여러 개의 리퍼러를 설정할 때는 \n 토큰으로 구분해 입력합니다.

### Auth Token 인증 접근 관리
Auth Token 인증 접근 관리는 콘텐츠 요청에 인증 토큰을 추가하여 CDN 에지 서버에서 검증된 토큰만 콘텐츠 접근을 허용하는 보안 기능입니다.
일회성으로 콘텐츠 접근 허용하거나 제한된 사용자만 콘텐츠를 접근 할 수 있도록 제어할 수 있습니다.
토큰이 없거나 유효하지 않은 토큰으로 콘텐츠를 요청한 경우, 403 Forbidden 응답이 전송되며 콘텐츠에 접근할 수 없습니다.

Auth Token 인증 접근을 CDN 서비스에 적용하려면 다음의 단계에 따라 작업이 필요합니다.

> **[주의]** 
>
> Auth Token 인증 접근 관리는 TOAST CDN을 이용해 서비스 중인 애플리케이션에서도 다음의 구현이 필요합니다.
> 1. 콘텐츠 접근에 필요한 토큰을 생성해야 합니다.
> 2. 클라이언트(최종 콘텐츠 소비자)가 생성된 토큰을 포함하여 콘텐츠를 요청할 수 있도록 해야합니다.
> 이 작업을 하지 않고 Auth Token 인증 접근 관리를 설정할 경우, 토큰 검증 실패로 인해 콘텐츠 요청이 실패될 수 있으므로 주의하시기 바랍니다.


#### 1. TOAST CDN 콘솔 > Auth Token 인증 접근 관리 설정

CDN 콘솔에서 다음의 내용을 참고하여 Auth Token 인증 접근 관리를 설정합니다.

![CDN서비스생성-Auth Token 인증 접근 관리](https://static.toastoven.net/prod_cdn/v2/console-cdn-create-auth-token.png)

- **토큰 인증 사용 여부**
    - **사용**: Auth Token 인증 접근 관리 기능을 활성화하여 토큰 검증한 후 콘텐츠에 접근할 수 있도록 합니다.
    - **미사용**: Auth Token 인증 접근 관리 기능을 비활성화 합니다.
- **토큰 위치**: 콘텐츠 요청 시 토큰을 전달할 위치를 선택합니다.  
    - **쿠키(Cookie)**: 표준 쿠키로 토큰을 전달합니다. 쿠키 사용을 지원하지 않는 장치 및 브라우저는 토큰 인증이 정상적으로 동작하지 않을 수 있으므로 주의하시기 바랍니다.  
    - **요청 헤더(Request header)**: 요청 헤더에 토큰을 전달합니다.  
    - **쿼리 문자열(Query string)**: 쿼리 문자열에 토큰을 전달합니다.  
- **토큰 이름**
    - 토큰값을 전달할 토큰의 이름입니다. **token** 으로 고정된 값이며 콘솔 설정에서 변경이 불가합니다.
- **토큰 암호화 키**
    - 토큰 생성에 필요한 암호화키 입니다. CDN 서비스를 생성 또는 수정하면 암호화 키는 자동으로 생성됩니다.
    - 암호화키는 외부로 노출되지 않도록 주의하시기 바랍니다.  
- **토큰 인증 대상 설정**  
    콘텐츠 접근 시 토큰을 인증할 파일 대상을 설정합니다.
    토큰 인증 대상 파일인 경우에만 토큰을 검증하며, 인증 대상 파일이 아닌 경우에는 토큰 검증을 수행하지 않으므로 토큰 없이 콘텐츠 접근이 가능합니다.
    지정된 요청 URL 경로 또는 파일 확장자만 토큰 검증을 하려면 요청 URL 경로와 확장자를 입력해주세요. 입력하지 않은 경우 모든 파일에 대해 토큰을 검증합니다.  
    - **인증 대상 설정**: 설정된 요청 URL 경로와 파일 확장자의 파일만 토큰을 검증합니다.
    - **인증 예외 대상 설정**: 설정된 요청 URL 경로와 파일 확장자를 제외한 파일의 토큰을 검증합니다.
    - **요청 URL 경로**: 콘텐츠 URL이 요청 URL 경로와 일치되는 경우 토큰 인증 대상 또는 예외 대상으로 설정합니다.
        - 요청 URL 경로는 '/'로 시작해야 하며 와일드카드 문자(여러 문자열: *, 단일 문자: ?)를 사용할 수 있습니다(예: /toast/*).
        - 요청 URL 경로는 쿼리 문자열은 포함하지 않습니다.
        - 요청 URL 경로는 아스키(ascii) 코드 문자만 입력 가능합니다.
        - 여러 개를 입력하려면 다음 줄에 입력하세요. 여러 개를 입력한 경우 하나만 일치해도 토큰 접근 제어가 동작합니다.  
        - 파일 확장자와 함께 입력한 경우에는 파일 확장자 조건이 일치해도 토큰 접근 제어가 동작합니다.
    - **파일 확장자**: 콘텐츠 URL이 파일 확장자와 일치되는 경우 토큰 인증 대상 또는 예외 대상으로 설정합니다.
        - '.'을 포함하지 않은 파일 확장자를 입력합니다(예: pdf, png).
        - 여러 개를 입력하려면 다음 줄에 입력하세요. 여러 개를 입력한 경우 하나만 일치해도 토큰 접근 제어가 동작합니다.  
        - 요청 경로 URL과 함께 입력한 경우에는 요청 경로 URL 조건이 일치해도 토큰 접근 제어가 동작합니다.

> **[주의] 요청 URL 경로와 파일 확장자**
> 요청 URL 경로와 파일 확장자 모두 설정한 경우, 두 조건 중 하나만 일치해도 토큰 접근 제어가 동작합니다.
> [예시] 요청 URL 경로 **/toast/***, 파일 확장자 **png** 가 설정된 경우: /toast 하위의 모든 파일 또는 파일 확장자가 png인 콘텐츠에 대해 토큰을 검증합니다.


#### 2. 토큰 생성 
최종 콘텐츠 사용자가 콘텐츠에 접근하려면 토큰과 함께 콘텐츠를 요청해야 합니다. 따라서, 토큰을 생성해 최종 콘텐츠 사용자에게 발급해야 합니다.
토큰 생성은 TOAST CDN을 이용해 서비스 중인 애플리케이션에서 구현되어야 합니다.
토큰 생성 방법은 다음의 샘플 코드를 참고하여 토큰을 생성합니다.

  <details>
  <summary>Java 샘플 코드</summary>
  이 샘플 코드는 아래와 같은 제약 사항이 있습니다.  
  JDK 7 이상, org.projectlombok:lombok, org.apache.commons:commons-lang3 라이브러리와 의존성이 있습니다. 
  
  ```java

  import org.apache.commons.lang3.StringUtils;
  import javax.crypto.Mac;
  import javax.crypto.spec.SecretKeySpec;
  import javax.xml.bind.DatatypeConverter;
  import java.io.UnsupportedEncodingException;
  import java.math.BigInteger;
  import java.net.URLEncoder;
  import java.security.InvalidKeyException;
  import java.security.NoSuchAlgorithmException;
  import java.util.Calendar;
  import java.util.TimeZone;
  import java.util.regex.Matcher;
  import java.util.regex.Pattern;

  public class ToastAuthTokenAccessControlExample {

      // TOAST 콘솔에서 확인한 인증 토큰 암호화 키
      private static final String AUTH_TOKEN_ENCRYPT_KEY = "{TOAST CDN 서비스의 토큰 암호화 키}";
      // 토큰 유효 시간(seconds)
      private static final Long TOKEN_DURATION_SECONDS = 3600L;


      public static void main(String[] args) throws AuthTokenException {

          String path = "/toast/%EC%9D%B8%EC%A6%9D/%E1%84%91%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AF.png";
          String singleWildcardPath = "/toast/%EC%9D%B8%EC%A6%9D/*";
          String[] multipleWildcardPath = {"/toast/%EC%9D%B8%EC%A6%9D*", "/toast/auth/*"};

          System.out.println(" ----------------- ");
          System.out.println(" 기본 토큰 발급 ");
          System.out.println(" ----------------- ");

          AuthToken authToken = new AuthToken(AUTH_TOKEN_ENCRYPT_KEY, TOKEN_DURATION_SECONDS);

          System.out.println("단일 URL 토큰: token=" + authToken.generateURLToken(path));
          System.out.println("와일드카드 토큰: token=" + authToken.generateWildcardPathToken(singleWildcardPath));
          System.out.println("멀티 와일드카드 토큰: token=" + authToken.generateWildcardPathToken(multipleWildcardPath));

          System.out.println(" ----------------- ");
          System.out.println(" 세션 식별자를 포함한 토큰 발급 ");
          System.out.println(" ----------------- ");

          AuthToken authTokenWithSession = new AuthToken(AUTH_TOKEN_ENCRYPT_KEY, TOKEN_DURATION_SECONDS, "example-sessionId");
          System.out.println("단일 URL 토큰: token=" + authTokenWithSession.generateURLToken(path));
          System.out.println("와일드카드 토큰: token=" + authTokenWithSession.generateWildcardPathToken(singleWildcardPath));
          System.out.println("복수 와일드카드 토큰: token=" + authTokenWithSession.generateWildcardPathToken(multipleWildcardPath));

      }


      public static class AuthToken {

          /** 토큰 암호화 알고리즘(SHA256 고정) **/
          private static final String HMAC_SHA_256 = "HmacSHA256";

          /** 토큰 암호화 키 (TOAST CDN 콘솔 > Auth Token 인증 접근 관리 > 암호화 키) **/
          private String key;

          /**  세션 식별자 */
          private String sessionId;

          /** 토큰의 유효 시간(단위: 초) */
          private Long durationSeconds;

          /** 토큰 생성 전 url encode 적용 여부 */
          private Boolean escapeEarly;

          /** 토큰 Body 필드의 구분자 */
          private final String fieldDelimiter = "~";

          /** wildcardPath 구분자 */
          private final String aclDelimiter = "!";


          public AuthToken(String key, Long durationSeconds) {
              this.key = key;
              this.sessionId = null;
              this.durationSeconds = durationSeconds;
              this.escapeEarly = true;

          }


          public AuthToken(String key, Long durationSeconds, String sessionId) {
              this.key = key;
              this.sessionId = sessionId;
              this.durationSeconds = durationSeconds;
              this.escapeEarly = true;
          }


          /**
          * 단일 URL에 대한 토큰을 생성합니다.
          * @param path : contents url (example: /auth/contents/example.png)
          * @return created token
          * @throws AuthTokenException
          */
          public String generateURLToken(String path) throws AuthTokenException {
              return generateToken(createExpireTime(), this.sessionId, path, null);

          }


          /**
          * 와일드카드 경로에 대한 토큰을 생성합니다.
          * @param wildcardPath : "/auth/contents/*"
          * @return 생성된 토큰값
          * @throws AuthTokenException
          */
          public String generateWildcardPathToken(String wildcardPath) throws AuthTokenException {
              return generateWildcardPathToken(new String[] {wildcardPath});
          }

          /**
          * 복수 개의 와일드카드 경로에 대한 토큰을 생성합니다.
          * @param wildcardPaths (example: ["/auth/contents/*", "/auth/*/images/*"])
          * @return 생성된 토큰값
          * @throws AuthTokenException
          */
          public String generateWildcardPathToken(String... wildcardPaths) throws AuthTokenException {
              return generateToken(createExpireTime(), this.sessionId, null, wildcardPaths);

          }


          private String createExpireTime() {
              Long nowSeconds = Calendar.getInstance(TimeZone.getTimeZone("UTC")).getTimeInMillis() / 1000L;
              Long exp = nowSeconds + this.durationSeconds;
              return exp.toString();
          }


          private String generateToken(String exp, String sessionId, String path, String[] wildcardPaths) throws AuthTokenException {

              try {

                  StringBuilder token = new StringBuilder();
                  token.append("exp=")
                      .append(exp)
                      .append(this.fieldDelimiter);

                  if (wildcardPaths != null && wildcardPaths.length > 0) {
                      token.append("acl=")
                          .append(escapeEarly(StringUtils.join(wildcardPaths, this.aclDelimiter)))
                          .append(this.fieldDelimiter);
                  }

                  if (sessionId != null && sessionId.length() > 0) {
                      token.append("id=")
                          .append(escapeEarly(sessionId))
                          .append(this.fieldDelimiter);
                  }

                  StringBuilder hashSource = new StringBuilder(token);
                  if (path != null && path.length() > 0) {
                      hashSource.append("url=")
                                .append(escapeEarly(path))
                                .append(this.fieldDelimiter);

                  }

                  // remove last fieldDelimiter char
                  hashSource.deleteCharAt(hashSource.length() - 1);

                  Mac hmac = Mac.getInstance(HMAC_SHA_256);
                  byte[] keyBytes = DatatypeConverter.parseHexBinary(this.key);
                  SecretKeySpec secretKey = new SecretKeySpec(keyBytes, HMAC_SHA_256);
                  hmac.init(secretKey);

                  byte[] hmacBytes = hmac.doFinal(hashSource.toString().getBytes());
                  return token.toString() + "hmac=" + String.format("%0" + (2 * hmac.getMacLength()) + "x", new BigInteger(1, hmacBytes));

              } catch (NoSuchAlgorithmException e) {
                  throw new AuthTokenException(e.getMessage());
              } catch (InvalidKeyException e) {
                  throw new AuthTokenException(e.getMessage());
              }

          }


          private String escapeEarly(final String text) throws AuthTokenException {
              if (this.escapeEarly == true) {
                  try {
                      StringBuilder newText = new StringBuilder(URLEncoder.encode(text, "UTF-8"));
                      Pattern pattern = Pattern.compile("%..");
                      Matcher matcher = pattern.matcher(newText);
                      String tmpText;
                      while (matcher.find()) {
                          tmpText = newText.substring(matcher.start(), matcher.end()).toLowerCase();
                          newText.replace(matcher.start(), matcher.end(), tmpText);
                      }
                      return newText.toString();
                  } catch (UnsupportedEncodingException e) {
                      return text;
                  } catch (Exception e) {
                      throw new AuthTokenException(e.getMessage());
                  }
              } else {
                  return text;
              }
          }

      }

      public static class AuthTokenException extends Exception {
          private static final long serialVersionUID = 1L;


          public AuthTokenException(String msg) {
              super(msg);
          }
      }

  }
  ```

   - AuthToken 클래스의 멤버 변수 설명
      - key: TOAST CDN 콘솔에 표시된 Auth Token 인증 제어 관리 > 토큰 암호화 키를 입력합니다.
      - sessionId: 단일 접근 요청에 대한 고유 식별자를 포함하여 토큰을 생성하려면 sessionId를 입력합니다.
         - 세션 ID 별로 유효한 토큰을 생성하여 일회성 토큰을 생성하거나 다양한 사례에 활용할 수 있습니다.
         - 세션 ID는 [출력 가능 아스키 문자표](https://ko.wikipedia.org/wiki/ASCII#%EC%B6%9C%EB%A0%A5_%EA%B0%80%EB%8A%A5_%EC%95%84%EC%8A%A4%ED%82%A4_%EB%AC%B8%EC%9E%90%ED%91%9C.)로 구성해야 합니다.
         - 세션 ID는 문자열의 길이는 최대 36바이트를 초과할 수 없습니다.
      - durationSeconds: 생성된 토큰이 유효한 시간(초), 유효 시간이 지난 토큰은 토큰 인증에 실패합니다.
        - 토큰 유효 시간을 너무 작게 설정하면 CDN 에지 서버에서 토큰 검증하기 전에 토큰이 만료될 수 있으니 유의하시기 바랍니다. 기대하는 토큰 유효 시간보다 10초이상 크게 설정하기를 권장합니다.
        - 토큰 생성 서버의 시간 동기화 설정 NTP (Network Time Protocol, NTP)이 유효한지 반드시 검증하시기 바랍니다. 동기화 되지 않은 시간 정보로 인해 토큰 유효 시간 검증이 실패할 수 있습니다.

  - AuthToken 클래스의 공개 메서드(Public Method)
    - public String generateURLToken(String path)
      - 단일 경로에 대한 토큰을 생성합니다.
      - [예시] path: authToken.generateURLToken("/auth/contents/example.png")
      - [주의] 경로 또는 세션 ID는 URL 인코딩 문자열로 변경한 후에 토큰을 생성하시기 바랍니다(예: **/toast/인증/파일.png** => **/toast/%EC%9D%B8%EC%A6%9D/%E1%84%91%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AF.png**).
      - [주의] `!`, `~` 문자는 예약된 문자로 사용되므로 경로 또는 세션 ID에 포함하지 않도록 합니다.
    
    - public String generateWildcardPathToken(String wildcardPath), public String generateWildcardPathToken(String... wildcardPaths)
      - 와일드카드 경로와 매핑되는 경로의 토큰을 생성합니다. 경로의 패턴이 일치하는 경우, 와일드카드 토큰 하나로 여러 콘텐츠 URL의 토큰을 인증할 수 있습니다.
      - [예시1] wildcardPath: authToken.generateWildcardPathToken("/auth/contents/*") : /auth/contents 하위의 모든 파일에 대해 토큰을 발급합니다.
      - [예시2] wildcardPath: authToken.generateWildcardPathToken("/auth/contents/*.png") : /auth/contents 경로의 png 파일에 대한 토큰을 발급합니다.
      - [예시3] wildcardPath: authToken.generateWildcardPathToken("/auth/contents/exmaple?.png") : /auth/contents 경로의 example 와 단일 문자가 결합된 png 파일에 대한 토큰을 발급합니다.
      - [주의] 경로 또는 세션 ID는 URL 인코딩 문자열로 변경한 후에 토큰을 생성하시기 바랍니다(예: **/toast/인증/파일.png** => **/toast/%EC%9D%B8%EC%A6%9D/%E1%84%91%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AF.png**).
      - [주의] **!**, **~** 문자는 예약된 문자로 사용되므로 경로 또는 세션 ID에 포함하지 않도록 합니다.
  - 생성된 토큰은 **exp={expirationTime}~acl={path!path!path}~id={sessionId}~hmac={HMAC}** 형식으로 생성됩니다.
    - [예시] 생성된 토큰: **`**exp=1600331503~acl=%2ftoast%2f*.png~id=session-id1~hmac=2509123dcabe2fc199e3ac44793e4e135a09590ff4ebf6a902ea26469ead7f91**

  </details>

#### 3. 생성된 토큰을 콘텐츠 요청에 포함
클라이언트(최종 콘텐츠 소비자)가 콘텐츠 요청시 콘솔에서 설정한 토큰 위치에 생성된 토큰값을 포함하여 요청하도록 합니다.

  - **토큰 위치: 쿠키**
    ```
    curl --cookie "token={생성된 토큰값}" \
    -X GET http://xxx.toastcdn.net/auth/contents/example.png
    ```
  - **토큰 위치: 요청 헤더**
    ```
    curl -H "token: {생성된 토큰값}" \
    -X GET http://xxx.toastcdn.net/auth/contents/example.png
    ```
  - **토큰 위치: 쿼리 문자열**
    ```
    curl -d "token={생성된 토큰값}" \
    -X GET http://xxx.toastcdn.net/auth/contents/example.png
    ```

## 設定

### CDNサービスの設定変更
サービスドメイン名と地域以外のCDNサービス設定を変更できます。
![CDNサービスの修正を有効化](https://static.toastoven.net/prod_cdn/v2/ja1/console-cdn-modify1_ja_20200518.png)

1. 変更するCDNサービスをCDNサービスリストから選択します。
2. 画面下、**設定**タブの**修正**ボタンをクリックします。

次のように変更可能な項目が有効になります。
![CDNサービスの修正を確認](https://static.toastoven.net/prod_cdn/v2/ja1/console-cdn-modify2_ja_20200518.png)

* 変更する設定内容を修正します。 
* **確認**ボタンをクリックして、変更を完了します。
* 説明とコールバック以外の設定を変更するには、全てのCDNサーバーに反映される必要があるため、修正作業に時間がかかる場合があります。

**修正作業は数十分以内に完了しますが、ドメインエイリアス設定の変更時は数時間かかる場合があります。**

> **[参考] CDNサービス修正中配布状態とサービス状態** 
> サービス修正作業が進行中の場合は、既存のCDNサービス設定で運用されます。
> 修正作業が失敗した場合、既存設定情報にロールバックされ、CDNサービスリストの配布状態が赤色で表示されます。設定情報にエラーがあったり、内部的なエラーが発生した場合は修正作業が失敗します。 

### CDNサービスの一時停止と再開
CDNサービスを一時的に中断または、再開することができます。


1. 一時停止するCDNサービスを選択します。
2. **一時停止**ボタンをクリックします。
![CDNサービス-一時停止](https://static.toastoven.net/prod_cdn/v2/ja1/console-cdn-pause_ja_20200518.png)
3. 証明書の連携をしているCDNサービスには、証明書の無効警告案内が表示されます。証明書の無効を回避するには、証明書更新開始日の前にCDNサービスを再開する必要があります。
![CDNサービス-一時停止](https://static.toastoven.net/prod_cdn/v2/ja1/console-cdn-restart_ja_20200518.png)
4. 一時停止状態のCDNサービスを再開するには再開するCDNサービスを選択します。
5. **再開**ボタンをクリックします。




> **[参考]一時停止と再開動作の遅延**
> CDNサービスの一時停止と再開は、CDNサービスドメインのDNSレコードを変更して動作します。 
> したがってキャッシュDNSサーバーでTTLの間キャッシュされているか、DNSの配信に応じて、一時停止/再開が完了しても、即時に一時停止/再開が動作しない場合もあります。

> **[注意]発行された証明書が連携されているCDNサービスの一時停止**
> 証明書を連携しているCDNサービスを一時停止する場合、証明書の更新ができません。
> **証明書管理** > 証明書リストの**証明書更新開始日**の前にCDNサービスを再開してください。
> 証明書更新開始日から5日間は、証明書更新期間のため、この期間に一時停止を行うと証明書が無効になる場合もあるため、注意してください。


### CDNサービスの削除 
CDNサービスを削除します。削除すると復旧できませんので注意してください。 

1. 削除するCDNサービスを選択します。
2. **削除**ボタンをクリックします。
![CDNサービス-削除](https://static.toastoven.net/prod_cdn/v2/ja1/console-cdn-delete_ja_20200518.png)
3. 証明書が連携されているCDNサービスは、証明書無効警告案内が表示されます。証明書が無効になるのを回避するには、サービス中の他のCDNサービスに証明書を連携してください。


> **[参考] CDNサービス削除の所要時間**
> CDNサービス削除作業は数時間(最大2～3時間)かかる場合があります。

> **[注意]発行された証明書が連携されているCDNサービスの削除**
> 証明書が連携されているCDNサービスを削除すると、証明書を更新できません。 
> **証明書管理**の証明書リストから、**証明書更新開始日**より前にサービス中の他CDNサービスへ連携してください。
> 証明書更新開始日から5日間は、証明書更新期間です。この期間に削除を行うと証明書が無効になる場合があるため、注意してください。


## CDNキャッシュの再配布(purge)
CDNキャッシュサーバーは、キャッシュ設定に従って、指定された時間オリジンサーバーのファイルをキャッシュします。ファイルをキャッシュすると、原本ファイルが変更されてもキャッシュが満了するまでは変更前の原本ファイルを維持します。 
変更された原本ファイルへ即時にアップデートするには、**キャッシュ再配布**をリクエストする必要があります。
キャッシュ再配布を行うと、リクエストしたコンテンツの古いキャッシュデータを削除し、オリジンサーバーから新しい原本ファイルを再度キャッシュします。 

1. 変更したいサービスをCDNサービスリストから選択します。
2. **キャッシュ再配布**タブをクリックします。
![CDNキャッシュ再配布](https://static.toastoven.net/prod_cdn/v2/ja1/console-cdn-purge_ja_20200518.png)

3. キャッシュ再配布タイプを選択します。
  - CDNサービスドメインに応じてサポートされるキャッシュ再配布タイプとリクエスト様式が異なるため注意してください。
  - ドメインの再配布タイプとリクエスト様式
    * 特定ファイル：再配布するコンテンツのURLを入力します。リクエストしたURLのみキャッシュが再配布されるため、ドメインエイリアスに複数のサービスドメインアドレスがある場合、各URLにリクエストする必要があります。
      * 例)基本サービスドメインアドレス： http://[サービスID].toastcdn.net/path/to/file1.jpg
      * 例)ドメインエイリアスドメインアドレス： http://customer.domain.com/path/to/file1.jpg
    * 全体ファイル：キャッシュファイルをすべて削除します。オリジンサーバーに膨大なトラフィックが流入する場合もあるため注意してください。
4. 選択したキャッシュ再配布タイプに合わせて、再配布するファイルを指定します。
5. **キャッシュ再配布**ボタンをクリックして、再配布をリクエストします。

キャッシュ再配布は使用量制限があるため、下記の表を参照して使用量が超過しないように注意してください。

|分類 |[サービスID].toastcdn.net |
|---|---|
| 制限単位 | プロジェクト別(AppKey) |
| 特定ファイル | 1秒間に1回リクエスト可能、リクエスト毎のURL数制限：200 URL |
| ワイルドカード | 未サポート |
| 全体ファイルタイプ | 5分毎のリクエスト可能数：1回 |

> **[注意] [サービスID].toastcdn.comサービスを作成した後、キャッシュ再配布失敗エラー**
> CDNサービスを作成した後、約1時間はキャッシュ再配布リクエストが失敗する場合があります。その後も継続して失敗する場合はサポートへお問い合わせください。

## 証明書の管理 
所有しているドメインでコンテンツをHTTPSで転送するには、所有しているドメインの証明書をCDNサーバーに配布する必要があります。証明書がない場合、クライアント(ブラウザ)とCDNエッジサーバー間の暗号化通信(HTTPS)を行うことができず、証明書エラーが発生します。
TOAST CDNの証明書管理は次のような機能を提供します。

- 単一ドメインタイプの証明書発行
- 全世界の拠点のCDNサーバーに証明書を配布(中国とロシアは除外)
- 証明書満了前の自動更新

### 新規証明書の発行 
**証明書管理**タブで証明書を発行できます。
![CDN新規証明書の発行](https://static.toastoven.net/prod_cdn/v2/ja1/console-certificate-create_ja_20200518.png)

1. **証明書管理**タブの**新規証明書発行**ボタンをクリックします。
2. 発行する証明書のドメインを全体ドメインアドレス(FQDN、fully qualified domain name)形式で入力します。
3. 証明書発行案内の内容を確認し、**確認**ボタンをクリックします。
4. 新規証明書の発行をリクエストすると、**証明書管理**タブの証明書ドメインが表示されます。証明書の状態が**ドメイン検証**状態に変更された後にドメイン検証作業を進行してください。 

> **[注意]証明書発行前の確認事項**
> 1. 所有しているドメインのみ証明書の発行が可能なため、先にドメインを購入してから進行してください。 
> 2. 他の認証機関(CA、certificate authority)で発行した証明書は利用できません。 
> 3. 単一ドメインの証明書発行のみ行えます。ワイルドカード、マルチドメインなどの 証明書はサポートしません。
> 4. 証明書の発行は1つのプロジェクトにつき5個に制限されます。限度の調整が必要な場合はTOASTサポートへお問い合わせください。
> 5. 新規証明書の発行をリクエストした後、ドメイン検証段階は数十分(最大1～2時間)後に変更される場合があります。証明書の状態がドメイン検証状態に変更されたらTOASTプロジェクトメンバーを対象にメールが送信されます。もしシステムエラーによりメールが送信されない場合、コンソールで状態を確認してください。 

### ドメイン検証 
新規証明書の発行をリクエストした後、証明書の状態が｢ドメイン検証｣になったら、ドメインの検証を行ってください。
ドメインの検証方法は、コンソールでドメインを選択して確認するか、プロジェクトメンバーに転送されたドメイン検証ガイドメールの内容を参照してください。

![CDNドメイン検証](https://static.toastoven.net/prod_cdn/v2/ja1/console-certificate-domain-validation_ja_20200518.png)

ドメイン検証は、発行をリクエストした証明書ドメインの実際の所有者であることを確認する段階です。ドメイン検証を行っていない場合、証明書の発行ができません。
ドメインの所収者であることを確認するために、ドメイン検証方式を通してドメインの制御権限を確認します。 
ドメイン検証方式には、**DNS TXTレコード追加**または**HTTPページ追加**方式があります。**2つの方式のうち1つを実施**してください。

![CDNドメイン検証](https://static.toastoven.net/prod_cdn/v2/ja1/console-certificate-domain-validation2_ja_20200518.png)

#### DNS TXTレコード追加方式 
ドメインのDNS制御権限を確認してドメインを検証します。

1. ドメインのDNSサービス提供業者のDNS管理ページでTXTレコードを追加します。
   DNSの設定方法はDNSサービス提供業者によって異なる場合があります。設定方法は該当サービス業者へお問い合わせください。
  - レコードタイプ：**TXT**
  - TTL：**60**. 60に設定できない場合は、できるだけ小さい値を設定してください。
  - レコード名：**_acme-challenge.[発行をリクエストした証明書ドメイン].**コンソールまたは送信されたメールガイドの**レコード名**を作成します。
  - レコード値：**任意の文字列**(コンソールまたは送信されたメールガイドの**レコード値**を作成します。)

2. nslookupコマンドを使って、追加したTXTレコードを確認します。DNS配信時間に応じて確認できるまで時間がかかる場合があります。
    `nslookup -type=TXT _acme-challenge.[発行リクエストした証明書ドメイン].`

次の画面はTOAST DNS+ サービスで設定した例です。DNSサービス提供業者によって設定方法は異なる場合があります。
![CDNドメイン検証](https://static.toastoven.net/prod_cdn/v2/ja1/console-certificate-domain-validation-dns_ja_20200518.png)


#### HTTPページ追加方式 
ドメインが接続されたWebサーバーにHTTPページを追加してドメインを検証します。

1. Webサーバーの**http://[発行リクエストした証明書ドメイン]/.well-known/acme-challenge/[任意の文字列]**パスにHTTPページを追加します。 
2. HTTPページの本文にコンソールまたは送信されたメールガイドの**ページコンテンツ(トークン)**の値を設定します。 
3. Webブラウザで**http://[発行リクエストした証明書ドメイン]/.well-known/acme-challenge/[任意の文字列]**URLで接続したら**ページコンテンツ(トークン)**の値が画面に表示されるかを確認します。 

> **[注意]ドメイン検証の注意事項**
> 1. ドメイン検証は、証明書発行リクエストを行った日から**5日以内**に実施する必要があります。**期間内に実施しなかった場合、証明書の発行は自動でキャンセル処理**が行われます。
> 2. ドメイン検証作業完了後、検証が成功した際は、数時間で証明書の発行および配布作業が行われます。1日以上実施されない場合、ドメイン検証作業の内容が正しいかを確認します。問題がないにもかかわらず実施されない場合はTOASTサポートへお問い合わせください。
> 3. ドメイン検証方式のHTTPページ追加方式は、HTTPサーバーが基本ポート80で運用中の場合にのみ使用できます。ポートの変更ができない場合は、DNS TXTレコード追加方式を利用してください。

### 証明書の発行および配布
ドメイン検証に成功したら、数時間以内に証明書の発行および配布作業が実施されます。 
コンソールで証明書の状態が**証明書発行および配布**段階と表示され、TOASTプロジェクトメンバーを対象に通知メールが送信されます。 
この段階では別途作業する内容はありません。

>  **[参考]証明書の発行と配布段階の作業時間**
> 証明書の発行および配布作業は最大9時間以上かかる場合があります。 

### CDNサービス連携
発行された証明書を利用するには、CDNサービスと連携する必要があります。 
この作業を行わなかったり、作業内容を維持しない場合、発行された証明書が無効になる場合があるため、注意してください。 

1. **CNAMEレコード設定**：証明書ドメインのDNSサービス提供業者のDNS管理で、次のCNAMEレコードを追加します。 
    - レコードタイプ：**CNAME**
    - TTL：任意の値。頻繁に変更する必要がある場合は小さい値に設定することを推奨します。レコード変更時、キャッシュDNSサーバーにTTL時間中はキャッシュされる場合があります。
    - レコード名：**[証明書ドメイン].**(例：test.alias.com.com.)
    - レコード値：**[連携するCDNサービスドメイン]**(例：xxxxxxxx.toastcdn.net)
次の画面はTOAST DNS+ サービスで設定した例です。DNSサービス提供業者によって設定方法は異なる場合があります。
![CDNサービス連携-CNAME委任](https://static.toastoven.net/prod_cdn/v2/ja1/console-certificate-service-cname_ja_20200518.png)

2. **ドメインエイリアス設定**：証明書を利用するCDNサービスにドメインエイリアス設定を追加します。
    -  **CDNサービス**タブで連携するCDNサービスを選択して**修正**ボタンをクリックします。ドメインエイリアスに証明書ドメインを追加して**確認** ボタンをクリックします。
![CDNサービス連携-ドメインエイリアス](https://static.toastoven.net/prod_cdn/v2/ja1/console-certificate-service-alias_ja_20200518.png)


>  **[注意]証明書満了注意事項**
> TOAST CDNで提供する証明書は、証明書が無効になる前に自動で証明書の更新を実施します。
> 自動的に証明書の更新を行うには、使用中の証明書がCDNサービスと連携している必要があります。
> CDNサービスと連携していない場合、証明書更新期間に更新することができず、証明書が無効になる場合があります。
> 証明書の更新は**証明書管理** > リストに表示された証明書更新開始日から**5日以内**に行われます。
> 証明書が無効にならないように、次の設定事項を常に維持してください。
> 
> 1. 証明書のドメインは、CNAMEレコードに連携するCDNサービスドメインアドレスに委任する必要があります。
> 2. 連携するCDNサービスのドメインエイリアスに証明書ドメインが設定されている必要があります。
> 3. 証明書が連携されたCDNサービスを一時停止すると、証明書を更新できません。証明書の更新開始日前に再開するか、運用中の他のCDNサービスに証明書を連携してください。
> 4. 証明書が連携されたCDNサービスを削除すると、証明書を更新できません。削除する前に、運用中の他のCDNサービスに証明書を連携してください。 

CDNサービス連携作業が完了すると、証明書の状態が｢正常｣と表示されます。
![CDN証明書正常状態](https://static.toastoven.net/prod_cdn/v2/ja1/console-certificate-active_ja_20200518.png)

## 統計

ネットワーク転送量、HTTP状態コード別の統計および、ダウンロードが最も多いコンテンツの順位統計を確認できます。
7日以内の統計データは正確ではないため、参考程度に利用してください。正確な統計データは7日後に確認してください。

1. **Contents Delivery > CDN**の**統計**タブをクリックします。
![cdn_08_201812](https://static.toastoven.net/prod_cdn/cdn_08_ja1_20200518.png)
2. 統計を確認するには、CDNサービスを選択します。
3. 検索期間を入力します。
4. 検索期間内のデータ周期は、選択した期間に応じて自動的に選択されます。
5. **検索**ボタンをクリックします。
