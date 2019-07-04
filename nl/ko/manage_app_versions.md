---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: app versions, disabling apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 앱 버전 관리
{: #manage_app_versions}

Mobile Foundation 애플리케이션 관리 기능은 Mobile Foundation 서버 사용자 및 관리자에게 해당 애플리케이션의 사용자 및 디바이스 액세스 권한에 대한 세부 단위의 제어를 제공합니다.

Mobile Foundation 서버는 모바일 인프라에 액세스하려는 모든 시도를 추적하고 애플리케이션, 사용자 및 애플리케이션이 설치된 디바이스에 대한 정보를 저장합니다. 애플리케이션, 사용자 및 디바이스 간의 맵핑이 서버의 모바일 애플리케이션 관리 기능에 대한 기초를 형성합니다.

Mobile Foundation Operations 콘솔을 사용하여 리소스에 대한 액세스를 모니터하고 관리할 수 있습니다. 특정 애플리케이션 버전을 관리할 수도 있습니다.

1.  Mobile Foundation Operations 콘솔로 이동하여 **애플리케이션**을 클릭하고 관리할 애플리케이션을 선택한 후 표시되는 **버전** 목록에서 관심 있는 애플리케이션의 특정 버전을 선택하십시오.
    ![애플리케이션 버전 관리](images/app_version_management.png)

2. **관리** 탭 아래에 선택한 애플리케이션 버전에 대한 애플리케이션 상태를 설정하는 옵션이 표시됩니다. 지원되는 애플리케이션 버전 상태는 다음과 같습니다.
   * 활성
   * 활성 및 알림
   * 액세스 사용 안함
3. **애플리케이션 액세스 > 상태** 아래에서 *액세스 사용 안함* 옵션을 선택하여 애플리케이션 버전을 사용 안함으로 설정할 수 있습니다.
4. **직접 업데이트** 섹션에서 Cordova 애플리케이션에 대한 업데이트된 웹 리소스를 업로드하도록 구성할 수도 있습니다. 그러면 이 특정 애플리케이션 버전을 사용하여 Mobile Foundation 서버에 연결하는 사용자에게 직접 업데이트를 사용하여 해당 애플리케이션을 업데이트하는 옵션이 제공됩니다.
5. **조치** 메뉴에서 다음 옵션을 사용하여 선택한 애플리케이션 버전에 대해 다음 조치를 수행할 수도 있습니다.
   *  버전 삭제
   *  버전 복제
   *  버전 내보내기


디바이스 관리에 대한 자세한 정보는 [디바이스 관리](/docs/services/mobilefoundation?topic=mobilefoundation-manage_devices#manage_devices)를 참조하십시오.
원격으로 앱 버전을 사용 안함으로 설정하는 방법에 대해 자세한 정보는 [원격으로 앱 버전을 사용 안함으로 설정](/docs/services/mobilefoundation?topic=mobilefoundation-remotely_disable_an_app_version#remotely_disable_an_app_version)을 참조하십시오.
{: note}
