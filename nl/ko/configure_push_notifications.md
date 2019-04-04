---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-28"

keywords: push notifications, notifications, FCM, GCM, APNS, WNS, authenticate notification

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


# 푸시 알림 구성
{: #configure_push_notifications}

푸시 알림을 iOS, Android 또는 Windows 디바이스에 전송하려면 먼저 FCM 세부사항(Android의 경우), APNS 인증서(iOS의 경우) 또는 WNS 인증 정보(Windows 8.1 Universal/Windows 10 UWP의 경우)를 사용하여 {{ site.data.keyword.mfserver_short_notm }}를 구성해야 합니다.
{: shortdesc}

그런 다음, 다음 옵션을 사용하여 알림을 전송할 수 있습니다.
* 모든 디바이스(브로드캐스트)
* 특정 태그에 등록된 디바이스
* 단일 디바이스 ID
* 사용자 ID
* iOS 디바이스만
* Android 디바이스만
* Windows 디바이스만
* 인증된 사용자를 기반으로

## 알림 설정
{: #setting-up-notifications }

알림 지원을 사용으로 설정하는 작업에는 {{ site.data.keyword.mfserver_short_notm }} 및 클라이언트 애플리케이션 모두에서의 여러 구성 단계가 포함됩니다. 서버 측 설정을 위해 계속 읽거나 [클라이언트 측 설정](#tutorials-to-follow-next)으로 이동하십시오.

서버 측에서 필요한 설정에는 필요한 벤더(APNS, FCM 또는 WNS) 구성 및 "push.mobileclient" 범위 맵핑이 포함됩니다.

### Firebase Cloud Messaging
{: #firebase-cloud-messaging }

Google은 [GCM을 더 이상 지원하지 않으며](https://developers.google.com/cloud-messaging/faq) 클라우드 메시징을 Firebase와 통합했습니다. GCM 프로젝트를 사용 중인 경우 [Android의 GCM 클라이언트 앱을 FCM으로 마이그레이션](https://developers.google.com/cloud-messaging/android/android-migrate-fcm)해야 합니다.
{: note}

Android 디바이스는 푸시 알림을 위해 FCM(Firebase Cloud Messaging) 서비스를 사용합니다.

FCM을 설정하려면 다음을 수행하십시오.

1. [Firebase 콘솔](https://console.firebase.google.com/?pli=1)을 방문하십시오.
2. 프로젝트를 작성하고 프로젝트 이름을 제공하십시오.
3. 설정 "톱니바퀴" 아이콘을 클릭하고 **프로젝트 설정**을 선택하십시오.
4. **클라우드 메시징** 탭을 클릭하여 **서버 API 키** 및 **발신인 ID**를 생성한 후 **저장**을 클릭하십시오.

[{{ site.data.keyword.mobilefirst_notm }} 푸시 서비스용 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_gcm_settings_put.html#Push-GCM-Settings--PUT-) 또는 [{{ site.data.keyword.mobilefirst_notm }} 관리 서비스용 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_gcm_settings_put.html#restservicesapi)를 사용하여 FCM을 설정할 수도 있습니다.
{: note}

조직에서 인터넷을 통한 트래픽을 제한하는 방화벽을 사용하는 경우에는 다음과 같은 단계를 수행해야 합니다.  
* FCM 클라이언트 앱이 메시지를 수신하기 위해 FCM과의 연결을 허용하도록 방화벽을 구성하십시오.
* 5228, 5229 및 5230 포트를 열어야 합니다. FCM은 일반적으로 5228만을 사용하지만 경우에 따라 5229 및 5230을 사용합니다.
* FCM은 특정 IP를 제공하지 않으므로 방화벽이 Google ASN 15169에 나열된 IP 블록에 포함된 모든 IP 주소에 대한 발신 연결을 허용하도록 해야 합니다.
* 포트 443에서 방화벽이 {{ site.data.keyword.mfserver_short_notm }}에서 fcm.googleapis.com으로의 발신 연결을 허용하는지 확인하십시오.
{: note }

<img class="gifplayer" alt="GCM 인증 정보 추가 이미지" src="images/gcm-setup.png"/>

### Apple Push Notifications Service
{: #apple-push-notifications-service }

iOS 디바이스는 푸시 알림을 위해 APNS(Apple Push Notification Service)를 사용합니다.  

APNS를 설정하려면 다음을 수행하십시오.

1. 개발 또는 프로덕션용 푸시 알림 인증서를 생성하십시오. 세부사항은 [여기](https://cloud.ibm.com/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1)에서 `iOS의 경우` 섹션을 참조하십시오.
2. {{ site.data.keyword.mfp_oc_short_notm }} → **[사용자 애플리케이션] → 푸시 → 푸시 설정**에서 인증서 유형을 선택하고 인증서의 파일 및 비밀번호를 제공하십시오. 그런 다음 **저장**을 클릭하십시오.

  * 푸시 알림을 전송하려면 {{ site.data.keyword.mfserver_short_notm }} 인스턴스에서 다음과 같은 서버에 액세스할 수 있어야 합니다.
    * 샌드박스 서버:
      * gateway.sandbox.push.apple.com:2195
      * feedback.sandbox.push.apple.com:2196
    * 프로덕션 서버:
      * gateway.push.apple.com:2195
      * Feedback.push.apple.com:2196
      * 1-courier.push.apple.com 5223
  * 개발 단계 중에는 apns-certificate-sandbox.p12 샌드박스 인증서 파일을 사용하십시오.
  * 프로덕션 단계 중에는 apns-certificate-production.p12 프로덕션 인증서 파일을 사용하십시오.
    * APNS 프로덕션 인증서는 이 인증서를 활용하는 애플리케이션이 Apple App Store에 제출된 경우에만 테스트할 수 있습니다.
    {: note }

MobileFirst는 유니버셜 인증서를 지원하지 않습니다.
{: note }

[{{ site.data.keyword.mobilefirst_notm }} 푸시 서비스용 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_apns_settings_put.html#Push-APNS-settings--PUT-) 또는 [{{ site.data.keyword.mobilefirst_notm }} 관리 서비스용 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_apns_settings_put.html?view=kc)를 사용하여 APNS를 설정할 수도 있습니다.
{: note}

<img class="gifplayer" alt="APNS 인증 정보 추가 이미지" src="images/apns-setup.png"/>

### Windows Push Notifications Service
{: #windows-push-notifications-service }

Windows 디바이스는 푸시 알림을 위해 WNS(Windows Push Notifications Service)를 사용합니다.  

WNS를 설정하려면 다음을 수행하십시오.

1. [Microsoft에서 제공된 지시사항](https://msdn.microsoft.com/en-in/library/windows/apps/hh465407.aspx)에 따라 **패키지 보안 ID(SID)** 및 **클라이언트 시크릿** 값을 생성하십시오.
2. {{ site.data.keyword.mfp_oc_short_notm }} → **[사용자 애플리케이션] → 푸시 → 푸시 설정**에서 이러한 값을 추가한 후 **저장**을 클릭하십시오.

> [{{ site.data.keyword.mobilefirst_notm }} 푸시 서비스용 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_wns_settings_put.html?view=kc) 또한 [{{ site.data.keyword.mobilefirst_notm }} 관리 서비스용 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_wns_settings_put.html?view=kc)를 사용하여 WNS를 설정할 수도 있습니다.

<img class="gifplayer" alt="WNS 인증 정보 추가 이미지" src="images/wns-setup.png"/>

### 범위 맵핑
{: #scope-mapping }

**push.mobileclient** 범위 요소를 애플리케이션에 맵핑하십시오.

1. {{ site.data.keyword.mfp_oc_short_notm }}을 로드하고 **[사용자의 애플리케이션] → 보안 → 범위-요소 맵핑**으로 이동하여 **새로 작성**을 클릭하십시오.
2. **범위 요소** 필드에 "push.mobileclient"를 쓰십시오. 그런 다음 **추가**를 클릭하십시오.

**추가적으로 사용 가능한 범위 목록**

**범위** | **설명**
---|---
apps.read |애플리케이션 리소스를 읽을 수 있는 권한입니다.
apps.write |애플리케이션 리소스를 작성, 업데이트 및 삭제할 수 있는 권한입니다.
gcmConf.read |GCM 구성 설정(API 키 및 SenderId)을 읽을 수 있는 권한입니다.
gcmConf.write |GCM 구성 설정을 업데이트 및 삭제할 수 있는 권한입니다.
apnsConf.read |APN 구성 설정을 읽을 수 있는 권한입니다.
apnsConf.write |APN 구성 설정을 업데이트 및 삭제할 수 있는 권한입니다.
devices.read |디바이스를 읽을 수 있는 권한입니다.
devices.write |디바이스를 작성, 업데이트 및 삭제할 수 있는 권한입니다.
subscriptions.read |구독을 읽을 수 있는 권한입니다.
subscriptions.write |구독을 작성, 업데이트 및 삭제할 수 있는 권한입니다.
messages.write |푸시 알림을 전송할 수 있는 권한입니다.
webhooks.read |이벤트 알림을 읽을 수 있는 권한입니다. 
webhooks.write |이벤트 알림을 전송할 수 있는 권한입니다.
smsConf.read |SMS 구성 설정을 읽을 수 있는 권한입니다. 
smsConf.write |SMS 구성 설정을 업데이트 및 삭제할 수 있는 권한입니다.
wnsConf.read |WNS 구성 설정을 읽을 수 있는 권한입니다.
wnsConf.write |WNS 구성 설정을 업데이트 및 삭제할 수 있는 권한입니다.

<img class="gifplayer" alt="범위 맵핑" src="images/scope-mapping.png"/>

### 인증된 알림
{: #authenticated-notifications }

인증된 알림은 하나 이상의 `사용자 ID`에 전송되는 알림입니다.  

**push.mobileclient** 범위 요소를 애플리케이션에 사용되는 보안 검사에 맵핑하십시오.  

1. {{ site.data.keyword.mfp_oc_short_notm }}을 로드하고 **[사용자 애플리케이션] → 보안 → 범위-요소 맵핑**으로 이동하여 **새로 작성**을 클릭하거나 기존 범위 맵핑 항목을 편집하십시오.
2. 보안 검사를 선택하십시오. 그런 다음 **추가**를 클릭하십시오.

  <img class="gifplayer" alt="인증된 알림" src="images/authenticated-notifications.png"/>

## 태그 정의
{: #defining-tags }
{{ site.data.keyword.mfp_oc_short_notm }} → **[사용자 애플리케이션] → 푸시 → 태그**에서 **새로 작성**을 클릭하십시오.  
적절한 `태그 이름` 및 `설명`을 제공한 후 **저장**을 클릭하십시오.

<img class="gifplayer" alt="태그 추가" src="images/adding-tags.png"/>

구독은 디바이스 등록 및 태그를 함께 결합합니다. 디바이스가 태그에서 등록 취소되면 연관된 모든 구독이 디바이스 자체에서 자동으로 구독 해지됩니다. 하나의 디바이스에 대한 여러 사용자가 있는 시나리오에서는 사용자 로그인 기준을 기반으로 모바일 애플리케이션에서 구독을 구현해야 합니다. 예를 들어, 사용자가 애플리케이션에 성공적으로 로그인한 후 구독 호출이 작성되고 구독 해지 호출은 로그아웃 조치 처리의 일부로 명시적으로 수행됩니다.

## 다음 튜토리얼
{: #tutorials-to-follow-next }

* [푸시 알림 전송](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)

이제 서버 측이 설정되었으므로 클라이언트 측을 설정하고 수신된 알림을 처리하십시오.

* [클라이언트 애플리케이션에서 푸시 알림 처리](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
