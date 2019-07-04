---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: integration, mobile foundation, secure gateway

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Secure Gateway 서비스를 사용하여 온프레미스 시스템에 대한 보안 연결 설정
{: #integrate_secure_gateway}

엔터프라이즈 모바일 앱을 빌드할 때 애플리케이션을 기존의 레코드 시스템과 통합하려는 경우가 자주 있습니다. 모바일 앱에서 온프레미스 데이터 센터에 저장된 데이터에 액세스하고 이를 활용하려면 [IBM Cloud](https://cloud.ibm.com/)에서 [Mobile Foundation 서비스](https://cloud.ibm.com/catalog/services/mobile-foundation)를 [Secure Gateway 서비스](https://cloud.ibm.com/catalog/services/secure-gateway)와 함께 사용할 수 있습니다. Secure Gateway 서비스에서는 빠르고 쉬운 보안 연결을 제공하며 연결하려는 원격 시스템과 IBM Cloud 사이에 터널을 설정합니다.

이 튜토리얼에서는 Secure Gateway 서비스를 사용하여 IBM Cloud에서 실행 중인 Mobile Foundation 어댑터에서 온프레미스 데이터 센터의 HTTP 엔드포인트에 액세스하는 방법을 설명합니다.

## 전제조건
{: #prereq_int_sec_gw}

이 튜토리얼을 완료하려면 SOR(System of Record) 데이터를 노출하는 엔터프라이즈 방화벽 내에 HTTP 엔드포인트가 있어야 합니다. 또는 [이 샘플 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/MobileFirst-Platform-Developer-Center/MFPSecureGatewayIonic/tree/master/NodeJSHTTPProject) `Node.js` 프로젝트를 사용하여 로컬 환경에 테스트 엔드포인트를 작성하십시오.

프로젝트 폴더로 이동하여 HTTP 서버를 실행하십시오.

```bash
npm install
node app.js
```
{: codeblock}

## Secure Gateway 서비스와 통합 시나리오
{: #secure_gateway}

다음 이미지에서는 이 튜토리얼에서 설명하는 통합 시나리오에 사용된 아키텍처를 보여줍니다.

![아키텍처 다이어그램](images/SecureGatewayArchi.png "디바이스, 클라우드 서비스 및 온프레미스 네트워크의 아키텍처 다이어그램")

## Secure Gateway 통합 구현
{: #implementing_sg_integration}

### Secure Gateway 서비스 인스턴스 작성
IBM Cloud에 로그인하여 [ Secure Gateway 서비스](https://cloud.ibm.com/catalog/services/secure-gateway/)의 인스턴스를 작성하십시오.

![IBM Cloud](images/SecureGatewayInst.gif "IBM Cloud 카탈로그에서 Secure Gateway 인스턴스 작성")

Secure Gateway 서비스 인스턴스를 작성한 후 다음 단계에 따라 IBM Cloud와 온프레미스 환경 사이의 Secure Gateway 서비스를 구성하십시오.

### 게이트웨이 추가
{: #add_gateway}

Secure Gateway 서비스 대시보드에서 **게이트웨이 추가**를 클릭하고 필요한 게이트웨이 이름을 제공하여 새 게이트웨이를 작성하십시오.

![게이트웨이 추가](images/AcmeAddGateway.gif "게이트웨이 UI 추가 단계")


### Secure Gateway 클라이언트 추가
{: #add_sg_client}

![클라이언트2 추가](images/AcmeAddClient.gif "클라이언트 UI 추가 단계")

**클라이언트** 탭의 새 게이트웨이에서 **클라이언트 연결**을 클릭하십시오.

선택한 클라이언트를 사용하고 온프레미스 환경에서 Secure Gateway 클라이언트를 실행할 수 있습니다. Secure Gateway 클라이언트를 설정하는 단계는 Secure Gateway 콘솔에서 사용할 수 있습니다.

이 튜토리얼에서는 Docker 컨테이너 옵션을 사용하여 Secure Gateway 클라이언트를 실행합니다.
다음 단계를 사용하십시오.
*   이미 설치되어 있지 않은 경우 온프레미스 시스템에 Docker를 설치하십시오.
*   터미널을 시작하고 서비스 콘솔에 표시된 명령을 사용하여 컨테이너에서 Secure Gateway 클라이언트를 실행하십시오.
    ```bash
    docker run –it ibmcom/secure-gateway-client <gatewayId>
    ```
    {: codeblock}
    `gatewayId`는 앞의 이미지에 표시된 대로 콘솔에서 찾을 수 있습니다.

### 대상 추가
{: #add_destination}

![목적지 추가](images/AcmeAddDest.gif "목적지 UI 추가 단계")

**대상** 탭의 새 게이트웨이에서 **대상 추가**를 클릭하십시오.

Secure Gateway 서비스를 사용하면 IBM Cloud 환경에서 온프레미스 엔드포인트에 연결하거나 온프레미스 환경에서 클라우드 리소스로 연결할 수 있습니다. 이 시나리오에서는 온프레미스를 리소스 위치로 선택하고 온프레미스 리소스의 호스트 이름 / IP 및 포트를 제공하십시오. 원하는 대상 이름도 제공하십시오.

Secure Gateway 클라이언트에서 리소스 엔드포인트로 요청을 전달할 수 있으려면 액세스 목록에 리소스를 추가해야 합니다.
Docker 런타임에서 컨테이너로 실행되는 Secure Gateway 클라이언트의 터미널에서 다음 명령을 실행하여 액세스 목록에 리소스를 추가하십시오.

```
acl allow <resourceHost>:<resourcePort>
```
{: codeblock}
`resourceHost` 및 `resourcePort`는 온프레미스 리소스 엔드포인트의 세부사항을 나타냅니다.

이제 대상이 구성되었습니다. Secure Gateway 서비스에서는 클라우드 환경에서 온프레미스 리소스에 액세스하는 데 사용할 수 있는 클라우드 호스트와 포트 세부사항을 채웁니다.

![목적지 탭](images/AcmeCloudPopulate.gif "호스트 및 포트 세부사항 화면")

### Mobile Foundation 및 Mobile Foundation 어댑터를 사용하여 Secure Gateway 서비스 구성
{: #configuration_sg_mfp}

이 튜토리얼에서는 IBM Cloud의 Mobile Foundation 서비스 인스턴스를 사용하여 Mobile Foundation 서버를 구성합니다. IBM Cloud의 Mobile Foundation 서비스를 사용하면 Liberty 런타임 시 Mobile Foundation 서버를 Cloud Foundry 애플리케이션으로 프로비저닝할 수 있습니다. Mobile Foundation 서비스를 사용하면 로컬 환경에서 개발된 모든 Mobile Foundation 프로젝트를 사용하고 IBM Cloud에서 실행할 수 있습니다.

### IBM Cloud의 Mobile Foundation 서버 설정
{: #mf_server_setup}

IBM Cloud 콘솔에서 [Mobile Foundation 서비스](https://cloud.ibm.com/catalog/services/mobile-foundation)의 인스턴스를 작성하십시오.

Mobile Foundation 서비스 콘솔에서 [Mobile Foundation 서버 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/ibmcloud/using-mobile-foundation/)를 작성하십시오.


### Mobile Foundation 어댑터 빌드 및 배치
{: #deploying_mf_adapter}

이 튜토리얼에서는 Mobile Foundation 어댑터를 사용하여 Secure Gateway 엔드포인트에 연결합니다. Mobile Foundation JavaHTTP 어댑터를 [다운로드 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/MobileFirst-Platform-Developer-Center/Adapters/tree/release80/JavaHTTP)하십시오.

[mfpdev-cli](/docs/services/mobilefoundation?topic=mobilefoundation-mobile_foundation_cli#mobile_foundation_cli) 명령을 사용하여 Mobile Foundation Operations 콘솔에서 어댑터를 빌드하고 배치하십시오.
```bash
mfpdev adapter build 
mfpdev adapter deploy
```
{: codeblock}

[여기 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/)에서 어댑터 빌드 및 배치에 관해 알아보십시오.
{: tip}

이전 섹션에서 가져온 JavaHTTP 어댑터의 리소스 엔드포인트에 관한 클라우드 호스트와 포트 세부사항을 제공하십시오.

![AdapterConfiguration ](images/AdapterConfiguration.png "Java HTTP 구성 페이지")

여기서 `cap-sg-prd-5.securegateway.appdomain.cloud`와 `18946`은 각각 Secure Gateway 호스트와 포트입니다.

이제 Mobile Foundation 어댑터가 구성되고 Secure Gateway 서비스를 사용하여 엔터프라이즈의 온프레미스 시스템과 Mobile Foundation 서비스를 사용할 수 있습니다.

### Mobile Foundation 샘플 앱 작성 및 등록
{: #registering_sample_app}

[여기](https://github.com/MobileFirst-Platform-Developer-Center/MFPSecureGatewayIonic/)에서 Mobile Foundation 샘플 앱을 다운로드하고 `Readme`의 지시사항을 따르며 Mobile Foundation Operations 콘솔에서 앱을 등록하십시오.

앱을 실행하고 로그인할 인증 정보를 제공하며 *로그인* 단추를 클릭하십시오. *Acme 기록기 페치* 단추를 클릭하여 Mobile Foundation Operations 콘솔에 배치된 JavaHTTP 어댑터를 사용하여 Secure Gateway를 통해 온프레미스 엔드포인트를 호출하십시오. 온프레미스 환경에서 원하는 데이터를 수신하십시오.

![앱에서 온프레미스 데이터 수신](images/AcmePublishersApp.gif "샘플 앱에서 데이터 수신")

Secure Gateway 서비스에 여러 대상을 구성하고 엔드포인트의 각 클라우드 호스트에 연결하도록 Mobile Foundation 어댑터를 배치하여 여러 온프레미스 엔드포인트에 연결할 수 있습니다. HTTPS와 애플리케이션 측 보안을 통해 엔드포인트와 통신하도록 추가 보안을 사용하여 Secure Gateway 서비스도 구성할 수 있습니다. [여기에서 세부사항](/docs/services/SecureGateway?topic=securegateway-getting-started-with-sg#getting-started-with-sg)을 찾을 수 있습니다.


## 요약
{: #summary_int_sec_gw}

이 튜토리얼을 사용하면 Secure Gateway 서비스를 사용하여 IBM Cloud에서 실행 중인 Mobile Foundation 어댑터와 온프레미스 HTTP 엔드포인트 간에 보안 연결을 설정할 수 있어야 합니다.
