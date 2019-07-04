---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation security, restrict backend access, tampered apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}

# 변조된 앱에서 백엔드에 액세스하지 못하게 방지
{: #prevent_tampered_apps_accessing_backend}

애플리케이션 인증을 사용하면 서비스를 사용하기 전에 애플리케이션의 유효성을 검사할 수 있으므로, 변조된 애플리케이션에서 백엔드 서비스에 액세스하지 못하게 방지할 수 있습니다.
{: shortdesc}

애플리케이션을 적절하게 보호하려면 사전 정의된 MobileFirst 애플리케이션 인증 보안 검사(``appAuthenticity``)를 사용으로 설정하십시오. 이 검사를 사용하도록 설정하면 서비스를 제공하기 전에 애플리케이션의 신뢰성을 검증합니다. 프로덕션 환경의 애플리케이션에는 이 기능이 사용으로 설정되어 있어야 합니다.

애플리케이션 인증을 사용으로 설정하려면 **MobileFirst Operations Console → [사용자 애플리케이션] → 인증**에서 화면에 표시되는 지시사항을 따르거나 다음 정보를 검토하십시오.

<dl>
  <dt>가용성</dt>
  <dd>애플리케이션 인증은 지원되는 모든 플랫폼(iOS, Android, Windows 8.1 Universal, Windows 10 UWP)의 Cordova 및 기본 애플리케이션에서 사용 가능합니다.</dd>
  <dt>제한사항</dt>
  <dd>애플리케이션 인증은 iOS에서 **Bitcode**를 지원하지 않습니다. 따라서 애플리케이션 인증을 사용하기 전에 Xcode 프로젝트 특성에서 Bitcode를 사용 안함으로 설정하십시오.</dd>
</dl>

## 애플리케이션 인증 플로우
{: #appauthenticityflow}

애플리케이션이 MobileFirst Server에 등록하는 동안 애플리케이션 인증 보안 검사가 실행됩니다. 이는 애플리케이션의 인스턴스가 처음으로 서버에 연결할 때 발생합니다. 기본적으로 인증 확인은 한 번만 실행됩니다.

앱 인증이 사용으로 설정되고 고객이 애플리케이션의 변경사항을 도입해야 하는 경우 애플리케이션 버전을 업그레이드해야 합니다.

이 동작을 사용자 정의하는 방법을 학습하려면 [애플리케이션 인증 구성](#configappauthenticity)을 참조하십시오.

### 애플리케이션 인증 사용
{: #enableappauthenticity}

애플리케이션에서 애플리케이션 인증을 사용으로 설정하려면 다음 작업을 수행하십시오.

1. 즐겨찾는 브라우저에서 MobileFirst Operations Console을 여십시오.
2. 탐색 사이드바에서 애플리케이션을 선택하고 **인증** 메뉴 항목을 클릭하십시오.
3. **상태** 상자의 **설정/해제** 단추를 토글하십시오.

![애플리케이션 인증 사용](/images/enable_application_authenticity.png)

MobileFirst Server에서는 서버에 처음 연결하려고 할 때 애플리케이션의 인증이 유효한지 검증합니다. 보호된 리소스에 이 유효성 검증도 적용하려면 보호 범위에 appAuthenticity 보안 검사를 추가하십시오.

### 애플리케이션 인증 사용 안함
{: #disableappauthenticity}

개발 중 애플리케이션에 대한 특정 변경사항으로 인해 인증 유효성 검증이 실패할 수 있습니다. 따라서 개발 프로세스 중에는 애플리케이션 인증을 사용 안함으로 설정하는 것이 좋습니다. 프로덕션 환경의 애플리케이션에는 이 기능이 사용으로 설정되어 있어야 합니다.

애플리케이션 인증을 사용 안함으로 설정하려면 **상태** 상자의 **설정/해제** 단추를 다시 토글하십시오.

## 애플리케이션 인증 구성
{: #configappauthenticity}

기본적으로 애플리케이션 인증은 클라이언트 등록 동안에만 확인됩니다. 그러나 다른 보안 검사와 마찬가지로, 사용자는 자원 보호 아래 지시사항에 따라 콘솔에서 ``appAuthenticity`` 보안 검사를 사용하여 애플리케이션 또는 자원을 보호하도록 결정할 수 있습니다.

다음 특성을 사용하여 사전 정의된 애플리케이션-인증 보안 검사를 구성할 수 있습니다.

* ``expirationSec``: 3600초/1시간으로 기본 설정됩니다. 인증 토큰이 만료될 때까지 기간을 정의합니다.

인증 검사가 완료된 후에는 설정 값을 기초로 토큰이 만료될 때까지 인증 검사가 다시 발생하지 않습니다.

``expirationSec`` 특성을 구성하려면 다음을 수행하십시오.

1. MobileFirst Operations Console을 로드하고 **[사용자의 애플리케이션] → 보안 → 보안 검사 구성**으로 이동하여 **새로 작성**을 클릭하십시오.
2. ``appAuthenticity`` 범위 요소를 검색하십시오.
3. 새 값을 초 단위로 설정하십시오.

    ![몇 초 내에 만기 구성](/images/configuring_expirationSec.png)

## BTS(Build Time Secret)
{: #buildtimesecret}

BTS(Build Time Secret)는 iOS 애플리케이션 전용으로, **인증 유효성 검증을 보강하는 선택적 도구**입니다. 이 도구는 빌드 시간에 결정되는 본인확인정보를 애플리케이션에 삽입하며, 이는 나중에 인증 유효성 검증 프로세스에 사용됩니다.

BTS 도구는 **MobileFirst Operations Console → 다운로드 센터**에서 다운로드할 수 있습니다.

Xcode에서 BTS 도구를 사용하려면 다음 작업을 수행하십시오.

1. **빌드 단계** 탭에서 **+** 단추를 클릭하여 새 **스크립트 실행 단계**를 작성하십시오.
2. BTS 도구의 경로를 복사하고 작성한 새 "스크립트 실행 단계"에서 붙여넣으십시오.
3. 스크립트 실행 단계를 끌어 **소스 컴파일 단계** 앞에 놓으십시오.

이 도구는 애플리케이션의 프로덕션 버전을 빌드할 때 사용해야 합니다.

## 애플리케이션 인증 문제점 해결
{: #troubleshooting-app-authenticity}

### 재설정
{: #trblreset}

애플리케이션 인증 알고리즘은 유효성 검증에 애플리케이션 데이터 및 메타데이터를 사용합니다. 애플리케이션 인증을 사용으로 설정한 다음 서버에 연결하는 첫 번째 디바이스에서 이 데이터 중 일부를 포함하는 애플리케이션의 "지문"을 제공합니다.

알고리즘에 새 데이터를 제공하여 이러한 지문을 재설정할 수 있습니다. 이는 개발 중에 유용합니다(예: Xcode에서 애플리케이션을 변경한 후). 지문을 재설정하려면 mfpadm CLI에서 **reset** 명령을 사용하십시오.

지문을 재설정한 후 appAuthenticity 보안 검사는 이전과 마찬가지로 작동합니다(사용자는 이를 쉽게 확인할 수 있음).

### 유효성 검증 유형
{: #trblvalidationtypes}

Mobile First Platform Foundation에서는 애플리케이션의 정적 및 동적 앱 인증을 제공합니다. 이러한 유효성 검증 유형은 앱 인증 시드를 생성하는 데 사용되는 알고리즘 및 속성에 따라 다릅니다. 기본적으로 애플리케이션 인증은 사용으로 설정되면 동적 유효성 검증 알고리즘을 사용합니다. 두 유효성 검증 유형 모두 애플리케이션의 보안을 보장합니다. 동적 앱 인증에서는 엄격한 유효성 검증을 사용하고 앱 인증을 확인합니다. 정적 앱 인증의 경우, 약간 완화된 알고리즘을 사용합니다. 이 알고리즘에서는 동적 앱 인증에서 사용한 모든 유효성 검증 검사를 사용하지 않습니다.

동적 앱 인증은 MobileFirst 콘솔에서 구성할 수 있습니다. 내부 알고리즘에서는 콘솔에서 선택한 옵션을 기반으로 앱 인증 데이터를 생성합니다. 정적 앱 인증의 경우 mfpadm CLI를 사용해야 합니다.

정적 앱 인증을 사용하고 유효성 검증 유형을 전환하려면 다음과 같이 mfpadm CLI를 사용하십시오.

```bash
mfpadm --url=  --user=  --passwordfile= --secure=false app version [RUNTIME] [APPNAME] [ENVIRONMENT] [VERSION] set authenticity-validation TYPE
```
{: codeblock}

TYPE은 동적 또는 정적일 수 있습니다.
