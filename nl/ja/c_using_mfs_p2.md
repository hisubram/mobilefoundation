---

copyright:
  years: 2016, 2019
lastupdated:  "2019-02-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

#	「プロフェッショナル 1 アプリケーション」プランを使用したセットアップ
{: #using_mobilefoundation_p2}

「プロフェッショナル 1 アプリケーション」プランでは、ユーザーは、さまざまなモバイル・オペレーティング・システムで 1 つのモバイル・アプリケーションを作成できます。
「{{site.data.keyword.mobilefoundation_short}}: プロフェッショナル 1 アプリケーション」サービス・インスタンスの作成後、以下の手順を読んでサービスを開始してください。

## 「プロフェッショナル 1 アプリケーション」プランの前提条件
{: #prerequisites_p2}

「{{site.data.keyword.mobilefoundation_short}}: プロフェッショナル 1 アプリケーション」サービス・インスタンスを構成する前に、以下の項目を考慮してください。
* 「{{site.data.keyword.mobilefoundation_short}}: プロフェッショナル 1 アプリケーション」は、{{site.data.keyword.Db2_on_Cloud_short}} および {{site.data.keyword.composeForPostgreSQL}} {{site.data.keyword.Bluemix_notm}} プランでのみサポートされます。

* {{site.data.keyword.Db2_on_Cloud_short}} または {{site.data.keyword.composeForPostgreSQL}} サービス・インスタンス資格情報へのアクセスができなければ、{{site.data.keyword.mobilefoundation_short}} サービス・インスタンスの設定を構成できません。

> **注**: {{site.data.keyword.Db2_on_Cloud_short}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL}} サービス・インスタンスは、{{site.data.keyword.Bluemix_notm}} `組織`内のどの`スペース`にも、また、アクセスできるどの`組織`にも存在できます。 {{site.data.keyword.Db2_on_Cloud_short}} または {{site.data.keyword.composeForPostgreSQL}} サービス・インスタンスが存在する`スペース`へのアクセス権限があることを確認します。


## データベース接続の構成
{: #configure_dashdb_p2}

###  構成のファースト・ステップ
{: #firststeps_p2}

「{{site.data.keyword.mobilefoundation_short}}: プロフェッショナル 1 アプリケーション」サービス・インスタンスを作成した後、手順に従って、始めてください。

### Db2 on Cloud サービス・インスタンスへの接続のセットアップ
{: #connect_dashdb_p2}

「{{site.data.keyword.mobilefoundation_short}}: プロフェッショナル 1 アプリケーション」サービス・インスタンスが作成されると、*「概要」*ページが表示されます。 ここでは、 {{site.data.keyword.mobilefoundation_short}} サービス・インスタンスが接続する必要のある、{{site.data.keyword.Db2_on_Cloud_short}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL}} サービス・インスタンスの接続情報を指定する必要があります。

既存の Db2 on Cloud インスタンスがない場合は、新規 {{site.data.keyword.Db2_on_Cloud_short}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL}} サービス・インスタンスを作成できます。

新規 Db2 サービス・インスタンスを作成するには、以下の手順を実行します。

1. *「概要」*ページで、**「新規サービスの作成」**セクションを選択します。

+ 可用性の高い {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスが必要な場合は、**「高可用性構成」**オプションで`「はい」`を選択します。

+ プランの詳細を確認し、**「作成」**をクリックします。

新規 {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスが作成されます。これは、8 GB RAM、2 vCPU、および 500 GB のストレージを備えた専用の {{site.data.keyword.Db2_on_Cloud_short}} インスタンスを提供します。

以下の手順に従って、既存の {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンス、または先ほど作成した {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスに接続します。

1. {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスが存在する {{site.data.keyword.Bluemix_notm}} `組織`を選択します。

+ 選択した`組織`で使用可能なスペースのリストから、{{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスが存在する {{site.data.keyword.Bluemix_notm}} `スペース`を選択します。   
> **注:** {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスが存在する`組織` および`スペース` がリストされない場合、その`組織` および`スペース` のメンバーであるかどうかを確認してください。 組織およびスペースに対して*開発者* 役割のアクセス権限を持っている必要があります。 {{site.data.keyword.mobilefoundation_short}} サービスは {{site.data.keyword.Db2_on_Cloud_short}} サービスから資格情報にアクセスします。

+ 既存の {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスに接続するための {{site.data.keyword.Db2_on_Cloud_short}} `サービス名` および `資格情報` を選択します。

+  指定された {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスへの接続をテストします。

+  **「追加」**をクリックします。 このアクションにより、構成された {{site.data.keyword.Db2_on_Cloud_short}} データベース・サービス・インスタンスで必要な表が作成されます。

数秒で `概要` ページにアクセスでき、ここで {{site.data.keyword.mobilefoundation_short}} サービスを使い始める上で役立つチュートリアルやビデオが提供されます。

{{site.data.keyword.mobilefoundation_short}} サービス・インスタンスで使用するように構成された {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスを変更することはできません。 ただし、同じ {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスを複数の {{site.data.keyword.mobilefoundation_short}} サービス・インスタンスで使用することは可能です。これは、各 {{site.data.keyword.mobilefoundation_short}} サービス・インスタンスが選択された {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンス内に独自のスキーマを作成するためです。
{: note}

## 「プロフェッショナル 1 アプリケーション」プランを使用して作成した MobileFirst サーバーの始動
{: #start_mobilefoundation_p2}

* {{site.data.keyword.mfserver_short_notm}} をデフォルト設定で始動するには、**「基本サーバーの始動」**をクリックしてください。

* この選択により、以下の設定で {{site.data.keyword.mfserver_long_notm}} が作成されます。
    -  1 GB のメモリー。 開発アクティビティー、中程度のテスト・アクティビティー、および小規模な実動ワークロードには、このサイズで十分です。

    -	`ユーザー名` と `パスワード` は自動的に生成されます。 サーバーの稼働中にこれらにアクセスできます。

    サーバーを作成するプロセスが開始されます。 このプロセスには約 10 分かかり、メッセージ・ウィンドウにはこの操作の進行が示されます。 完了すると、以下のことを確認できるダッシュボードが表示されます。

      -	実行中のサーバーの状況 (状態、サイズ)。

      -	サーバーの経路が作成されます。 {{site.data.keyword.mfserver_short_notm}} に接続するには、モバイル・アプリケーションでこの経路を使用します。

      -	{{site.data.keyword.mfp_oc_short_notm}} にアクセスするための個人の`ユーザー名` と`パスワード`。 `パスワード` は非表示です。 表示するには**「パスワードの表示」**アイコンをクリックします。

*	**「コンソールの起動」**をクリックして {{site.data.keyword.mfp_oc_short_notm}} を開きます。

このコンソールを使用して、モバイル・アプリ、アダプター、およびモバイル・デバイスの管理、モバイル・バックエンドとしてのサーバーの使用、プッシュ通知の送信などを行うことができます。

## 「プロフェッショナル 1 アプリケーション」プランを使用した場合の MobileFirst サーバーの再作成
{: #recreate_mobilefoundation_p2}

*	**「再作成」**をクリックしてサーバーを再作成します。

* このアクションは、既存のサーバーを停止し、データを削除します。 更新されたバージョンがある場合は、更新されたバージョンで新しいサーバー・インスタンスが作成されます。 このアクションは、完了するまでに数分かかります。

アプリおよびアダプターに関する情報など、前のサーバー・インスタンスのデータは、構成された {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンス内に保持されます。 このデータは、サーバーの再作成に使用されます。
{: note}

##	「プロフェッショナル 1 アプリケーション」プランでの拡張構成のセットアップ
{: #using_mfs_advanced_p2}

拡張設定またはカスタム設定を使用してサーバーを作成するには、`「概要」` ページの**「拡張構成を使用したサーバーの始動 (Start Server with Advanced Configuration)」**を使用します。 また、**「構成 (Configuration)」**タブをクリックして、サーバー構成をカスタマイズするためにサーバー設定を更新することもできます。 {{site.data.keyword.mobilefoundation_short}} では、拡張設定にアクセスできます。

*	**「トポロジー (Topology)」**タブから、サーバーのサイズと、必要性に基づいてインスタンスの数を選択できます。 開発と簡単なテストにはデフォルトの 1 GB のサーバーで十分です。
  - ニーズに応じて、ご使用のサーバーに合ったサイズを選択してください。

  - **「インスタンス」**は作成されたノード数を表示します。

## Mobile Analytics: 「プロフェッショナル 1 アプリケーション」プランの場合
{: #mobile_analytics_p2}

Mobile Analytics サーバーが含まれ、「Mobile Foundation: 開発者」プランのサービス・インスタンスで事前構成されています。

* {{site.data.keyword.mfp_oc_short_notm}} から、Mobile Analytics コンソールを起動します。

Mobile Analytics について詳しくは、[ここ](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app){: new_window}を参照してください。
