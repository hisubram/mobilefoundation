---

copyright:
  years: 2018
lastupdated: "2018-12-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:java: .ph data-hd-programlang='java'}
{:ruby: .ph data-hd-programlang='ruby'}
{:c#: .ph data-hd-programlang='c#'}
{:objectc: .ph data-hd-programlang='Objective C'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:php: .ph data-hd-programlang='PHP'}
{:swift: .ph data-hd-programlang='swift'}

# Cordova/웹 애플리케이션(Javascript)용 API

다음 표에는 Javascript 애플리케이션에서 수행할 수 있는 함수와 해당 API 메소드가 나열되어 있습니다.

| 함수 | 설명 |
|----------|-------------|
| `WL.Client`, `WL.App` | 애플리케이션 초기화 및 다시 로드, 애플리케이션 텍스트 세계화 |
| `WLAuthorizationManager` | 클라이언트 ID 및 권한 헤더 얻기 |
| `WL.Logger` | 환경에 대한 로그에 로그 메시지 인쇄 |
| `WL.NativePage` | 현재 표시된 웹 기반 화면을 기본적으로 작성된 페이지로 전환 |
| `WLResourceRequest` | 보호된 리소스 및 보호되지 않은 리소스로 요청 전송 |
| `WL.JSONStore` | 경량의 문서 중심 스토리지 시스템을 제공하는 클라이언트 측 API |

## 추가 정보
{: #additional-information }
### Options 오브젝트
{: #the-optional-object }
`options` 오브젝트에는 모든 메소드에 공통된 특성이 포함됩니다. 이 오브젝트는 {{ site.data.keys.mf_server }}에 대한 모든 비동기 호출에서 사용됩니다.

경우에 따라 특정 메소드에만 적용 가능한 특성으로 기능이 보강됩니다. 특정 메소드 설명의 일부로 이러한 특성에 대해 자세히 설명됩니다.

options 오브젝트의 공통 특성은 다음과 같습니다.

```javascript
options = {
    onSuccess: successHandler(response),
    onFailure: failureHandlder(response),
    invocationContext: invocation-context
};
```
{: code}
각 특성의 의미는 다음과 같습니다.

| 특성 | 설명 |
|----------|-------------|
| `onSuccess` | 선택사항입니다. 비동기 호출이 정상적으로 완료될 때 호출될 함수입니다. `onSuccess` 함수의 구문은 `success-handler-function(response)`입니다. 여기서 `response`는 최소한 다음 특성을 포함하는 오브젝트입니다. {::nomarkdown}<ul><li><b>invocationContext</b> - <code>options</code> 오브젝트에서 원래 {{ site.data.keys.mf_server }}에 전달된 <code>invocationContext</code> 오브젝트이거나 <code>invocationContext</code> 오브젝트가 전달되지 않은 경우 <code>undefined</code>입니다.</li><li><b>status</b> - HTTP 응답 상태</li></ul>{:/} **참고:** `response` 오브젝트에 추가 특성이 포함된 메소드의 경우 특정 메소드 설명의 일부로 이러한 특성에 대해 자세히 설명됩니다. |
| `onFailure` | 선택사항입니다. 비동기 호출이 실패한 경우 호출될 함수입니다. 이러한 실패에는 서버 연결 실패 또는 제한시간 초과된 호출과 같이 비동기 호출 중에 발생한 서버 측 오류 및 클라이언트 측 오류가 모두 포함됩니다. **참고:** 예외를 처리하여 실행을 중지하는 클라이언트 측 오류의 경우 이 함수가 호출되지 않습니다. onFailure 함수의 구문은 `failure-handler-function(response)`입니다. 여기서 `response`는 다음 특성을 포함하는 오브젝트입니다.{::nomarkdown}<ul><li><b>invocationContext</b> - <code>options</code> 오브젝트에서 원래 {{ site.data.keys.mf_server }}에 전달된 <code>invocationContext</code> 오브젝트이거나 <code>invocationContext</code> 오브젝트가 전달되지 않은 경우 <code>undefined</code>입니다.</li><li><b>errorCode</b> - 오류 코드 문자열입니다. 리턴될 수 있는 모든 오류 코드는 <b>worklight.js</b> 파일의 <code>WL.ErrorCode</code> 오브젝트에 상수로 정의됩니다.</li><li><b>errorMsg</b> - {{ site.data.keys.mf_server }}에서 제공되는 오류 메시지입니다. 이 메시지는 개발자 전용이며 사용자에게 표시되지 않아야 합니다. 이는 사용자의 언어로 번역되지 않습니다.</li><li><b>status</b> - HTTP 응답 상태</li></ul>{:/} **참고:** `response` 오브젝트에 추가 특성이 포함된 메소드의 경우 특정 메소드 설명의 일부로 이러한 특성에 대해 자세히 설명됩니다. |
| `invocationContext` | 선택사항입니다. 성공 및 실패 핸들러에 리턴되는 오브젝트입니다. `invocationContext` 오브젝트는 서비스에서 리턴될 때 호출 비동기 서비스의 컨텍스트를 유지하는 데 사용됩니다. 예를 들어, 동일한 성공 핸들러를 사용하여 연속적으로 `invokeProcedure` 메소드가 호출될 수 있습니다. 성공 핸들러가 처리되는 invokeProcedure에 대한 호출을 식별할 수 있어야 합니다. 한 가지 솔루션은 `invocationContext` 오브젝트를 정수로 구현하고 각 `invokeProcedure` 호출마다 해당 값을 1씩 증분시키는 것입니다. 성공 핸들러를 호출할 때 {{ site.data.keys.product_adj }} 프레임워크가 `invokeProcedure` 메소드와 연관된 options 오브젝트의 `invocationContext` 오브젝트에 전달합니다. `invocationContext` 오브젝트의 값을 사용하여 처리되는 결과가 연관된 `invokeProcedure`에 대한 호출을 식별할 수 있습니다. |

## WL.ClientMessages 오브젝트
{: #the-wlclientmessages-object }
`WL.ClientMessages` 오브젝트에 저장된 시스템 메시지의 목록을 보고 이러한 시스템 메시지의 번역을 사용으로 설정할 수 있습니다.

코드의 일부 파트는 애플리케이션이 성공적으로 초기화된 후에만 실행되므로 글로벌 JavaScript 레벨에서 시스템 메시지를 대체해야 합니다.
{: note}

`WL.ClientMessages` 오브젝트는 **worklight/messages/messages.json** 파일에 정의된 시스템 메시지를 저장하는 오브젝트입니다. 이 파일은 {{ site.data.keys.product }}을 생성한 프로젝트의 환경 폴더에 있습니다. 시스템 메시지의 번역을 사용으로 설정하려면 다음 코드 예제에 표시된 대로 `WL.ClientMessages` 오브젝트에서 이 메시지의 값을 대체해야 합니다.

```javascript
WL.ClientMessages.invalidUsernamePassword="The custom user name and password are not valid";
```
{: code}


* [JavaScript API 참조](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/#javascript-api-reference)
* [Objective-C API 참조(Cordova용)](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/#objective-c-api-reference-for-cordova)
{: note}
