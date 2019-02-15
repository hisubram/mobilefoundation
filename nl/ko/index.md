---

copyright:
  years: 2016, 2019
lastupdated:  "2018-11-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

# 시작하기 튜토리얼
{: #gettingstartedtemplate}

{{site.data.keyword.mobilefoundation_long}}는 엔터프라이즈 모바일 앱을 개발, 테스트 및 실행할 수 있는 {{site.data.keyword.mfp_full}} 환경의 설정을 신속히 처리합니다. {{site.data.keyword.mobilefoundation_short}}은 Developer, Professional Per Device 및 Professional 1 Application과 같은 여러 서비스 플랜을 제공합니다.
{: shortdesc}

Professional 1 Application 플랜을 사용하면 지원되는 운영 플랫폼 중 하나에서 빌드된 단일 애플리케이션을 관리할 수 있습니다. 지원되는 운영 체제는 Android, iOS, Windows 또는 모바일 웹입니다. Developer 플랜은 개발과 테스트에 가장 적합합니다. [여기](https://console.bluemix.net/catalog/services/mobile-foundation)에서 사용 가능한 모든 플랜을 검토할 수 있습니다.

이 시작하기 튜토리얼을 통해 지원되는 플랜 중 하나를 사용하여 {{site.data.keyword.mobilefoundation_short}} 서비스 인스턴스를 작성할 수 있습니다. 그런 다음 애플리케이션을 등록할 수 있습니다. 등록된 애플리케이션을 다운로드 및 편집하고 어댑터를 배치한 후 마지막으로 애플리케이션을 테스트하십시오.

## 시작하기 전에
{: #prereqs}

{{site.data.keyword.Bluemix}} 계정 및 {{site.data.keyword.mobilefoundation_short}} 서비스의 인스턴스가 필요합니다.

## 1단계: {{site.data.keyword.mobilefoundation_short}} 서비스의 인스턴스 작성
{: #step1create}

1. {{site.data.keyword.Bluemix_notm}} **카탈로그**에서 [**{{site.data.keyword.mobilefoundation_short}}**](https://{domainName}/catalog/services/mobile-foundation)을 선택하십시오. 서비스 구성 화면이 열립니다.
2. 서비스 인스턴스에 이름을 지정하거나 미리 설정된 이름을 사용하십시오.
3. 서비스 인스턴스를 작성할 지역, 조직 및 영역을 선택하십시오.
4. **가격 플랜**을 선택하고 **작성**을 클릭하십시오.

## 2단계: 모바일 채널 빌드
{: #buildmobilechannel}

### {{site.data.keyword.mobilefoundation_short}}의 경우: Developer 플랜
{: #buildchanneldevplan}

{{site.data.keyword.mobilefoundation_short}}: Developer의 인스턴스를 작성한 후에는 다음 단계를 완료하여 모바일 채널 빌드를 시작할 수 있습니다.

* Mobile Foundation 서버에 즉시 액세스하여 작업할 수 있습니다.

  이 선택사항은 다음 설정으로 {{site.data.keyword.mfserver_long_notm}}를 작성합니다.
  *	1GB의 메모리. 이 크기는 개발, 간단한 테스트 활동 및 소규모 프로덕션 워크로드에 충분합니다.

  * CLI를 사용하여 Mobile Foundation 서버에 액세스하려면 IBM Cloud 콘솔의 왼쪽 탐색 분할창에서 **서비스 인증 정보**를 클릭할 때 사용 가능한 인증 정보가 필요합니다.

### {{site.data.keyword.mobilefoundation_short}}의 경우: Professional Per Device 플랜
{: #buildchannelprofdeviceplan}

{{site.data.keyword.mobilefoundation_short}}: Professional Per Device 서비스의 인스턴스를 작성한 후에는 다음 단계를 완료하여 모바일 채널의 빌드를 시작할 수 있습니다.

  1.  {{site.data.keyword.Bluemix_notm}}에서 기존 {{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} 서비스에 연결하십시오.

      1.  {{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} 서비스 인스턴스가 있는 {{site.data.keyword.Bluemix_notm}} `Organization`을 선택하십시오.

      + 선택한 `Organization`에서 사용 가능한 영역 목록에서 {{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} 서비스 인스턴스가 있는 {{site.data.keyword.Bluemix_notm}} `Space`를 선택하십시오.

      + {{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} `Service Name` 및 `Credentials`를 선택하여 기존 {{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} 서비스 인스턴스에 연결하십시오.

      + **연결 테스트**를 클릭하여 선택한 {{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} 서비스 인스턴스에 대한 연결을 테스트하십시오.

      + **추가**를 클릭한 후 선택한 {{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} 서비스에 대한 확인을 요청하는 팝업 창에서 **계속**을 클릭하십시오. 이 조치는 구성된 {{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} 데이터베이스 서비스 인스턴스에 필수 테이블을 작성합니다.

{{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} 연결을 {{site.data.keyword.mobilefoundation_short}} 인스턴스에 추가한 후에는 이를 변경할 수 없습니다.
      {: note} 
  2.  서버를 작성하고 시작하십시오.

      1. 기본 구성으로 {{site.data.keyword.mobilefirst_notm}} 서버 인스턴스를 작성하고 **기본 서버 시작**을 클릭하십시오.

      + 이 선택은 다음 설정으로 {{site.data.keyword.mfserver_long_notm}}를 프로비저닝합니다.
          - 각각 1GB 메모리가 있는 두 개의 노드. 이 크기는 개발, 일반적인 테스트 활동 및 소규모 프로덕션 워크로드에 충분합니다.

          -	`username`과 `password`가 자동으로 생성됩니다. 서버가 시작되고 실행되면 이에 액세스할 수 있습니다.

          서버를 작성하는 프로세스가 시작됩니다. 이 프로세스는 약 10분 정도 소요되며, 메시지 창은 이 조작의 진행상태를 표시합니다. 완료되면 대시보드가 표시되며, 여기서 다음을 볼 수 있습니다.
            -	실행 중인 서버의 상태(상태, 크기).
            -	사용자가 사용할 수 있도록 서버 라우트가 작성됩니다. 모바일 애플리케이션에서 이 라우트를 사용하여 {{site.data.keyword.mfserver_short_notm}}에 연결하십시오.
            -	{{site.data.keyword.mfp_oc_short_notm}}에 액세스하기 위한 개인 `username` 및 `password`. `password`는 숨겨집니다. **비밀번호 표시** 아이콘을 클릭하면 볼 수 있습니다.

      +	**콘솔 실행**을 클릭하여 {{site.data.keyword.mfp_oc_short_notm}}을 여십시오.      

      토폴로지, 보안 및 기타 서버 구성에 대해 고급 구성으로 {{site.data.keyword.mobilefirst_notm}} 서버 인스턴스를 작성하려면 **고급 구성으로 서버 시작**을 클릭하십시오. 자세한 정보는 [고급 구성 설정](c_using_mfs_p5.html#using_mfs_advanced_p5)을 참조하십시오.
      {: tip}

### {{site.data.keyword.mobilefoundation_short}}의 경우: Professional 1 Application 플랜
{: #buildchannelprof1appplan}

{{site.data.keyword.mobilefoundation_short}}: Professional 1 Application 서비스의 인스턴스를 작성한 후에는 다음 단계를 완료하여 모바일 채널의 빌드를 시작할 수 있습니다.

  1.  {{site.data.keyword.Bluemix_notm}}에서 기존 {{site.data.keyword.Db2_on_Cloud_long}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL_full}} 서비스에 연결하십시오.

      1.  {{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} 서비스 인스턴스가 있는 {{site.data.keyword.Bluemix_notm}} `Organization`을 선택하십시오.

      + 선택한 `Organization`에서 사용 가능한 영역 목록에서 {{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} 서비스 인스턴스가 있는 {{site.data.keyword.Bluemix_notm}} `Space`를 선택하십시오.

      + {{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} `Service Name` 및 `Credentials`를 선택하여 기존 {{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} 서비스 인스턴스에 연결하십시오.

      + **연결 테스트**를 클릭하여 선택한 {{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} 서비스 인스턴스에 대한 연결을 테스트하십시오.

      + **추가**를 클릭한 후 선택한 {{site.data.keyword.Db2_on_Cloud_short}} 또는 {{site.data.keyword.composeForPostgreSQL}} 서비스에 대한 확인을 요청하는 팝업 창에서 **계속**을 클릭하십시오. 이 조치는 구성된 {{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} 데이터베이스 서비스 인스턴스에 필수 테이블을 작성합니다.

      {{site.data.keyword.Db2_on_Cloud_short}}(**Lite** 플랜 이외의 모든 플랜) 또는 {{site.data.keyword.composeForPostgreSQL}} 연결을 {{site.data.keyword.mobilefoundation_short}} 인스턴스에 추가한 후에는 이를 변경할 수 없습니다.
      {: note}

  2.  서버를 작성하고 시작하십시오.

      1. 기본 구성으로 {{site.data.keyword.mobilefirst_notm}} 서버 인스턴스를 작성하고 **기본 서버 시작**을 클릭하십시오.

        `The basic server instance includes a single node, 1 GB of memory.`

      + `username` 및 `password`가 사용자를 위해 자동으로 생성됩니다. 서버가 시작되고 실행되면 이에 액세스할 수 있습니다.  

        서버를 작성하는 프로세스가 시작됩니다. 이 프로세스는 약 10분 정도 소요되며, 메시지 창은 이 조작의 진행상태를 표시합니다. 완료되면 대시보드가 표시되며, 여기서 다음을 볼 수 있습니다.
          -	실행 중인 서버의 상태(상태, 크기).
          -	사용자가 사용할 수 있도록 서버 라우트가 작성됩니다. 모바일 애플리케이션에서 이 라우트를 사용하여 {{site.data.keyword.mfserver_short_notm}}에 연결하십시오.
          -	{{site.data.keyword.mfp_oc_short_notm}}에 액세스하기 위한 개인 `username` 및 `password`. `password`는 숨겨집니다. **비밀번호 표시** 아이콘을 클릭하면 볼 수 있습니다.

      +  **콘솔 실행**을 클릭하여 {{site.data.keyword.mfp_oc_short_notm}}을 여십시오.  

      토폴로지, 보안 및 기타 서버 구성에 대해 고급 구성으로 {{site.data.keyword.mobilefirst_notm}} 서버 인스턴스를 작성하려면 **고급 구성으로 서버 시작**을 클릭하십시오. 자세한 정보는 [고급 구성 설정](c_using_mfs_p2.html#using_mfs_advanced_p2)을 참조하십시오.
      {: tip}

[Using the Mobile Foundation service to set up MobileFirst Server![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/bluemix/using-mobile-foundation/){: new_window}로 이동하여 {{site.data.keyword.mobilefoundation_short}}을 시작하는 방법에 대해 자세히 알아보십시오.
{: note}

## 3단계: {{site.data.keyword.mobilefoundation_short}}에 사용자의 애플리케이션 등록
{: #registerapp}

Mobile Foundation 서버 인스턴스를 작성하고 시작한 후에 다음 단계를 수행하여 Android 애플리케이션을 등록할 수 있습니다.

  1.  URL(`http://<your-server-host>:<server-port>/mfpconsole`)을 로드하여 {{site.data.keyword.mfp_oc_short_notm}}을 시작하십시오. 프로비저닝할 때 생성된 `username` 및 `password`를 사용하십시오.

  + {{site.data.keyword.mfp_oc_short_notm}} **대시보드**에서 **애플리케이션** 옆의 **새로 작성**을 클릭하십시오.

  + **애플리케이션 이름**으로 *MFPStarterAndroid*를 제공하십시오.

  + *Android*로 **플랫폼을 선택**하십시오.

  + **애플리케이션 ID**로 *com.ibm.mfpstarterandroid*를 제공하십시오.

  + **버전**으로 *1.0*을 입력하십시오.

  + **애플리케이션 등록**을 클릭하십시오.

## 4단계: 샘플 애플리케이션 다운로드
{: #downloadapp}

  1.  {{site.data.keyword.mfp_oc_short_notm}} **대시보드**의 **애플리케이션** 아래에서 **MFPStarterAndroid**를 선택하십시오.

  + **스타터 코드 가져오기**를 클릭하고 Android 애플리케이션 샘플 다운로드를 선택하십시오.

## 5단계: 샘플 애플리케이션 편집
{: #editapp}

  1. 이전 단계에서 Android Studio로 다운로드한 샘플 Android 앱을 가져오십시오.

  + Android Studio의 **프로젝트** 사이드바 메뉴에서 **app → java → com.ibm.mfpstarterandroid → ServerConnectActivity.java** 파일을 선택하십시오.

    * 다음 가져오기를 추가하십시오.
      ```java
      import java.net.URI;
      import java.net.URISyntaxException;
      import android.util.Log;
      ```
      {: codeblock}

    * `WLAuthorizationManager.getInstance().obtainAccessToken`에 대한 호출을 바꾸고 다음 코드 스니펫을 붙여넣으십시오.

        ```java
          WLAuthorizationManager.getInstance().obtainAccessToken("", new WLAccessTokenListener() {
            @Override
            public void onSuccess(AccessToken token) {
                System.out.println("Received the following access token value: " + token);
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        titleLabel.setText("Yay!");
                        connectionStatusLabel.setText("Connected to MobileFirst Server");
                    }
                });

                URI adapterPath = null;
                try {
                    adapterPath = new URI("/adapters/javaAdapter/resource/greet");
                } catch (URISyntaxException e) {
                    e.printStackTrace();
                }

                WLResourceRequest request = new WLResourceRequest(adapterPath, WLResourceRequest.GET);

                request.setQueryParameter("name","world");
                request.send(new WLResponseListener() {
                    @Override
                    public void onSuccess(WLResponse wlResponse) {
                        // Will print "Hello world" in LogCat.
                        Log.i("MobileFirst Quick Start", "Success: " + wlResponse.getResponseText());
                    }

                    @Override
            public void onFailure(WLFailResponse wlFailResponse) {
                        Log.i("MobileFirst Quick Start", "Failure: " + wlFailResponse.getErrorMsg());
                    }
                });
            }

            @Override
            public void onFailure(WLFailResponse wlFailResponse) {
                System.out.println("Did not receive an access token from server: " + wlFailResponse.getErrorMsg());
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        titleLabel.setText("Bummer...");
                        connectionStatusLabel.setText("Failed to connect to MobileFirst Server");
                    }
                });
            }
        });
        ```
        {: codeblock}  

## 6단계: 어댑터 배치
{: #deployadapter}

  1. 이 [어댑터 아티팩트](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/javaAdapter.adapter){: download}를 다운로드하고 **조치 → 어댑터 배치**를 사용하여 {{site.data.keyword.mfp_oc_short_notm}}에서 이를 배치하십시오.

## 7단계: 애플리케이션 테스트
{: #testapp}

  1. Android Studio의 **프로젝트** 사이드바 메뉴에서 **app → src → main → assets → mfpclient.properties** 파일을 선택하고 사용자의 MobileFirst Server 서버에 대한 올바른 값으로 `protocol`, `host` 및 `port` 특성을 편집하십시오.

   값은 일반적으로 https, *your-server-address* 및 443입니다.
   {: tip}

  2. Android Studio에서 **앱 실행**을 클릭하십시오.
     * 디바이스 에뮬레이터에서 시작된 앱을 볼 수 있습니다.
     * 애플리케이션에서 **MobileFirst Server Ping**을 클릭하면 `Connected to MobileFirst Server`가 표시됩니다.
     * 애플리케이션이 MobileFirst Server에 연결할 수 있으면 배치된 Java 어댑터를 사용하는 리소스 요청 호출이 발생합니다.
     * 그런 다음 어댑터 응답이 Android Studio의 LogCat 보기에 출력됩니다.


## 다음 단계
{: #nextsteps}

[Quick Start tutorials ![외부 링크 아이콘](../../icons/launch-glyph.svg "Quick Start tutorials")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/){: new_window}에 따라 더 많은 샘플 애플리케이션에 대해 작업하고 {{site.data.keyword.mobilefoundation_short}}의 작업을 탐색할 수 있습니다.

Quick Start에는 iOS, Android, 웹, Cordova, Windows, React Native, Ionic 및 Xamarin 앱에 대한 {{site.data.keyword.mobilefoundation_short}}의 작업을 설명하는 튜토리얼이 있습니다.

# 관련 링크
{: #rellinks  notoc}

## 관련 링크
{: #general notoc}

*	[IBM MobileFirst Platform Foundation V8.0.0 제품 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}
*	[IBM MobileFirst Platform Developer Center ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://mobilefirstplatform.ibmcloud.com){: new_window}
