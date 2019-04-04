---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-19"

keywords: mobile foundation security, authentication, using challenge handlers

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
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:xml: .ph data-hd-programlang='xml'}

# 인증 강화
{: #step_up_authentication}

리소스는 여러 보안 검사를 통해 보호할 수 있습니다. 이러한 시나리오에서 MobileFirst Server는 관련된 모든 인증 확인을 동시에 애플리케이션에 전송합니다.

보안 검사는 다른 보안 검사에도 종속될 수 있습니다. 따라서 인증 확인을 보내는 시기를 제어할 수 있는 것이 중요합니다.

예를 들어 이 튜토리얼에서는 사용자 이름과 비밀번호로 보호되는 두 개의 리소스가 있는 애플리케이션을 설명합니다. 여기서 두 번째 리소스에는 추가 PIN 코드도 필요합니다.

## 보안 검사 참조
{: #referencing-a-security-check}

두 개의 보안 검사 `StepUpPinCode` 및 `StepUpUserLogin`을 작성하십시오.

이 예에서 `StepUpPinCode`는 `StepUpUserLogin`에 따라 달라집니다. 사용자는 `StepUpUserLogin`에 성공적으로 로그인한 후에만 PIN 코드를 입력하도록 요청받아야 합니다. 이를 위해 `StepUpPinCode`가 `StepUpUserLogin` 클래스를 참조할 수 있어야 합니다.

MobileFirst 프레임워크에서는 참조를 삽입하도록 어노테이션을 제공합니다.

`StepUpPinCode` 클래스에서 클래스 레벨에 다음을 추가하십시오.

```java
@SecurityCheckReference
private transient StepUpUserLogin userLogin;
```
{: codeblock}

>**중요**: 같은 어댑터에서 두 보안 검사 구현을 모두 번들해야 합니다.
{: important}

이 참조를 해결하기 위해 프레임워크에서는 적절한 클래스가 있는 보안 검사를 검색하고 종속적 보안 검사에 참조를 삽입합니다.

동일한 클래스의 보안 검사가 둘 이상 있을 경우 어노테이션에는 선호하는 검사의 고유 이름을 지정하는 데 사용할 수 있는 선택적인 name 매개변수가 있습니다.

## 상태 머신
{: #state-machine}

`CredentialsValidationSecurityCheck`(`StepUpPinCode` 및 `StepUpUserLogin` 둘 다 포함)를 확장하는 모든 클래스는 단순 상태 머신을 상속합니다. 지정된 시점에 보안 검사는 다음 상태 중 하나일 수 있습니다.

* `STATE_ATTEMPTING`: 인증 확인이 전송되었고 보안 검사가 클라이언트 응답을 대기 중입니다. 시도 개수는 이 상태 동안 유지보수됩니다.
* `STATE_SUCCESS`: 인증 정보의 유효성이 검증되었습니다.
* `STATE_BLOCKED`: 최대 시도 횟수에 도달했고 검사가 잠금 상태에 있습니다.

현재 상태는 상속된 `getState()` 메소드를 사용하여 얻을 수 있습니다.

`StepUpUserLogin`에서 convenience 메소드를 추가하여 사용자가 현재 로그인 상태인지 확인하십시오. 이 메소드는 나중에 튜토리얼에서 사용됩니다.

```java
public boolean isLoggedIn(){
    return this.getState().equals(STATE_SUCCESS);
}
```
{: codeblock}

## Authorize 메소드
{: #the-authorize-method}

`SecurityCheck` 인터페이스는 `authorize`라는 메소드를 정의합니다. 이 메소드에서는 인증 확인 전송 또는 요청 유효성 검사 같은 보안 검사의 기본 로직 구현을 담당합니다.

`StepUpPinCode`에서 확장하는 `CredentialsValidationSecurityCheck` 클래스에는 이미 이 메소드의 구현이 포함되어 있습니다. 그러나 이 경우 목표는 `authorize` 메소드의 기본 동작을 시작하기 전에 StepUpUserLogin의 상태를 확인하는 것입니다.

그러려면 `authorize` 메소드를 **대체**하십시오.

```java
@Override
public void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response) {
    if(userLogin.isLoggedIn()){
        super.authorize(scope, credentials, request, response);
    }
}
```
{: codeblock}

이 구현에서는 `StepUpUserLogin` 참조의 현재 상태를 확인합니다.

* 상태가 `STATE_SUCCESS`(사용자가 로그인됨)인 경우 보안 검사의 정상 플로우가 계속됩니다.
* `StepUpUserLogin`이 다른 상태에 있으면 아무 작업도 수행하지 않습니다. 인증 확인이 전송되지 않으며 성공도 실패도 아닙니다.

리소스가 `StepUpPinCode` 및 `StepUpUserLogin` **둘 다**로 보호된다고 가정하여, 이 플로우는 2차 인증 정보(PIN 코드)를 프롬프트하기 전에 사용자가 로그인했는지 확인합니다. 클라이언트는 두 보안 검사가 활성화된 경우에도 동시에 두 인증 확인을 수신하지 않습니다.

또는 자원이 `StepUpPinCode`**로만** 보호되는 경우(프레임워크가 이 보안 검사만 활성화) `StepUpUserLogin`을 수동으로 트리거하기 위해 `authorize` 구현을 변경할 수 있습니다.

```java
@Override
public void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response) {
    if(userLogin.isLoggedIn()){
        //If StepUpUserLogin is successful, continue the normal processing of StepUpPinCode
        super.authorize(scope, credentials, request, response);
    } else {
        //In any other case, process StepUpUserLogin instead.
        userLogin.authorize(scope, credentials, request, response);
    }
}
```
{: codeblock}

## 현재 사용자 검색
{: #retrieve-current-user}

일부 데이터베이스에서 현재 사용자의 PIM 코드를 검색할 수 있도록 `StepUpPinCode` 보안 검사에서 이 사용자의 ID를 파악하려고 합니다.

`StepUpUserLogin` 보안 검사에서 **권한 부여 컨텍스트**에서 현재 사용자를 얻기 위해 다음 메소드를 추가하십시오.

```java
public AuthenticatedUser getUser(){
    return authorizationContext.getActiveUser();
}
```
{: codeblock}

`StepUpPinCode`에서 `userLogin.getUser()` 메소드를 사용하여 `StepUpUserLogin` 보안 검사에서 현재 사용자를 가져오고 이 특정 사용자에 대한 올바른 PIN 코드를 확인할 수 있습니다.

```java
@Override
protected boolean validateCredentials(Map<String, Object> credentials) {
    //Get the correct PIN code from the database
    User user = userManager.getUser(userLogin.getUser().getId());

    if(credentials!=null && credentials.containsKey(PINCODE_FIELD)){
        String pinCode = credentials.get(PINCODE_FIELD).toString();

        if(pinCode.equals(user.getPinCode())){
            errorMsg = null;
            return true;
        }
        else{
            errorMsg = "Wrong credentials. Hint: " + user.getPinCode();
        }
    }
    return false;
}
```
{: codeblock}

## 인증 확인 핸들러
{: #challenge-handlers}

클라이언트 측에서 여러 단계를 처리하는 특수 API는 없습니다. 대신 각 인증 확인 핸들러에서 자체 인증 확인을 처리합니다. 이 예에서 두 개의 별도 인증 확인 핸들러를 등록해야 합니다. 하나는 `StepUpUserLogin`에서 인증 확인을 처리하고 다른 하나는 `StepUpPincode`에서 인증 확인을 처리하는 데 사용합니다.
