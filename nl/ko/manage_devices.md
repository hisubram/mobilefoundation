---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: device management

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 디바이스 관리
{: #manage_devices}

Mobile Foundation Operations 콘솔에서 디바이스 액세스 및 디바이스 상태를 관리할 수 있습니다. 디바이스는 Mobile Foundation 클라이언트 SDA에서 지정된 디바이스 ID라는 ID로 고유하게 식별됩니다. 디바이스에 대한 표시 이름을 설정할 수도 있습니다. 디바이스 ID 및 디바이스 표시 이름 필드가 모두 검색을 위해 색인화됩니다.

애플리케이션 개발자는 `WLClient` 클래스의 `setDeviceDisplayName` 메소드를 사용하여 디바이스 표시 이름을 설정할 수 있습니다. `WLClient` 문서는 [여기](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/)를 참조하십시오. Java 어댑터 개발자는 `com.ibm.mfp.server.registration.external.model MobileDeviceData` 클래스의 `setDeviceDisplayName` 메소드를 사용하여 디바이스 표시 이름을 설정할 수도 있습니다.
{: tip}

Mobile Foundation 서버는 는 서버에 액세스하는 모든 디바이스에 대한 상태 정보를 유지보수합니다.
가능한 상태 값은 다음과 같습니다.
* 활성
* 유실
* 도난됨
* 만료됨
* 사용 안함

디바이스의 기본 상태는 **활성**이며, 이는 이 디바이스로부터의 액세스가 차단되지 않음을 표시합니다. 상태를 **유실**, **도난됨** 또는 **사용 안함**으로 변경하여 디바이스에서 애플리케이션 리소스에 액세스하지 못하게 차단할 수 있습니다. 다시 액세스를 허용하려는 경우 언제든지 **활성** 상태를 복원할 수 있습니다.

**만료됨** 상태는 디바이스가 사전 구성된 비활동 기간에 도달한 후 Mobile Foundation 서버에서 설정되는 특수 상태입니다. 이 상태는 라이센스 추적에 사용되며 디바이스의 액세스 권한에는 영향을 미치지 않습니다. **만료됨** 상태의 디바이스가 서버에 다시 연결되면 해당 상태가 **활성**으로 복원되고 디바이스에 서버에 대한 액세스 권한이 부여됩니다.

## 디바이스 차단
{: #block_devices}

1. Mobile Foundation Operations 콘솔에서 **디바이스** 페이지로 이동하십시오.
2. **검색** 필드를 사용하여 디바이스와 연관된 사용자 ID를 제공하거나 디바이스의 표시 이름(디바이스에 설정된 경우)을 제공하여 디바이스를 검색하십시오.
3. 검색 결과에 리턴된 각 디바이스에 대해 디바이스 ID, 표시 이름, 디바이스 모델, 운영 체제 및 디바이스와 연관된 사용자 ID 목록을 볼 수 있습니다.
4. 디바이스 상태 열에 디바이스의 상태가 표시됩니다. 디바이스의 상태를 **유실**, **도난됨** 또는 **사용 안함**으로 변경하여 보호된 애플리케이션 리소스에 대한 해당 디바이스의 액세스를 차단할 수 있습니다.

   상태를 다시 **활성**으로 변경하면 원래 액세스 권한이 복원됩니다.
   {: note}


## 디바이스 등록 취소
{: #unregister_devices}

1. Mobile Foundation Operations 콘솔에서 **디바이스** 페이지로 이동하십시오.
2. **검색** 필드를 사용하여 디바이스와 연관된 사용자 ID를 제공하거나 디바이스의 표시 이름(디바이스에 설정된 경우)을 제공하여 디바이스를 검색하십시오.
3. 검색 결과에 리턴된 각 디바이스에 대해 디바이스 ID, 표시 이름, 디바이스 모델, 운영 체제 및 디바이스와 연관된 사용자 ID 목록을 볼 수 있습니다.
4. **조치** 열에서 *등록 취소*를 선택하십시오.

   디바이스를 등록 취소하면 디바이스에 설치된 모든 Mobile Foundation 애플리케이션의 등록 데이터가 삭제됩니다. 또한 디바이스 표시 이름, 디바이스와 연관된 사용자 목록 및 애플리케이션에서 이 디바이스에 대해 등록한 공용 속성이 삭제됩니다.
   {: note}


디바이스를 **차단**하려는 경우 해당 디바이스를 **등록 취소**하지 마십시오. 디바이스를 등록 취소하면 모든 디바이스 관련 데이터가 제거됩니다. 사용자가 동일한 디바이스를 사용하여 Mobile Foundation 서버에 액세스하려고 시도하면 디바이스를 등록하라는 프롬프트가 표시되고 등록하면 서버에 해당 디바이스의 새 디바이스 ID가 작성됩니다. 즉, 디바이스가 Mobile Foundation 서버에 대한 액세스 권한을 다시 얻습니다.
{: important}
