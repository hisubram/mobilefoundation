---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-13"

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


# {{ site.data.keyword.cloud_notm }} {{ site.data.keyword.jazzhub_short }} を使用した {{ site.data.keyword.mobilefoundation_short }} アプリのデリバリー
{: #delivering_mobile_foundation_apps_using_ibm_cloud_devops_services}

このチュートリアルは、{{ site.data.keyword.cloud_notm }} 上で {{ site.data.keyword.jazzhub_title }} を使用して、IBM {{ site.data.keyword.mobilefoundation_short }} へのアプリおよびアダプターのデリバリーを自動化するために役立ちます。


以下の図は、パイプラインの概要を示しています。

![overview_of_pipeline](images/p00_overview_of_pipeline.png)


## 前提条件
{: #mpfapps-devops-prerequisites }

* [{{ site.data.keyword.cloud_notm }}](https://cloud.ibm.com/registration) アカウント
* [mfpdev-cli](https://www.npmjs.com/package/mfpdev-cli)
* アプリと [MFP アダプター](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/)のサンプル
* [GitHub](http://github.com/) アカウント
* **オプション:** [Bitbar](https://bitbar.com/testing/) インスタンスと Bitbar API キー (必要に応じて任意のサービスを使用できます)。


## 継続的デリバリー・サービスとツールチェーンの作成
{: #creating-continuous-delivery-service-and-toolchain }

* {{ site.data.keyword.cloud_notm }} カタログで「Continuous Delivery」を検索します (または[ここをクリック](https://cloud.ibm.com/catalog/services/continuous-delivery)します)。
* サービス名、地域、その他を指定して、サービスを作成します。

    以下の例では、サービス名に「MFP App/Adapter delivery Test」、地域/場所に「ロンドン」、リソース・グループに「デフォルト」を使用します。

    ![configuring_continuous_delivery_service](images/p01_configuring_continuous_delivery_service.png)

* 左側のハンバーガー・メニュー内の {{ site.data.keyword.jazzhub_title }} セクションで、ツールチェーンを作成し、「Build your own ツールチェーン」を検索してツールチェーンを最初から作成します。


    ![search_build_your_own_toolchain](images/p02_search_build_your_own_toolchain.png)

* ツールチェーン名、地域、その他を指定して構成します。


## バージョン管理およびパイプライン・トリガーのための GitHub とツールチェーンの統合
{: #integrating-github-with-the-toolchain}

* 左側メニューにあるツールチェーンの概要で、**「ツールの追加」**をクリックして、GitHub を検索します。
* GitHub ツールの **GitHub サーバー・アドレス**、**リポジトリー・タイプ**、および**リポジトリー URL** を構成します。
* 新しいリポジトリーを作成したり、既存のリポジトリーをフォーク、複製、または使用したりすることができます。

    以下の例では、GitHub サーバーに「[https://github.com](http://github.com/)」、リポジトリー・タイプに「Existing」、リポジトリー URL に「https://github.com/sagar20896/mfp-devops-20181210030116092」を使用します。

    ![configuring_toolchain](images/p03_configuring_toolchain.png)

### ツールチェーンへのデリバリー・パイプラインの追加
{: #adding-the-delivery-pipeline-to-the-toolchain}

**「ツールの追加」**を使用して、*Delivery Pipeline* を検索します。パイプラインを構成して、**「統合の作成」**をクリックします。

### パイプラインへのステージの追加
{: #adding-stages-to-the-pipeline}

#### ステージ 1 - {{ site.data.keyword.mobilefoundation_short }} のセットアップ

{: #stage1-setting-up-mobile-foundation}

このステージでは、{{ site.data.keyword.mobilefoundation_short }} のインスタンスをスピンアップします。パイプラインへの GitHub の追加はこのステージで行われ、変更内容が Git リポジトリーにプッシュされるときには常にパイプラインがトリガーされます。以下のステップは、GitHub 構成を示しています。必要に応じてステージ・トリガーを設定できます。手動または自動にすることができます。

以下の例では、入力タイプを *Git repository*、Git リポジトリーを *mfp-devops-20181210030116092*、Git URL を *https://github.com/sagar20896/mfp-devops-20181210030116092*、ブランチを *master* に設定します。

![first_stage_git_input](images/p4_first_stage_git_input.png)

- **「ステージの追加」**をクリックして、図に示されているように、GitHub リポジトリーを指すように**「入力」**タブを構成します。
- **「ジョブ」**タブで**「ジョブの追加」**をクリックして、ジョブ・タイプに*「デプロイ」*を選択します。**「デプロイヤー・タイプ」**に*「Cloud Foundry」*を選択します。
- API キーがない場合は、{{ site.data.keyword.cloud_notm }} アカウント用に[ここで](https://cloud.ibm.com/iam/#/apikeys)作成できます。

必要に応じて、他のフィールドを選択するか、そこに値を入力します。以下の行を**デプロイ・スクリプト**に追加します。

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

上記のスクリプトでは、Cloud Foundry CLI を使用して {{ site.data.keyword.mobilefoundation_short }} サービス・インスタンスを作成します。

![stage1_jobs_tab_config](images/p05_stage1_jobs_tab_config.png)

**「環境プロパティー」**タブで、プロパティー *INSTANCE\_NAME* を MobileFoundation インスタンスに付ける名前で (テキスト・プロパティーとして) 追加します。これは多くのステージで ID として使用されます。

![stage1_environment_properties](images/p06_stage1_environment_properties.png)

#### ステージ 2 - アダプターの構築
{: #stage2-building-an-adapter}

このステージでは、アダプター・ソース・コードをプルして、それを構築します。

- **「ステージの追加」**をクリックして、GitHub リポジトリーを入力として追加するためのステージを、各自の好みに合わせて構成します。**「ステージ・トリガー」**は*「前のステージが完了したらジョブを実行」*に設定する必要があります。

- **「ジョブ」**タブに切り替えて、構築ジョブを追加します。ビルダー・タイプに *Maven* を選択し、ビルド・スクリプトに以下の行を追加します。

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

上記のスクリプトでは、[mfpdev-cli](https://www.npmjs.com/package/mfpdev-cli) をインストールして、リポジトリーの `adapters/JavaAdapter` でアダプター・コマンドを使用することにより、アダプターを構築します。

以下の例では、**ビルダー・タイプ**に *npm* を使用し、ビルド・スクリプトで提供されるスクリプトを使用します。**「作業ディレクトリー」**パラメーターと**「ビルド・アーカイブ・ディレクトリー」**パラメーターは空のままにします。

![build_adapter_stage_jobs_config](images/p07_build_adapter_stage_jobs_config.png)

#### ステージ 3 - アダプターのデプロイ
{: #stage3-deploying-an-adapter}

このステージでは、最初のステージで作成した {{ site.data.keyword.mobilefoundation_short }} インスタンスにアダプターをデプロイします。

- ステージを追加します。
- 入力を構成します。
- 入力タイプを**「ビルド成果物」**に設定し、**「ステージ」**をアダプターが構築されるステージの名前に設定し、ビルド・アダプター・ステージでアダプターを構築するジョブを設定します。
- **「ステージ・トリガー」**は*「前のステージが完了したらジョブを実行」*に設定する必要があります。
- **「ジョブ」**タブからデプロイ・ジョブを追加します。
- **「デプロイヤー・タイプ」**に*「Cloud Foundry」*を選択し、他の項目は各自の好みに合わせて構成します。

以下の例では、**「デプロイヤー・タイプ」**に*「Cloud Foundry」*、**「{{ site.data.keyword.cloud_notm }} 地域」**に*「ダラス」*、API キーに最初のステージで作成されたものと同じ値を使用します。

![deploy_adapter](images/p08_deploy_adapter.png)


以下の**「デプロイ・スクリプト」**を使用します。

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

このスクリプトは、mfpdev-cli のアダプター・デプロイ・コマンドを使用して、アダプターを MFP インスタンスにデプロイします。

- **「環境プロパティー」**タブの*「INSTANCE_NAME」*に、{{ site.data.keyword.mobilefoundation_short }} インスタンスの作成時に最初のステージで設定したものと同じ値を設定します。最初のステージで作成したインスタンスにアダプターがデプロイされるように、同じインスタンス名を使用します。


#### ステージ 4 - アダプターのテスト
{: #stage4-test-adapter}

このステージでは、以前のステージで構築されてデプロイされたアダプターをテストします。サンプル・アダプター・リポジトリーでは、アダプターのエンドポイントをテストするスクリプトが「adapters/JavaAdapter/tests」にあります。

- **「入力」**を、構築されたアダプターの構成と同じ*「GitHub リポジトリー」*に設定します。

- **「ジョブ」**タブに*「デプロイ・ジョブ」*を追加します。**「デプロイヤー・タイプ」**に*「Cloud Foundry」*を選択します。デプロイには cloud foundry と簡単に統合するためのオプションがあるので、テスト・ジョブではなくデプロイ・ジョブを使用します。

以下のスクリプトを使用して、アダプターをテストします。

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

- 環境プロパティーに *INSTANCE_NAME* を追加します。値は以前のステージで追加したものと同じ値にします。

アダプターをテストするために API テスト・フレームワークを使用できます。この例では、アダプターを呼び出して正確性をテストするシェル・スクリプトがあります。


#### ステージ 5 - Fastlane によるアプリの構築
{: #stage5-building-apps-with-fastlane}

このステージでの入力は、以前のステージで使用した GitHub リポジトリーとなります。このステージは、直前のステージ (アダプターのテスト) を過ぎた後にトリガーする必要があります。

アプリを構築するために、**「ジョブ」**タブのデプロイ・ジョブ・テンプレートを使用します。**「デプロイヤー・タイプ」**に*「Cloud Foundry」*を使用します。

以下のスクリプトを使用して、アプリを構築します。

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

上記のスクリプトでは、アプリを構築してリリースするために、`mfpdev-cli` を使用してアプリを {{ site.data.keyword.mobilefoundation_short }} [Fastlane](https://fastlane.tools/) に登録します。


スクリプトで使用される環境変数は、次の**「環境プロパティー」**タブで定義されます。

環境プロパティーで、以下のプロパティーを追加します。

- *INSTANCE_NAME* は **mfp インスタンス名**
- *appPublishUrl* は、テスト・ステージでプルできるようにアプリを公開する必要のある**リポジトリー**。
- *gitPushUser* は **GitHub ユーザー名**
- *gitPushEmail* は **GitHub ユーザーの E メール**
- *gitPushToken* は **git プッシュ・トークン**
- *apkGitPushUrl* は **https://$gitPushToken:x-oauth-basic@github.com/<path><SPACE><branch>**
    `例: https://$gitPushToken:x-oauth-basic@github.com/ShinojEdakkara/mfp-apps master`


#### ステージ 6 - Bitbar を使用したアプリのテスト
{: #stage6-test-the-app-using-bitbar}

このステージも制約がありません。必要に応じて任意のテスト・フレームワークを使用できます。この例では、[Bitbar](https://bitbar.com/testing/) を使用します。

- 使用するテスト・プロジェクトについての GitHub リポジトリーの詳細を追加します (このプロジェクトは、テストのサポートに使用されるすべてのテスト・スクリプトと成果物で構成されます)。

- Bitbar の場合、**「ジョブ」**タブの下にビルド・ジョブを追加し、**「ビルダー・タイプ」**に*「Maven」*を選択します。

以下のスクリプトのようなビルド・スクリプトを使用します。

```
	#!/bin/bash
	cd /home/pipeline/$BUILD_ID/appium/sample-scripts/java
	mvn clean install -X -Dtest=$test -DexecutionType=$executionType -DapiKey=$bitbarApiKey -DapplicationPath=$applicationPath -Dtestdroid_project=$testdroid_project
```
{: codeblock}

上記のスクリプトでは、いくつかの環境変数が必要になります。

- *screenshot\_dir* は **/home/pipeline/home/pipeline/$BUILD\_ID/target**
- *applicationPath* はテストするアプリケーションへの **GitHub パス**
- *executionType* は**実行タイプ**
- *test* は実行する**テスト**
- *bitbarApiKey* は **Bitbar API キー**
- *testdroid_project* は**プロジェクト**。


#### ステージ 7 - {{ site.data.keyword.mobilefoundation_short }} の解体
{: #stage7-tearing-down-mobile-foundation}

このステージでは、上記のステージで構築、デプロイ、およびテストするために最初のステージで作成された {{ site.data.keyword.mobilefoundation_short }} インスタンスを解体します。

このステージでは入力はありません。

- **「ジョブ」**タブに*「デプロイ・ジョブ」*を作成します。
- 以下のスクリプトを使用します。

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

- **「環境プロパティー」**タブの*「INSTANCE_NAME」*に、{{ site.data.keyword.mobilefoundation_short }} インスタンスの作成時に最初のステージで設定した値を設定します。
