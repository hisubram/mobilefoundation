---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile foundation, integration, devops, ibmcloud, pipeline

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


# {{ site.data.keyword.cloud_notm }} {{ site.data.keyword.jazzhub_short }}를 사용하여 {{ site.data.keyword.mobilefoundation_short }} 앱 전달
{: #delivering_mobile_foundation_apps_using_ibm_cloud_devops_services}

이 튜토리얼은 {{ site.data.keyword.cloud_notm }}의 {{ site.data.keyword.jazzhub_title }}를 사용하여 IBM {{ site.data.keyword.mobilefoundation_short }}에 앱과 어댑터를 전달하는 작업을 자동화하는 데 도움을 줍니다.

다음 이미지는 파이프라인의 개요를 제공합니다.

![overview_of_pipeline](images/p00_overview_of_pipeline.png "DevOps 파이프라인의 여섯 단계")


## 전제조건
{: #mpfapps-devops-prerequisites }

* [{{ site.data.keyword.cloud_notm }}](https://cloud.ibm.com/registration) 계정
* [mfpdev-cli](https://www.npmjs.com/package/mfpdev-cli)
* 샘플 앱 및 [MFP 어댑터](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/)
* [GitHub](http://github.com/) 계정
* *선택사항:* [Bitbar](https://bitbar.com/testing/) 인스턴스 및 Bitbar API 키(요구사항에 따라 임의의 서비스를 사용할 수 있음)


## Continuous Delivery 서비스 및 도구 체인 작성
{: #creating-continuous-delivery-service-and-toolchain }

* {{ site.data.keyword.cloud_notm }} 카탈로그에서 "Continuous Delivery"를 검색하십시오(또는 [여기를 클릭](https://cloud.ibm.com/catalog/services/continuous-delivery)).
* 서비스 이름, 지역 등을 제공하여 서비스를 작성하십시오.

    다음 예에서는 서비스 이름을 *MFP App/Adapter delivery Test*로, 지역/위치를 *London*으로, 리소스 그룹을 *Default*로 사용합니다.

    ![configuring_continuous_delivery_service](images/p01_configuring_continuous_delivery_service.png "Mobile Foundation 서비스 인스턴스에 대한 카탈로그 작성 페이지")

* 탐색 메뉴에서 **DevOps**를 선택한 다음 **도구 체인 작성**을 클릭하고  "자체 도구 체인 빌드"를 검색하여 처음부터 새로 도구 체인을 작성하십시오.

    ![search_build_your_own_toolchain](images/p02_search_build_your_own_toolchain.png "자체 도구 체인 빌드에 대한 검색 결과를 사용하여 자체 도구 체인 페이지 작성")

* 구성할 도구 체인 이름, 지역 등을 제공하십시오.


## 버전 제어 및 파이프라인 트리거를 위해 GitHub를 도구 체인과 통합
{: #integrating-github-with-the-toolchain}

* 탐색의 **개요** 페이지에서 **도구 추가**를 클릭하고 GitHub를 검색하십시오.
* **GitHub 서버 주소**, **저장소 유형** 및 **저장소 URL**에 대해 GitHub 도구를 구성하십시오.
* 새 저장소를 작성하거나 기존 저장소를 분기, 복제 또는 사용할 수 있습니다.

    다음 예에서는 GitHub 서버를 "[https://github.com](http://github.com/)"으로, 저장소 유형을 *Existing*으로, 저장소 URL을 *https://github.com/sagar20896/mfp-devops-20181210030116092*로 사용합니다.

    ![configuring_toolchain](images/p03_configuring_toolchain.png "GitHub 서버, 저장소 유형 및 저장소 URL 필드를 표시하는 통합 구성 화면")

### 도구 체인에 Delivery Pipeline 추가
{: #adding-the-delivery-pipeline-to-the-toolchain}

**도구 추가**를 사용하여 *Delivery Pipeline*을 검색하십시오. 파이프라인을 구성하고 **통합 작성**을 클릭하십시오.

### 파이프라인에 단계 추가
{: #adding-stages-to-the-pipeline}

#### 1단계 - {{ site.data.keyword.mobilefoundation_short }} 설정

{: #stage1-setting-up-mobile-foundation}

이 단계에서는 {{ site.data.keyword.mobilefoundation_short }}의 인스턴스를 스핀업합니다. 파이프라인에 GitHub를 추가하는 작업이 이 단계에 있으며 변경사항이 git 저장소에 푸시될 때마다 파이프라인을 트리거합니다. 다음 단계는 GitHub 구성을 보여줍니다. 요구사항에 따라 단계 트리거를 설정할 수 있습니다. 수동 또는 자동일 수 있습니다.

다음 예에서는 입력 유형을 *Git 저장소*로, Git 저장소를 *mfp-devops-20181210030116092*로, Git URL을 *https://github.com/sagar20896/mfp-devops-20181210030116092*로, 분기를 *마스터*로 설정합니다.

![first_stage_git_input](images/p4_first_stage_git_input.png "입력 탭이 선택된 Mobile Foundation 설정 화면")

- **단계 추가**를 클릭하고 이미지에 표시된 대로 GitHub 저장소를 가리키도록 **입력** 탭을 구성하십시오.
- **작업** 탭에서 **작업 추가**를 클릭하고 *배치*를 작업 유형으로 선택하십시오. **배치자 유형**을 *Cloud Foundry*로 선택하십시오.
- API 키가 없는 경우 [여기](https://cloud.ibm.com/iam/#/apikeys)에서 {{ site.data.keyword.cloud_notm }} 계정에 대한 API 키를 작성할 수 있습니다.

필요에 따라 다른 필드를 선택하고 채우십시오. **배치 스크립트**에 다음 행을 추가하십시오.

```
	#!/bin/bash
	token=$(cf oauth-token | awk '{print $2}')
	json={\"deploy\":\"true\",\"name\":\"$INSTANCE_NAME\",\"token\":\"$token\"}
	echo "Input: $json"
	cf cs "Mobile Foundation" Developer $INSTANCE_NAME -c $json
	echo "Created Service Instance"
	sleep 1m
	cf create-service-key $INSTANCE_NAME Credentials-1
	echo "Created Service Keys"
	credentials=$(cf service-key $INSTANCE_NAME Credentials-1)
	echo "Credentials: $credentials"
	password=$(echo $credentials | grep \"password\": | awk -F "\"password\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	echo "Password: $password"
	url=$(echo $credentials | grep \"url\": | awk -F "\"url\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	echo "URL: $url"
	user=$(echo $credentials | grep \"user\": | awk -F "\"user\": " '{print $2 }' | awk -F , '{print $1 }' | awk -F \  '{print $1 }'| sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	echo "User: $user"
	until $(curl --output /dev/null --silent --head --fail $url/mfpconsole); do
	printf '.'
	sleep 5
	done
	# Deploying the confidential client
	curl -u $user:$password -X PUT -H "Content-Type: application/json" -d '{"clients":[{"id":"test","displayName":"Test","secret":"test","allowedScope":"*"},{"id": "admin","displayName": "admin","secret": "darwIn$hfr","allowedScope": "push.* mfp.admin.plugins settings.read"},{"id": "push","displayName": "push","secret": "darwIn$hfr","allowedScope": "authorization.introspect"}]}' $url/mfpadmin/management-apis/2.0/runtimes/mfp/confidentialclients
	exit
```
{: codeblock}

앞의 스크립트에서는 Cloud Foundry CLI를 사용하여 {{ site.data.keyword.mobilefoundation_short }} 서비스 인스턴스를 작성합니다.

![stage1_jobs_tab_config](images/p05_stage1_jobs_tab_config.png "작업 탭이 선택된 Mobile Foundation 설정 화면")

**환경 특성** 탭에서 *INSTANCE\_NAME*을 MobileFoundation 인스턴스 이름이 될 값(텍스트 특성)으로 추가하십시오. 이 특성은 여러 단계에서 ID로 사용됩니다.

![stage1_jobs_tab_config](images/p06_stage1_environment_properties.png "환경 특성 탭이 선택된 Mobile Foundation 설정 화면")

#### 2단계 - 어댑터 빌드
{: #stage2-building-an-adapter}

이 단계에서는 어댑터 소스 코드를 가져와서 빌드합니다.

- **단계 추가**를 클릭하고 환경 설정에 따라 GitHub 저장소를 입력으로 추가하기 위한 단계를 구성하십시오. **단계 트리거**는 *이전 단계 완료 시 작업 실행*으로 설정되어야 합니다.

- **작업** 탭으로 전환하여 빌드 작업을 추가하십시오. 빌더 유형을 *Maven*으로 선택하고 빌드 스크립트에 다음 행을 추가하십시오.

```
	#!/bin/bash
	# To use Node.js 6.7.0, uncomment the following line:
	export PATH=/opt/IBM/node-v6.7.0/bin:$PATH
	echo "#### npm install -g mfpdev-cli"
	npm install -g mfpdev-cli
	echo "#### mfpdev --version"
	mfpdev --version
	echo "#### cd adapters/JavaAdapter"
	cd adapters/JavaAdapter
	echo "#### Build the adapter : mfpdev adapter build"
	mfpdev adapter build
```
{: codeblock}

앞의 스크립트에서는 저장소의 `adapters/JavaAdapter`에서 [mfpdev-cli](https://www.npmjs.com/package/mfpdev-cli)를 설치하여 adapter 명령을 통해 어댑터를 빌드합니다.

다음 예에서는 **빌더 유형**을 *npm*으로 사용하고 빌드 스크립트에서 제공된 스크립트를 사용합니다. **작업 디렉토리**와 **빌드 아카이브 디렉토리** 매개변수는 비워 둡니다.

![build_adapter_stage_jobs_config](images/p07_build_adapter_stage_jobs_config.png "작업 탭이 선택된 BuildAdapter 화면")

#### 3단계 - 어댑터 배치
{: #stage3-deploying-an-adapter}

이 단계에서는 첫 번째 단계에서 작성한 {{ site.data.keyword.mobilefoundation_short }} 인스턴스에 어댑터를 배치합니다.

- 단계를 추가하십시오.
- 입력을 구성하십시오.
- 입력 유형을 **빌드 아티팩트**로 설정하고 **단계**를 어댑터가 빌드된 스테이지 이름으로 설정한 후 어댑터 빌드 단계에서 어댑터를 빌드하는 작업을 설정하십시오.
- **단계 트리거**는 *이전 단계 완료 시 작업 실행*으로 설정되어야 합니다.
- **작업** 탭에서 배치 작업을 추가하십시오.
- **배치자 유형**을 *Cloud Foundry*로 선택하고 환경 설정에 따라 나머지를 구성하십시오.

다음 예에서는 **배치자 유형**을 *Cloud Foundry*로, **{{ site.data.keyword.cloud_notm }} 지역**을 *댈러스*로 사용하며 첫 번째 단계에서 작성한 것과 동일한 API 키를 사용합니다.

![deploy_adapter](images/p08_deploy_adapter.png "작업 탭이 선택된 배치 화면")


아래의 **배치 스크립트**를 사용하십시오.

```
	#!/bin/bash
	set -x
	# To use Node.js 6.7.0, uncomment the following line:
	export PATH=/opt/IBM/node-v6.7.0/bin:$PATH

	echo "Getting server and credentials"
	# get mfpserver url, username and password

	      credentials=$(cf service-key $INSTANCE_NAME Credentials-1)
	 echo "Credentials: $credentials"
	 password=$(echo $credentials | grep \"password\": | awk -F "\"password\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	 echo "Password: $password"
	 url=$(echo $credentials | grep \"url\": | awk -F "\"url\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/' | sed -e 's/https/http/g')
	 echo "URL: $url"
	 user=$(echo $credentials | grep \"user\": | awk -F "\"user\": " '{print $2 }' | awk -F , '{print $1 }' | awk -F \  '{print $1 }'| sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	 echo "User: $user"

	url=$url:80
	echo "#### npm install -g mfpdev-cli"
	npm install -g mfpdev-cli
	echo "#### mfpdev --version"
	mfpdev --version
	echo "#### cd adapters/JavaAdapter"
	cd adapters/JavaAdapter
	echo "#### adding server definition"
	mfpdev server add server1 --url $url --login $user --password $password --setdefault
	echo "#### Deploy Adapter : mfpdev adapter deploy"
	mfpdev adapter deploy
```
{: codeblock}

이 스크립트는 mfpdev-cli의 adapter deploy 명령을 사용하여 MFP 인스턴스에 어댑터를 배치합니다.

- **환경 특성** 탭에서 *INSTANCE_NAME*을 {{ site.data.keyword.mobilefoundation_short }} 인스턴스가 작성될 때 첫 번째 단계에서 설정한 값과 동일한 값으로 설정하십시오. 어댑터가 첫 번째 단계에서 작성한 인스턴스에 배치되도록 동일한 인스턴스 이름을 사용합니다.


#### 4단계 - 어댑터 테스트
{: #stage4-test-adapter}

이 단계에서는 이전 단계에서 빌드되고 배치된 어댑터를 테스트합니다. 샘플 어댑터 저장소의 'adapters/JavaAdapter/tests'에 어댑터 엔드포인트를 테스트하기 위한 스크립트가 있습니다.

- **입력**을 빌드 어댑터의 구성과 동일한 *GitHub 저장소*로 설정하십시오.

- *작업* 탭에서 **배치 작업**을 추가하십시오. **배치자 유형**을 *Cloud Foundry*로 선택하십시오. 배치에 Cloud Foundry와 쉽게 통합할 수 있는 옵션이 있으므로 테스트 작업 대신 배치 작업을 사용합니다.

다음 스크립트를 사용하여 어댑터를 테스트하십시오.

```
	#!/bin/bash
	set -x
	# To use Node.js 6.7.0, uncomment the following line:
	export PATH=/opt/IBM/node-v6.7.0/bin:$PATH

	echo "Getting server and credentials"
	# get mfpserver url, username and password

	 credentials=$(cf service-key $INSTANCE_NAME Credentials-1)
	 echo "Credentials: $credentials"
	 password=$(echo $credentials | grep \"password\": | awk -F "\"password\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	 echo "Password: $password"
	 url=$(echo $credentials | grep \"url\": | awk -F "\"url\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/' | sed -e 's/https/http/g')
	 echo "URL: $url"
	 user=$(echo $credentials | grep \"user\": | awk -F "\"user\": " '{print $2 }' | awk -F , '{print $1 }' | awk -F \  '{print $1 }'| sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	 echo "User: $user"

	url=$url:80
	echo "#### npm install -g mfpdev-cli"
	npm install -g mfpdev-cli
	echo "#### mfpdev --version"
	mfpdev --version
	echo "#### adding server definition"
	mfpdev server add server1 --url $url --login $user --password $password --setdefault

	# Make sure to have the test confidential client. Try another attempt to deploy test confidential client
	curl -u $user:$password -X PUT -H "Content-Type: application/json" -d '{"clients":[{"id":"test","displayName":"Test","secret":"test","allowedScope":"*"},{"id": "admin","displayName": "admin","secret": "darwIn$hfr","allowedScope": "push.* mfp.admin.plugins settings.read"},{"id": "push","displayName": "push","secret": "darwIn$hfr","allowedScope": "authorization.introspect"}]}' $url/mfpadmin/management-apis/2.0/runtimes/mfp/confidentialclients

	echo "#### cd adapters/JavaAdapter/tests"
	cd adapters/JavaAdapter/tests
	echo "#### Testing Adapter Endpoints"
	./resource.sh
	./resource_greet.sh
	./resource_protected.sh
	./resource_testpost.sh
```
{: codeblock}

- 환경 특성에서 *INSTANCE_NAME*을 이전 단계에서 추가한 값과 동일한 값으로 추가하십시오.

API 테스트 프레임워크를 사용하여 어댑터를 테스트할 수 있습니다. 이 예에서는 어댑터를 호출하고 정확성을 테스트하는 쉘 스크립트가 있습니다.


#### 5단계 - Fastlane으로 앱 빌드
{: #stage5-building-apps-with-fastlane}

이 단계의 입력은 이전 단계에서 사용한 GitHub 저장소여야 합니다. 이 단계는 이전 단계(어댑터 테스트)가 통과된 후에 트리거되어야 합니다.

앱을 빌드하기 위해 **작업** 탭에서 배치 작업 템플리트를 사용합니다. *Cloud Foundry*를 **배치자 유형**으로 사용하십시오.

다음 스크립트를 사용하여 앱을 빌드하십시오.

```
	# To use Node.js 6.7.0, uncomment the following line:
	export LANG=en_US.UTF-8

	export PATH=/opt/IBM/node-v6.7.0/bin:$PATH

	echo "Getting server and credentials"
	# get mfpserver url, username and password

	 credentials=$(cf service-key $INSTANCE_NAME Credentials-1)
	 echo "Credentials: $credentials"
	 password=$(echo $credentials | grep \"password\": | awk -F "\"password\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	 echo "Password: $password"
	 url=$(echo $credentials | grep \"url\": | awk -F "\"url\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/' | sed -e 's/https/http/g')
	 echo "URL: $url"
	 user=$(echo $credentials | grep \"user\": | awk -F "\"user\": " '{print $2 }' | awk -F , '{print $1 }' | awk -F \  '{print $1 }'| sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	 echo "User: $user"

	url=$url:80
	echo "#### npm install -g mfpdev-cli"
	npm install -g mfpdev-cli
	echo "#### mfpdev --version"
	mfpdev --version
	echo "#### cd apps/ResourceRequestAndroid"
	cd apps/ResourceRequestAndroid
	#echo "#### adding server definition"
	mfpdev server add server1 --url $url --login $user --password $password --setdefault
	echo "#### Register App with MFP Server : mfpdev app register"
	mfpdev app register


	export JAVA_HOME=/opt/IBM/java8
	cd /home/pipeline

	# Android sdk
	wget https://dl.google.com/android/repository/sdk-tools-linux-3859397.zip
	sudo apt-get install unzip
	unzip /home/pipeline/sdk-tools-linux-3859397.zip
	echo 'y' | /home/pipeline/tools/bin/sdkmanager --licenses
	echo 'y' | /home/pipeline/tools/bin/sdkmanager "platform-tools" "platforms;android-26"

	# Prereq for installing Fastlane: Install RVM
	gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
	\curl -L https://get.rvm.io | bash -s stable --ruby
	source /home/pipeline/.rvm/scripts/rvm get stable --autolibs=enable
	gem -v

	# Install Fastlane
	gem install fastlane -NV

	# Build the apk file
	cd /home/pipeline/$BUILD_ID/apps/ResourceRequestAndroid
	fastlane beta

	mkdir /home/pipeline/temp
	cp ./app/build/outputs/apk/app-release.apk /home/pipeline/temp
	mkdir /home/pipeline/appupload

	# Push the generated apk for git hub for testing
	git config --global user.name $gitPushUser
	git config --global user.email $gitPushEmail
	git config --global push.default matching
	cd /home/pipeline/appupload

	git clone $appPublishUrl .
	ls
	rm -rf

	cp /home/pipeline/temp/app-release.apk .
	git add app-release.apk
	git commit -m "released a new version of apk - build : ($BUILD_ID)"
	echo $apkGitPushUrl
	git push $apkGitPushUrl
```
{: codeblock}

앞의 스크립트에서는 `mfpdev-cli`를 사용하여 앱을 빌드하고 릴리스하기 위해 앱을 {{ site.data.keyword.mobilefoundation_short }} [Fastlane](https://fastlane.tools/)에 등록합니다.

이 스크립트에서 사용되는 환경 변수는 다음 **환경 특성** 탭에 정의됩니다.

환경 특성에서 다음 특성을 추가하십시오.

- *INSTANCE_NAME* - **mfp 인스턴스 이름**
- *appPublishUrl* - 테스트 단계에서 가져올 수 있도록 앱이 공개되어야 하는 **저장소**
- *gitPushUser* - **GitHub 사용자 이름**
- *gitPushEmail* - **GitHub 사용자의 이메일**
- *gitPushToken* - **git 푸시 토큰**
- *apkGitPushUrl* - **https://$gitPushToken:x-oauth-basic@github.com/<path><SPACE><branch>**
   (예: `https://$gitPushToken:x-oauth-basic@github.com/ShinojEdakkara/mfp-apps master`)


#### 6단계 - Bitbar를 사용하여 앱 테스트
{: #stage6-test-the-app-using-bitbar}

이 단계도 조정 가능합니다. 요구사항에 따라 테스트 프레임워크를 사용할 수 있습니다. 이 예에서는 [Bitbar](https://bitbar.com/testing/)를 사용합니다.

- 보유하고 있는 테스트 프로젝트(이 프로젝트에는 테스트를 지원하는 데 사용되는 모든 테스트 스크립트 및 아티팩트가 포함될 수 있음)에 대한 GitHub 저장소 세부사항을 추가하십시오.

- Bitbar의 경우 **작업** 탭에서 빌드 작업을 추가하고 **빌더 유형**을 *Maven*으로 선택하십시오.

다음 스크립트와 유사한 빌드 스크립트를 사용하십시오.

```
	#!/bin/bash
	cd /home/pipeline/$BUILD_ID/appium/sample-scripts/java
	mvn clean install -X -Dtest=$test -DexecutionType=$executionType -DapiKey=$bitbarApiKey -DapplicationPath=$applicationPath -Dtestdroid_project=$testdroid_project
```
{: codeblock}

앞의 스크립트에는 몇 가지 환경 변수가 필요합니다.

- *screenshot\_dir* - **/home/pipeline/home/pipeline/$BUILD\_ID/target**
- *applicationPath* - 테스트할 애플리케이션에 대한 **GitHub 경로**
- *executionType* - **실행 유형**
- *test* - 수행할 **테스트**
- *bitbarApiKey* - **Bitbar API 키**
- *testdroid_project* - **프로젝트**


#### 7단계 - {{ site.data.keyword.mobilefoundation_short }} 삭제
{: #stage7-tearing-down-mobile-foundation}

이 단계에서는 이전 단계에서 빌드, 배치 및 테스트할 수 있도록 첫 번째 단계에서 작성된 {{ site.data.keyword.mobilefoundation_short }} 인스턴스를 삭제합니다.

이 단계에 대한 입력은 없습니다.

- *작업* 탭에서 **배치 작업**을 작성하십시오.
- 다음 스크립트를 사용하십시오.

```
	#!/bin/bash
	sleep 60
	cf dsk -f $INSTANCE_NAME-Analytics Credentials-1
	cf ds -f $INSTANCE_NAME-Analytics
	cf dsk -f $INSTANCE_NAME Credentials-1
	cf ds -f $INSTANCE_NAME
	cf d -f $INSTANCE_NAME-Server
	exit
```
{: codeblock}

- **환경 특성** 탭에서 *INSTANCE_NAME*을 {{ site.data.keyword.mobilefoundation_short }} 인스턴스가 작성될 때 첫 번째 단계에서 설정한 값으로 설정하십시오.
