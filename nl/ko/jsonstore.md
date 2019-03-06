---

copyright:
  years: 2018, 2019
lastupdated:  "2019-02-12"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:pre: .pre}


# JSONStore
{: #jsonstore }
{{site.data.keyword.mobilefoundation_short}} **JSONStore**는 경량의 문서 중심 스토리지 시스템을 제공하는 선택적 클라이언트 측 API입니다. JSONStore는 **JSON 문서**의 지속적 스토리지를 사용으로 설정합니다. 애플리케이션에서 실행 중인 디바이스가 오프라인 상태인 경우에도 애플리케이션에 포함된 문서를 JSONStore에서 사용할 수 있습니다. 항상 사용 가능한 이 지속적 스토리지는 예를 들어 디바이스에 사용 가능한 네트워크 연결이 없는 경우 사용자에게 문서에 대한 액세스를 부여하는 데 유용할 수 있습니다.

이 문서에서는 때때로 JSONStore를 설명하는 데 도움을 주기 위해 개발자에게 익숙한 관계형 데이터베이스 용어가 사용됩니다. 그러나 관계형 데이터베이스와 JSONStore 간에는 여러 차이점이 있습니다. 예를 들어, 관계형 데이터베이스에서 데이터를 저장하는 데 사용되는 엄격한 스키마는 JSONStore의 접근 방식과 다릅니다. JSONStore에서는 JSON 컨텐츠를 저장하고 검색해야 하는 컨텐츠의 색인을 작성할 수 있습니다.

## 주요 기능
{: #key-features-jsonstore }
* 효율적인 검색을 위해 데이터 색인 작성
* 저장된 데이터의 로컬로만 수행된 변경사항 추적에 사용되는 메커니즘
* 다중 사용자에 대한 지원
* 저장된 데이터의 AES 256 암호화는 보안 및 기밀성을 제공합니다. 단일 디바이스에 둘 이상의 사용자가 있는 경우 비밀번호 보호를 사용하여 사용자별로 보호를 세그먼트화할 수 있습니다.

하나의 저장소에 여러 콜렉션이 있을 수 있으며, 각 콜렉션에는 여러 문서가 포함될 수 있습니다. 또한 다중 저장소로 구성된 MobileFirst 애플리케이션이 있을 수도 있습니다. 자세한 정보는 JSONStore 다중 사용자 지원을 참조하십시오.

## 지원 레벨
{: #support-level-jsonstore }
* JSONStore는 네이티브 iOS 및 Android 애플리케이션에서 지원됩니다(네이티브 Windows(Universal 및 UWP)의 경우 지원되지 않음).
* JSONStore는 Cordova iOS, Android 및 Windows(Universal 및 UWP) 애플리케이션에서 지원됩니다.


## 일반 JSONStore 용어
{: #general-jsonstore-terminology }
### 문서
{: #document }
문서는 JSONStore의 기본 구성 요소입니다.

JSONStore 문서는 자동으로 생성된 ID(`_id`) 및 JSON 데이터가 있는 JSON 오브젝트입니다. 이는 데이터베이스 용어의 레코드 또는 행과 유사합니다. `_id`의 값은 항상 특정 콜렉션 내부의 고유 정수입니다. `JSONStoreInstance` 클래스의 `add`, `replace` 및 `remove`와 같은 일부 함수는 문서 또는 오브젝트의 배열을 사용합니다. 이러한 메소드는 한 번에 여러 문서 또는 오브젝트에 대한 오퍼레이션을 수행하는데 유용합니다.

**단일 문서**  

```javascript
var doc = { _id: 1, json: {name: 'carlos', age: 99} };
```
{: codeblock}

**문서 배열**

```javascript
var docs = [
  { _id: 1, json: {name: 'carlos', age: 99} },
  { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### 콜렉션
{: #collection }
JSONStore 콜렉션은 데이터베이스 용어의 테이블과 유사합니다.  
다음 코드 예제는 문서가 디스크에 저장되는 방법은 아니지만 상위 레벨에서 콜렉션이 표시되는 방식을 시각화하는 데 좋은 방법입니다.

```javascript
[
    { _id: 1, json: {name: 'carlos', age: 99} },
    { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### 저장소
{: #store }
저장소는 하나 이상의 콜렉션으로 구성된 지속적 JSONStore 파일입니다.  
저장소는 데이터베이스 용어의 관계형 데이터베이스와 유사합니다. 저장소를 JSONStore라고도 합니다.

### 검색 필드
{: #search-fields }
검색 필드는 키-값 쌍입니다.  
검색 필드는 빠른 검색을 위해 색인화된 키이며, 데이터베이스 용어의 열 필드 또는 속성과 유사합니다.

추가 검색 필드는 색인화되지만 저장되는 JSON 데이터의 일부가 아닌 키입니다. 이러한 필드는 JSON 콜렉션에서 값의 색인이 작성되어 보다 빠르게 검색하는 데 사용될 수 있는 키를 정의합니다.

올바른 데이터 유형은 문자열, 부울, 숫자 및 정수입니다. 이러한 유형은 유형 힌트일 뿐이며 유형 유효성 검증이 없습니다. 또한 이러한 유형은 색인 작성 가능 필드가 저장되는 방법을 판별합니다. 예를 들어, `{age: 'number'}`는 1을 1.0으로 색인화하고 `{age: 'integer'}`는 1을 1로 색인화합니다.

**검색 필드 및 추가 검색 필드**

```javascript
var searchField = {name: 'string', age: 'integer'};
var additionalSearchField = {key: 'string'};
```
{: codeblock}

오브젝트 자체가 아니라 오브젝트 내부의 키만 색인화할 수 있습니다. 배열은 패스 스루 방식으로 처리됩니다. 즉, 배열 또는 배열의 특정 색인(arr[n])을 색인화할 수 없지만 배열 내부의 오브젝트를 색인화할 수는 있습니다.

**배열 내부의 값 색인화**

```javascript

var searchFields = {
    'people.name' : 'string', // matches carlos and tim on myObject
    'people.age' : 'integer' // matches 99 and 100 on myObject
};

var myObject = {
    people : [
        {name: 'carlos', age: 99},
        {name: 'tim', age: 100}
    ]
};
```
{: codeblock}

### 조회
{: #queries }
조회는 검색 필드 또는 추가 검색 필드를 사용하여 문서를 찾는 오브젝트입니다.  
다음 예제에서는 이름 검색 필드가 문자열 유형이고 연령 검색 필드가 정수 유형이라고 가정합니다.

**`name`이 `carlos`와 일치하는 문서 찾기**

```javascript
var query1 = {name: 'carlos'};
```
{: codeblock}

**`name`이 `carlos`와 일치하고 `age`가 `99`인 문서 찾기**

```javascript
var query2 = {name: 'carlos', age: 99};
```
{: codeblock}

### 조회 파트
{: #query-parts }
조회 파트는 추가 고급 검색을 빌드하는 데 사용됩니다. 일부 JSONStore 오퍼레이션(예: `find` 또는 `count`의 일부 버전)에서 조회 파트가 사용됩니다. 조회 파트 내의 모든 항목은 `AND` 문으로 결합되지만 조회 파트 자체는 `OR` 문으로 결합됩니다. 조회 파트 내의 모든 항목이 **true**인 경우에만 검색 기준에서 일치사항을 리턴합니다. 둘 이상의 조회 파트를 사용하여 하나 이상의 조회 파트를 충족하는 일치사항을 검색할 수 있습니다.

최상위 레벨 검색 필드에서만 작동하는 조회 파트로 찾으십시오. 예를 들어, `name.first`가 아니라 `name`으로 찾으십시오. 모든 검색 필드가 최상위 레벨인 여러 콜렉션을 사용하여 이 동작을 처리하십시오. 최상위 레벨이 아닌 검색 필드에서 작업하는 조회 파트 오퍼레이션은 `equal`, `notEqual`, `like`, `notLike`, `rightLike`, `notRightLike`, `leftLike` 및 `notLeftLike`입니다. 최상위 레벨이 아닌 검색 필드를 사용하는 경우 동작이 판별되지 않습니다.

## 기능 테이블
{: #features-table }
JSONStore 기능을 다른 데이터 스토리지 기술 및 형식의 해당 기능과 비교하십시오.

JSONStore는 MobileFirst 플러그인을 사용하는 Cordova 애플리케이션 내부에 데이터를 저장하는 데 사용되는 JavaScript API, 네이티브 iOS 애플리케이션용 Objective-C API 및 네이티브 Android 애플리케이션용 Java API입니다. 여기에 참조용으로 제공되는 여러 JavaScript 스토리지 기술 간 비교를 통해 JSONStore와 해당 기술이 어떻게 다른지 확인하십시오.

JSONStore는 LocalStorage, 색인화된 DB, Cordova 스토리지 API 및 Cordova 파일 API 등의 기술과 유사합니다. 다음 테이블은 JSONStore에서 제공하는 일부 기능이 다른 기술과 비교하여 어떻게 다른지 보여줍니다. JSONStore 기능은 iOS 및 Android 디바이스와 시뮬레이터에서만 사용 가능합니다.

|기능                                            |JSONStore      |LocalStorage |IndexedDB |Cordova 스토리지 API |Cordova 파일 API |
|----------------------------------------------------|----------------|--------------|-----------|---------------------|------------------|
|Android 지원(Cordova 및 네이티브 애플리케이션)|	     ✔ 	      |    ✔	    |        ✔	     |         ✔	           |         ✔	      |
|iOS 지원(Cordova 및 네이티브 애플리케이션)	     |	     ✔ 	      |    ✔	    |        ✔	     |         ✔	           |         ✔	      |
|Windows 8.1 Universal 및 Windows 10 UWP(Cordova 애플리케이션)          |	     ✔ 	      |     ✔	    |     ✔	     |        -	           |         ✔	      |
|데이터 암호화	                                 |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
|최대 스토리지	                                 |사용 가능한 공간 |    ~5MB     |   ~5MB 	 |사용 가능한 공간	   |사용 가능한 공간  |
|신뢰할 수 있는 스토리지(참고 참조)	                     |	     ✔ 	      |      -	    |     -	     |         ✔	           |         ✔	      |
|로컬 변경사항 추적	                     |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
|다중 사용자 지원                                 |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
|색인 작성	                                         |	     ✔ 	      |      -	    |        ✔	     |        ✔	           |         -	      |
|스토리지 유형	                                 |JSON 문서 |키-값 쌍 |JSON 문서 |관계형(SQL) |문자열     |

신뢰할 수 있는 스토리지는 다음 이벤트 중 하나가 발생하는 경우를 제외하고 데이터가 삭제되지 않음을 의미합니다.
* 애플리케이션이 디바이스에서 제거됩니다.
* 데이터를 제거하는 메소드 중 하나가 호출됩니다.
{: note}

## 다중 사용자 지원
{: #multiple-user-support }
JSONStore를 사용하여 단일 MobileFirst 애플리케이션에 여러 콜렉션으로 구성된 다중 저장소를 작성할 수 있습니다.

init(JavaScript) 또는 open(네이티브 iOS 및 네이티브 Android) API는 사용자 이름으로 옵션 오브젝트를 선택할 수 있습니다. 서로 다른 저장소는 파일 시스템에서 별도의 파일입니다. 사용자 이름은 저장소의 파일 이름으로 사용됩니다. 보안 및 개인정보 보호를 이유로 이러한 별도의 저장소를 서로 다른 비밀번호로 암호화할 수 있습니다. closeAll API를 사용하면 모든 콜렉션에 대한 액세스가 제거됩니다. changePassword API를 호출하여 암호화된 저장소의 비밀번호를 변경할 수도 있습니다.

예제 유스 케이스는 여러 직원이 실제 디바이스(예: iPad 또는 Android 태블릿) 및 MobileFirst 애플리케이션을 공유하는 경우입니다. 다중 사용자 지원은 직원이 서로 다른 교대조에서 근무하고 MobileFirst 애플리케이션을 사용하는 동안 여러 고객의 개인용 데이터를 처리하는 경우에 유용합니다.

## 보안
{: #security-jsonstore }
암호화를 통해 저장소에 있는 모든 콜렉션을 보호할 수 있습니다.

저장소의 모든 콜렉션을 암호화하려면 `init`(JavaScript) 또는 `open`(네이티브 iOS 및 네이티브 Android) API에 비밀번호를 전달하십시오. 비밀번호가 전달되지 않으면 저장소 콜렉션의 문서가 암호화되지 않습니다.

일부 보안 아티팩트(예: salt)는 키 체인(iOS), 공유 환경 설정(Android) 및 인증 정보 보관(Windows Universal 8.1 및 Windows 10 UWP)에 저장됩니다. 저장소는 256비트 고급 암호화 표준(AES) 키로 암호화됩니다. 모든 키는 PBKDF2(Password-Based Key Derivation Function 2)로 강화됩니다. 애플리케이션에 대한 데이터 콜렉션을 암호화하도록 선택할 수 있지만 암호화된 형식 및 일반 텍스트 형식 간에 전환하거나 저장소 내에서 형식을 혼합할 수는 없습니다.

저장소에서 데이터를 보호하는 키는 사용자가 제공하는 사용자 비밀번호를 기반으로 합니다. 키가 만료되지 않지만 changePassword API를 호출하여 변경할 수 있습니다.

데이터 보호 키(DPK)는 저장소의 컨텐츠를 복호화하는 데 사용되는 키입니다. 애플리케이션이 설치 제거되는 경우에도 DPK는 iOS 키 체인에 보관됩니다. 키 체인의 키와 JSONStore가 애플리케이션에 배치한 다른 모든 항목을 제거하려면 destroy API를 사용하십시오. 이 프로세스는 암호화된 DPK가 공유 환경 설정에 저장되고 애플리케이션 설치 제거 시 삭제되므로 Android에 적용될 수 없습니다.

처음으로 JSONStore에서 비밀번호를 사용하여 콜렉션을 열 때(즉, 개발자가 저장소 내부의 데이터를 암호화하려는 경우) JSONStore에 랜덤 토큰이 필요합니다. 해당 랜덤 토큰은 클라이언트 또는 서버에서 가져올 수 있습니다.

localKeyGen 키가 JSONStore API의 JavaScript 구현에 존재하고 값이 true인 경우 암호로 보안된 토큰이 로컬로 생성됩니다. 그렇지 않으면 토큰이 서버에 접속하여 생성되므로 MobileFirst Server에 대한 연결이 필요합니다. 이 토큰은 처음으로 비밀번호를 사용하여 저장소를 열 때만 필요합니다. 기본적으로 네이티브 구현(Objective-C 및 Java)에서는 암호로 보안된 토큰을 로컬로 생성합니다. 또는 secureRandom 옵션을 통해 전달할 수도 있습니다.

다음 사이에 절충됩니다.
* 오프라인에서 저장소를 열고 클라이언트를 신뢰하여 랜덤 토큰을 생성하도록 함(덜 안전함) 또는 
* 연결이 필요한 MobileFirst Server에 대한 액세스 권한을 사용하여 저장소를 열고 서버를 신뢰함(더 안전함)

### 보안 유틸리티
{: #security-utilities }
MobileFirst 클라이언트 측 API는 사용자의 데이터를 보호하는 데 도움이 되는 일부 보안 유틸리티를 제공합니다. JSON 오브젝트를 보호하려는 경우 JSONStore와 같은 기능이 매우 유용합니다. 그러나 JSONStore 콜렉션에 2진 BLOB을 저장하는 것은 권장되지 않습니다.

대신 파일 시스템에 2진 데이터를 저장하고 파일 경로 및 기타 메타데이터를 JSONStore 콜렉션 내부에 저장하십시오. 이미지와 같은 파일을 보호하려면 base64 문자열로 인코딩하고 암호화한 후 디스크에 출력을 쓸 수 있습니다. 

데이터를 복호화하려면 JSONStore 콜렉션에서 메타데이터를 찾고 디스크에서 암호화된 데이터를 읽은 후 저장된 메타데이터를 사용하여 이를 복호화할 수 있습니다. 

이 메타데이터에는 키, salt, IV(Initialization Vector), 파일 유형, 파일 경로 등이 포함될 수 있습니다.

[JSONStore 보안 유틸리티](/docs/services/mobilefoundation?topic=mobilefoundation-security_utilities#security_utilities)에 대해 자세히 알아보십시오.
{: tip}

### Windows 8.1 Universal 및 Windows 10 UWP 암호화
{: #windows-81-universal-and-windows-10-uwp-encryption }
암호화를 통해 저장소에 있는 모든 콜렉션을 보호할 수 있습니다.

JSONStore는 [SQLCipher ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://sqlcipher.net/){: new_window}를 기본 데이터베이스 기술로 사용합니다. SQLCipher는 Zetetic에서 생성되는 SQLite의 빌드이며, LLC는 데이터베이스에 암호화 계층을 추가합니다.

JSONStore는 모든 플랫폼에서 SQLCipher를 사용합니다. Android 및 iOS에서 Community Edition으로 알려진 오픈 소스 버전의 무료 SQLCipher를 사용할 수 있으며, 이는 Mobile Foundation에 포함된 JSONStore 버전에 통합됩니다. Windows 버전의 SQLCipher는 상업용 라이센스에서만 사용할 수 있고 Mobile Foundation에서 직접 재배포할 수 없습니다.

대신 Windows 8 Universal용 JSONStore에는 SQLite가 기본 데이터베이스로 포함되어 있습니다. 이러한 플랫폼에 대해 데이터를 암호화해야 하는 경우 고유 SQLCipher 버전을 확보하고 Mobile Foundation에 포함된 SQLite 버전을 스왑 아웃해야 합니다.

암호화가 필요하지 않은 경우 JSONStore가 Mobile Foundation의 SQLite 버전을 사용하여 완전히 작동됩니다(암호화 제외).

#### SQLite를 Windows Universal 및 Windows UWP용 SQLCipher로 대체
{: #replacing-sqlite-with-sqlcipher-for-windows-universal-and-windows-uwp }
1. Windows Runtime Commercial Edition용 SQLCipher과 함께 제공되는 Windows Runtime 8.1/10용 SQLCipher 확장기능을 실행하십시오.
2. 확장기능 설치가 완료되면 방금 작성된 **sqlite3.dll** 파일의 SQLCipher 버전을 찾으십시오. x86, x64, ARM에 대한 버전이 하나씩 존재합니다.

   ```bash
   C:\Program Files (x86)\Microsoft SDKs\Windows\v8.1\ExtensionSDKs\SQLCipher.WinRT81\3.0.1\Redist\Retail\<platform>
   ```
   {: codeblock}

3. MobileFirst 애플리케이션에 이 파일을 복사하고 대체하십시오.

   ```bash
   <Worklight project name>\apps\<application name>\windows8\native\buildtarget\<platform>
   ```
   {: codeblock}

## 성능
{: #performance-jsonstore }
다음은 JSONStore 성능에 영향을 줄 수 있는 요인입니다.

### 네트워크
{: #network-jsonstore }
* 모든 더티 문서를 어댑터에 전송하는 것과 같은 오퍼레이션을 수행하기 전에 네트워크 연결을 확인하십시오.
* 네트워크를 통해 클라이언트에 전송되는 데이터의 양은 성능에 중대한 영향을 미칩니다. 백엔드 데이터베이스 내부의 모든 항목을 복사하는 대신 애플리케이션에 필요한 데이터만 전송하십시오.
* 어댑터를 사용 중인 경우 compressResponse 플래그를 true로 설정하는 것을 고려하십시오. 이렇게 하면 응답이 압축되어 일반적으로 더 적은 대역폭을 사용하고 압축을 사용하지 않을 때보다 전송 시간이 단축됩니다.

### 메모리
{: #memory-jsonstore }
* JavaScript API를 사용하면 JSONStore 문서가 네이티브(Objective-C, Java 또는 C#) 계층과 JavaScript 계층 간에 문자열로 직렬화 및 역직렬화됩니다. 가능한 메모리 문제를 완화하는 한 가지 방법은 find API를 사용할 때 한계와 오프셋을 사용하는 것입니다. 이렇게 하면 결과에 대해 할당되는 메모리의 양을 제한하고 페이지 번호 매기기(페이지당 X개의 결과 표시)와 같은 사항을 구현할 수 있습니다.
* 궁극적으로 문자열로 직렬화 및 역직렬화되는 긴 키 이름을 사용하는 대신 해당 긴 키 이름을 짧은 키 이름으로 맵핑할 것을 고려하십시오(예: `myVeryVeryVerLongKeyName`을 `k` 또는 `key`에 맵핑). 어댑터에서 클라이언트로 전송할 때 짧은 키 이름으로 맵핑하고 데이터를 백엔드로 다시 전송할 때 원래의 긴 키 이름으로 맵핑하는 것이 가장 좋습니다.
* 저장소 내부의 데이터를 다양한 콜렉션으로 분할하는 것을 고려하십시오. 모놀리식 문서를 단일 콜렉션에 포함시키는 대신 작은 문서를 여러 콜렉션에 포함시키십시오. 이 고려사항은 데이터가 관련된 정도 및 해당 데이터의 유스 케이스에 따라 다릅니다.
* 오브젝트 배열에 add API를 사용하면 메모리 문제가 발생할 수 있습니다. 이 문제를 완화하려면 한 번에 사용되는 JSON 오브젝트 수를 줄이고 이러한 메소드를 호출하십시오.
* JavaScript 및 Java™에는 가비지 콜렉터가 있는 반면 Objective-C에는 ARC(Automatic Reference Counting)가 있습니다. ARC가 작동하도록 허용하지만 완전히 의존하지는 마십시오. 더 이상 사용되지 않는 참조를 무효화하고 프로파일링 도구를 사용하여 예상 시점에 메모리 사용량이 감소하는지 확인하십시오.

### CPU
{: #cpu-jsonstore }
* 색인 작성을 수행하는 add 메소드 호출 시 사용되는 검색 필드 및 추가 검색 필드의 양은 성능에 영향을 줍니다. find 메소드에 대한 조회에 사용되는 값의 색인만 작성하십시오.
* 기본적으로 JSONStore는 해당 문서에 대한 로컬 변경사항을 추적합니다. add, remove 및 replace API를 사용할 때 `markDirty` 플래그를 **false**로 설정하여 이 동작을 사용 안함으로 설정함으로써 주기 수를 줄일 수 있습니다.
* 보안을 사용으로 설정하면 콜렉션 내부의 문서에 대해 작업하는 `init` 또는 `open` API 및 기타 오퍼레이션에서 약간의 오버헤드가 추가됩니다. 보안이 실제로 필요한지 여부를 고려하십시오. 예를 들어, open API는 암호화 및 복호화에 사용되는 암호화 키를 생성해야 하므로 암호화 시 속도가 훨씬 느립니다.
* `replace` 및 `remove` API는 모든 발생을 대체하거나 제거하기 위해 전체 콜렉션을 검토해야 하므로 콜렉션 크기에 따라 달라집니다. 각 레코드를 검토해야 하기 때문에 암호화가 사용되는 경우 모든 레코드를 복호화해야 하므로 속도가 훨씬 느려집니다. 콜렉션 규모가 큰 경우 이러한 성능 저하가 보다 뚜렷하게 나타납니다.
* `count` API는 비교적 부담이 큽니다. 그러나 해당 콜렉션에 대한 개수를 유지하는 변수를 유지할 수 있습니다. 콜렉션에서 저장하거나 제거할 때마다 해당 API를 업데이트하십시오.
* `find` API(`find`, `findAll` 및 `findById`)는 일치 여부를 확인하기 위해 모든 문서를 복호화해야 하므로 암호화에 영향을 받습니다. 조회로 찾는 경우 한계가 초과되면 결과 수 한계에 도달할 때 중지되므로 잠재적으로 더 빨라집니다. 다른 검색 결과가 남아 있는지 알아보기 위해 JSONStore가 나머지 문서를 복호화할 필요가 없습니다.

## 동시성
{: #concurrency-jsonstore }
### JavaScript의 동시성
{: #javascript-jsonstore }
콜렉션에 대해 수행할 수 있는 대부분의 오퍼레이션(예: add 및 find)은 비동기적입니다. 이러한 오퍼레이션은 오퍼레이션이 올바르게 완료되면 해결되고 실패가 발생하면 거부되는 jQuery 프라미스를 리턴합니다. 이러한 프라미스는 성공 및 실패 콜백과 유사합니다.

jQuery Deferred는 해결되거나 거부될 수 있는 프라미스입니다. 다음 예제는 JSONStore에 특정하지 않지만 일반적인 사용법을 이해하는 데 도움이 됩니다.

프라미스 및 콜백 대신 JSONStore `success` 및 `failure` 이벤트를 청취할 수도 있습니다. 이벤트 리스너에 전달되는 인수에 따라 조치를 수행하십시오.

**프라미스 정의 예제**

```javascript
var asyncOperation = function () {
  // Assumes that you have jQuery defined via $ in the environment
  var deferred = $.Deferred();

  setTimeout(function() {
    deferred.resolve('Hello');
  }, 1000);

  return deferred.promise();
};
```

**프라미스 사용법 예제**

```javascript
// The function that is passed to .then is executed after 1000 ms.
asyncOperation.then(function (response) {
  // response = 'Hello'
});
```

**콜백 정의 예제**

```javascript
var asyncOperation = function (callback) {
  setTimeout(function() {
    callback('Hello');
  }, 1000);
};
```

**콜백 사용법 예제**

```javascript
// The function that is passed to asyncOperation is executed after 1000 ms.
asyncOperation(function (response) {
  // response = 'Hello'
});
```

**이벤트 예제**

```javascript
$(document.body).on('WL/JSONSTORE/SUCCESS', function (evt, data, src, collectionName) {

  // evt - Contains information about the event
  // data - Data that is sent ater the operation (add, find, etc.) finished
  // src - Name of the operation (add, find, push, etc.)
  // collectionName - Name of the collection
});
```

### Objective-C의 동시성
{: #objective-c-jsonstore }
JSONStore에 네이티브 iOS API를 사용하는 경우 모든 오퍼레이션이 동기 디스패치 큐에 추가됩니다. 이 동작은 저장소에 접촉하는 오퍼레이션이 기본 스레드가 아닌 스레드에서 적절하게 실행되도록 합니다. 자세한 정보는 [GCD(Grand Central Dispatch) ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.apple.com/library/ios/documentation/Performance/Reference/GCD_libdispatch_Ref/Reference/reference.html#//apple_ref/c/func/dispatch_sync){: new_window}의 Apple 문서를 참조하십시오.

### Java의 동시성 
{: #java-jsonstore }
JSONStore에 네이티브 Android API를 사용하는 경우 모든 오퍼레이션이 기본 스레드에서 실행됩니다. 비동기 동작을 수행하려면 스레드를 작성하거나 스레드 풀을 사용해야 합니다. 모든 저장소 오퍼레이션은 스레드로부터 안전합니다.

## 분석
{: #analytics-jsonstore }
JSONStore와 관련된 분석 정보의 주요 부분을 수집할 수 있습니다.

### 파일 정보
{: #file-information }
분석 플래그가 **true**로 설정된 JSONStore API가 호출되면 파일 정보가 애플리케이션 세션당 한 번 수집됩니다. 애플리케이션을 메모리로 로드하거나 메모리에서 제거할 때 애플리케이션 세션이 정의됩니다. 이 정보를 사용하여 애플리케이션에서 JSONStore 컨텐츠가 사용하는 공간의 양을 판별할 수 있습니다.

### 성능 메트릭
{: #performance-metrics }
JSONStore API가 호출될 때마다 오퍼레이션 시작 및 종료 시간에 대한 정보가 포함된 성능 메트릭이 수집됩니다. 이 정보를 사용하여 다양한 오퍼레이션에 소요되는 시간(밀리초)을 판별할 수 있습니다.

### iOS의 JSONStore 예
{: #ios-example}
```objc
JSONStoreOpenOptions* options = [JSONStoreOpenOptions new];
[options setAnalytics:YES];

[[JSONStore sharedInstance] openCollections:@[...] withOptions:options error:nil];
```

### Android의 JSONStore 예
{: #android-example }
```java
JSONStoreInitOptions initOptions = new JSONStoreInitOptions();
initOptions.setAnalytics(true);

WLJSONStore.getInstance(...).openCollections(..., initOptions);
```

### JavaScript의 JSONStore 예
{: #java-script-example }
```javascript
var options = {
  analytics : true
};

WL.JSONStore.init(..., options);
```

## 외부 데이터에 대한 작업
{: #working-with-external-data }
여러 가지 개념으로 외부 데이터에 대해 작업할 수 있습니다(**가져오기** 및 **푸시**).

### 가져오기
{: #pull }
많은 시스템에서 가져오기라는 용어를 사용하여 외부 소스에서 데이터를 가져오는 것을 나타냅니다.  
세 가지 중요한 부분이 있습니다.

#### 외부 데이터 소스에서 가져오기
{: #external-data-source-pull }
이 소스는 데이터베이스, REST 또는 SOAP API 등일 수 있습니다. 유일한 요구사항은 MobileFirst Server에서 액세스하거나 클라이언트 애플리케이션에서 직접 액세스할 수 있어야 한다는 점입니다. 이 소스가 JSON 형식으로 데이터를 리턴하도록 하는 것이 가장 좋습니다.

#### 가져오기용 전송 계층
{: #transport-layer-pull }
이 소스는 외부 소소의 데이터를 내부 소스(저장소 내부의 JSONStore 콜렉션)로 가져오는 방법입니다. 한 가지 대안은 어댑터입니다.

#### 가져오기용 내부 데이터 소스 API
{: #internal-data-source-api-pull }
이 소스는 JSON 데이터를 콜렉션에 추가하는 데 사용할 수 있는 JSONStore API입니다.

파일, 입력 필드 또는 변수의 하드코드로 된 데이터에서 읽은 데이터로 내부 저장소를 채울 수 있습니다. 네트워크 통신이 필요한 외부 소스에서 독점적으로 가져올 필요는 없습니다.
{: note}

다음 코드 예제는 모두 JavaScript와 유사한 의사 코드로 작성됩니다.

전송 계층에 어댑터를 사용하십시오. 어댑터를 사용하는 경우의 일부 장점에는 XML에서 JSON으로 변환, 보안, 필터링 및 서버 측 코드와 클라이언트 측 코드의 분리가 포함됩니다.
{: note}
**외부 데이터 소스: 백엔드 REST 엔드포인트**  
데이터베이스에서 데이터를 읽고 JSON 오브젝트 배열로 리턴하는 REST 엔드포인트가 있다고 가정하십시오.

```javascript
app.get('/people', function (req, res) {

  var people = database.getAll('people');

  res.json(people);
});
```
{: codeblock}

리턴되는 데이터는 다음 예제와 유사할 수 있습니다.

```xml
[{id: 0, name: 'carlos', ssn: '111-22-3333'},
 {id: 1, name: 'mike', ssn: '111-44-3333'},
 {id: 2, name: 'dgonz' ssn: '111-55-3333')]
```
{: codeblock}

**전송 계층: 어댑터**  
people이라는 어댑터를 작성했으며 getPeople이라는 프로시저를 정의했다고 가정하십시오. 이 프로시저는 REST 엔드포인트를 호출하고 JSON 오브젝트 배열을 클라이언트에 리턴합니다. 여기서 추가 작업(예: 데이터 서브세트만 클라이언트에 리턴)을 수행할 수 있습니다.

```javascript
function getPeople () {

  var input = {
    method : 'get',
    path : '/people'
  };

  return MFP.Server.invokeHttp(input);
}
```
{: codeblock}

클라이언트에서 WLResourceRequest API를 사용하여 데이터를 가져올 수 있습니다. 또한 클라이언트에서 어댑터로 일부 매개변수를 전달할 수도 있습니다. 한 가지 예제는 클라이언트가 마지막으로 어댑터를 통해 외부 소스에서 새 데이터를 가져온 시간이 포함된 날짜입니다.

```javascript
var adapter = 'people';
var procedure = 'getPeople';

var resource = new WLResourceRequest('/adapters' + '/' + adapter + '/' + procedure, WLResourceRequest.GET);
resource.send()
.then(function (responseFromAdapter) {
  // ...
});
```
{: codeblock}

`WLResourceRequest` API에 전달할 수 있는 `compressResponse`, `timeout` 및 기타 매개변수를 사용할 수 있습니다.  
{: note}

선택적으로 어댑터를 건너뛰고 jQuery.ajax 등을 사용하여 저장하려는 데이터가 포함된 REST 엔드포인트에 직접 접속할 수 있습니다.

```javascript
$.ajax({
  type: 'GET',
  url: 'http://example.org/people',
})
.then(function (responseFromEndpoint) {
  // ...
});
```
{: codeblock}

**내부 데이터 소스 API: JSONStore**
백엔드에서 응답을 얻은 후에 JSONStore를 사용하여 해당 데이터에 대해 작업할 수 있습니다.
JSONStore는 로컬 변경사항을 추적하는 방법을 제공합니다. 이를 통해 일부 API가 문서를 더티 상태로 표시할 수 있습니다. API는 문서에 대해 수행된 마지막 오퍼레이션 및 문서가 더티로 표시된 시간을 기록합니다. 그런 다음 이 정보를 사용하여 데이터 동기화와 같은 기능을 구현할 수 있습니다.

change API는 데이터 및 일부 옵션을 사용합니다.

**replaceCriteria**  
이러한 검색 필드는 입력 데이터의 일부입니다. 이는 이미 콜렉션 내부에 있는 문서를 찾는 데 사용됩니다. 예를 들어, 다음과 같습니다.

```javascript
['id', 'ssn']
```
{: codeblock}

위를 대체 기준으로 선택하는 경우 다음 배열을 입력 데이터로 전달하십시오.

```javascript
[{id: 1, ssn: '111-22-3333', name: 'Carlos'}]
```
{: codeblock}

또한 `people` 콜렉션에 다음 문서가 이미 포함되어 있습니다.

```javascript
{_id: 1,json: {id: 1, ssn: '111-22-3333', name: 'Carlitos'}}
```
{: codeblock}

`change` 오퍼레이션은 다음 조회와 정확히 일치하는 문서를 찾습니다.

```javascript
{id: 1, ssn: '111-22-3333'}
```
{: codeblock}

그런 다음 `change` 오퍼레이션이 입력 데이터로 대체를 수행하면 콜렉션에 다음이 포함됩니다.

```javascript
{_id: 1, json: {id:1, ssn: '111-22-3333', name: 'Carlos'}}
```
{: codeblock}

이름이 `Carlitos`에서 `Carlos`로 변경되었습니다. 둘 이상의 문서가 대체 기준과 일치하면 일치하는 모든 문서가 각 입력 데이터로 대체됩니다.

**addNew**  
대체 기준과 일치하는 문서가 없는 경우 change API에서 이 플래그의 값을 확인합니다. 이 플래그가 **true**로 설정된 경우 change API가 새 문서를 작성하여 저장소에 추가합니다. 그렇지 않으면 추가 조치가 수행되지 않습니다.

**markDirty**  
change API에서 대체되거나 추가된 문서를 더티로 표시할지 여부를 판별합니다.

어댑터에서 데이터 배열이 리턴됩니다.

```javascript
.then(function (responseFromAdapter) {

  var accessor = WL.JSONStore.get('people');

  var data = responseFromAdapter.responseJSON;

  var changeOptions = {
    replaceCriteria : ['id', 'ssn'],
    addNew : true,
    markDirty : false
  };

  return accessor.change(data, changeOptions);
})

.then(function() {
  // ...
})
```
{: codeblock}

다른 API를 사용하여 저장된 로컬 문서에 대한 변경사항을 추적할 수 있습니다. 항상 오퍼레이션을 수행하는 콜렉션에 대한 액세서를 가져오십시오.

```javascript
var accessor = WL.JSONStore.get('people')
```
{: codeblock}

그런 다음 데이터(JSON 오브젝트의 배열)을 추가하고 이를 더티로 표시할지 여부를 결정할 수 있습니다. 일반적으로 외부 소스에서 변경사항을 가져올 때 markDirty 플래그를 false로 설정할 수 있습니다. 데이터를 로컬로 추가하는 경우 이 플래그를 true로 설정하십시오.

```javascript
accessor.add(data, {markDirty: true})
```
{: codeblock}

또한 문서를 대체하고 대체한 문서를 더티로 표시할지 여부를 선택할 수 있습니다.

```javascript
accessor.replace(doc, {markDirty: true})
```
{: codeblock}

마찬가지로 문서를 제거하고 제거를 더티로 표시할지 여부를 선택할 수 있습니다. 제거되고 더티로 표시된 문서는 find API를 사용할 때 표시되지 않습니다. 그러나 실제로 콜렉션에서 문서를 제거하는 `markClean` API를 사용할 때까지 해당 문서가 콜렉션 내부에 계속 남아 있습니다. 문서가 더티로 표시되지 않으면 해당 문서가 실제로 콜렉션에서 제거됩니다.

```javascript
accessor.remove(doc, {markDirty: true})
```
{: codeblock}

### 푸시
{: #push }
많은 시스템에서 푸시라는 용어를 사용하여 외부 소스에 데이터를 전송하는 것을 나타냅니다.

세 가지 중요한 부분이 있습니다.

#### 푸시용 내부 데이터 소스 API
{: #internal-data-source-api-push }
이 소스는 로컬 전용 변경사항(더티)을 포함한 문서를 리턴하는 JSONStore API입니다.

#### 푸시용 전송 계층
{: #transport-layer-push }
이 소스는 변경사항을 전송하기 위해 외부 데이터 소스에 접속하는 방법입니다.

#### 외부 데이터 소스로 푸시
{: #external-data-source-push }
이 소스는 일반적으로 클라이언트가 데이터에 대해 작성한 업데이트를 수신하는 데이터베이스, 그 중에서도 REST 또는 SOAP 엔드포인트입니다.

다음 코드 예제는 모두 JavaScript와 유사한 의사 코드로 작성됩니다.

전송 계층에 어댑터를 사용하십시오. 어댑터를 사용하는 경우의 일부 장점에는 XML에서 JSON으로 변환, 보안, 필터링 및 서버 측 코드와 클라이언트 측 코드의 분리가 포함됩니다.
{: note}

**내부 데이터 소스 API: JSONStore**  
콜렉션에 대한 액세서를 가져온 후 `getAllDirty` API를 호출하여 더티로 표시된 모든 문서를 가져오십시오. 이러한 문서에는 전송 계층을 통해 외부 데이터 소스에 전송하려는 로컬 전용 변경사항이 포함되어 있습니다.

```javascript
var accessor = WL.JSONStore.get('people');

accessor.getAllDirty()

.then(function (dirtyDocs) {
  // ...
});
```
{: codeblock}

`dirtyDocs` 인수는 다음 예제와 유사합니다.

```javascript
[{_id: 1,
  json: {id: 1, ssn: '111-22-3333', name: 'Carlos'},
  _operation: 'add',
  _dirty: '1395774961,12902'}]
```
{: codeblock}

필드는 다음과 같습니다.
* `_id`: JSONStore에서 사용하는 내부 필드. 모든 문서에 고유 항목이 지정됩니다.
* `json`: 저장된 데이터
* `_operation`: 문서에 대해 마지막으로 수행된 오퍼레이션. 가능한 값은 add, store, replace 및 remove입니다.
* `_dirty`: 문서가 더티로 표시된 시간을 나타내기 위해 숫자로 저장된 시간소인

**전송 계층: MobileFirst 어댑터**  
더티 문서를 어댑터에 전송하도록 선택할 수 있습니다. `updatePeople` 프로시저로 정의되는 `people` 어댑터가 있다고 가정하십시오.

```javascript
.then(function (dirtyDocs) {
  var adapter = 'people',
  procedure = 'updatePeople';

  var resource = new WLResourceRequest('/adapters/' + adapter + '/' + procedure, WLResourceRequest.GET)
  resource.setQueryParameter('params', [dirtyDocs]);
  return resource.send();
})

.then(function (responseFromAdapter) {
  // ...
})
```
{: codeblock}

`WLResourceRequest` API에 전달할 수 있는 `compressResponse`, `timeout` 및 기타 매개변수를 사용할 수 있습니다.
{: note}

MobileFirst Server에서 어댑터에는 다음 예제와 유사한 `updatePeople` 프로시저가 있습니다.

```javascript
function updatePeople (dirtyDocs) {

  var input = {
    method : 'post',
    path : '/people',
    body: {
      contentType : 'application/json',
      content : JSON.stringify(dirtyDocs)
    }
  };

  return MFP.Server.invokeHttp(input);
}
```
{: codeblock}

클라이언트에서 `getAllDirty` API의 출력을 릴레이하는 대신 백엔드에서 예상되는 형식과 일치하도록 페이로드를 업데이트해야 할 수 있습니다. 대체, 제거 및 포함을 별도의 백엔드 API 호출로 분할해야 할 수 있습니다.

선택적으로 `dirtyDocs` 배열에 대해 반복하고 `_operation` 필드를 확인할 수 있습니다. 그런 다음 대체를 한 프로시저로 전송하고 제거 및 포함을 다른 프로시저로 전송하십시오. 이전 예제에서는 모든 더티 문서를 어댑터에 대량으로 전송합니다.

```javascript
var len = dirtyDocs.length;
var arrayOfPromises = [];
var adapter = 'people';
var procedure = 'addPerson';
var resource;

while (len--) {

  var currentDirtyDoc = dirtyDocs[len];

  switch (currentDirtyDoc._operation) {

    case 'add':
    case 'store':

    resource = new WLResourceRequest('/adapters/people/addPerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);

      arrayOfPromises.push(resource.send());

    break;

    case 'replace':
    case 'refresh':

    resource = new WLResourceRequest('/adapters/people/replacePerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);


      arrayOfPromises.push(resource.send());

    break;

    case 'remove':
    case 'erase':

    resource = new WLResourceRequest('/adapters/people/removePerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);

      arrayOfPromises.push(resource.send());
  }
}

$.when.apply(this, arrayOfPromises)
.then(function () {
  var len = arguments.length;

  while (len--) {
    // Look at the responses in arguments[len]
  }
});
```
{: codeblock}

선택적으로 어댑터를 건너뛰고 REST 엔드포인트에 직접 접속할 수 있습니다.

```javascript
.then(function (dirtyDocs) {

  return $.ajax({
    type: 'POST',
    url: 'http://example.org/updatePeople',
    data: dirtyDocs
  });
})

.then(function (responseFromEndpoint) {
  // ...
});
```
{: codeblock}

**외부 데이터 소스: 백엔드 REST 엔드포인트**  
백엔드는 변경사항을 승인하거나 거부한 후 클라이언트로 응답을 다시 릴레이합니다. 클라이언트가 응답을 확인한 후에 업데이트된 문서를 markClean API로 전달할 수 있습니다.

```javascript
.then(function (responseFromAdapter) {

  if (responseFromAdapter is successful) {
    WL.JSONStore.get('people').markClean(dirtyDocs);
  }
})

.then(function () {
  // ...
})
```
{: codeblock}

문서가 클린으로 표시되면 `getAllDirty` API의 출력에 표시되지 않습니다.

## JSONStore 문제점 해결
{: #troubleshooting-jsonstore }

## 도움을 요청할 때 정보 제공
{: #provide-information-when-you-ask-for-help }
충분한 정보를 제공하지 않아서 위험이 발생하는 것보다 자세한 정보를 제공하는 것이 더 좋습니다. 다음 목록은 JSONStore 문제에 도움을 주는 데 필요한 정보에 대한 좋은 시작점입니다.

* 운영 체제 및 버전. 예: Windows XP SP3 Virtual Machine 또는 Mac OSX 10.8.3
* Eclipse 버전. 예: Eclipse Indigo 3.7 Java EE
* JDK 버전. 예: Java SE Runtime Environment(빌드 1.7)
* {{ site.data.keys.product }} 버전. 예: IBM Worklight V5.0.6 Developer Edition
* iOS 버전. 예: iOS Simulator 6.1 또는 iPhone 4S iOS 6.0(더 이상 사용되지 않음, 더 이상 사용되지 않는 기능 및 API 요소 참조)
* Android 버전. 예: Android Emulator 4.1.1 또는 Samsung Galaxy Android 4.0 API 레벨 14
* Windows 버전. 예: Windows 8, Windows 8.1 또는 Windows Phone 8.1
* adb 버전. 예: Android Debug Bridge 버전 1.0.31
* iOS의 Xcode 출력이나 Android의 logcat 출력과 같은 로그

## 문제 격리
{: #try-to-isolate-the-issue }
문제점을 더 정확하게 보고하기 위해 문제를 격리하려면 다음 단계를 따르십시오.

1. 에뮬레이터(Android) 또는 시뮬레이터(iOS)를 재설정하고 destroy API를 호출하여 클린 시스템으로 시작하십시오.
2. 지원되는 프로덕션 환경에서 실행 중인지 확인하십시오.
    * Android >= 2.3 ARM v7/ARM v8/x86 에뮬레이터 또는 디바이스
    * iOS >= 6.0 시뮬레이터 또는 디바이스(더 이상 사용되지 않음)
    * Windows 8.1/10 ARM/x86/x64 시뮬레이터 또는 디바이스
3. init 또는 open API에 비밀번호를 전달하지 않음으로써 암호화를 끄십시오.
4. JSONStore에서 생성된 SQLite 데이터베이스 파일을 살펴보십시오. 암호화가 꺼져 있어야 합니다.

   * Android 에뮬레이터:
   
   ```bash
   $ adb shell
   $ cd /data/data/com.<app-name>/databases/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

   * iOS 시뮬레이터:

   ```bash
   $ cd ~/Library/Application Support/iPhone Simulator/7.1/Applications/<id>/Documents/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```  

   * Windows 8.1 Universal/Windows 10 UWP 시뮬레이터

   ```bash
   $ cd C:\Users\<username>\AppData\Local\Packages\<id>\LocalState\wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

   * **참고:** 웹 브라우저(Firefox, Chrome, Safari, Internet Explorer)에서 실행되는 JavaScript 전용 구현은 SQLite 데이터베이스를 사용하지 않습니다. 파일은 HTML5 LocalStorage에 저장됩니다.
   * `.schema`를 포함하는 `searchFields`를 살펴보고 `SELECT * FROM <collection-name>;`. sqlite3를 종료하려면 `.exit`를 입력하십시오. init 메소드에 사용자 이름을 전달하는 경우 이 파일은 **the-username.sqlite**입니다. 사용자 이름을 전달하지 않는 경우 이 파일은 기본적으로 **jsonstore.sqlite**입니다.
5. (Android만 해당) 상세 JSONStore를 사용으로 설정하십시오.

   ```bash
   adb shell setprop log.tag.jsonstore-core VERBOSE
   adb shell getprop log.tag.jsonstore-core
   ```

6. 디버거를 사용하십시오.

## 일반적인 문제
{: #common-issues-jsonstore }
다음 JSONStore 특성을 이해하면 발생할 수 있는 일반적인 문제 중 일부를 해결하는 데 도움이 됩니다.  

* 2진 데이터를 JSONStore에 저장하기 위한 유일한 방법은 먼저 base64로 인코딩하는 것입니다. 실제 파일 대신 파일 이름이나 경로를 JSONStore에 저장하십시오.
* 네이티브 코드에서 JSONStore 데이터에 액세스하는 것은 {{ site.data.keys.v62_product_full }} V6.2.0에서만 가능합니다.
* 모바일 운영 체제에서 부과되는 한계를 넘어서 JSONStore 내부에 저장할 수 있는 데이터의 양에 대한 한계는 없습니다.
* JSONStore는 지속적인 데이터 스토리지를 제공합니다. 메모리에만 저장되는 것은 아닙니다.
* 콜렉션 이름이 숫자 또는 기호로 시작하는 경우 init API가 실패합니다. IBM Worklight V5.0.6.1 이상은 적절한 오류를 리턴합니다. `4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING`
* 검색 필드에서 숫자와 정수 사이에 차이가 있습니다. 유형이 `number`인 경우 `1` 및 `2`와 같은 숫자 값은 `1.0` 및 `2.0`으로 저장됩니다. 유형이 `integer`이면 `1` 및 `2`로 저장됩니다.
* 애플리케이션이 강제로 중지되거나 충돌하는 경우 애플리케이션이 다시 시작되고 `init` 또는 `open` API가 호출될 때 항상 오류 코드 -1로 실패합니다. 이 문제점이 발생하는 경우 먼저 `closeAll` API를 호출하십시오.
* JSONStore의 JavaScript 구현에서는 코드가 연속으로 호출될 것으로 예상합니다. 다음 오퍼레이션을 호출하기 전에 오퍼레이션이 완료되기를 기다리십시오.
* 트랜잭션은 Cordova용 Android 2.3.x 애플리케이션에서 지원되지 않습니다.
* 64비트 디바이스에서 JSONStore를 사용하는 경우 `java.lang.UnsatisfiedLinkError: dlopen failed: "..." is 32-bit instead of 64-bit` 오류가 표시될 수 있습니다.
* 이 오류는 Android 프로젝트에 64비트 네이티브 라이브러리가 있으며 이러한 라이브러리를 사용하는 경우 JSONStore가 현재 작동하지 않음을 의미합니다. 확인하려면 Android 프로젝트의 **src/main/libs** 또는 **src/main/jniLibs**로 이동하여 x86_64 또는 arm64-v8a 폴더가 있는지 확인하십시오. 있는 경우 이러한 폴더를 삭제하십시오. 그러면 JSONStore가 다시 작동할 수 있습니다.
* 일부 경우(또는 환경)에는 JSONStore 플러그인이 초기화되기 전에 플로우가 `wlCommonInit()`로 들어갑니다. 이로 인해 JSONStore 관련 API 호출이 실패합니다. `cordova-plugin-mfp` 부트스트랩은 완료 시 `wlCommonInit` 함수를 트리거하는 `WL.Client.init`를 자동으로 호출합니다. 이 초기화 프로세스는 JSONStore 플러그인의 경우 다릅니다. JSONStore 플러그인에는 `WL.Client.init` 호출을 _정지_할 방법이 없습니다. 여러 환경에서 `mfpjsonjslloaded`가 완료되기 전에 플로우가 `wlCommonInit()`로 들어가는 경우가 발생할 수 있습니다.
개발자는 `mfpjsonjsloaded` 및 `mfpjsloaded` 이벤트의 순서를 지정하기 위해 `WL.CLient.init`를 수동으로 호출할 수 있습니다. 이렇게 하면 플랫폼 특정 코드가 있을 필요가 없습니다.

  수동으로 `WL.CLient.init`의 호출을 구성하려면 아래 단계를 따르십시오.                             

  1. `config.xml`에서 `clientCustomInit` 특성을 **true**로 변경하십시오.

  + `index.js` 파일에서 다음을 수행하십시오.                                   
    * 파일의 시작 부분에 다음 행을 추가하십시오.                
      ```javascript
      document.addEventListener('mfpjsonjsloaded', initWL, false);
      ```           
    * `wlCommonInit()`에 `WL.JSONStore.init` 호출을 남겨 두십시오.                    

    * 다음 함수를 추가하십시오.  
    ```javascript                                         
function initWL(){                                                     
        var options = typeof wlInitOptions !== 'undefined' ? wlInitOptions
        : {};                                                                
        WL.Client.init(options);                                           
    } 
    ```                                                                     

이 함수는 `mfpjsonjsloaded` 이벤트(`wlCommonInit` 외부)를 기다리고 스크립트가 로드되었는지 확인한 후 `wlCommonInit`를 트리거할 `WL.Client.init`를 호출합니다. 그런 다음 `WL.JSONStore.init`를 호출합니다.

## 저장소 내부
{: #store-internals }
JSONStore 데이터가 저장되는 방법의 예를 볼 수 있습니다.

이 단순화된 예제의 주요 요소는 다음과 같습니다.

* `_id`는 고유한 ID(예: AUTO INCREMENT PRIMARY KEY)입니다.
* `json`에는 저장된 JSON 오브젝트의 정확한 표시가 포함됩니다.
* `name` 및 age는 검색 필드입니다.
* `key`는 추가 검색 필드입니다.

| _id | key | name | age | JSON |
|-----|-----|------|-----|------|
|1   | c   | carlos | 99 | {name: 'carlos', age: 99} |
|2   | t   | tim   | 100 | {name: 'tim', age: 100} |

`{_id : 1}, {name: 'carlos'}`, `{age: 99}, {key: 'c'}` 조회 중 하나 또는 조합을 사용하여 검색하는 경우 리턴되는 문서는 `{_id: 1, json: {name: 'carlos', age: 99} }`입니다.

기타 내부 JSONStore 필드는 다음과 같습니다.

* `_dirty`: 문서가 더티 상태로 표시되었는지 여부를 판별합니다. 이 필드는 문서의 변경사항을 추적하는 데 유용합니다.
* `_deleted`: 문서를 삭제되거나 삭제되지 않은 것으로 표시합니다. 이 필드는 콜렉션에서 오브젝트를 제거하고 나중에 이를 사용하여 백엔드에 대한 변경사항을 추적하며 제거 여부를 결정하는 데 유용합니다.
* `_operation`: 문서에 대해 수행될 마지막 오퍼레이션(예: replace)을 반영하는 문자열입니다.

## JSONStore 오류
{: #jsonstore-errors }
### JavaScript 오류
{: #javascript-errors }
JSONStore는 오류 오브젝트를 사용하여 실패 원인에 대한 메시지를 리턴합니다.

JSONStore 조작(예: `JSONStoreInstance` 클래스의 `add` 메소드 및 `find`) 동안 오류가 발생하는 경우 오류 오브젝트가 리턴됩니다. 이 오브젝트는 실패 원인에 대한 정보를 제공합니다.

```javascript
var errorObject = {
  src: 'find', // Operation that failed.
  err: -50, // Error code.
  msg: 'PERSISTENT\_STORE\_FAILURE', // Error message.
  col: 'people', // Collection name.
  usr: 'jsonstore', // User name.
  doc: {_id: 1, {name: 'carlos', age: 99}}, // Document that is related to the failure.
  res: {...} // Response from the server.
}
```
{: codeblock}
모든 키/값 쌍이 모든 오류 오브젝트의 일부인 것은 아닙니다. 예를 들어, doc 값은 문서(예: `JSONStoreInstance` 클래스의 `remove` 메소드)가 문서를 제거하는 데 실패하여 조작이 실패한 경우에만 사용 가능합니다.

### Objective-C 오류
{: #objective-c-errors }
실패할 수 있는 모든 API에는 NSError 오브젝트에 대해 주소를 취하는 오류 매개변수가 사용됩니다. 오류를 알리지 않으려면 `nil`을 전달할 수 있습니다. 조작이 실패하는 경우 주소는 NSError로 채워집니다. 여기에는 오류가 있고 가능한 일부 `userInfo`가 있습니다. `userInfo`에는 추가 세부사항(예: 실패를 야기한 문서)가 포함될 수 있습니다.

```objc
// This NSError points to an error if one occurs.
NSError* error = nil;

// Perform the destroy.
[JSONStore destroyDataAndReturnError:&error];
```

### Java 오류
{: #java-errors }
모든 Java API 호출에서는 발생한 오류에 따라 특정 예외를 처리합니다. 각 예외를 별도로 처리하거나, 모든 JSONStore 예외에 대한 보호로 `JSONStoreException`을 발견할 수 있습니다.

```java
try {
  WL.JSONStore.closeAll();
}

catch(JSONStoreException e) {
  // Handle error condition.
}
```
{: codeblock}
### 오류 코드 목록
{: #list-of-error-codes }
공통 오류 코드 및 해당 설명 목록:

|오류 코드      | 설명 |
|----------------|-------------|
| -100 UNKNOWN_FAILURE | Unrecognized error. |
| -75 OS\_SECURITY\_FAILURE | This error code is related to the requireOperatingSystemSecurity flag. It can occur if the destroy API fails to remove security metadata that is protected by operating system security (Touch ID with passcode fallback), or the init or open APIs are unable to locate the security metadata. It can also fail if the device does not support operating system security, but operating system security usage was requested. |
| -50 PERSISTENT\_STORE\_NOT\_OPEN | JSONStore is closed. Try calling the open method in the JSONStore class class first to enable access to the store. |
| -48 TRANSACTION\_FAILURE\_DURING\_ROLLBACK | There was a problem with rolling back the transaction. |
| -47 TRANSACTION\\_FAILURE\_DURING\_REMOVE\_COLLECTION |Cannot call removeCollection while a transaction is in progress. |
| -46 TRANSACTION\_FAILURE\_DURING\_DESTROY | Cannot call destroy while there are transactions in progress. |
| -45 TRANSACTION\_FAILURE\_DURING\_CLOSE\_ALL | Cannot call closeAll while there are transactions in place. |
| -44 TRANSACTION\_FAILURE\_DURING\_INIT | Cannot initialize a store while there are transactions in progress. |
| -43 TRANSACTION_FAILURE | There was a problem with transactions. |
| -42 NO\_TRANSACTION\_IN\_PROGRESS | Cannot commit to rolled back a transaction when there is no transaction is progress |
| -41 TRANSACTION\_IN\_POGRESS | Cannot start a new transaction while another transaction is in progress. |
| -40 FIPS\_ENABLEMENT\_FAILURE |Something is wrong with FIPS. |
| -24 JSON\_STORE\_FILE\_INFO\_ERROR | Problem getting the file information from the file system. |
| -23 JSON\_STORE\_REPLACE\_DOCUMENTS\_FAILURE | Problem replacing documents from a collection. |
| -22 JSON\_STORE\_REMOVE\_WITH\_QUERIES\_FAILURE | Problem removing documents from a collection. |
| -21 JSON\_STORE\_STORE\_DATA\_PROTECTION\_KEY\_FAILURE | Problem storing the Data Protection Key (DPK). |
| -20 JSON\_STORE\_INVALID\_JSON\_STRUCTURE | Problem indexing input data. |
| -12 INVALID\_SEARCH\_FIELD\_TYPES | Check that the types that you are passing to the searchFields are stringinteger,number, orboolean. |
| -11 OPERATION\_FAILED\_ON\_SPECIFIC\_DOCUMENT | An operation on an array of documents, for example the replace method can fail while it works with a specific document. The document that failed is returned and the transaction is rolled back. On Android, this error also occurs when trying to use JSONStore on unsupported architectures. |
| -10 ACCEPT\_CONDITION\_FAILED | The accept function that the user provided returned false. |
| -9 OFFSET\_WITHOUT\_LIMIT | To use offset, you must also specify a limit. |
| -8 INVALID\_LIMIT\_OR\_OFFSET | Validation error, must be a positive integer. |
| -7 INVALID_USERNAME | Validation error (Must be [A-Z] or [a-z] or [0-9] only). |
| -6 USERNAME\_MISMATCH\_DETECTED | To log out, a JSONStore user must call the closeAll method first. There can be only one user at a time. |
| -5 DESTROY\_REMOVE\_PERSISTENT\_STORE\_FAILED |A problem with the destroy method while it tried to delete the file that holds the contents of the store. |
| -4 DESTROY\_REMOVE\_KEYS\_FAILED | Problem with the destroy method while it tried to clear the keychain (iOS) or shared user preferences (Android). |
| -3 INVALID\_KEY\_ON\_PROVISION | Passed the wrong password to an encrypted store. |
| -2 PROVISION\_TABLE\_SEARCH\_FIELDS\_MISMATCH | Search fields are not dynamic. It is not possible to change search fields without calling the destroy method or the removeCollection method before you call the init or openmethod with the new search fields. This error can occur if you change the name or type of the search field. For example: {key: 'string'} to {key: 'number'} or {myKey: 'string'} to {theKey: 'string'}. |
| -1 PERSISTENT\_STORE\_FAILURE | Generic Error. A malfunction in native code, most likely calling the init method. |
| 0 SUCCESS |어떤 경우에, JSONStore 네이티브 코드는 성공을 표시하기 위해 0을 리턴합니다. |
| 1 BAD\_PARAMETER\_EXPECTED\_INT | 유효성 검증 오류입니다. |
| 2 BAD\_PARAMETER\_EXPECTED\_STRING | 유효성 검증 오류입니다. |
| 3 BAD\_PARAMETER\_EXPECTED\_FUNCTION | 유효성 검증 오류입니다. |
| 4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING | 유효성 검증 오류입니다. |
| 5 BAD\_PARAMETER\_EXPECTED\_OBJECT | 유효성 검증 오류입니다. |
| 6 BAD\_PARAMETER\_EXPECTED\_SIMPLE\_OBJECT | 유효성 검증 오류입니다. |
| 7 BAD\_PARAMETER\_EXPECTED\_DOCUMENT | 유효성 검증 오류입니다. |
| 8 FAILED\_TO\_GET\_UNPUSHED\_DOCUMENTS\_FROM\_DB |더티 상태로 표시된 모든 문서를 선택하는 조회가 실패했습니다. 조회의 SQL 예제는 다음과 같습니다. SELECT * FROM [collection] WHERE _dirty > 0. |
| 9 NO\_ADAPTER\_LINKED\_TO\_COLLECTION | JSONStoreCollection 클래스에서 push 및 load 메소드와 같은 함수를 사용하려면 어댑터를 init 메소드에 전달해야 합니다. |
| 10 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ARRAY\_OF\_DOCUMENTS |유효성 검증 오류 |
| 11 INVALID\_PASSWORD\_EXPECTED\_ALPHANUMERIC\_STRING\_WITH\_LENGTH\_GREATER\_THAN\_ZERO |유효성 검증 오류 |
| 12 ADAPTER_FAILURE | WL.Client.invokeProcedure 호출 문제점, 특히 어댑터와 연결하는 문제점입니다. 이 오류는 백엔드를 호출하려고 하는 어댑터에서의 실패와 다릅니다. |
| 13 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ID |유효성 검증 오류 |
| 14 CAN\_NOT\_REPLACE\_DEFAULT\_FUNCTIONS | 기존 함수(find 및 add)를 바꾸기 위해 JSONStoreCollection 클래스에서 enhance 함수를 호출하는 것은 허용되지 않습니다. |
| 15 COULD\_NOT\_MARK\_DOCUMENT\_PUSHED |푸시가 문서를 어댑터에 보내지만 JSONStore가 문서를 더티 상태가 아닌 것으로 표시하는데 실패합니다. |
| 16 COULD\_NOT\_GET\_SECURE\_KEY |비밀번호로 콜렉션을 초기화하려면, {{ site.data.keys.mf_server }}와의 연결이 있어야 합니다. '보안 랜덤 토큰'을 리턴하기 때문입니다. IBM  Worklight  V5.0.6 이상에서는 개발자가 options 오브젝트를 통해 init 메소드에 로컬로 {localKeyGen: true}를 전달하는 보안 랜덤 토큰을 생성할 수 있습니다. |
| 17 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER |WL.Client.invokeProcedure가 실패 콜백을 호출하여 데이터를 로드할 수 없습니다. |
| 18 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER\_INVALID\_LOAD\_OBJ | init 메소드로 전달된 로드 오브젝트가 유효성 검증을 통과하지 못했습니다. |
| 19 INVALID\_KEY\_IN\_LOAD\_OBJECT | add 메소드를 호출할 때 로드 오브젝트에서 사용한 키에 문제점이 있습니다. |
| 20 UNDEFINED\_PUSH\_OPERATION |서버에 더티 문서를 푸시하기 위한 프로시저가 정의되어 있습니다. 예를 들어, init 메소드(새 문서가 더티 상태임, 오퍼레이션 = 'add') 및 push 메소드(오퍼레이션 = 'add'로 새 문서를 찾음)가 호출되었지만 add 프로시저를 포함하는 add 키가 콜렉션에 링크된 어댑터에 없습니다. 어댑터 링크는 init 메소드 내부에서 수행됩니다. |
| 21 INVALID\_ADD\_INDEX\_KEY | 추가 검색 필드에 문제점이 있습니다. |
| 22 INVALID\_SEARCH\_FIELD | 검색 필드 중 하나가 올바르지 않습니다. 전달된 검색 필드가 _id,json,_deleted 또는 _operation이 아닌지 확인하십시오. |
| 23 ERROR\_CLOSING\_ALL | 일반 오류입니다. 네이티브 코드가 closeAll 메소드를 호출할 때 오류가 발생했습니다. |
| 24 ERROR\_CHANGING\_PASSWORD | 비밀번호를 변경할 수 없습니다. 예를 들어 전달된 이전 비밀번호가 잘못되었습니다. |
| 25 ERROR\_DURING\_DESTROY | 일반 오류입니다. 네이티브 코드에서 destroy 메소드를 호출할 때 오류가 발생했습니다. |
| 26 ERROR\_CLEARING\_COLLECTION | 일반 오류입니다. 네이티브 코드에서 removeCollection 메소드를 호출할 때 오류가 발생했습니다. |
| 27 INVALID\_PARAMETER\_FOR\_FIND\_BY\_ID | 유효성 검증 오류입니다. |
| 28 INVALID\_SORT\_OBJECT | JSON 오브젝트 중 하나가 올바르지 않으므로 정렬용으로 제공된 배열이 올바르지 않습니다. 올바른 구문은 JSON 오브젝트의 배열이며, 각 오브젝트에는 단일 특성만 포함됩니다. 이 특성은 정렬할 필드와 오름차순 또는 내림차순 여부를 검색합니다. 예: {searchField1 : "ASC"}. |
| 29 INVALID\_FILTER\_ARRAY | 결과 필터링을 위해 제공된 배열이 올바르지 않습니다. 이 배열의 올바른 구문은 문자열 배열이며, 각 문자열은 검색 필드 또는 내부 JSONStore 필드입니다. 자세한 정보는 '저장소 내부'를 참조하십시오. |
| 30 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_OBJECTS | 배열이 유일한 JSON 오브젝트의 배열이 아닌 경우 유효성 검증 오류가 발생합니다. |
| 31 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_CLEAN\_DOCUMENTS | 유효성 검증 오류입니다. |
| 32 BAD\_PARAMETER\_WRONG\_SEARCH\_CRITERIA | 유효성 검증 오류입니다. |
