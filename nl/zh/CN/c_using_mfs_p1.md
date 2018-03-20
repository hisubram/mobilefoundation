---

copyright:
  years: 2016, 2018
lastupdated:  "2018-03-07"

---

#	使用 Developer 套餐
{: #using_mobilefoundation_p1}

使用 Developer 套餐创建 {{site.data.keyword.mobilefoundation_short}} 服务实例后，请访问 {{site.data.keyword.Bluemix_notm}} 上的“概述”页面。在此页面上，为您提供了教程和视频，可帮助您开始使用服务。

## 使用 MobileFirst 服务器
{: #start_mobilefoundation_p1}
* 您可以立即访问和使用 MobileFirst 服务器。

  此选择将为 {{site.data.keyword.mfserver_long_notm}} 供应以下设置：
  *	1 GB 内存。此大小足够用于开发、轻量测试活动和小规模生产工作负载。

  * 要使用 CLI 访问 MobileFirst 服务器，您将需要凭证。在 IBM Cloud 控制台的左侧导航窗格中单击**服务凭证**即可获取凭证。

<!--  The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation. When complete a dashboard is displayed where you can see:
    *	The status of your server that is running (state, size).

    *	The server route created for you. Use this route in your mobile application to connect to the {{site.data.keyword.mfserver_short_notm}}.

    *	Your personal `username` and `password` to access the {{site.data.keyword.mfp_oc_short_notm}}. The `password` is hidden. Click **Show Password** icon to visualize it.

*	Click **Launch Console** to launch the {{site.data.keyword.mfp_oc_short_notm}}.-->

现在，您可以管理移动应用程序和移动设备、使用服务器作为移动后端以及发送推送通知等等。

## Mobile Analytics 服务
{: #adding_analytics_server_dev}

Mobile Foundation: Developer 套餐服务实例中包含并预配置了 Mobile Analytics 服务器。

<!-- You can now monitor your mobile application on {{site.data.keyword.mobilefirst}} server by adding a Mobile Analytics service to the {{site.data.keyword.mobilefoundation_short}} service instance. Developer plan creates the Mobile Analytics service in a container group with a single node having 1 GB memory.

* Click **Add Analytics** to add the Mobile Analytics service to the {{site.data.keyword.mobilefoundation_short}} service instance.

  The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation.  -->

* 从 {{site.data.keyword.mfp_oc_short_notm}} 启动 Mobile Analytics 服务控制台。

* 在 {{site.data.keyword.mfserver_short_notm}} 与 Mobile Analytics 服务之间启用单点登录。为 Mobile Analytics 服务配置与 {{site.data.keyword.mfserver_short_notm}} 相同的 LTPA 密钥和用户凭证。您可以像登录 {{site.data.keyword.mfp_oc_short_notm}} 一样，使用相同的 `username` 和 `password` 登录到“移动分析”控制台。

有关 Mobile Analytics 的更多信息，请参阅 [MobileFirst Foundation Operational Analytics ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}。

> **注：**删除 {{site.data.keyword.mobilefoundation_short}} 服务实例将除去 Mobile Analytics 服务实例。

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

请参阅 [{{site.data.keyword.mobilefoundation_long}} 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}，以了解更多详细信息。
