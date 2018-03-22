---

copyright:
  years: 2016, 2018
lastupdated:  "2018-03-07"

---

#	使用 Developer 方案
{: #using_mobilefoundation_p1}

使用 Developer 方案建立 {{site.data.keyword.mobilefoundation_short}} 服務實例之後，請存取 {{site.data.keyword.Bluemix_notm}} 上的 Overview 頁面。在此頁面上，提供指導教學及視訊，協助您開始使用服務。

## 使用 MobileFirst 伺服器
{: #start_mobilefoundation_p1}
* 您可以立即存取與使用 MobileFirst 伺服器。

  此選項會使用下列設定佈建 {{site.data.keyword.mfserver_long_notm}}：
  *	1 GB 的記憶體。此大小就足以進行開發、輕量型測試活動及小規模正式作業工作負載。

  * 使用 CLI 存取 MobileFirst 伺服器時需要認證，您可以從 IBM Cloud 主控台的左導覽窗格按一下**服務認證**來取得認證。

<!--  The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation. When complete a dashboard is displayed where you can see:
    *	The status of your server that is running (state, size).

    *	The server route created for you. Use this route in your mobile application to connect to the {{site.data.keyword.mfserver_short_notm}}.

    *	Your personal `username` and `password` to access the {{site.data.keyword.mfp_oc_short_notm}}. The `password` is hidden. Click **Show Password** icon to visualize it.

*	Click **Launch Console** to launch the {{site.data.keyword.mfp_oc_short_notm}}.-->

您現在可以管理行動應用程式及行動裝置、使用伺服器作為行動後端、傳送推送通知，以及執行其他作業。

## Mobile Analytics 服務
{: #adding_analytics_server_dev}

Mobile Foundation: Developer 方案服務實例已包含並預先配置 Mobile Analytics 伺服器。

<!-- You can now monitor your mobile application on {{site.data.keyword.mobilefirst}} server by adding a Mobile Analytics service to the {{site.data.keyword.mobilefoundation_short}} service instance. Developer plan creates the Mobile Analytics service in a container group with a single node having 1 GB memory.

* Click **Add Analytics** to add the Mobile Analytics service to the {{site.data.keyword.mobilefoundation_short}} service instance.

  The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation.  -->

* 從 {{site.data.keyword.mfp_oc_short_notm}} 啟動 Mobile Analytics 服務主控台。

* 已啟用 {{site.data.keyword.mfserver_short_notm}} 與 Mobile Analytics 服務之間的單一登入。使用相同的 LTPA 金鑰及使用者認證將 Mobile Analytics 服務配置為 {{site.data.keyword.mfserver_short_notm}}。您可以使用與用來登入
{{site.data.keyword.mfp_oc_short_notm}} 相同的
`username` 及 `password` 來登入 Mobile Analytics 主控台。

如需 Mobile Analytics 的相關資訊，您可以參閱 [MobileFirst Foundation Operational Analytics ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}。

> **附註：**刪除 {{site.data.keyword.mobilefoundation_short}} 服務實例會移除 Mobile Analytics 服務實例。

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

如需詳細資料，請參閱 [{{site.data.keyword.mobilefoundation_long}} 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}。
