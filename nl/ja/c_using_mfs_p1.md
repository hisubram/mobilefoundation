---

copyright:
  years: 2016, 2018
lastupdated:  "2018-02-14"

---

#	「開発者」プランの使用
{: #using_mobilefoundation_p1}

「開発者」プランを使用して、{{site.data.keyword.mobilefoundation_short}} サービス・インスタンスを作成した後、{{site.data.keyword.Bluemix_notm}} の「概要」ページにアクセスします。このページには、サービスを使い始める上で役立つチュートリアルやビデオが用意されています。

## MobileFirst サーバーの始動
{: #start_mobilefoundation_p1}
* {{site.data.keyword.mfserver_short_notm}} をデフォルト設定で始動するには、**「基本サーバーの始動」**をクリックしてください。

  この選択により、以下の設定で {{site.data.keyword.mfserver_long_notm}} がプロビジョンされます。
  *	1 GB のメモリー。 開発アクティビティー、簡単なテスト・アクティビティー、および小規模な実動ワークロードには、このサイズで十分です。

  *	`ユーザー名` と `パスワード` は自動的に生成されます。 サーバーの稼働中にこれらにアクセスできます。

  プロビジョン・プロセスが始動します。 このプロセスには約 10 分かかり、メッセージ・ウィンドウにはこの操作の進行が示されます。 完了すると、以下のことを確認できるダッシュボードが表示されます。
    *	実行中のサーバーの状況 (状態、サイズ)。

    *	自動的に作成されたサーバーの経路。 {{site.data.keyword.mfserver_short_notm}} に接続するには、モバイル・アプリケーションでこの経路を使用します。

    *	{{site.data.keyword.mfp_oc_short_notm}} にアクセスするための個人の`ユーザー名` と`パスワード`。 `パスワード` は非表示です。 表示するには**「パスワードの表示」**アイコンをクリックします。

*	**「コンソールの起動」**をクリックして {{site.data.keyword.mfp_oc_short_notm}} を起動します。

このコンソールを使用して、モバイル・アプリとモバイル・デバイスの管理、モバイル・バックエンドとしてのサーバーの使用、プッシュ通知の送信などを行えます。

##  Mobile Analytics サービスの追加
{: #adding_analytics_server_dev}

 Mobile Analytics サービスを {{site.data.keyword.mobilefoundation_short}} サービス・インスタンスに追加することにより、{{site.data.keyword.mobilefirst}} サーバー上のモバイル・アプリケーションをモニターできるようになりました。「開発者」プランでは、単一のノードと 1 GB メモリーを持つコンテナー・グループに、Mobile Analytics サービスを作成します。

* **「Analytics の追加」**をクリックして、Mobile Analytics サービスを {{site.data.keyword.mobilefoundation_short}} サービス・インスタンスに追加します。

  プロビジョン・プロセスが始動します。 このプロセスには約 10 分かかり、メッセージ・ウィンドウにはこの操作の進行が示されます。  

* {{site.data.keyword.mfp_oc_short_notm}} から、Mobile Analytics サービス・コンソールを起動します。

* {{site.data.keyword.mfserver_short_notm}} と Mobile Analytics サービスとの間で、シングル・サインオンが使用可能になります。Mobile Analytics サービスは、{{site.data.keyword.mfserver_short_notm}} と同じ LTPA 鍵とユーザー資格情報で構成されます。{{site.data.keyword.mfp_oc_short_notm}} のログインに使用したのと同じ `username` と `password` を使用して、Mobile Analytics コンソールにログインできます。

Mobile Analytics について詳しくは、[MobileFirst Foundation Operational Analytics ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}を参照してください。

> **注:** {{site.data.keyword.mobilefoundation_short}} サービス・インスタンスを削除するか、{{site.data.keyword.mfserver_short_notm}} を再作成すると、Mobile Analytics サービス・インスタンスが削除されます。

##  Mobile Analytics サービスの削除
{: #deleting_analytics_server_dev}

{{site.data.keyword.mobilefoundation_short}} サービス・ダッシュボードから、{{site.data.keyword.mobilefoundation_short}} サービス・インスタンスに追加された Mobile Analytics サービスを削除できるようになりました。

* **「Analytics の削除」**をクリックして、{{site.data.keyword.mobilefoundation_short}} サービス・インスタンスに追加された Mobile Analytics サービスを削除します。

 **「Analytics の削除」** をクリックすると、Analytics サーバー・インスタンスが削除されます。Analytics インスタンスの削除プロセスには、約 10 分かかります。画面を最新表示して、更新された状況を確認できます。 Analytics インスタンスを削除すると、**「Analytics の追加」**ボタンが再び使用可能になります。Mobile Analytics サービスを再度追加する場合は、このボタンをクリックできます。


## MobileFirst サーバーの再作成
{: #recreate_mobilefoundation_p1}

*	**「再作成」**をクリックしてサーバーを再作成します。

* このアクションは、既存のサーバーを停止し、データを削除します。 モバイル・サーバー内のデータはすべて失われます。 更新されたバージョンがある場合は、更新されたバージョンで新しいサーバー・インスタンスが作成されます。 このアクションは、完了するまでに数分かかります。

##	拡張構成のセットアップ
{: #using_mfs_advanced_p1}

拡張設定またはカスタム設定を使用してサーバーを作成するには、`「概要」` ページの**「拡張構成を使用したサーバーの始動 (Start Server with Advanced Configuration)」**を使用します。 また、**「構成 (Configuration)」**タブをクリックして、サーバー構成をカスタマイズするためにサーバー設定を更新することもできます。 {{site.data.keyword.mobilefoundation_short}} では、拡張設定にアクセスできます。

*	**「トポロジー (Topology)」**タブから、サーバーのサイズと、必要なインスタンスの数を選択できます。 開発と中程度のテストにはデフォルトの 1 GB のサーバーで十分です。

  - ニーズに応じて、ご使用のサーバーに合ったサイズを選択してください。

* **「ノード (Nodes)」**は作成されたノード数を表示します。 このフィールドは「{{site.data.keyword.mobilefoundation_short}}: 開発者」では編集できません。 「開発者」プランで、ノードの数<!--in your {{site.data.keyword.IBM_notm}} container group-->は、**1** にデフォルト設定されます。

詳細については、[{{site.data.keyword.mobilefoundation_long}} documentation ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}を参照してください。
