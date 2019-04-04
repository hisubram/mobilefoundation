---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-28"

keywords: push notifications, notification, sending silent notifications

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

# 자동 알림
{: #silent_notifications}

자동 알림은 사용자에게 방해가 되지 않도록 경보를 표시하지 않는 알림입니다. 자동 알림이 도착하면 애플리케이션을 포그라운드로 가져오지 않고 애플리케이션 처리 코드가 백그라운드에서 실행됩니다. 현재 자동 알림은 버전 7 이상이 설치된 iOS 디바이스에서 지원됩니다. 버전 7 미만이 설치된 iOS 디바이스에 자동 알림이 전송되면 애플리케이션이 백그라운드에서 실행 중인 경우 알림이 무시됩니다. 애플리케이션이 포그라운드에서 실행 중인 경우에는 알림 콜백 메소드가 호출됩니다.
{: shortdesc}

## 자동 푸시 알림 전송
{: #sending-silent-push-notifications }

알림을 준비하고 전송하십시오. 자세한 정보는 [푸시 알림 전송](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)을 참조하십시오.

iOS에 대해 지원되는 세 가지 유형의 알림은 `DEFAULT`, `SILENT` 및 `MIXED` 상수로 표시됩니다. 유형이 명시적으로 지정되지 않은 경우에는 `DEFAULT` 유형으로 간주됩니다.

`MIXED` 유형 알림의 경우 백그라운드에서 앱이 자동 알림을 깨우고 처리하는 동안 메시지가 디바이스에 표시됩니다. `MIXED` 유형 알림에 대한 콜백 메소드는 두 번 호출됩니다(자동 알림이 디바이스에 도달할 때 한 번 호출되고 알림을 눌러 애플리케이션이 열릴 때 한 번 호출됨).

요구사항에 따라 **{{ site.data.keyword.mfp_oc_short_notm }} → [사용자 애플리케이션] → 푸시 → 알림 전송 → iOS 사용자 정의 설정** 아래에서 적절한 유형을 선택하십시오.

알림이 자동 알림이면 **경보**, **사운드** 및 **배지** 특성은 무시됩니다.
{: note}

![{{ site.data.keyword.mfp_oc_short_notm }}에서 iOS 자동 알림에 대한 알림 유형 설정](images/notification-type-for-silent-notifications.png)

## Cordova 애플리케이션에서 자동 푸시 알림 처리
{: #handling-silent-push-notifications-in-cordova-applications }

JavaScript 푸시 알림 콜백 메소드에서 다음 단계를 수행해야 합니다.

1. 알림 유형을 확인하십시오. 예를 들어 다음과 같습니다.

   ```javascript
   if(props['content-available'] == 1) {
        //Silent Notification or Mixed Notification. Perform non-GUI tasks here.
   } else {
        //Normal notification
   }
   ```
   {: codeblock}

2. 알림이 자동 또는 혼합 알림인 경우에는 백그라운드 작업을 완료한 후 `WL.Client.Push.backgroundJobDone` API를 호출하십시오.

## 네이티브 iOS 애플리케이션에서 자동 푸시 알림 처리
{: #handling-silent-push-notifications-in-native-ios-applications }

자동 알림을 수신하려면 다음 단계를 수행해야 합니다.

1. 원격 알림 수신 시 백그라운드 태스크를 수행하는 애플리케이션 기능을 사용으로 설정하십시오.
2. `content-available` 키가 **1**로 설정되어 있는지 확인하여 알림이 자동 알림인지 여부를 확인하십시오.
3. 알림 처리를 완료한 후 즉시 핸들러 매개변수의 블록을 호출해야 합니다. 그렇지 않으면 앱이 종료됩니다. 앱이 알림을 처리하고 지정된 완료 핸들러 블록을 호출하는 데는 최대 30초가 걸립니다.
