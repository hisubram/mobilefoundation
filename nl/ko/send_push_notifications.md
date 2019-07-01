---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: push notifications, notifications, sending notification, HTTP/2

subcollection:  mobilefoundation
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
{:reactnative: .ph data-hd-programlang='React Native'}
{:csharp: .ph data-hd-programlang='csharp'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}
{:xml: .ph data-hd-programlang='xml'}


# 푸시 알림 전송
{: #send_push_notifications}

{{ site.data.keyword.mfp_oc_short_notm }}에서 또는 REST API를 통해 푸시 알림을 전송할 수 있습니다.
{: shortdesc}

* {{ site.data.keyword.mfp_oc_short_notm }}을 사용하는 경우 두 가지 유형의 알림(태그 및 브로드캐스트)을 전송할 수 있습니다.
* REST API를 사용하는 경우 모든 양식의 알림(태그, 브로드캐스트 및 인증됨)을 전송할 수 있습니다.

## {{ site.data.keyword.mfp_oc_short_notm }}에서 푸시 알림 전송
{: #sending-push-notification-from-mobilefirst-operations-console }

단일 디바이스 ID, 단일 또는 여러 사용자 ID, iOS 디바이스나 Android 디바이스만 또는 태그를 구독하는 디바이스에 알림을 전송할 수 있습니다.

### 태그 알림
{: #tag-notifications }

태그 알림은 특정 태그를 구독하는 모든 디바이스를 대상으로 하는 알림 메시지입니다. 태그는 사용자의 관심 주제를 나타내며 선택된 관심사에 따라 알림을 수신할 수 있는 기능을 제공합니다.

{{ site.data.keyword.mfp_oc_short_notm }} → **[사용자 애플리케이션] → 푸시 → 푸시 전송 탭**의 **받는 사람** 탭에서 **태그별 디바이스**를 선택하고 **알림 텍스트**를 제공하십시오. 그런 다음 **전송**을 클릭하십시오.

<img class="gifplayer" alt="태그별 전송" src="images/sending-by-tag.png"/>

### 브로드캐스트 알림
{: #broadcast-notifications }

브로드캐스트 알림은 모든 구독 디바이스를 대상으로 하는 태그 푸시 알림의 한 양식입니다. 브로드캐스트 알림은 예약된 `Push.all` 태그(모든 디바이스에 대해 자동 작성됨)를 구독하여 푸시 사용 {{ site.data.keyword.mobilefirst_notm }} 애플리케이션에 대해 기본적으로 사용으로 설정됩니다. 프로그래밍 방식으로 `Push.all` 태그의 구독을 해지할 수 있습니다.

In the {{ site.data.keyword.mfp_oc_short_notm }} → **[사용자 애플리케이션] → 푸시 → 알림 전송 탭**의 **받는 사람** 탭에서 **모두**를 선택하고 **알림 텍스트**를 제공하십시오. 그런 다음 **전송**을 클릭하십시오.

<img class="gifplayer" alt="모두에게 전송" src="images/sending-to-all.png"/>

## REST API를 사용하여 푸시 알림 전송
{: #sending-push-notifications-using-rest-apis }

REST API를 사용하여 알림을 전송하는 경우 모든 양식의 알림(태그, 브로드캐스트 및 인증된 알림)을 전송할 수 있습니다.

알림을 전송하기 위해 REST 엔드포인트에 대한 POST를 사용하여 요청이 작성됩니다. `imfpush/v1/apps/<application-identifier>/messages`.  
다음은 예제 URL입니다.

```
https://myserver.com:443/imfpush/v1/apps/com.sample.PinCodeSwift/messages
```

모든 푸시 알림 REST API를 검토하려면 사용자 문서에서 [REST API 런타임 서비스 주제](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/c_restapi_runtime.html)를 참조하십시오.

### 알림 페이로드
{: #notification-payload }

요청에는 다음과 같은 페이로드 특성이 포함될 수 있습니다.

|페이로드 특성|정의
|--- | ---
|message |전송할 경보 메시지입니다. |
|settings |설정은 알림의 다양한 속성입니다. |
|target |대상 세트는 이용자 ID, 디바이스, 플랫폼 또는 태그일 수 있습니다. 대상 중 하나만 설정할 수 있습니다. |
|deviceIds |디바이스 ID로 표시되는 디바이스의 배열입니다. 이러한 ID를 보유한 디바이스가 알림을 수신합니다. 이는 유니캐스트 알림입니다. |
|notificationType |메시지를 전송하는 데 사용되는 채널(푸시 또는 SMS)을 표시하는 정수 값입니다. 허용되는 값은 1(푸시만), 2(SMS만) 및 3(푸시 및 SMS 모두)입니다. |
|platforms |디바이스 플랫폼의 배열입니다. 이러한 플랫폼에서 실행 중인 디바이스가 알림을 수신합니다. 원되는 값은 A(Apple/iOS), G(Google/Android) 및 M(Microsoft/Windows)입니다. |
|tagNames |태그 이름으로 지정되는 태그의 배열입니다. 이러한 태그를 구독하는 디바이스가 알림을 수신합니다. 태그 기반 알림의 경우 이 유형의 대상을 사용하십시오. |
|userIds |알림을 전송할 사용자 ID로 표시되는 사용자의 배열입니다. 이는 유니캐스트 알림입니다. |
|phoneNumber |디바이스를 등록하고 알림을 수신하는 데 사용되는 전화번호입니다. 이는 유니캐스트 알림입니다. |
{: caption="표 1. 페이로드 특성" caption-side="top"}

**푸시 알림 페이로드 JSON 예제**

```json
{
    "message" : {
    "alert" : "Test message",
  },
  "settings" : {
    "apns" : {
      "badge" : 1,
      "iosActionKey" : "Ok",
      "payload" : "",
      "sound" : "song.mp3",
      "type" : "SILENT",
    },
    "gcm" : {
      "delayWhileIdle" : ,
      "payload" : "",
      "sound" : "song.mp3",
      "timeToLive" : ,
    },
  },
  "target" : {
    // The following list is for demonstration purposes only - per the documentation only 1 target is allowed to be used at a time.
    "deviceIds" : [ "MyDeviceId1", ... ],
    "platforms" : [ "A,G", ... ],
    "tagNames" : [ "Gold", ... ],
    "userIds" : [ "MyUserId", ... ],
  },
}
```

**SMS 알림 페이로드 JSON 예제**

```json
{
  "message": {
    "alert": "Hello World from an SMS message"
  },
  "notificationType":3,
   "target" : {
     "deviceIds" : ["38cc1c62-03bb-36d8-be8e-af165e671cf4"]
   }
}
```

## 알림 전송
{: #sending-the-notification }

다양한 도구를 사용하여 알림을 전송할 수 있습니다. 테스트를 위해 **Postman**이 사용됩니다. 다음 단계는 설정하는 방법을 설명합니다.

1. [기밀 클라이언트를 구성하십시오](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients/).
  REST API를 통한 푸시 알림 전송에서는 공백으로 구분된 범위 요소 `messages.write` 및 `push.application.<applicationId>`를 사용합니다. 
  <img class="gifplayer" alt="기밀 클라이언트 구성" src="images/push-confidential-client.png"/>
2. [액세스 토큰을 작성하십시오](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients#obtaining-an-access-token).  
3. **http://localhost:9080/imfpush/v1/apps/com.sample.PushNotificationsAndroid/messages**에 대한 **POST** 요청을 작성하십시오.
  - 원격 {{ site.data.keyword.mobilefirst_notm }}를 사용 중인 경우 `hostname` 및 `port` 값을 사용자 고유의 값으로 대체하십시오.
  - 애플리케이션 ID 값을 사용자 고유의 값으로 업데이트하십시오.
4. 헤더를 설정하십시오.
    - `**Authorization**: Bearer eyJhbGciOiJSUzI1NiIsImp ...`
    - **Bearer** 뒤의 값을 2단계의 액세스 토큰 값으로 대체하십시오.
    ![권한 헤더](images/postman_authorization_header.png "권한 헤더")
5. 본문을 설정하십시오.
  - [알림 페이로드](#notification-payload) 헤더에 설명된 대로 해당 특성을 업데이트하십시오.
  - 예를 들어, **userIds** 속성이 있는 **target** 특성을 추가하여 등록된 특정 사용자에게 알림을 전송할 수 있습니다.
    ```json
    {
         "message" : {
             "alert" : "Hello World!"
         }
    }
    ```

    ![권한 본문](images/postman_json.png "권한 본문")

    **전송** 단추를 클릭하면 디바이스에 알림이 수신됩니다.

    ![샘플 애플리케이션의 이미지](images/notifications-app.png "모바일 디바이스의 푸시 알림")

## 알림 사용자 정의
{: #customizing-notifications }

알림 메시지를 전송하기 전에 다음과 같은 알림 속성을 사용자 정의할 수도 있습니다.  

{{ site.data.keyword.mfp_oc_short_notm }} → **[사용자 애플리케이션] → 푸시 → 태그 → 알림 전송 탭**에서 **iOS/Android 사용자 정의 설정** 섹션을 펼쳐서 알림 속성을 변경하십시오.

### Android
{: #android }

* 알림 사운드, 알림이 FCM 스토리지에 저장될 수 있는 기간, 사용자 정의 페이로드 등
* 알림 제목을 변경하려면 Android 프로젝트의 **strings.xml** 파일에 `push_notification_tile`을 추가하십시오.

### iOS
{: #ios }

* 알림 사운드, 사용자 정의 페이로드, 조치 키 제목, 알림 유형 및 배지 번호

  ![푸시 알림 사용자 정의](images/customizing-push-notifications.png "푸시 전송 탭이 선택된 MobileFirst Operations Console 푸시 페이지")

## APN 푸시 알림에 대한 HTTP/2 지원
{: #http2-support-for-apns-push-notifications}

APN(Apple Push Notification) 서비스는 HTTP/2 네트워크 프로토콜 기반의 새 API를 지원합니다. HTTP/2에 대한 지원은 다음을 포함하여 많은 이점을 제공합니다.

* 메시지 길이가 2KB에서 4KB로 증가하여 알림에 더 많은 컨텐츠를 추가할 수 있습니다.
* 클라이언트와 서버 간에 다중 연결이 필요 없으므로 처리량이 향상됩니다.
* 유니버셜 푸시 알림 클라이언트 SSL 인증서를 지원합니다.

>이제 {{ site.data.keyword.mobilefirst_notm }}의 푸시 알림이 레거시 TCP 소켓 기반 알림과 함께 HTTP/2 기반 APN 푸시 알림을 지원합니다.

### HTTP/2 사용
{: #enabling-http2}

JNDI 특성을 사용하여 HTTP/2 기반 알림을 사용으로 설정할 수 있습니다.
```xml
<jndiEntry jndiName="imfpush/mfp.push.apns.http2.enabled" value= "true"/>
```
{: codeblock}

JNDI 특성이 추가되면 레거시 TCP 소켓 기반 알림이 사용되지 않고 HTTP/2 기반 알림만 사용으로 설정됩니다.
{: note}

### HTTP/2에 대한 프록시 지원
{: #proxy-support-for-http2}

HTTP 프록시를 통해 HTTP/2 기반 알림을 전송할 수 있습니다. 프록시를 통한 알림 라우팅을 사용으로 설정하려면 [여기](#proxy-support)를 참조하십시오.

## 프록시 지원
{: #proxy-support }
프록시 설정을 사용하여 알림을 Android 및 iOS 디바이스에 전송하는 데 사용할 선택적 프록시를 설정할 수 있습니다. `push.apns.proxy.*` 및 `push.gcm.proxy.*` 구성 특성을 사용하여 프록시를 설정할 수 있습니다. 자세한 정보는 [{{ site.data.keyword.mfserver_short_notm }} 푸시 서비스의 JNDI 특성 목록](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/installation-configuration/production/server-configuration/#list-of-jndi-properties-for-mobilefirst-server-push-service)을 참조하십시오.

## 다음 단계
{: #next-tutorial-to-follow }
이제 서버 측이 설정되었으므로 클라이언트 측을 설정하고 다음 튜토리얼을 사용하여 수신된 알림을 처리하십시오.

* [클라이언트 애플리케이션에서 푸시 알림 처리](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
