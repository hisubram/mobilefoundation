---

copyright:
  years: 2016, 2018
lastupdated:  "2018-02-14"

---

#	「開発者専門」プランの使用
{: #using_mobilefoundation_p3}

「{{site.data.keyword.mobilefoundation_short}}: 開発者専門」は、チーム・ベースの開発およびテスト用に適しています。このプランは、実動には適しません。

「開発者商用」プランを使用して {{site.data.keyword.mobilefoundation_short}} サービス・インスタンスを作成した後、{{site.data.keyword.Bluemix_notm}} の`「概要」`ページにアクセスします。このページで、サービスを使い始める上で役立つチュートリアルやビデオが提供されます。

## 前提条件
{: #prerequisites_p3}

「{{site.data.keyword.mobilefoundation_short}}: 開発者専門」サービス・インスタンスを構成する前に、以下の項目を考慮してください。
* 「{{site.data.keyword.mobilefoundation_short}}: 開発者専門」は、{{site.data.keyword.Db2_on_Cloud_short}} {{site.data.keyword.Bluemix_notm}} プランでのみサポートされます。

* {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンス資格情報へのアクセス権限がないと、{{site.data.keyword.mobilefoundation_short}} サービス・インスタンスの設定を構成することはできません。

> **注**: {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスは、{{site.data.keyword.Bluemix_notm}} `組織`内のどの`スペース`にも、また、アクセスできるどの`組織`にも存在できます。 {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスが存在する`スペース`へのアクセス権限があることを確認します。


## データベース接続の追加
{: #configure_dashdb_p3}

###  最初の手順
{: #firststeps_p3}

「{{site.data.keyword.mobilefoundation_short}}: 開発者専門」サービス・インスタンスを作成した後、手順に従って、始めてください。

### Db2 on Cloud サービス・インスタンスへの接続のセットアップ
{: #connect_dashdb_p3}

「開発者商用」プランを使用して {{site.data.keyword.mobilefoundation_short}} サービス・インスタンスを作成した後、*「概要」*ページが表示されます。このページで、{{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスの接続情報を指定する必要があります。{{site.data.keyword.mobilefoundation_short}} サービス・インスタンスは、指定された {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスに接続する必要があります。

既存の {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスがない場合は、作成できます。

以下の手順に従って、Db2 on Cloud サービス・インスタンスを作成します。

1. *「概要」*ページで、**「新規サービスの作成」**セクションを選択します。

+ 可用性の高い {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスが必要な場合は、**「高可用性の構成」**オプションで`「はい」`を選択します。

+ プランの詳細を確認し、**「作成」**をクリックします。

新規 {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスが作成されます。これは、8 GB RAM、2 vCPU、および 500 GB のストレージを備えた専用の {{site.data.keyword.Db2_on_Cloud_short}} インスタンスを提供します。

次の手順を実行して、既存または新規の {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスに接続します。

1. {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスが存在する {{site.data.keyword.Bluemix_notm}} `組織`を選択します。

+ 選択した`組織`で使用可能なスペースのリストから、{{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスが存在する {{site.data.keyword.Bluemix_notm}} `スペース`を選択します。   
> **注:** {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスが存在する`組織`および`スペース`がリストされない場合、その`組織`および`スペース`のメンバーであるかどうかを確認してください。{{site.data.keyword.mobilefoundation_short}} サービスは {{site.data.keyword.Db2_on_Cloud_short}} サービスから資格情報にアクセスするため、組織およびスペースに対して*開発者* 役割のアクセス権限を持っている必要があります。

+ 既存の {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスに接続するための {{site.data.keyword.Db2_on_Cloud_short}} `サービス名` および `資格情報` を選択します。

+  指定された {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスへの接続をテストします。

+  **「追加」**をクリックします。 このアクションにより、構成された {{site.data.keyword.Db2_on_Cloud_short}} データベース・サービス・インスタンスで必要な表が作成されます。

数秒で `概要` ページにアクセスでき、ここで {{site.data.keyword.mobilefoundation_short}} サービスを使い始める上で役立つチュートリアルやビデオが提供されます。

> **注:** {{site.data.keyword.mobilefoundation_short}} サービス・インスタンスで使用するように構成された {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスを変更することはできません。 ただし、同じ {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンスを複数の {{site.data.keyword.mobilefoundation_short}} サービス・インスタンスで使用することは可能です。これは、各 {{site.data.keyword.mobilefoundation_short}} サービス・インスタンスが選択された {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンス内に独自のスキーマを作成するためです。

## MobileFirst サーバーの始動
{: #start_mobilefoundation_p3}

* {{site.data.keyword.mfserver_short_notm}} をデフォルト設定で始動するには、**「基本サーバーの始動」**をクリックしてください。

* この選択により、以下の設定で {{site.data.keyword.mfserver_long_notm}} がプロビジョンされます。
    - 1 GB のメモリーを備えた単一ノード。 開発アクティビティー、中程度のテスト・アクティビティー、および小規模な実動ワークロードには、このサイズで十分です。

    -	`ユーザー名`と`パスワード`は、自動的に生成されます。 サーバーの稼働中にこれらにアクセスできます。

    サーバーのプロビジョン・プロセスが始動します。 このプロセスには約 10 分かかり、メッセージ・ウィンドウにはこの操作の進行が示されます。 完了すると、以下のことを確認できるダッシュボードが表示されます。

      -	実行中のサーバーの状況 (状態、サイズ)。

      -	サーバーの経路が作成されます。 {{site.data.keyword.mfserver_short_notm}} に接続するには、モバイル・アプリケーションでこの経路を使用します。

      -	{{site.data.keyword.mfp_oc_short_notm}} にアクセスするための個人の`ユーザー名` と`パスワード`。 `パスワード` は非表示です。 表示するには**「パスワードの表示」**アイコンをクリックします。

*	**「コンソールの起動」**をクリックして {{site.data.keyword.mfp_oc_short_notm}} を開きます。

このコンソールを使用して、モバイル・アプリ、アダプター、およびモバイル・デバイスの管理、モバイル・バックエンドとしてのサーバーの使用、プッシュ通知の送信などを行うことができます。

##  Mobile Analytics サービスの追加
{: #adding_analytics_server_p3}

 Mobile Analytics サービス・インスタンスを {{site.data.keyword.mobilefoundation_short}} サービス・インスタンスに接続することにより、{{site.data.keyword.mobilefirst}} サーバー上のモバイル・アプリケーションをモニターできるようになりました。

 「開発者商用」プランでは、Mobile Analytics サービス・インスタンスを作成します。

 <!--Users can also attach volumes to the containers to persist data. The volume once selected cannot be changed. 20 GB is the default file share space available to the user. If the user needs additional storage space to persist analytics data, he is required to buy additional file share and create a volume using this file share. He can then select this new volume while deploying the analytics server.

 For more information on adding volumes to {{site.data.keyword.containerlong}}, refer to [Storing persistent data in a volume by using the {{site.data.keyword.Bluemix_notm}} Dashboard ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.ng.bluemix.net/docs/containers/container_volumes_ov.html#container_volumes_ui){: new_window}. -->

* **「Analytics の追加」**をクリックして、Mobile Analytics サービス・インスタンスを {{site.data.keyword.mobilefoundation_short}} サービス・インスタンスに追加します。

<!--* You can choose the Mobile Analytics service configuration, a minimum of 1 GB and a maximum of 2 GB memory is supported for the Analytics server configuration.-->
  プロビジョン・プロセスが始動します。 このプロセスには約 10 分かかり、メッセージ・ウィンドウにはこの操作の進行が示されます。  

* {{site.data.keyword.mfp_oc_short_notm}} から、Mobile Analytics サービス・コンソールを起動します。

* {{site.data.keyword.mfserver_short_notm}} と Mobile Analytics サービスとの間で、シングル・サインオンが使用可能になります。Mobile Analytics サービスは、{{site.data.keyword.mfserver_short_notm}} と同じ LTPA 鍵とユーザー資格情報で構成されます。{{site.data.keyword.mfp_oc_short_notm}} のログインに使用したのと同じ `username` と `password` を使用して、Mobile Analytics コンソールにログインできます。

Mobile Analytics サービスについて詳しくは、[MobileFirst Foundation Operational Analytics ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}を参照してください。

> **注:** {{site.data.keyword.mobilefoundation_short}} サービス・インスタンスを削除すると、Mobile Analytics サービス・インスタンスが削除されます。

##  Mobile Analytics サービスの削除
{: #deleting_analytics_server_p3}

{{site.data.keyword.mobilefoundation_short}} サービス・ダッシュボードから、{{site.data.keyword.mobilefoundation_short}} サービス・インスタンスに追加された Mobile Analytics サービスを削除できるようになりました。

* **「Analytics の削除」**をクリックして、{{site.data.keyword.mobilefoundation_short}} サービス・インスタンスに追加されている Mobile Analytics サービスを削除します。

  **「Analytics の削除」** をクリックすると、Analytics サーバー・インスタンスが削除されます。Analytics インスタンスの削除プロセスには、約 10 分かかります。画面を最新表示して、更新された状況を確認できます。 Analytics インスタンスを削除すると、**「Analytics の追加」**ボタンが再び使用可能になります。Mobile Analytics サービスを再度追加する場合は、このボタンをクリックできます。

## MobileFirst サーバーの再作成
{: #recreate_mobilefoundation_p3}

*	**「再作成」**をクリックしてサーバーを再作成します。

* このアクションは、既存のサーバーを停止し、データを削除します。 更新されたバージョンがある場合は、更新されたバージョンで新しいサーバー・インスタンスが作成されます。 このアクションは、完了するまでに数分かかります。

> **注**: アプリおよびアダプターに関する情報など、前のサーバー・インスタンスのデータは、構成された {{site.data.keyword.Db2_on_Cloud_short}} サービス・インスタンス内に保持されます。このデータは、サーバーの再作成に使用されます。

##	拡張構成のセットアップ
{: #using_mfs_advanced_p3}

拡張設定またはカスタム設定を使用してサーバーを作成するには、`「概要」` ページの**「拡張構成を使用したサーバーの始動 (Start Server with Advanced Configuration)」**を使用します。 また、**「設定」**タブをクリックして、サーバー構成をカスタマイズするためにサーバー設定を更新することもできます。{{site.data.keyword.mobilefoundation_short}} では、拡張設定にアクセスできます。

*	**「トポロジー (Topology)」**タブから、必要に応じてサーバーのサイズとメモリーを選択できます。 デフォルトのサーバーは、1 GB のメモリーで作成されます。
  - 必要に応じて、サーバーのメモリーを最大 2 GB に変更できます。

  - **「インスタンス」**は作成されたノード数を表示します。このフィールドは、「{{site.data.keyword.mobilefoundation_short}}: 開発者専門」では編集できません。 「開発者商用」プランでは、ノードの数は、デフォルトの **1** になります。

詳しくは、[{{site.data.keyword.mobilefoundation_long}} documentation ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}を参照してください。
