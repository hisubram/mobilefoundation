---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-13"

keywords: mobile foundation, integration, cloud object storage, COS, ibm cloud

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

# {{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}}와 함께 {{ site.data.keyword.cos_full_notm }} 사용
{: #using_ibm_cloud_object_storage_with_ibm_mobile_foundation}

{{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}}(MF)은 개인화된 코그너티브 컨텍스트를 지원하는 차세대 모바일 앱의 개발과 배치를 지원하기 위해 특별히 디자인된 엔터프라이즈급 기능을 제공합니다. {{ site.data.keyword.cos_full_notm }}(COS)는 구조화되지 않은 데이터에 대한 유연하고 비용 효율적이며 확장 가능한 클라우드 스토리지입니다. 이 방법 안내서에는 {{ site.data.keyword.mobilefoundation_short}}을 사용하는 모바일 애플리케이션이 ionic 애플리케이션을 통해 데이터를 {{ site.data.keyword.cos_full_notm }}에 연결하고 페치하거나 업로드할 수 있는 방법을 설명합니다. 이 방법 튜토리얼에 대한 ionic 애플리케이션, 어댑터 및 관련 파일은 [여기](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App)에서 사용할 수 있습니다.
{: shortdesc}


## 전제조건
{: #cos-prerequisites}

1. `npm install -g mfpdev-cli`를 실행하여 [mfpdev-cli](https://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_wl_cli_description.html)를 설치하십시오. 이 cli는 ionic 앱을 등록하고 어댑터를 MF 서버에 배치하는 데 사용됩니다. 또는 MF 서버 대시보드에서 이러한 활동을 수행할 수 있습니다.

2. 시스템에 [{{ site.data.keyword.cloud_notm}} CLI](https://console.bluemix.net/docs/cli/index.html#overview)를 설치하십시오.

3. `npm install -g ionic`을 실행하여 ionic cli를 설치하십시오.

4. `npm install -g cordova`를 실행하여 cordova를 설치하십시오.


## {{ site.data.keyword.mobilefoundation_short}} 서버 설정
{: #mobile-foundation-server-setup}

{{ site.data.keyword.mobilefoundation_short}} 서버는 {{ site.data.keyword.cloud_notm}}에서 설정됩니다. {{ site.data.keyword.mobilefoundation_short}} 서버의 {{ site.data.keyword.cloud_notm}} 인스턴스를 다음과 같이 설정하십시오.

* {{ site.data.keyword.cloud_notm}} 카탈로그에서 "{{ site.data.keyword.mobilefoundation_short}}"을 검색하십시오. **{{ site.data.keyword.mobilefoundation_short}}** 타일을 클릭하십시오.

    ![MFPCatalog](images/mfp_catalog.png)

* {{ site.data.keyword.mobilefoundation_short}} 서버 인스턴스에 적합한 이름을 입력하고 **작성**을 클릭하십시오.

    ![BMMFPNew](images/bmmfpnew.png)

* 새 MF 서버 인스턴스가 작성되고 로그인 인증 정보를 요청합니다.

    ![MFPLogin](images/mfp_login.png)

* MF 서버에 로그인하기 위한 인증 정보는 왼쪽 메뉴에 있는 **인증 정보** 탭에서 찾을 수 있습니다.

    ![MFPcredentials](images/mfp_credentials.png)

* 이러한 인증 정보를 제공하고 로그인하여 MF 대시보드에 진입하십시오.

    ![MFPDashboard](images/mfp_dashboard.png)


## Cloud Object Storage 설정
{: #cloud-object-storage-setup}

* {{ site.data.keyword.cloud_notm}} 카탈로그에서 "Cloud Object Storage"를 검색하십시오. **Object Storage** 타일을 클릭하십시오.

    ![카탈로그](images/catalog.png)

* COS 인스턴스에 이름을 제공하고 **작성**을 클릭하십시오.

    ![COS 작성](images/cos_create.png)

* 다음으로, 왼쪽 분할창 메뉴 옵션에서 **버킷**을 클릭하십시오. 버킷에 대한 적합한 이름(이 예에서는 버킷의 이름을 `sharedgallery`로 지정하도록 선택함)을 제공하고 **작성**을 클릭하십시오.

    ![버킷 작성](images/bucketcreate.png)

* 버킷이 작성된 후 버킷의 랜딩 페이지는 다음 이미지와 유사하며 데이터를 직접 업로드할 수 있는 옵션을 제공합니다.

    ![버킷 랜딩 페이지](images/bucketlanding.png)

* 여기서 [샘플](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App)의 **Short stories** 폴더에 제공된 세 개의 파일을 추가하십시오.


## MFP-COS Ionic 앱 및 Java 어댑터
{: #mfp-cos-ionic-app-and-java-adapter}

이 [git 저장소](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App)를 다운로드하거나 복제하십시오.
이 저장소는 두 개의 주요 컴포넌트로 구성됩니다.

1. MF Java 어댑터
2. ionic 모바일 애플리케이션

### mfpdev-cli 구성
{: #configuring-mfpdev-cli}

cli에 서버 세부사항을 추가하십시오. 명령 프롬프트에서 `mfpdev server add`를 실행하십시오.

```
? Enter the name of the new server profile:
```

서버의 이름을 제공하고 입력을 누르십시오. 이 샘플에서 제공되는 이름은 `mfpserver`입니다.

```
? Enter the fully qualified URL of this server:
```

서버의 URL을 입력하십시오. {{ site.data.keyword.cloud_notm}}의 MF 서버의 경우 서비스 인증 정보 탭에 URL이 포함되어 있습니다. {{ site.data.keyword.cloud_notm}}의 https MF 서버에 대한 포트는 443이며 http MF 서버 인스턴스에 대한 포트는 80입니다.

```
? Enter the MobileFirst Server administrator login ID: (admin)
```

`admin`을 입력하고 **Enter**를 누르십시오.

```
? Enter the MobileFirst Server administrator password:
```

서비스 인증 정보에서 사용 가능한 비밀번호를 입력하십시오.

```
? Save the administrator password for this server?: (Y/n)
```

환경 설정에 따라 *Y/N*를 입력하고 프롬프트에서 요청된 대로 세부사항을 입력하십시오.

```
? Enter the MobileFirst Server connection timeout in seconds: 30
```

기본 제한시간을 30초로 설정하십시오.

```
? Make this server the default?: (Y/n)
```

*Y*를 입력하고 **Enter**를 누르십시오.

***예상 출력***:

```
Verifying server configuration...
The following runtimes are currently installed on this server: mfp
Server profile 'mfpserver' added successfully.
```

### MF Java 어댑터 구성
{: #configuring-the-mf-java-adapter}

COS 인스턴스에 연결하려면 COS 인스턴스의 일부 세부사항이 `adapter.xml` 파일에 제공되어야 합니다. 다음 필드의 값을 제공하십시오.

1. **endpointURL**: 이 필드는 COS 오브젝트의 공용 엔드포인트 URL입니다. 이 URL은 COS 대시보드의 **버킷(왼쪽 메뉴 옵션에 있음) -> <your-bucket-name>(이 예에서는 `sharedgallery`) -> 구성 -> 엔드포인트 -> 공용**에서 찾을 수 있습니다.
2. **AuthToken**: 이 튜토리얼에서는 IAM 인증을 사용합니다.

Java 어댑터를 COS의 인스턴스에 연결하려면 IAM 또는 HMAC를 사용하는 인증이 필요합니다. 다음은 IAM 토큰을 가져오기 위한 단계입니다. IAM 및 HMAC 인증 프로세스에 대한 추가 세부사항을 보려면 [여기](https://cloud.ibm.com/docs/services/cloud-object-storage/api-reference/api-reference-buckets.html#bucket-operations#AuthenticationOptions)를 클릭하십시오.

#### {{ site.data.keyword.cloud_notm}} CLI를 사용하여 IAM Oauth 토큰 얻기
{: #obtaining-iam-oath-token-using-ibm-cloud-cli}

1. 먼저, API 키가 있는지 확인하십시오. [{{ site.data.keyword.cloud_notm}} Identity and Access Management](https://cloud.ibm.com/iam/#/users)에서 API 키를 가져오십시오.
2. CLI를 사용하여 {{ site.data.keyword.cloud_notm}} 플랫폼에 로그인하십시오.

  ```bash
	ibmcloud login --apikey <value>
  ```
  {: codeblock}
  출력은 다음 스니펫과 유사합니다.

	```text
	Authenticating...
	OK

	Targeted account <account-name> (<account-id>)

	Targeted resource group default

	API endpoint:     https://api.ng.bluemix.net (API version: 	2.75.0)
	Region:           us-south
	User:             <email-address>
	Account:          <account-name> (<account-id>)
	Resource group:   default
	```

3. {{ site.data.keyword.cloud_notm}} 계정의 모든 서비스 인스턴스를 가져오려면 CLI에서 다음 명령을 실행하십시오.

	```bash
  ibmcloud resource service-instances
  ```
  {: codeblock}

	예상 출력:

	```text
 	Retrieving service instances in resource group Default and all 	locations under account <account-name> as <email-address>...
	OK
	Name                                               Location     	State    Type               Tags
	<resource-instance-name>                           global       	active   service_instance
	```

4. COS 인스턴스의 세부사항을 가져오려면 다음 명령을 실행하십시오. <instance-name>은 COS 서비스의 이름(이 튜토리얼의 경우 `newObject`)입니다.

  ```bash
	ibmcloud resource service-instance <instance-name>
  ```
  {: codeblock}

	예상 출력:

	```text
	Retrieving service instance <sinstance-name> in resource group 	Default under account <account-name> as<email-address>...
	OK

	Name:                  <resource-instance-name>
	ID:                    <resource-instance-id>
	Location:                 global
	Service Name:          cloud-object-storage
	Resource Group Name:   Default
	State:                 active
	Type:                  service_instance
	Tags:
	```

5. IAM 토큰을 가져오려면 다음 명령을 실행하십시오.
  ```bash
	ibmcloud iam oauth-tokens
  ```

	예상 출력:

	```text
	IAM token:  Bearer <token>
	UAA token:  Bearer <refresh-token>
	```

*endpointURL* 및 *authToken* 값을 추가한 후 어댑터를 빌드하십시오. 명령 프롬프트에서 어댑터의 루트 폴더로 이동하여 다음을 실행하십시오.

```bash
mfpdev adapter build
```
{: codeblock}

`*.adapter` 파일이 `target` 폴더에 작성됩니다. 다음 명령을 실행하십시오.

```bash
mfpdev adapter deploy
```
{: codeblock}

어댑터가 MF 인스턴스에 배치됩니다.

또는 MF 서버 대시보드에서 어댑터를 배치할 수 있습니다. MF 서버 대시보드를 열고 왼쪽 메뉴에서 **어댑터 -> 새로 작성**을 클릭하여 다음 이미지에 표시된 페이지를 여십시오.

![MFPNewAdapterRegister](images/mfp_new_adapter_register.png)

그런 다음 **어댑터 배치**를 클릭하고 **target** 폴더의 `.adapter` 파일을 업로드하십시오.


### Ionic 앱 구성
{: #configuring-the-ionic-app}

앱에서 다음 단계를 수행하십시오.

1. Ionic 애플리케이션을 포함하는 폴더로 이동하십시오.

2. cordova MF 플러그인을 추가하십시오.

	```bash
  ionic cordova plugin add cordova-plugin-mfp
  ```

3. Android 또는 iOS 플랫폼을 추가하십시오.

  ```bash
	ionic cordova platform add android
  ```
	또는

	```bash
	ionic cordova platform add ios
	```

4. 다음을 실행하여 앱을 MF 서버에 등록하십시오.

  ```bash
	mfpdev app register
	```

	또는 MF 서버 대시보드에서 앱을 등록할 수 있습니다. MF 서버 대시보드를 열고 왼쪽 메뉴에서 **애플리케이션 -> 새로 작성**을 클릭하십시오.

	다음 이미지에 표시된 대로 페이지가 로드됩니다.

	![MFPNewAppRegister](images/mfp_new_app_register.png)

	요청된 세부사항을 입력하십시오. '애플리케이션 이름' 텍스트 상자에 애플리케이션의 이름을 지정하십시오. 필수 플랫폼을 선택하십시오.

	Android의 경우 **패키지** 텍스트 상자에 *애플리케이션 ID*를 입력합니다. 이 매개변수는 `AndroidManifest.xml`에 있는 Android 애플리케이션의 패키지입니다. **버전** 텍스트 상자 필드는 `AndroidManifest.xml`의 *versionName* 값으로 채워져야 합니다.

	iOS의 경우 **번들 ID** 텍스트 상자에 *애플리케이션 ID*(대소문자 구분)를 입력합니다. 이 매개변수는 iOS 애플리케이션의 `mfpclient.plist`에서 찾을 수 있습니다. **버전** 텍스트 상자 필드는 iOS 애플리케이션에 있는 `mfpclient.plist` 파일의 *version* 값으로 채워져야 합니다.
5. 변경사항을 추가된 환경에 적용하려면 `ionic cordova prepare`를 실행하십시오.
6. 다음을 실행하십시오.
  ```bash
	ionic cordova build android
  ```
	또는
  ```bash
	ionic cordova build ios
  ```
	위 명령을 실행하면 typescript 변경사항이 환경에 추가되었는지 확인할 수 있습니다.

7. 디바이스를 연결하거나 에뮬레이터 또는 시뮬레이터를 실행하고 다음 명령을 실행하십시오.

	```bash
  ionic cordova run android
	```
	또는
	```bash
	ionic cordova run ios
  ```

### Ionic 애플리케이션 탐색
{: #navigating-the-ionic-application}

Ionic 애플리케이션은 사용자가 작성한 COS 인스턴스에서 이전에 업로드된([Cloud Object Storage 설정](#cloud-object-storage-setup) 섹션의 마지막 단계) 짧은 스토리 목록을 표시합니다. 특정 스토리 옵션을 선택하면 선택한 스토리가 로드되어 표시됩니다. 또한 COS 인스턴스에 업로드되는 스토리를 추가하는 옵션이 제공됩니다.

초기 COS 오브젝트 목록은 다음 이미지와 유사합니다.

![이전 COS](images/cos_before.png)

애플리케이션의 홈 페이지에는 "모든 스토리지 가져오기" 또는 "스토리 추가"를 위한 옵션이 제공됩니다.

![앱 홈 화면](images/app-home-screen.png)

**모든 스토리 가져오기**를 클릭하면 COS에서 사용 가능한 스토리가 표시됩니다.

![앱 스토리 선택](images/app-select-story.png)

드롭 다운 메뉴에서 로드될 스토리를 선택하고 **로드**를 클릭하십시오.

![앱 스토리가 로드됨](images/app-story-loaded.png)

다음으로 **스토리 추가** 단추를 클릭하여 자체 스토리를 추가하십시오. 스토리지의 제목과 컨텐츠를 텍스트 영역에 입력하고 **추가**를 클릭하십시오.

![앱 추가 입력](images/app-add-input.png)

스토리가 추가되면 스토리가 성공적으로 추가되었다는 메시지가 표시됩니다.

![앱 스토리가 추가됨](images/app-story-added.png)

이제 애플리케이션에서 추가된 스토리도 COS 대시보드에 있습니다.

![추가된 COS](images/cos_added.png)


`ibmcloud iam oauth-tokens`를 사용하여 얻은 IAM oauth 토큰에는 만기 시간이 있으며 `403 - Forbidden` 예외와 함께 어댑터가 실패합니다. 따라서 앱이 예상대로 작동하려면 배치된 어댑터의 토큰이 유효한지 확인해야 합니다.
{: note}
