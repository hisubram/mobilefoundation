---

copyright:
  years: 2016, 2018
lastupdated:  "2018-03-07"

---

#	「開発者」プランの使用
{: #using_mobilefoundation_p1}

「開発者」プランを使用して、{{site.data.keyword.mobilefoundation_short}} サービス・インスタンスを作成した後、{{site.data.keyword.Bluemix_notm}} の「概要」ページにアクセスします。 このページには、サービスを使い始める上で役立つチュートリアルやビデオが用意されています。

## MobileFirst Server を使用した作業
{: #start_mobilefoundation_p1}
* MobileFirst Server に即座にアクセスして作業を行うことができます。

  この選択により、以下の設定で {{site.data.keyword.mfserver_long_notm}} がプロビジョンされます。
  *	1 GB のメモリー。 開発アクティビティー、簡単なテスト・アクティビティー、および小規模な実動ワークロードには、このサイズで十分です。

  * CLI を使用して MobileFirst Server にアクセスするには、IBM Cloud コンソールの左側のナビゲーション・ペインで**「サービス資格情報」**をクリックして表示される、資格情報が必要です。

<!--  The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation. When complete a dashboard is displayed where you can see:
    *	The status of your server that is running (state, size).

    *	The server route created for you. Use this route in your mobile application to connect to the {{site.data.keyword.mfserver_short_notm}}.

    *	Your personal `username` and `password` to access the {{site.data.keyword.mfp_oc_short_notm}}. The `password` is hidden. Click **Show Password** icon to visualize it.

*	Click **Launch Console** to launch the {{site.data.keyword.mfp_oc_short_notm}}.-->

これで、モバイル・アプリとモバイル・デバイスの管理、モバイル・バックエンドとしてのサーバーの使用、プッシュ通知の送信などを行うことができます。

## Mobile Analytics サービス
{: #adding_analytics_server_dev}

Mobile Analytics サーバーが含まれ、「Mobile Foundation: 開発者」プランのサービス・インスタンスで事前構成されています。

<!-- You can now monitor your mobile application on {{site.data.keyword.mobilefirst}} server by adding a Mobile Analytics service to the {{site.data.keyword.mobilefoundation_short}} service instance. Developer plan creates the Mobile Analytics service in a container group with a single node having 1 GB memory.

* Click **Add Analytics** to add the Mobile Analytics service to the {{site.data.keyword.mobilefoundation_short}} service instance.

  The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation.  -->

* {{site.data.keyword.mfp_oc_short_notm}} から、Mobile Analytics サービス・コンソールを起動します。

* {{site.data.keyword.mfserver_short_notm}} と Mobile Analytics サービスとの間で、シングル・サインオンが使用可能になります。 Mobile Analytics サービスは、{{site.data.keyword.mfserver_short_notm}} と同じ LTPA 鍵とユーザー資格情報で構成されます。 {{site.data.keyword.mfp_oc_short_notm}} のログインに使用したのと同じ `username` と `password` を使用して、Mobile Analytics コンソールにログインできます。

Mobile Analytics について詳しくは、[MobileFirst Foundation Operational Analytics ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}を参照してください。

> **注:** {{site.data.keyword.mobilefoundation_short}} サービス・インスタンスを削除すると、Mobile Analytics サービス・インスタンスが削除されます。

<!--##  Deleting Mobile Analytics service
{: #deleting_analytics_server_dev}

You can now delete the Mobile Analytics service that was added to the {{site.data.keyword.mobilefoundation_short}} service instance, from the {{site.data.keyword.mobilefoundation_short}} service dashboard.

* Click **Delete Analytics** to delete the  Mobile Analytics service that was added to the {{site.data.keyword.mobilefoundation_short}} service instance.

 Clicking **Delete Analytics** deletes the analytics server instance. The process of deleting analytics instance takes about 10 minutes. You can refresh the screen to view the updated status. Deletion of analytics instance reenables the **Add Analytics** button. If you choose to add the Mobile Analytics service again, you can click this button.


## Re-creating the MobileFirst server
{: #recreate_mobilefoundation_p1}

*	Click **Recreate** to re-create the server.

* This action stops your existing server and deletes the data. All the data in your mobile server is lost. A new server instance is created with an updated version, if available. This action takes a few minutes to complete.

##	Setting up advanced configuration
{: #using_mfs_advanced_p1}

Use the **Start Server with Advanced Configuration** from the `Overview` page to create the server with advanced or custom settings. You can also update the server settings to customize your server configuration by clicking the **Configuration** tab. {{site.data.keyword.mobilefoundation_short}} gives you access to some advanced settings.

*	From the **Topology** tab, you can select the server size and the number of instances you need. The default 1 GB server is enough for development and moderate testing.

  - Select the correct size for your server based on your need.

* **Nodes** displays the number of nodes that are created. This field is not editable in {{site.data.keyword.mobilefoundation_short}}: Developer. The number of nodes is defaulted to **1** in the Developer plan.-->

詳細については、[{{site.data.keyword.mobilefoundation_long}} documentation ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}を参照してください。
