---

copyright:
  years: 2016, 2019
lastupdated:  "2019-02-12"

keywords: mobile foundation, mobile analytics, professional plan, configure database

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

#	Professional Per Device 플랜을 사용하여 설정
{: #using_mobilefoundation_p5}

Professional Per Device 플랜을 사용하면 사용자가 모바일 사용자 또는 디바이스의 수와 관계없이 프로덕션에서 모바일 애플리케이션을 빌드, 테스트 및 실행할 수 있습니다. 비용은 일별 클라이언트 디바이스의 수를 기반으로 합니다. 이 플랜은 대규모 배치 및 고가용성을 지원합니다.
{{site.data.keyword.mobilefoundation_short}}: Professional Per Device 서비스 인스턴스를 작성한 후 다음 프로시저를 읽고 서비스를 시작하십시오.

## Professional Per Device 플랜의 필수 소프트웨어
{: #prerequisites_p5}

{{site.data.keyword.mobilefoundation_short}}: Professional Per Device 서비스 인스턴스를 구성하기 전에 다음을 고려하십시오.
* {{site.data.keyword.mobilefoundation_short}}: Professional Per Device는 {{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} {{site.data.keyword.Bluemix_notm}} 플랜과 함께 사용하는 경우에만 지원됩니다.

* {{site.data.keyword.mobilefoundation_short}} 서비스 인스턴스의 설정을 구성할 수 있으려면 {{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} 서비스 인스턴스 인증 정보에 액세스할 수 있어야 합니다.

{{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} 서비스 인스턴스는 {{site.data.keyword.Bluemix_notm}} `Organization` 또는 액세스 권한이 있는 기타 `Organization`의 모든 `Space`에 존재할 수 있습니다. {{site.data.keyword.Db2_on_Cloud_short}} 또는 {{site.data.keyword.composeForPostgreSQL}} 서비스 인스턴스가 있는 `Space`에 액세스할 수 있는 권한이 있는지 확인하십시오.
{: note}

## 데이터베이스 연결 추가
{: #configure_dashdb_p5}

###  첫 번째 단계
{: #firststeps_p5}

{{site.data.keyword.mobilefoundation_short}}: Professional Per Device 서비스 인스턴스를 작성한 후 프로시저에 따라 시작하십시오.

### 데이터베이스 연결 설정
{: #connect_dashdb_p5}

{{site.data.keyword.mobilefoundation_short}}: Professional Per Device 서비스 인스턴스가 작성된 후에 {{site.data.keyword.mobilefoundation_short}} 서비스 인스턴스가 연결되어야 하는 {{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} 서비스 인스턴스에 대한 연결 정보를 지정하는 *개요* 페이지가 표시됩니다.

기존에 {{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} 서비스 인스턴스가 없는 경우에는 새로 작성할 수도 있습니다.

다음 단계에 따라 새로운 Db2 on Cloud 서비스 인스턴스를 작성하십시오.

1. *개요* 페이지에서 **새 서비스 작성** 섹션을 선택하십시오.

+ 고가용성 {{site.data.keyword.Db2_on_Cloud_short}} 서비스 인스턴스를 원하는 경우 **고가용성 구성** 옵션에서 `Yes`를 선택하십시오.

+ 플랜 세부사항을 검토하고 **작성**을 클릭하십시오.

새 {{site.data.keyword.Db2_on_Cloud_short}} 서비스 인스턴스가 작성되며, 전용 {{site.data.keyword.Db2_on_Cloud_short}} 인스턴스에 8GB RAM, 2개의 vCPU 및 500GB의 스토리지를 제공합니다.

다음 단계에 따라 기존 {{site.data.keyword.Db2_on_Cloud_short}} 서비스 인스턴스 또는 사용자가 작성한 {{site.data.keyword.Db2_on_Cloud_short}} 서비스 인스턴스에 연결하십시오.

1. {{site.data.keyword.Db2_on_Cloud_short}} 서비스 인스턴스가 있는 {{site.data.keyword.Bluemix_notm}} `Organization`을 선택하십시오.

+ 선택된 `Organization`에 사용 가능한 영역 목록에서 {{site.data.keyword.Db2_on_Cloud_short}} 서비스 인스턴스가 존재하는 {{site.data.keyword.Bluemix_notm}} `Space`를 선택하십시오.   
{{site.data.keyword.Db2_on_Cloud_short}} 서비스 인스턴스가 있는 `Organization` 및 `Space`가 나열되지 않는 경우에는 해당 `Organization` 및 `Space`의 구성원인지 여부를 확인하십시오. {{site.data.keyword.mobilefoundation_short}} 서비스가 {{site.data.keyword.Db2_on_Cloud_short}} 서비스의 인증 정보에 액세스하므로 조직 및 영역에 대한 *Developer* 역할 액세스 권한을 가져야 합니다.
{: note}
+ {{site.data.keyword.Db2_on_Cloud_short}} `Service Name` 및 `Credentials`를 선택하여 기존 {{site.data.keyword.Db2_on_Cloud_short}} 서비스 인스턴스에 연결하십시오.

+  지정된 {{site.data.keyword.Db2_on_Cloud_short}} 서비스 인스턴스에 대한 연결을 테스트하십시오.

+  **추가**를 클릭하십시오. 이 조치는 구성된 {{site.data.keyword.Db2_on_Cloud_short}} 데이터베이스 서비스 인스턴스에 필수 테이블을 작성합니다.

잠시 후에는 {{site.data.keyword.mobilefoundation_short}} 서비스를 시작하는 데 도움이 되는 튜토리얼과 동영상을 제공하는 `개요` 페이지에 액세스할 수 있습니다.

{{site.data.keyword.mobilefoundation_short}} 서비스 인스턴스에서 사용되도록 구성된 {{site.data.keyword.Db2_on_Cloud_short}} 서비스 인스턴스를 변경할 수 없습니다. 그러나 각 {{site.data.keyword.mobilefoundation_short}} 서비스 인스턴스가 선택된 {{site.data.keyword.Db2_on_Cloud_short}} 서비스 인스턴스 내에 자체 스키마를 작성하므로, 다중 {{site.data.keyword.mobilefoundation_short}} 서비스 인스턴스에 걸쳐 동일한 {{site.data.keyword.Db2_on_Cloud_short}} 서비스 인스턴스를 사용할 수 있습니다.
{: note}

## Professional Per Device 플랜을 사용하여 작성된 MobileFirst 서버 시작
{: #start_mobilefoundation_p5}

* 기본 설정으로 {{site.data.keyword.mfserver_short_notm}}를 시작하려면 **기본 서버 시작**을 클릭하십시오.

* 이 선택사항은 다음 설정으로 {{site.data.keyword.mfserver_long_notm}}를 작성합니다.
    -  각각 1GB 메모리가 있는 두 개의 노드. 이 크기는 개발, 일반적인 테스트 활동 및 소규모 프로덕션 워크로드에 적합합니다.

    -	`username`과 `password`가 자동으로 생성됩니다. 서버가 시작되고 실행되면 이에 액세스할 수 있습니다.

서버를 작성하는 프로세스가 시작됩니다. 이 프로세스는 약 10분 정도 소요되며, 메시지 창은 이 조작의 진행상태를 표시합니다. 완료되면 대시보드가 표시되며, 여기서 다음을 볼 수 있습니다.

  -	실행 중인 서버의 상태(상태, 크기).

  -	사용자가 사용할 수 있도록 서버 라우트가 작성됩니다. 모바일 애플리케이션에서 이 라우트를 사용하여 {{site.data.keyword.mfserver_short_notm}}에 연결하십시오.

  -	{{site.data.keyword.mfp_oc_short_notm}}에 액세스하기 위한 개인 `username` 및 `password`. `password`는 숨겨집니다. **비밀번호 표시** 아이콘을 클릭하면 볼 수 있습니다.

*	**콘솔 실행**을 클릭하여 {{site.data.keyword.mfp_oc_short_notm}}을 여십시오.


콘솔을 사용하여 모바일 앱, 어댑터 및 모바일 디바이스를 관리하고 서버를 모바일 백엔드로 사용할 수 있으며 푸시 알림 전송 등의 작업을 수행할 수 있습니다.

## Professional Per Device 플랜을 사용할 때 MobileFirst 서버 다시 작성
{: #recreate_mobilefoundation_p5}

*	**재작성**을 클릭하여 서버를 재작성하십시오.

* 이 조치는 기존 서버를 중지하고 데이터를 삭제합니다. 업데이트된 버전이 있는 경우 새 서버 인스턴스가 업데이트된 버전으로 작성됩니다. 이 조치를 완료하려면 수 분이 소요됩니다.

앱과 어댑터에 대한 정보를 포함하는 이전 서버 인스턴스의 데이터가 구성된 {{site.data.keyword.Db2_on_Cloud_short}} 서비스 인스턴스에서 지속됩니다. 이 데이터는 서버를 다시 작성하는 데 사용됩니다.
{: note}

##	Professional Per Device 플랜의 고급 구성 설정
{: #using_mfs_advanced_p5}

`개요` 페이지에서 **고급 구성으로 서버 시작**을 사용하면 고급 또는 사용자 정의 설정으로 서버를 작성할 수 있습니다. 또한 **설정** 탭을 클릭하여 서버 구성을 사용자 정의할 수 있도록 서버 설정을 업데이트할 수도 있습니다. {{site.data.keyword.mobilefoundation_short}}은 일부 고급 설정에 대한 액세스를 제공합니다.

*	**토폴로지** 탭에서 필요한 서버 크기와 서버 인스턴스의 수를 선택할 수 있습니다. 기본값인 1GB 서버는 개발 및 간단한 테스트에 충분합니다.
  - 사용자 요구사항에 따라 적절한 서버 크기를 선택하십시오.

  - **인스턴스**는 작성된 인스턴스의 수를 표시합니다.

## Professional Per Device 플랜의 Mobile Analytics
{: #mobile_analytics_p5}

Mobile Analytics 서버는 Mobile Foundation: Developer 플랜 서비스 인스턴스에 포함되고 사전 구성됩니다.

* {{site.data.keyword.mfp_oc_short_notm}}에서 Mobile Analytics 콘솔을 실행하십시오.

Mobile Analytics에 대한 자세한 정보는 [여기](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app){: new_window}를 참조하십시오.
