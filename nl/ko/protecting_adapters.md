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
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:xml: .ph data-hd-programlang='xml'}

# 어댑터 보호
{: #protecting_adapters}

어댑터에서는 다음 [Java](#protect-java-adapter-resources) 및 [JavaScript](#protect-javascript-adapter-resources) 섹션에 설명되어 있는 바와 같이 Java 메소드 또는 JavaScript 자원 프로시저, 또는 전체 Java 자원 클래스에 대해 보호 범위를 지정할 수 있습니다. 범위는 공백으로 구분된 하나 이상의 범위 요소 문자열(“scopeElement1 scopeElement2 …”)로 정의되거나 기본 범위를 적용하도록 널로 정의됩니다.

기본 MobileFirst 범위는 리소스에 액세스하는 데 액세스 토큰이 필요한 `RegisteredClient`이며, 리소스 요청이 MobileFirst Server에 등록된 애플리케이션에서 작성되었는지 확인합니다. 이 보호는 [자원 보호를 사용 안함으로 설정](#disabling-resource-protection)하지 않는 한 항상 적용됩니다. 따라서 자원은 해당 자원에 대해 범위를 설정하지 않아도 보호됩니다.

>**참고**: `RegisteredClient`는 예약된 MobileFirst 키워드입니다. 이 이름을 사용하여 사용자 정의 요소 또는 보안 검사를 정의하지 마십시오.
{.note}

### Java 어댑터 자원 보호
{: #protect-java-adapter-resources}

JAX-RS 메소드 또는 클래스에 보호 범위를 지정하려면 메소드 또는 클래스 선언에 `@OAuthSecurity` 어노테이션을 추가하고 어노테이션의 `scope` 요소를 원하는 범위로 설정하십시오. `YOUR_SCOPE`를 하나 이상의 범위 요소 문자열(“scopeElement1 scopeElement2 …”)로 바꾸십시오.

```java
@OAuthSecurity(scope = "YOUR_SCOPE")
```
{: codeblock}

클래스 범위는 고유의 `@OAuthSecurity` 어노테이션이 있는 메소드를 제외한 클래스 내의 모든 메소드에 적용됩니다.

>**참고**: `@OAuthSecurity` 어노테이션의 사용된 요소가 `false`로 설정된 경우 범위 요소는 무시됩니다. [Java 자원 보호를 사용 안함으로 설정](#disabling-java-resource-protection)을 참조하십시오.

**예제**

다음 코드는 `UserAuthentication` 및 `Pincode` 범위 요소를 포함하는 범위를 사용하여 `helloUser` 메소드를 보호합니다.

```java
@GET
@Path("/{username}")
@OAuthSecurity(scope = "UserAuthentication Pincode")
public String helloUser(@PathParam("username") String name){
    ...
}
```
{: codeblock}

다음 코드는 사전 정의된 `LtpaBasedSSO` 보안 검사를 사용하여 `WebSphereResources` 클래스를 보호합니다.

```java
@Path("/users")
@OAuthSecurity(scope = "LtpaBasedSSO")
public class WebSphereResources {
    ...
}
```
{: codeblock}

### JavaScript 어댑터 자원 보호
{: #protect-javascript-adapter-resources}

JavaScript 프로시저에 보호 범위를 지정하려면 **adapter.xml** 파일에서 <procedure> 요소의 범위 속성을 기본 범위로 설정하십시오. `PROCEDURE_NANE`은 프로시저 이름으로 바꾸고 `YOUR SCOPE`는 하나 이상의 범위 요소 문자열(“scopeElement1 scopeElement2 …”)로 바꾸십시오.

```javascript
<procedure name="PROCEDURE_NANE" scope="YOUR_SCOPE">
```
{: codeblock}

>**참고**: <procedure> 요소의 `secured` 속성이 false로 설정된 경우 `scope` 속성이 무시됩니다. [JavaScript 자원 보호를 사용 안함으로 설정](#disabling-javascript-resource-protection)을 참조하십시오.
{.note}

**예**

다음 코드는 `UserAuthentication` 및 `Pincode` 범위 요소를 포함하는 범위를 사용하여 `userName` 프로시저를 보호합니다.

```javascript
<procedure name="userName" scope="UserAuthentication Pincode">
```
{: codeblock}

## 어댑터 리소스 보호를 사용 안함으로 설정
{: #disabling-adapter-resource-protection}

다음 Java 및 JavaScript 섹션에 요약된 대로 특정 Java 또는 JavaScript 어댑터 리소스나 전체 Java 클래스의 [기본 MobileFirst 리소스 보호](#protecting_adapters_resources)를 사용 안함으로 설정할 수 있습니다. 리소스 보호를 사용 안함으로 설정하면 MobileFirst 보안 프레임워크에서 리소스에 액세스하는 데 토큰이 필요하지 않습니다.

### Java 리소스 보호를 사용 안함으로 설정
{: #disabling-java-resource-protection}

Java 리소스 메소드 또는 클래스에 대해 OAuth 보호를 완전히 사용 안함으로 설정하려면 `@OAuthSecurity` 어노테이션을 리소스 또는 클래스 선언에 추가하고 `enabled` 요소의 값을 `false`로 설정하십시오.

```java
@OAuthSecurity(enabled = false)
```
{: codeblock}

어노테이션의 `enabled` 요소의 기본값은 `true`입니다. `enabled` 요소가 `false`로 설정되면 `scope` 요소가 무시되며 리소스 또는 리소스 클래스가 보호되지 않습니다.

>**참고**: 보호되지 않은 클래스의 메소드에 범위를 지정하면 리소스 어노테이션의 `enabled` 요소를 `false`로 설정하지 않은 한 해당 메소드가 클래스 어노테이션에 관계없이 보호됩니다.
{.note}

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
{: codeblock}

다음 코드는 `MyUnprotectedResources` 클래스에 대해 리소스 보호를 사용 안함으로 설정합니다.

```java
    @Path("/users")
    @OAuthSecurity(enabled = "false")
    public class MyUnprotectedResources {
        ...
    }
```
{: codeblock}

### JavaScript 리소스 보호를 사용 안함으로 설정
{: #disabling-javascript-resource-protection}

JavaScript 어댑터 리소스(프로시저)의 OAuth 보호를 완전히 사용 안함으로 설정하려면 다음과 같이 **adapter.xml** 파일에서 <procedure>의 `secured` 속성을 `false`로 설정하십시오.

```javascript
<procedure name="procedureName" secured="false">
```
{: codeblock}

`secured` 속성이 `false`로 설정되면 `scope` 속성이 무시되며 리소스가 보호되지 않습니다.

**예**

다음 코드에서는 `userName` 프로시저에 대해 리소스 보호를 사용 안함으로 설정합니다.

```javascript
<procedure name="userName" secured="false">
```
{: codeblock}

## 보호되지 않는 리소스
{: #unprotected-resources}

보호되지 않는 리소스는 액세스 토큰이 필요하지 않은 리소스입니다. MobileFirst 보안 프레임워크에서는 보호되지 않은 리소스에 대한 액세스는 관리하지 않으며 해당 리소스에 액세스하는 클라이언트 ID의 유효성을 검증하거나 확인하지 않습니다. 따라서 보호되지 않는 리소스에 대해서는 직접 업데이트, 디바이스 액세스 차단 또는 애플리케이션 사용 안함 원격 설정과 같은 기능이 지원되지 않습니다.

