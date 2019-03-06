---

copyright:
  years: 2018, 2019
lastupdated: "2018-10-16"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 중간자 공격(Man-in-the-middle attack) 방지
{: #prevent_man_in_the_middle_attack}


공용 네트워크를 통해 통신할 때는 정보를 안전하게 보내고 받는 것이 매우 중요합니다. 이러한 통신을 보호하는 데 널리 사용되는 프로토콜이 SSL/TLS입니다. SSL/TLS에서는 디지털 인증서를 사용하여 인증 및 암호화를 제공합니다. 인증서 체인 검증에 의존하는 이러한 프로토콜은 모바일 디바이스와 백엔드 시스템 사이에 전달되는 모든 트래픽을 권한 없는 사용자가 보고 수정할 수 있을 때 발생하는 중간자 공격을 포함하여 많은 위험한 공격에 취약합니다.
{: shortdesc}

[인증서 고정](#cert_pinning)을 통해 보안 공격을 줄이고 중간자(MitM) 공격을 방지하십시오.


>**참고**: 이 기능은 선택적이며 Professional 플랜에만 사용할 수 있습니다.

인증서 고정 프로세스는 다음 단계로 구성됩니다.

1. [인증서 설정](#certsetup)을 수행하십시오.
2. 보안 요청을 수행하기 전에 올바른 [인증서 고정 API](#certpinapi) 메소드를 호출하도록 클라이언트 코드를 구성하십시오.

SSL 핸드쉐이크(서버에 대한 첫 번째 요청) 중에 Mobile Foundation Client SDK에서 서버 인증서의 공용 키가 앱에 저장된 인증서의 공용 키와 일치하는지 확인합니다.

>**참고**: 인증 기관에서 구매한 인증서를 사용해야 합니다. 자체 서명한 인증서는 **지원되지 않습니다**.

## 인증서 고정
{: #cert_pinning}

인증서 체인 검증(예: SSL/TLS)에 의존하는 프로토콜은 모바일 디바이스와 백엔드 시스템 사이에 전달되는 모든 트래픽을 권한 없는 사용자가 보고 수정할 수 있을 때 발생하는 중간자 공격을 포함하여 많은 위험한 공격에 취약합니다. 인증서가 올바른 인증서이고 유효한지 신뢰할 수 있도록 신뢰할 수 있는 인증 기관(CA)에 속하는 루트 인증서에서 디지털 서명을 수행합니다. 운영 체제 및 브라우저에서는 신뢰할 수 있는 CA에서 발행하고 서명한 인증서를 쉽게 확인할 수 있도록 이러한 신뢰 CA 루트 인증서의 목록을 유지보수합니다.

IBM Mobile Foundation에서는 **인증서 고정**을 사용하도록 설정하는 API를 제공합니다. 기본 iOS, 기본 Android 및 크로스 플랫폼 Cordova MobileFirst 애플리케이션에서 지원됩니다.

## 인증서 고정 프로세스
{: #certpinprocess}

인증서 고정은 예상되는 공개 키와 호스트를 연관시키는 프로세스입니다. 운영 체제 또는 브라우저에서 인식하는 신뢰할 수 있는 CA 루트 인증서에 해당하는 인증서가 아니라 도메인 이름의 특정 인증서만 승인하도록 클라이언트 코드를 구성할 수 있습니다. 인증서 사본은 클라이언트 애플리케이션에 배치됩니다. SSL 핸드쉐이크(서버에 대한 첫 번째 요청) 중에 MobileFirst Client SDK에서 서버 인증서의 공용 키가 앱에 저장된 인증서의 공용 키와 일치하는지 확인합니다.

>**참고**: 클라이언트 애플리케이션을 사용하여 여러 인증서를 고정할 수도 있습니다. 모든 인증서의 사본은 클라이언트 애플리케이션에 배치되어야 합니다. SSL 핸드쉐이크(서버에 대한 첫 번째 요청) 중에 MobileFirst Client SDK에서 서버 인증서의 공용 키가 앱에 저장된 인증서 중 하나의 공용 키와 일치하는지 확인합니다.

**중요**

* 일부 모바일 운영 체제에서는 인증서 유효성 검증 확인 결과를 캐시할 수 있습니다. 따라서 코드에서 보안 요청을 작성하기 **전에** 인증서 고정 API 메소드를 호출해야 합니다. 그렇지 않으면 후속 요청에서 인증서 유효성 검증 및 고정 확인을 건너뛸 수 있습니다.
* 인증서 고정 이후에도 관련 호스트와의 모든 통신에 Mobile Foundation API만 사용하십시오. 동일한 호스트와 상호작용하기 위해 서드파티 API를 사용하면 모바일 운영 체제에서 검증되지 않은 인증서를 캐싱하는 것과 같이 예상치 못한 동작이 발생할 수 있습니다.
* 인증서 고정 API 메소드를 다시 호출하면 이전의 고정 작업을 대체합니다.

고정 프로세스가 성공하면 제공된 인증서 내부의 공용 키를 사용하여 보안 요청 SSL/TLS 핸드쉐이크 중에 MobileFirst Server 인증서의 무결성을 확인합니다. 고정 프로세스에 실패하면 서버에 대한 모든 SSL/TLS 요청이 클라이언트 애플리케이션에서 거부됩니다.

## 인증서 설정
{: #certsetup}

인증 기관에서 구매한 인증서를 사용해야 합니다. 자체 서명한 인증서는 **지원되지 않습니다**. 지원되는 환경과의 호환성을 위해 **DER**(Distinguished Encoding Rules, International Telecommunications Union X.690 표준에 정의됨) 형식으로 인코딩된 인증서를 사용해야 합니다.

인증서는 MobileFirst Server와 애플리케이션 모두에 배치해야 합니다. 인증서를 다음과 같이 배치하십시오.

* MobileFirst Server(WebSphere Application Server, WebSphere Application Server Liberty 또는 Apache Tomcat): SSL/TLS 및 인증서 구성 방법에 관한 정보는 특정 Application Server 문서를 참조하십시오.
* 애플리케이션에서 다음을 수행하십시오.
    * 기본 iOS: 애플리케이션 **번들**에 인증서 추가
    * 기본 Android: **assets** 폴더에 인증서 배치
    * Cordova: 인증서를 **app-name\www\certificates** 폴더에 배치(폴더가 해당 위치에 없으면 새로 작성)

## 인증서 고정 API
{: #certpinapi}

인증서 고정은 과부화된 API 메소드 즉, 매개변수 ``certificateFilename``이 있는 하나의 메소드(``certificateFilename``은 인증서 파일 이름임)와 매개변수 ``certificateFilenames``가 있는 두 번째 메소드(``certificateFilenames``은 인증서 파일 이름의 배열임)로 구성됩니다.

### Android
{: #certpinapiandroid}

* 단일 인증서:
 
    구문: pinTrustedCertificatePublicKeyFromFile(String certificateFilename); 

    예:

    ```java
    WLClient.getInstance().pinTrustedCertificatePublicKey("myCertificate.cer");
    ```

* 다중 인증서:

    구문: pinTrustedCertificatePublicKeyFromFile(String[] certificateFilename);

    예:

    ```java
    String[] certificates={"myCertificate.cer","myCertificate1.cer"};
    WLClient.getInstance().pinTrustedCertificatePublicKey(certificates);
    ```

인증서 고정 메소드를 사용하면 다음 두 가지 경우에 예외가 발생합니다.

* 파일이 없는 경우
* 파일 형식이 잘못된 경우

### iOS
{: #certpinapiios}

* 단일 인증서 고정

    구문: pinTrustedCertificatePublicKeyFromFile:(NSString*) certificateFilename;

    인증서 고정 메소드를 사용하면 다음 두 가지 경우에 예외가 발생합니다.

    * 파일이 없는 경우
    * 파일 형식이 잘못된 경우

* 다중 인증서 고정 
 
    구문: pinTrustedCertificatePublicKeyFromFiles:(NSArray*) certificateFilenames;

    인증서 고정 메소드를 사용하면 다음 두 가지 경우에 예외가 발생합니다.

    * 인증서 파일이 없음
    * 올바른 형식으로 된 인증서 파일이 없음

**Objective-C**:

예: 단일 인증서:

```
[[WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFile:@"myCertificate.cer"];
```

예: 다중 인증서:

```
NSArray *arrayOfCerts = [NSArray arrayWithObjects:@“Cert1”,@“Cert2”,@“Cert3",nil];
[[WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFiles:arrayOfCerts];
```

**Swift**:

예: 단일 인증서:

```
WLClient.sharedInstance().pinTrustedCertificatePublicKeyFromFile("myCertificate.cer")
```

예: 다중 인증서:

```
let arrayOfCerts : [Any] = ["Cert1", "Cert2”, "Cert3”];
WLClient.sharedInstance().pinTrustedCertificatePublicKey( fromFiles: arrayOfCerts)
```

인증서 고정 메소드를 사용하면 다음 두 가지 경우에 예외가 발생합니다.

* 파일이 없는 경우
* 파일 형식이 잘못된 경우

### Cordova
{: #certpinapicordova}

* 단일 인증서 고정:

    ```javascript
    WL.Client.pinTrustedCertificatePublicKey('myCertificate.cer').then(onSuccess, onFailure);
    ```

* 다중 인증서 고정:

    ```javascript
    WL.Client.pinTrustedCertificatePublicKey(['Cert1.cer','Cert2.cer','Cert3.cer']).then(onSuccess, onFailure);
    ```

인증서 고정 메소드에서는 다음과 같은 프라미스를 리턴합니다.

* 인증서 고정 메소드는 고정이 완료되는 경우 onSuccess 메소드를 호출합니다.
* 인증서 고정 메소드는 다음과 같은 두 가지 경우에 onFailure 콜백을 트리거합니다.
* 파일이 없는 경우
* 파일 형식이 잘못된 경우

나중에 인증서가 고정되지 않은 서버로 보안된 요청이 작성되면 특정 요청(예: ``obtainAccessToken`` 또는 ``WLResourceRequest``)의 ``onFailure`` 콜백이 호출됩니다.

>[API 참조](/docs/services/mobilefoundation?topic=mobilefoundation-client_sdks#client_sdks)에서 인증서 고정 API 메소드에 관해 자세히 알아보십시오.
