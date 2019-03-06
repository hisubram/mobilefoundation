---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-19"

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


# 인증 및 보안
{: #basic_authentication}

MobileFirst 보안 프레임워크는 [OAuth 2.0](http://oauth.net/) 프로토콜을 기반으로 합니다. 이 프로토콜에 따라 리소스에 액세스하기 위해 필요한 권한을 정의하는 **범위**를 사용하여 리소스를 보호할 수 있습니다. 보호된 리소스에 액세스하려면 클라이언트에게 부여되는 권한의 범위를 캡슐화하는, 일치하는 **액세스 토큰**을 클라이언트가 제공해야 합니다.

OAuth 프로토콜은 리소스가 호스팅되는 리소스 서버와 권한 부여 서버의 역할을 구분합니다.

* 권한 부여 서버는 클라이언트 권한 부여 및 토큰 생성을 관리합니다.
* 리소스 서버는 권한 부여 서버를 사용하여 클라이언트가 제공하는 액세스 토큰을 유효성 검증하고 요청된 리소스의 보호 범위와 일치하게 합니다.

보호 프레임워크는 OAuth 프로토콜을 구현하는 권한 부여 서버를 중심으로 빌드되고 클라이언트가 액세스 토큰을 얻기 위해 상호작용하는 OAuth 엔드포인트를 노출합니다. 보안 프레임워크는 권한 부여 서버 및 기반이 되는 OAuth 프로토콜의 맨 위에 사용자 정의 권한 부여 로직을 구현하기 위한 빌딩 블록을 제공합니다. 기본적으로 MobileFirst Server는 **권한 부여 서버**로도 작동합니다. 그러나 IBM WebSphere DataPower 어플라이언스가 권한 부여 서버 역할을 하고 MobileFirst Server와 상호작용하도록 구성할 수 있습니다.

그러면 클라이언트 애플리케이션에서 이 토큰을 사용하여 MobileFirst Server 자체이거나 외부 서버일 수 있는 **리소스 서버**의 리소스에 액세스할 수 있습니다. 리소스 서버에서는 클라이언트에서 요청된 자원에 대한 액세스 권한을 부여받을 수 있는지 확인하기 위해 토큰의 유효성을 검사합니다. 리소스 서버와 권한 부여 서버를 분리하여 MobileFirst Server 외부에서 실행 중인 리소스의 보안을 강화할 수 있습니다.

애플리케이션 개발자는 각 보호된 리소스에 대한 필수 범위를 정의하고 보안 검사 및 인증 확인 핸들러를 구현하여 해당 리소스에 대한 액세스를 보호합니다. 서버 측 보안 프레임워크 및 클라이언트 측 API를 사용하면 권한 부여 서버와의 상호작용 및 OAuth 메시지 교환을 투명하게 처리하여 개발자가 권한 부여 로직에만 초점을 둘 수 있습니다.

## 권한 부여 엔티티
{: #acs_authorization_entitiesty}

### 액세스 토큰
{: #acs_access_tokens}

MobileFirst 액세스 토큰은 클라이언트의 권한 부여 권한을 설명하는 디지털 서명된 엔티티입니다. 특정 범위의 클라이언트 권한 부여 요청이 승인되고 클라이언트가 인증되고 나면 권한 부여 서버의 토큰 엔드포인트에서 요청된 액세스 토큰이 포함된 HTTP 응답을 클라이언트에 전송합니다.

#### 액세스 토큰 구조

MobileFirst 액세스 토큰에는 다음 정보가 포함되어 있습니다.

* **클라이언트 ID**: 클라이언트의 고유 ID입니다.
* **범위**: 토큰에 부여된 범위입니다(OAuth 범위 참조). 이 범위는 필수 애플리케이션 범위를 포함하지 않습니다.
* **토큰 만기 시간**: 토큰이 올바르지 않게 되는(만기되는) 시간(초)입니다.

#### 토큰 만기

만기 시간이 경과할 때까지 부여된 액세스 토큰은 유효한 상태를 유지합니다. 액세스 토큰의 만기 시간은 범위에 있는 모든 보안 검사의 만기 시간 중에서 가장 짧은 만기 시간으로 설정됩니다. 그러나 가장 짧은 만기 시간까지의 기간이 애플리케이션의 최대 토큰 만기 기간보다 긴 경우 토큰의 만기 시간은 현재 시간과 최대 만기 기간을 더한 값으로 설정됩니다. 기본 최대 토큰 만기 기간(유효 기간)은 3,600초(1시간)이지만 ``maxTokenExpiration`` 특성의 값을 설정하여 이를 구성할 수 있습니다.

**최대 액세스 토큰 만기 기간 구성**
{: #acs_config-max-access-tokens}

다음과 같은 대체 방법 중 하나를 사용하여 애플리케이션의 최대 액세스 토큰 만기 기간을 구성하십시오.

* MobileFirst Operations Console 사용
    1. **[사용자 애플리케이션]** → **보안** 탭을 선택하십시오.
    2. **토큰 구성** 섹션에서, **토큰 만기 기간(초)** 필드의 값을 선호하는 값으로 설정한 다음 **저장**을 클릭하십시오. 언제든지 이 프로시저를 반복하여 최대 토큰 만기 기간을 변경하거나, **기본값 복원**을 선택하여 기본값을 복원할 수 있습니다.

* 애플리케이션의 구성 파일 편집

    1. **명령행 창**에서 프로젝트의 루트 폴더로 이동하여 ``mfpdev app pull``을 실행하십시오.
    2. **[project-folder]\mobilefirst** 폴더에 있는 구성 파일을 여십시오.
    3. `maxTokenExpiration` 특성을 정의하여 파일을 편집하고 해당 값을 최대 액세스 토큰 만기 기간(초)으로 설정하십시오.
        ```java
        {
            ...
            "maxTokenExpiration": 7200
        }
        ```
        {: codeblock}
    4. ``mfpdev app push`` 명령을 실행하여 업데이트된 구성 JSON 파일을 배치하십시오.

**액세스 토큰 응답 구조**
{: #acs_access-tokens-structure}

액세스 토큰 요청에 대한 성공적 HTTP 응답에는 액세스 토큰 및 추가 데이터를 포함한 JSON 오브젝트가 들어 있습니다. 다음은 권한 부여 서버의 유효한 토큰 응답의 예제입니다.

```json
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-store
Pragma: no-cache
{
    "token_type": "Bearer",
    "expires_in": 3600,
    "access_token": "yI6ICJodHRwOi8vc2VydmVyLmV4YW1",
    "scope": "scopeElement1 scopeElement2"
}
```
{: codeblock}

토큰 응답 JSON 오브젝트에는 다음과 같은 특성 오브젝트가 있습니다.

* **token_type**: 토큰 유형은 [OAuth 2.0 Bearer 토큰 사용법 스펙](https://tools.ietf.org/html/rfc6750)에 따라 항상 "*Bearer*"입니다.
* **expires_in**: 액세스 토큰의 만기 시간(초)입니다.
* **access_token**: 생성된 액세스 토큰입니다(실제 액세스 토큰은 예제에 표시된 것보다 김).
* **scope**: 요청된 범위입니다.

**expires_in** 및 **scope** 정보는 토큰 자체(**access_token**) 내에도 포함됩니다.

>**참고**: 사용자가 하위 레벨의 `WLAuthorizationManager` 클래스를 사용하고 클라이언트 및 권한 인증과 리소스 서버 자체 사이에 OAuth 상호작용을 관리하는 경우 또는 기밀 클라이언트를 사용하는 경우에 올바른 액세스 토큰 응답의 구조가 관련됩니다. 상위 레벨 `WLResourceRequest` 클래스(보호된 리소스에 액세스하는 데 필요한 OAuth 플로우를 캡슐화함)를 사용하는 경우에는 보안 프레임워크가 액세스 토큰 응답 처리를 수행합니다. [클라이언트 보안 API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_oauth_client_apis.html?view=kc#c_oauth_client_apis) 및 [기밀 클라이언트](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients/)를 참조하십시오.

### 새로 고치기 토큰
{: #acs_refresh_tokens}

새로 고치기 토큰은 액세스 토큰이 만기될 때 새 액세스 토큰을 가져오는 데 사용할 수 있는 특수 유형의 토큰입니다. 새 액세스 토큰을 요청하려는 경우 유효한 새로 고치기 토큰을 제공할 수 있습니다. 새로 고치기 토큰은 유지 기간이 긴 토큰이며 액세스 토큰과 비교하여 더 오랜 시간 동안 유효한 상태로 남습니다.

새로 고치기 토큰을 사용하면 사용자가 영원히 인증된 상태로 남아 있을 수 있으므로 애플리케이션에서 주의하여 사용해야 합니다. 애플리케이션 제공자가 사용자를 정기적으로 인증하지 않는 소셜 미디어 애플리케이션, 전자 상거래 애플리케이션, 제품 카탈로그 찾아보기 및 이러한 유틸리티 애플리케이션에서는 새로 고치기 토큰을 사용할 수 있습니다. 사용자 인증을 자주 수행하는 애플리케이션에서는 새로 고치기 토큰을 사용하지 않아야 합니다.
MobileFirst 새로 고치기 토큰

MobileFirst 새로 고치기 토큰은 클라이언트의 권한 부여 권한을 설명하는 액세스 토큰과 같은 디지털 서명된 엔티티입니다. 새로 고치기 토큰은 동일한 범위의 새로운 액세스 토큰을 가져오는 데 사용할 수 있습니다. 특정 범위의 클라이언트 권한 부여 요청이 승인되고 클라이언트가 인증되고 나면 권한 부여 서버의 토큰 엔드포인트에서 요청된 액세스 토큰 및 새로 고치기 토큰이 포함된 HTTP 응답을 클라이언트에 전송합니다. 액세스 토큰이 만료되면 클라이언트에서 새로 고치기 토큰을 권한 부여 서버의 토큰 엔드포인트로 전송하여 새로운 액세스 토큰 및 새로 고치기 토큰 세트를 가져옵니다.

#### 새로 고치기 토큰 구조

MobileFirst 액세스 토큰과 비슷하게 MobileFirst 새로 고치기 토큰에는 다음 정보가 포함되어 있습니다.

* **클라이언트 ID**: 클라이언트의 고유 ID입니다.
* **범위**: 토큰에 부여된 범위입니다(OAuth 범위 참조). 이 범위는 필수 애플리케이션 범위를 포함하지 않습니다.
* **토큰 만기 시간**: 토큰이 올바르지 않게 되는(만기되는) 시간(초)입니다.

**토큰 만기**

새로 고치기 토큰의 토큰 만기 기간은 일반 액세스 토큰 만기 기간보다 깁니다. 새로 고치기 토큰은 부여되고 나면 만기 시간이 경과할 때까지 유효한 상태를 유지합니다. 이 유효 기간 내에 클라이언트에서 새로 고치기 토큰을 사용하여 새로운 액세스 토큰 및 새로 고치기 토큰 세트를 가져올 수 있습니다. 새로 고치기 토큰에는 30일의 고정 만기 기간이 있습니다. 클라이언트에서 새 액세스 토큰 및 새로 고치기 토큰 세트를 성공적으로 수신할 때마다 새로 고치기 토큰 만기가 재설정되어 클라이언트에서 토큰이 절대 만기되지 않습니다. 액세스 토큰 만기 규칙은 **액세스 토큰** 섹션에 설명된 것과 동일합니다.

**새로 고치기 토큰 기능 사용**
{: #acs_enable-refresh-token}

클라이언트 측 및 서버 측에서 각각 다음 특성을 사용하여 새로 고치기 토큰 기능을 사용할 수 있습니다.

**클라이언트 측 특성(Android)**
*파일 이름*: mfpclient.properties
*특성 이름*: wlEnableRefreshToken
*특성 값*: true
예를 들어 다음과 같습니다.
*wlEnableRefreshToken*=true

**클라이언트 측 특성(iOS)**
*파일 이름*: mfpclient.plist
*특성 이름*: wlEnableRefreshToken
*특성 값*: true
예를 들어 다음과 같습니다.
*wlEnableRefreshToken*=true

**서버 측 특성**
*파일 이름*: server.xml
*특성 이름*: mfp.security.refreshtoken.enabled.apps
*특성 값*: application bundle id separated by ‘;’

예를 들어 다음과 같습니다.

```xml
<jndiEntry jndiName="mfp/mfp.security.refreshtoken.enabled.apps" value='"com.sample.android.myapp1;com.sample.android.myapp2"'/>
```
{: codeblock}
서로 다른 플랫폼마다 다른 번들 ID를 사용하십시오.

**새로 고치기 토큰 응답 구조**

다음은 권한 부여 서버의 유효한 새로 고치기 토큰 응답의 예제입니다.

```json
    HTTP/1.1 200 OK
    Content-Type: application/json
    Cache-Control: no-store
    Pragma: no-cache
    {
        "token_type": "Bearer",
        "expires_in": 3600,
        "access_token": "yI6ICJodHRwOi8vc2VydmVyLmV4YW1",
        "scope": "scopeElement1 scopeElement2",
        "refresh_token": "yI7ICasdsdJodHRwOi8vc2Vashnneh "
    }
```        
{: codeblock}

새로 고치기 토큰 응답에는 액세스 토큰 응답 구조의 일부로 설명된 다른 특성 오브젝트 외에 추가 특성 오브젝트 refresh_token이 있습니다.

>**참고**: 새로 고치기 토큰은 액세스 토큰에 비해 수명이 깁니다. 따라서 새로 고치기 토큰 기능은 주의해서 사용해야 합니다. 정기적인 사용자 인증이 필요하지 않은 애플리케이션이 새로 고치기 토큰 기능을 사용하는 데 적합한 후보입니다.

>MobileFirst에서는 CD 업데이트 3부터 iOS에서 새로 고치기 토큰 기능을 지원합니다.

#### 보안 검사
{: #acs_securitychecks}

보안 검사는 서버 측 애플리케이션 리소스를 보호하기 위한 보안 로직을 구현하는 서버 측 엔티티입니다. 보안 검사의 단순한 예는 사용자의 인증 정보를 수신하고 사용자 레지스트리에 대해 인증 정보를 확인하는 사용자 로그인 보안 검사입니다. 또 다른 예로는 모바일 애플리케이션의 인증 유효성을 검증하므로 애플리케이션의 리소스에 액세스하는 불법적인 시도로부터 보호하는 사전 정의된 MobileFirst 애플리케이션 인증 보안 검사가 있습니다. 동일한 보안 검사를 여러 리소스를 보안하는 데 사용할 수도 있습니다.

보안 검사는 일반적으로 클라이언트가 검사를 통과하기 위해 특정 방식으로 응답하게 하는 보안 인증 확인을 발행합니다. 이 핸드쉐이크는 OAuth 액세스 토큰 획득 플로우의 일부로 발생합니다. 클라이언트에서는 **인증 확인 핸들러**를 사용하여 보안 검사의 인증 확인을 처리합니다.

**기본 제공 보안 검사**

다음 사전 정의된 보안 검사를 사용할 수 있습니다.

* 애플리케이션 인증
* LTPA 기반 싱글 사인온(SSO)
* 직접 업데이트

#### 인증 확인 핸들러 엔티티
{: #challengehandler_entity}

보호된 리소스에 액세스하려고 할 때 클라이언트는 인증 확인에 직면할 수 있습니다. 인증 확인은 클라이언트가 이 리소스에 액세스를 허용받았는지 확인하기 위한 서버의 프롬프트, 보안 테스트, 질문입니다. 가장 일반적으로 이 인증 확인에서는 사용자 이름 및 비밀번호와 같은 인증 정보를 요청합니다.

인증 확인 핸들러는 클라이언트 측 보안 로직 및 관련 사용자 상호작용을 구현하는 클라이언트 측 엔티티입니다. 

>**중요**: 인증 확인은 수신된 후에는 무시할 수 없습니다. 응답을 하거나 취소해야 합니다. 인증 확인을 무시하면 예상치 않은 동작이 발생할 수 있습니다.

### 범위
{: #scopes}

어댑터와 같은 리소스에 범위를 지정하여 이를 권한 부여되지 않은 액세스로부터 보호할 수 있습니다.

범위는 공백으로 구분된 하나 이상의 범위 요소 문자열(“scopeElement1 scopeElement2 …”)로 정의되거나 기본 범위(RegisteredClient)를 적용하도록 널로 정의됩니다. 리소스에서 리소스 보호를 사용 안함으로 설정하지 않는 한, MobileFirst 보안 프레임워크에서는 리소스에 범위가 지정되지 않은 경우에도 모든 어댑터 리소스의 액세스 토큰이 필요합니다.

#### 범위 요소
{: #scopeelements}

범위 요소는 다음 중 하나일 수 있습니다.

* 보안 검사의 이름.
* 이 리소스에 필요한 보안 레벨을 정의하는 `access-restricted` 또는 `deletePrivilege`와 같은 임의의 키워드. 이 키워드는 나중에 보안 검사와 맵핑됩니다.

#### 범위 맵핑
{: #scopemapping}

기본적으로 사용자가 **범위**에 작성한 **범위 요소**는 **동일한 이름의 보안 검사**에 맵핑됩니다. 예를 들어 `PinCodeAttempts`라는 보안 검사를 작성하는 경우 범위 내에서 동일한 이름의 범위 요소를 사용할 수 있습니다.

범위 맵핑은 범위 요소를 보안 검사에 맵핑할 수 있게 합니다. 클라이언트가 범위 요소를 요청할 때 적용될 보안 검사를 정의합니다. 예를 들어 범위 요소 `access-restricted`를 사용자의 `PinCodeAttempts` 보안 검사에 맵핑할 수 있습니다.

리소스에 액세스하려는 애플리케이션에 따라 다르게 리소스를 보호하려는 경우 범위 맵핑이 유용합니다. 0개 이상으로 된 보안 검사 목록에 범위를 맵핑할 수도 있습니다.

예: scope = `access-restricted deletePrivilege`

* 앱 A에서
    * `access-restricted`는 `PinCodeAttempts`에 맵핑됩니다.
    * `deletePrivilege`는 빈 문자열에 맵핑됩니다.
* 앱 B에서
    * `access-restricted`는 `PinCodeAttempts`에 맵핑됩니다.
    * `deletePrivilege`는 `UserLogin`에 맵핑됩니다.

>사용자의 범위 요소를 빈 문자열에 맵핑하려면 **새 범위 요소 맵핑 추가** 팝업 메뉴에서 보안 검사를 선택하지 마십시오.

![범위 맵핑](/images/scope_mapping.png)

필요한 구성을 사용하여 애플리케이션의 구성 JSON 파일을 수동으로 편집하고 변경사항을 MobileFirst Server로 다시 푸시할 수도 있습니다.

1. **명령행 창**에서 프로젝트의 루트 폴더로 이동하여 `mfpdev app pull`을 실행하십시오.
2. **[project-folder]\mobilefirst** 폴더에 있는 구성 파일을 여십시오.
3. `scopeElementMapping` 특성을 정의하여 파일을 편집하십시오. 이 특성에서 각각 선택한 범위 요소의 이름으로 구성된 데이터 쌍과 요소가 맵핑되는 0개 이상의 공백으로 구분된 보안 검사 문자열을 정의하십시오. 예:

   ```java
     "scopeElementMapping": {
         "UserAuth": "UserAuthentication",
         "SSOUserValidation": "LtpaBasedSSO CredentialsValidation"
     }
   ```
4. `mfpdev app push` 명령을 실행하여 업데이트된 구성 JSON 파일을 배치하십시오.

>또한 원격 서버에 업데이트 구성을 푸시할 수 있습니다. MobileFirst CLI 사용을 참조하여 MobileFirst 아티팩트 튜토리얼을 관리하십시오.

### 리소스 보호
{: #protecting-resources}

OAuth 모델에서 보호 리소스는 액세스 토큰이 필요한 리소스입니다. MobileFirst 보안 프레임워크를 사용하여 MobileFirst Server의 인스턴스에서 호스팅되는 리소스와 외부 서버의 리소스를 모두 보호할 수 있습니다. 리소스의 액세스 토큰을 얻는 데 필요한 권한을 정의하는 범위를 지정하여 리소스를 보호합니다.

다양한 방법으로 사용자의 리소스를 보호할 수 있습니다.

#### 필수 애플리케이션 범위
{: #mandatoryappscope}

애플리케이션 레벨에서, 애플리케이션이 사용하는 모든 리소스에 적용될 범위를 정의할 수 있습니다. 보안 프레임워크는 요청된 리소스 범위의 보안 검사 외에 이러한 검사(존재하는 경우)를 실행합니다.

>**참고**:
>* 필수 애플리케이션 범위는 보호되지 않은 리소스에 액세스할 때 적용되지 않습니다.
>* 리소스 범위에 부여되는 액세스 토큰에는 필수 애플리케이션 범위가 포함되지 않습니다.

MobileFirst Operations Console에 있는 탐색 사이드바의 **애플리케이션** 섹션에서 애플리케이션을 선택한 다음 **보안** 탭을 선택하십시오. **필수 애플리케이션 범위**에서 **범위에 추가**를 선택하십시오.

![필수 애플리케이션 범위](/images/mandatory-application-scope.png)

필요한 구성을 사용하여 애플리케이션의 구성 JSON 파일을 수동으로 편집하고 변경사항을 MobileFirst Server로 다시 푸시할 수도 있습니다.

1. **명령행 창**에서 프로젝트의 루트 폴더로 이동하여 `mfpdev app pull`을 실행하십시오.
2. **project-folder\mobilefirst** 폴더에 있는 구성 파일을 여십시오.
3. `mandatoryScope` 특성을 정의하고 특성 값을 선택한 범위 요소의 공백으로 구분된 목록을 포함하는 범위 문자열로 설정하여 파일을 편집하십시오. 예:

    ```java
        "mandatoryScope": "appAuthenticity PincodeValidation"
    ```
4. mfpdev app push 명령을 실행하여 업데이트된 구성 JSON 파일을 배치하십시오.

>또한 원격 서버에 업데이트 구성을 푸시할 수 있습니다.

#### 어댑터 리소스 보호
{: #protectadapterres}

어댑터에서 Java 메소드 또는 JavaScript 리소스 프로시저 또는 전체 Java 리소스 클래스의 보호 범위를 지정할 수 있습니다. 범위는 공백으로 구분된 하나 이상의 범위 요소 문자열(“scopeElement1 scopeElement2 …”)로 정의되거나 기본 범위를 적용하도록 널로 정의됩니다. 어댑터 리소스 보호에 관한 자세한 정보는 [어댑터 보호](/docs/services/mobilefoundation?topic=mobilefoundation-protecting_adapters#protecting_adapters)를 참조하십시오.

### 리소스 보호를 사용 안함으로 설정
{: #disablingresprotection}

다음 Java 및 JavaScript 섹션에 요약된 대로 특정 Java 또는 JavaScript 어댑터 리소스나 전체 Java 클래스의 기본 MobileFirst 리소스 보호를 사용 안함으로 설정할 수 있습니다. 리소스 보호를 사용 안함으로 설정하면 MobileFirst 보안 프레임워크에서 리소스에 액세스하는 데 토큰이 필요하지 않습니다.

#### Java 리소스 OAuth 보호를 사용 안함으로 설정
{: #disablejavaresoauthprotection}

Java 리소스 메소드 또는 클래스에 대해 OAuth 보호를 완전히 사용 안함으로 설정하려면 `@OAuthSecurity` 어노테이션을 리소스 또는 클래스 선언에 추가하고 `enabled` 요소의 값을 `false`로 설정하십시오.

```
@OAuthSecurity(enabled = false)
```

어노테이션의 `enabled` 요소의 기본값은 `true`입니다. `enabled` 요소가 `false`로 설정되면 `scope` 요소가 무시되며 리소스 또는 리소스 클래스가 보호되지 않습니다.

>**참고**: 보호되지 않은 클래스의 메소드에 범위를 지정하면 리소스 어노테이션의 `enabled` 요소를 `false`로 설정하지 않은 한 해당 메소드가 클래스 어노테이션에 관계없이 보호됩니다.

**예제**

다음 코드는 `helloUser` 메소드에 대해 리소스 보호를 사용 안함으로 설정합니다.

```java
    @GET
    @Path("/{username}")
    @OAuthSecurity(enabled = "false")
    public String helloUser(@PathParam("username") String name){
        ...
    }
```

다음 코드는 `MyUnprotectedResources` 클래스에 대해 리소스 보호를 사용 안함으로 설정합니다.

```java
    @Path("/users")
    @OAuthSecurity(enabled = "false")
    public class MyUnprotectedResources {
        ...
    }
```

#### JavaScript 리소스 OAuth 보호를 사용 안함으로 설정
{: #disablejavascriptresoauthprotection}

JavaScript 어댑터 리소스(프로시저)의 OAuth 보호를 완전히 사용 안함으로 설정하려면 다음과 같이 **adapter.xml** 파일에서 <procedure>의 `secured` 속성을 `false`로 설정하십시오.

```javascript
<procedure name="procedureName" secured="false">
```

`secured` 속성이 `false`로 설정되면 `scope` 속성이 무시되며 리소스가 보호되지 않습니다.

**예**

다음 코드에서는 `userName` 프로시저에 대해 리소스 보호를 사용 안함으로 설정합니다.

```javascript
<procedure name="userName" secured="false">
```

### 보호되지 않은 리소스 정의
{: #defunprotectedresources}

보호되지 않는 리소스는 액세스 토큰이 필요하지 않은 리소스입니다. MobileFirst 보안 프레임워크에서는 보호되지 않은 리소스에 대한 액세스는 관리하지 않으며 해당 리소스에 액세스하는 클라이언트 ID의 유효성을 검증하거나 확인하지 않습니다. 따라서 보호되지 않는 리소스에 대해서는 직접 업데이트, 디바이스 액세스 차단 또는 애플리케이션 사용 안함 원격 설정과 같은 기능이 지원되지 않습니다.

### 외부 리소스 보호
{: #protecextresources}

외부 리소스를 보호하기 위해 외부 리소스 서버에 액세스 토큰 유효성 검증 모듈과 함께 리소스 필터를 추가합니다. 토큰 유효성 검증 모듈에서는 리소스에 대한 OAuth 클라이언트 액세스 권한을 부여하기 전에 MobileFirst 액세스 토큰의 유효성을 검증하기 위해 보안 프레임워크 권한 부여 서버의 세부검사기 엔드포인트를 사용합니다. MobileFirst 런타임용 MobileFirst REST API를 사용하여 외부 서버의 고유 액세스 토큰 유효성 검증 모듈을 작성할 수 있습니다. 또는 외부 Java 리소스 보호용으로 제공된 MobileFirst 확장기능 중 하나를 사용하십시오.
