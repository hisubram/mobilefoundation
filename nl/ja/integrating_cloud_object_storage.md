---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-24"

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

# {{ site.data.keyword.cos_full_notm }} と {{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}} を組み合わせた使用
{: #using_ibm_cloud_object_storage_with_ibm_mobile_foundation}

{{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}} (MF) は、パーソナライズされたコンテキスト重視の次世代コグニティブ・モバイル・アプリの構築と展開をサポートするように独自に設計された、エンタープライズ・グレードの機能を提供します。 {{ site.data.keyword.cos_full_notm }} (COS) は、柔軟性があり、コスト効率が高く、拡張が容易な、非構造化データ用のクラウド・ストレージです。 この使用法ガイドでは、{{ site.data.keyword.mobilefoundation_short}} を使用するモバイル・アプリケーションで、Ionic アプリケーションを介して {{ site.data.keyword.cos_full_notm }} に接続してデータを取り出したりアップロードしたりする方法について説明します。 この使用法チュートリアルで使用する Ionic アプリケーション、アダプター、関連ファイルは、[ここ](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App)から入手できます。
{: shortdesc}


## 前提条件
{: #cos-prerequisites}

1. `npm install -g mfpdev-cli` を実行して、[mfpdev-cli](https://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_wl_cli_description.html) をインストールします。 この CLI を使用して、Ionic アプリを登録し、アダプターを MF サーバーにデプロイします。 代わりに MF サーバーのダッシュボードからこれらのアクティビティーを実行することもできます。

2. ご使用のマシンに [{{ site.data.keyword.cloud_notm}} CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-ibmcloud-cli) をインストールします。

3. `npm install -g ionic` を実行して、Ionic CLI をインストールします

4. `npm install -g cordova` を実行して、Cordova をインストールします


## {{ site.data.keyword.mobilefoundation_short}} サーバーのセットアップ
{: #mobile-foundation-server-setup}

{{ site.data.keyword.cloud_notm}} 上で {{ site.data.keyword.mobilefoundation_short}} サーバーをセットアップします。 {{ site.data.keyword.mobilefoundation_short}} サーバーの {{ site.data.keyword.cloud_notm}} インスタンスを以下のようにセットアップします。

* {{ site.data.keyword.cloud_notm}} カタログで、*{{ site.data.keyword.mobilefoundation_short}}* を検索します。**「{{ site.data.keyword.mobilefoundation_short}}」**タイルをクリックします。

    ![MFP カタログ](images/mfp_catalog.png)

* {{ site.data.keyword.mobilefoundation_short}} サーバー・インスタンスの適切な名前を入力し、**「作成」**をクリックします。

    ![新規 BMMFP](images/bmmfpnew.png)

* 新しい MF サーバー・インスタンスが作成され、ログイン資格情報の入力が求められます。

    ![MFP ログイン](images/mfp_login.png)

* MF サーバーにログインするための資格情報は、メニューの**「資格情報」**タブにあります。

    ![MFP 資格情報](images/mfp_credentials.png)

* この資格情報を入力してログインし、MF ダッシュボードに入ります。

    ![MFP ダッシュボード](images/mfp_dashboard.png)


## Cloud Object Storage のセットアップ
{: #cloud-object-storage-setup}

* {{ site.data.keyword.cloud_notm}} カタログで、*Cloud Object Storage* を検索します。**「Object Storage」**タイルをクリックします。

    ![カタログ](images/catalog.png)

* COS インスタンスに名前を付け、**「作成」**をクリックします。

    ![COS の作成](images/cos_create.png)

* 次に、メニュー・オプションで**「バケット」**をクリックします。バケットの適切な名前 (このサンプルではバケットの名前に `sharedgallery` を選択しました) を入力し、**「作成」**をクリックします。

    ![バケットの作成](images/bucketcreate.png)

* バケットを作成し終えると、バケットのランディング・ページは以下のイメージのようになり、データを直接アップロードするオプションが表示されます。

    ![バケットのランディング・ページ](images/bucketlanding.png)

* ここで、[サンプル](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App)内の**「Short stories」**フォルダー内にある 3 つのファイルを追加します。


## MFP-COS Ionic アプリと Java アダプター
{: #mfp-cos-ionic-app-and-java-adapter}

この [Git リポジトリー](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App)をダウンロードするか複製します。 このリポジトリーは、以下の 2 つの主なコンポーネントで構成されます。

1. MF Java アダプター
2. Ionic モバイル・アプリケーション

### mfpdev-cli の構成
{: #configuring-mfpdev-cli}

サーバーの詳細情報を CLI に追加します。 コマンド・プロンプトで、`mfpdev server add` を実行します。

```
? Enter the name of the new server profile:
```

サーバーの名前を入力し、Enter キーを押します。 このサンプルでは、名前として `mfpserver` を入力します。

```
? Enter the fully qualified URL of this server:
```

サーバーの URL を入力します。 {{ site.data.keyword.cloud_notm}} 上の MF サーバーの場合、サービス資格情報タブに URL が記載されています。 {{ site.data.keyword.cloud_notm}} 上の HTTPS MF サーバーのポートは 443 で、HTTP MF サーバー・インスタンスのポートは 80 です。

```
? Enter the MobileFirst Server administrator login ID: (admin)
```

`admin` と入力して、**Enter** キーを押します。

```
? Enter the MobileFirst Server administrator password:
```

サービス資格情報で使用できるパスワードを入力します。

```
? Save the administrator password for this server?: (Y/n)
```

各自の好みに合わせて *Y/N* を入力し、プロンプトで求められる詳細情報を入力します。

```
? Enter the MobileFirst Server connection timeout in seconds: 30
```

デフォルトのタイムアウトとして 30 秒を設定します

```
? Make this server the default?: (Y/n)
```

*Y* を入力して、**Enter** キーを押します。

***予期される出力***:

```
Verifying server configuration...
The following runtimes are currently installed on this server: mfp
Server profile 'mfpserver' added successfully.
```

### MF Java アダプターの構成
{: #configuring-the-mf-java-adapter}

COS インスタンスに接続するには、`adapter.xml` ファイル内に COS インスタンスの詳細情報を入力する必要があります。 以下のフィールドの値を指定します。

1. **endpointURL**: このフィールドは、COS オブジェクトのパブリック・エンドポイントの URL です。 この URL は、COS のダッシュボード上の**「バケット」(メニュー・オプション上) -> <your-bucket-name> (このサンプルでは `sharedgallery`) ->「構成」->「エンドポイント」->「パブリック」**の下にあります。
2. **AuthToken**: このチュートリアルでは、IAM 認証を使用しています。

Java アダプターが COS のインスタンスに接続するには、IAM または HMAC を使用する認証が必要です。 IAM トークンを取得するステップを以下に示します。 IAM と HMAC の認証プロセスの詳細については、[ここ](https://cloud.ibm.com/docs/services/cloud-object-storage/api-reference/api-reference-buckets.html#bucket-operations#AuthenticationOptions)をクリックしてください。

#### {{ site.data.keyword.cloud_notm}} CLI を使用した IAM Oauth トークンの入手
{: #obtaining-iam-oath-token-using-ibm-cloud-cli}

1. 最初に、API キーがあることを確認します。 [{{ site.data.keyword.cloud_notm}} ID およびアクセス管理](https://cloud.ibm.com/iam/#/users)から API キーを取得します。
2. CLI を使用して {{ site.data.keyword.cloud_notm}} プラットフォームにログインします。

  ```bash
	ibmcloud login --apikey <value>
  ```
  {: codeblock}
  出力は次のスニペットのようになります。

	```text
	認証中です...
	OK

	Targeted account <account-name> (<account-id>)

	Targeted resource group default

	API endpoint:     https://api.us-south.cf.cloud.ibm.com (API version: 	2.128.0)
	Region:           us-south
	User:             <email-address>
	Account:          <account-name> (<account-id>)
	Resource group:   default
	```

3. {{ site.data.keyword.cloud_notm}} アカウント上のすべてのサービス・インスタンスを取得するには、CLI 上で次のコマンドを実行します。

	```bash
  ibmcloud resource service-instances
  ```
  {: codeblock}

	予期される出力:

	```text
 	Retrieving service instances in resource group Default and all 	locations under account <account-name> as <email-address>...
	OK
	Name                                               Location     	State    Type               Tags
	<resource-instance-name>                           global       	active   service_instance
	```

4. 次のコマンドを実行して、COS インスタンスの詳細を取得します。 <instance-name> は COS サービスの名前です (このチュートリアルでは `newObject`)。

  ```bash
	ibmcloud resource service-instance <instance-name>
  ```
  {: codeblock}

	予期される出力:

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

5. IAM トークンを取得するには、次のコマンドを実行します。
  ```bash
	ibmcloud iam oauth-tokens
  ```

	予期される出力:

	```text
	IAM token:  Bearer <token>
	UAA token:  Bearer <refresh-token>
	```

*endpointURL* と *authToken* の値の追加後に、アダプターを構築します。 コマンド・プロンプトでアダプターのルート・フォルダーにナビゲートし、次のコマンドを実行します

```bash
mfpdev adapter build
```
{: codeblock}

`target` フォルダー内に `*.adapter` ファイルが作成されます。 次のコマンドを実行します。

```bash
mfpdev adapter deploy
```
{: codeblock}

アダプターが MF インスタンスにデプロイされます。

別の方法として、アダプターを MF サーバーのダッシュボード上にデプロイできます。 メニューで MF サーバーのダッシュボードを開き、**「アダプター」->「新規」**をクリックして、以下のイメージで示されているページを開きます。

![MFPNewAdapterRegister](images/mfp_new_adapter_register.png)

続いて**「アダプターのデプロイ」**をクリックして、**target** フォルダーから `.adapter` ファイルをアップロードします。


### Ionic アプリの構成
{: #configuring-the-ionic-app}

アプリ内で、以下のステップを実行します。

1. Ionic アプリケーションを格納するフォルダーにナビゲートします。

2. Cordova MF プラグインを追加します

	```bash
  ionic cordova plugin add cordova-plugin-mfp
  ```

3. Android または iOS プラットフォームを追加します

  ```bash
	ionic cordova platform add android
  ```
	または

	```bash
	ionic cordova platform add ios
	```

4. 次のコマンドを実行して、アプリを MF サーバーに登録します

  ```bash
	mfpdev app register
	```

	別の方法として、MF サーバーのダッシュボード上でアプリを登録できます。 MF サーバーのダッシュボードを開き、メニューで**「アプリケーション」->「新規」**をクリックします。

	以下のイメージで示されているページがロードされます。

	![MFPNewAppRegister](images/mfp_new_app_register.png)

	要求された詳細を入力します。 テキスト・ボックス「アプリケーション名」内でアプリケーションに名前を付けます。 必要なプラットフォームを選択します。

	Android の場合、**「パッケージ」**テキスト・ボックスで*アプリケーション ID* を使用できます。 このパラメーターは、Android アプリケーションのパッケージの `AndroidManifest.xml` に示されています。 **「バージョン」**テキスト・ボックス・フィールドに、`AndroidManifest.xml` にある *versionName* 値を入力する必要があります。

	iOS の場合、**「バンドル ID」**テキスト・ボックスで*アプリケーション ID* を使用できます (大/小文字の区別あり)。 このパラメーターは、iOS アプリケーションの `mfpclient.plist` に示されています。 **「バージョン」**テキスト・ボックス・フィールドに、iOS アプリケーションの `mfpclient.plist` ファイルにある *version* 値を入力する必要があります。

5. `ionic cordova prepare` を実行して、追加した環境に変更内容をパーコレートします。
6. 次のコマンドを実行します
  ```bash
	ionic cordova build android
  ```
	または
  ```bash
	ionic cordova build ios
  ```
	実行すると、TypeScript の変更が環境に追加されます。

7. デバイスを接続するか、エミュレーターかシミュレーターを実行して、次のコマンドを実行します。

	```bash
  ionic cordova run android
	```
	または
	```bash
	ionic cordova run ios
  ```

### Ionic アプリケーションのナビゲート
{: #navigating-the-ionic-application}

Ionic アプリケーションは、作成した COS インスタンスからアップロードしたショート・ストーリーのリストを表示します (このアップロードは [Cloud Object Storage のセットアップ](#cloud-object-storage-setup)のセクションの最後のステップで説明されています)。 特定のストーリーのオプションを選択すると、選択されたストーリーがロードされて表示されます。 ストーリーを追加するオプションも提供されます。追加すると、COS インスタンスにアップロードされます。

初期 COS オブジェクト・リストは次のイメージのようになります。

![作業前の COS](images/cos_before.png)

アプリケーションのホーム・ページには、*「ストーリーをすべて取得 (Get all stories)」* または*「ストーリーの追加 (Add story)」* オプションがあります。

![アプリのホーム画面](images/app-home-screen.png)

**「ストーリーをすべて取得 (Get all stories)」**をクリックすると、COS で入手可能なストーリーが表示されます。

![アプリでのストーリーの選択](images/app-select-story.png)

ロードするストーリーをドロップダウン・メニューから選択し、**「ロード (Load)」**をクリックします

![アプリでのストーリーのロード](images/app-story-loaded.png)

次に、**「ストーリーの追加 (Add story)」**ボタンをクリックし、所有しているストーリーを追加します。 テキスト域でストーリーのタイトルと内容を入力し、**「追加 (Add)」**をクリックします。

![アプリでの追加入力](images/app-add-input.png)

ストーリーを追加し終えると、ストーリーが正常に追加されたことを示すメッセージが表示されます。

![アプリでのストーリーの追加完了](images/app-story-added.png)

この時点で、アプリケーションからも COS ダッシュボードにストーリーが追加されています。

![COS に追加済み](images/cos_added.png)


`ibmcloud iam oauth-tokens` を使用して入手した IAM Oauth トークンには有効期限があり、`403 - 禁止`の例外でアダプターに障害が発生します。 したがって、アプリが期待どおりに機能するには、デプロイされているアダプター上でトークンが有効であることを確認する必要があります。
{: note}
