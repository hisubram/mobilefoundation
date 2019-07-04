---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: disable apps, remote disabling of apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 원격으로 앱 버전을 사용 안함으로 설정
{: #remotely_disable_an_app_version}

이 섹션에서는 특정 모바일 운영 체제의 특정 애플리케이션 버전에 대한 사용자 액세스를 사용 안함으로 설정하는 방법과 사용자에게 사용자 정의 메시지를 제공하는 방법을 설명합니다.

Mobile Foundation Operations 콘솔을 사용하여 앱 액세스를 관리할 수 있습니다.

1. 콘솔 탐색 사이드바의 **애플리케이션** 섹션에서 애플리케이션 버전을 선택한 후 **관리** 탭을 선택하십시오.
2. 상태를 **액세스 사용 안함**으로 변경하십시오.
3. **최신 버전의 URL** 필드에서 새 애플리케이션 버전의 URL(일반적으로 해당 공용 또는 개인용 앱 스토어에 있음)을 제공하십시오.
   일부 환경에서는 Application Center에서 애플리케이션 버전의 세부사항 보기에 직접 액세스할 수 있도록 URL을 제공합니다. [애플리케이션 특성](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/appcenter/appcenter-console/#application-properties)을 참조하십시오.
   {: tip}

4. **기본 알림 메시지** 필드에 사용자가 애플리케이션에 액세스하려고 할 때 표시할 사용자 정의 알림 메시지를 추가하십시오. 다음 샘플 메시지는 사용자에게 최신 버전으로 업그레이드하도록 지시합니다.
   `This version is no longer supported. Please upgrade to the latest version.`
5. **지원되는 로케일** 섹션에서 선택적으로 다른 언어로 된 알림 메시지를 제공할 수 있습니다.
6. **저장**을 클릭하여 변경사항을 적용하십시오.

사용자가 원격으로 사용 안함으로 설정된 애플리케이션을 실행하면 구성된 사용자 정의 메시지가 포함된 대화 상자 창이 표시됩니다. 이 메시지는 보호된 리소스에 대한 액세스가 필요한 애플리케이션 상호작용이 발생하거나 애플리케이션에서 액세스 토큰을 획득하려고 하는 경우에 표시됩니다. 버전 업그레이드 URL을 제공한 경우 대화 상자에 기본 **닫기** 단추 이외에 새 버전으로 업그레이드하기 위한 **새 버전 가져오기** 단추가 표시됩니다. <br/>버전을 업그레이드하지 않고 대화 상자 창을 닫으면 보호되지 않은 애플리케이션 리소스에 대한 작업을 계속 수행할 수 있습니다. 그러나 보호된 리소스에 대한 액세스가 필요한 애플리케이션 상호작용이 발생하는 경우에는 대화 상자 창이 다시 표시되고 애플리케이션 또는 사용자에게 해당 리소스에 대한 액세스 권한이 부여되지 않습니다.
