---

copyright:
  years: 2016, 2018
lastupdated:  "2018-02-13"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:tip: .tip}

# 入门教程
{: #gettingstartedtemplate}

{{site.data.keyword.mobilefoundation_long}} 加快设置 {{site.data.keyword.mfp_full}} 环境，您可使用此环境开发、测试和操作企业移动应用程序。{{site.data.keyword.mobilefoundation_short}} 提供了以下不同服务套餐：Developer、Developer Pro、Professional Per Capacity 和 Professional 1 Application。
{:shortdesc}

使用 Professional 1 Application 套餐，可管理任何或所有受支持操作系统（例如 Android、iOS、Windows 或移动 Web）上构建的单个应用程序。Developer 套餐最适合进行开发和测试。您可以在[此处](https://console.bluemix.net/catalog/services/mobile-foundation)查看所有可用的套餐。

## 开始之前
{: #prereqs}

您将需要 {{site.data.keyword.Bluemix}} 帐户和 {{site.data.keyword.mobilefoundation_short}} 服务的实例。

## 步骤 1：创建 {{site.data.keyword.mobilefoundation_short}} 服务的实例
{: #step1create}

1. 在 {{site.data.keyword.Bluemix_notm}} **目录**中，选择 **{{site.data.keyword.mobilefoundation_short}}**。这将打开服务配置屏幕。
2. 对您的服务实例命名，或者使用预设的名称。
3. 选择想要在其中创建服务实例的区域、组织和空间。
4. 选择**价格套餐**，然后单击**创建**。

## 步骤 2：构建移动通道
{: #buildmobilechannel}

### 对于 {{site.data.keyword.mobilefoundation_short}}: Developer 套餐
{: #buildchanneldevplan}

创建 {{site.data.keyword.mobilefoundation_short}}: Developer 的实例后，可以通过完成以下步骤开始构建移动通道。

  1.  使用缺省配置创建 {{site.data.keyword.mobilefirst_notm}} 服务器实例，然后单击**启动基本服务器**。

    `基本服务器实例包含单一节点和 1 GB 内存。`

  +	自动为您生成 `username` 和 `password`。服务器启动并运行时，您可以对其进行访问。

    供应过程启动。此过程会花费大约 10 分钟，并且消息窗口会指示此操作的进度。完成后，会显示仪表板，在其中您可以看到：
    * 正在运行的服务器的状态（状态和大小）。
    *	为您创建的服务器路径。在您的移动应用程序中使用此路径可连接到 {{site.data.keyword.mfserver_short_notm}}。
    *	用于访问 {{site.data.keyword.mfp_oc_short_notm}} 的个人 `username` 和 `password`。`password` 会隐藏。单击**显示密码**图标以使其可见。

  + 单击**启动控制台**以启动 {{site.data.keyword.mfp_oc_short_notm}}。

要使用拓扑、安全性和其他服务器配置的高级配置来创建 {{site.data.keyword.mobilefirst_notm}} 服务器实例，请单击**使用高级配置启动服务器**。请参阅[设置高级配置](c_using_mfs_p1.html#using_mfs_advanced_p1)，以获取更多信息。
{: tip}

### 对于 {{site.data.keyword.mobilefoundation_short}}: Developer Pro 套餐
{: #buildchanneldevproplan}

创建 {{site.data.keyword.mobilefoundation_short}}: Developer Pro 服务的实例后，可以通过完成以下步骤开始构建移动通道。

  1.  连接到 {{site.data.keyword.Bluemix_notm}} 上的某个现有 {{site.data.keyword.Db2_on_Cloud_short}} 服务。

      1.  选择 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例所在的 {{site.data.keyword.Bluemix_notm}} `组织`。

      + 从所选`组织`内可用的空间列表中，选择具有 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例的 {{site.data.keyword.Bluemix_notm}} `空间`。

      + 选择 {{site.data.keyword.Db2_on_Cloud_short}} `服务名称`和`凭证`以连接到现有 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例。

      + 通过单击**测试连接**，测试与所选 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例的连接。

      + 单击**添加**，然后会出现弹出窗口，询问您是否确认所选的 {{site.data.keyword.Db2_on_Cloud_short}} 服务，请单击**继续**。此操作可在配置的 {{site.data.keyword.Db2_on_Cloud_short}} 数据库服务实例中创建需要的表。

      > **注：**添加与 {{site.data.keyword.mobilefoundation_short}} 实例的 {{site.data.keyword.Db2_on_Cloud_short}} 连接后，您将无法对其进行更改。

  2.  创建并启动服务器。

      1. 使用缺省配置创建 {{site.data.keyword.mobilefirst_notm}} 服务器实例，然后单击**启动基本服务器**。

      + 此选择将为 {{site.data.keyword.mfserver_long_notm}} 供应以下设置：

          - 单个节点具有 1GB 内存。此大小足够用于开发、中等测试活动和小规模生产工作负载。

          -	自动为您生成 `username` 和 `password`。服务器启动并运行时，您可以对其进行访问。

          供应服务器的过程启动。此过程会花费大约 10 分钟，并且消息窗口会指示此操作的进度。完成后，会显示仪表板，在其中您可以看到：
            -	正在运行的服务器的状态（状态和大小）。
            -	为您创建了服务器路径。在您的移动应用程序中使用此路径可连接到 {{site.data.keyword.mfserver_short_notm}}。
            -	用于访问 {{site.data.keyword.mfp_oc_short_notm}} 的个人 `username` 和 `password`。`password` 会隐藏。单击**显示密码**图标以使其可见。

      +	单击**启动控制台**以打开 {{site.data.keyword.mfp_oc_short_notm}}。  

      要使用拓扑、安全性和其他服务器配置的高级配置来创建 {{site.data.keyword.mobilefirst_notm}} 服务器实例，请单击**使用高级配置启动服务器**。请参阅[设置高级配置](c_using_mfs_p3.html#using_mfs_advanced_p3)，以获取更多信息。
      {: tip}

### 对于 {{site.data.keyword.mobilefoundation_short}}: Professional Per Capacity 套餐
{: #buildchannelprofcapacityplan}

创建 {{site.data.keyword.mobilefoundation_short}}: Professional Per Capacity 服务的实例后，可以通过完成以下步骤开始构建移动通道。

  1.  连接到 {{site.data.keyword.Bluemix_notm}} 上的某个现有 {{site.data.keyword.Db2_on_Cloud_short}} 服务。

      1.  选择 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例所在的 {{site.data.keyword.Bluemix_notm}} `组织`。

      + 从所选`组织`内可用的空间列表中，选择具有 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例的 {{site.data.keyword.Bluemix_notm}} `空间`。

      + 选择 {{site.data.keyword.Db2_on_Cloud_short}} `服务名称`和`凭证`以连接到现有 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例。

      + 通过单击**测试连接**，测试与所选 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例的连接。

      + 单击**添加**，然后会出现弹出窗口，询问您是否确认所选的 {{site.data.keyword.Db2_on_Cloud_short}} 服务，请单击**继续**。此操作可在配置的 {{site.data.keyword.Db2_on_Cloud_short}} 数据库服务实例中创建需要的表。

      > **注：**添加与 {{site.data.keyword.mobilefoundation_short}} 实例的 {{site.data.keyword.Db2_on_Cloud_short}} 连接后，您将无法对其进行更改。

  2.  创建并启动服务器。

      1. 使用缺省配置创建 {{site.data.keyword.mobilefirst_notm}} 服务器实例，然后单击**启动基本服务器**。

      + 此选择将为 {{site.data.keyword.mfserver_long_notm}} 供应以下设置：
          -  2 个节点，每个 1GB 内存。此大小适合用于开发、中等测试活动和小规模生产工作负载。

          -	自动为您生成 `username` 和 `password`。服务器启动并运行时，您可以对其进行访问。

          供应服务器的过程启动。此过程会花费大约 10 分钟，并且消息窗口会指示此操作的进度。完成后，会显示仪表板，在其中您可以看到：
            -	正在运行的服务器的状态（状态和大小）。
            -	为您创建了服务器路径。在您的移动应用程序中使用此路径可连接到 {{site.data.keyword.mfserver_short_notm}}。
            -	用于访问 {{site.data.keyword.mfp_oc_short_notm}} 的个人 `username` 和 `password`。`password` 会隐藏。单击**显示密码**图标以使其可见。

      +	单击**启动控制台**以打开 {{site.data.keyword.mfp_oc_short_notm}}。      

      要使用拓扑、安全性和其他服务器配置的高级配置来创建 {{site.data.keyword.mobilefirst_notm}} 服务器实例，请单击**使用高级配置启动服务器**。请参阅[设置高级配置](c_using_mfs_p4.html#using_mfs_advanced_p4)，以获取更多信息。
      {: tip}

### 对于 {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application 套餐
{: #buildchannelprof1appplan}

创建 {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application 服务的实例后，可以通过完成以下步骤开始构建移动通道。

  1.  连接到 {{site.data.keyword.Bluemix_notm}} 上的某个现有 {{site.data.keyword.Db2_on_Cloud_long}} 服务。

      1.  选择 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例所在的 {{site.data.keyword.Bluemix_notm}} `组织`。

      + 从所选`组织`内可用的空间列表中，选择具有 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例的 {{site.data.keyword.Bluemix_notm}} `空间`。

      + 选择 {{site.data.keyword.Db2_on_Cloud_short}} `服务名称`和`凭证`以连接到现有 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例。

      + 通过单击**测试连接**，测试与所选 {{site.data.keyword.Db2_on_Cloud_short}} 服务实例的连接。

      + 单击**添加**，然后会出现弹出窗口，询问您是否确认所选的 {{site.data.keyword.Db2_on_Cloud_short}} 服务，请单击**继续**。此操作可在配置的 {{site.data.keyword.Db2_on_Cloud_short}} 数据库服务实例中创建需要的表。

      > **注：**添加与 {{site.data.keyword.mobilefoundation_short}} 实例的 {{site.data.keyword.Db2_on_Cloud_short}} 连接后，您将无法对其进行更改。

  2.  创建并启动服务器。

      1. 使用缺省配置创建 {{site.data.keyword.mobilefirst_notm}} 服务器实例，然后单击**启动基本服务器**。

        `基本服务器实例包含单一节点和 1 GB 内存。`

      + 自动为您生成 `username` 和 `password`。服务器启动并运行时，您可以对其进行访问。  

        供应服务器的过程启动。此过程会花费大约 10 分钟，并且消息窗口会指示此操作的进度。完成后，会显示仪表板，在其中您可以看到：
          -	正在运行的服务器的状态（状态和大小）。
          -	为您创建了服务器路径。在您的移动应用程序中使用此路径可连接到 {{site.data.keyword.mfserver_short_notm}}。
          -	用于访问 {{site.data.keyword.mfp_oc_short_notm}} 的个人 `username` 和 `password`。`password` 会隐藏。单击**显示密码**图标以使其可见。

      +  单击**启动控制台**以打开 {{site.data.keyword.mfp_oc_short_notm}}。  

      要使用拓扑、安全性和其他服务器配置的高级配置来创建 {{site.data.keyword.mobilefirst_notm}} 服务器实例，请单击**使用高级配置启动服务器**。请参阅[设置高级配置](c_using_mfs_p2.html#using_mfs_advanced_p2)，以获取更多信息。
      {: tip}

转至[使用 Mobile Foundation 服务设置 MobileFirst 服务器 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/bluemix/using-mobile-foundation/){: new_window}，以了解有关开始使用 {{site.data.keyword.mobilefoundation_short}} 的更多信息。
{: tip}

## 步骤 3：在 {{site.data.keyword.mobilefoundation_short}} 中注册应用程序
{: #registerapp}

创建并启动 Mobile Foundation 服务器实例后，您可以按照以下步骤注册 Android 应用程序。

  1.  装入 URL http://your-server-host:server-port/mfpconsole 以启动 {{site.data.keyword.mfp_oc_short_notm}}。使用供应时生成的 `username` 和 `password`。

  + 在 {{site.data.keyword.mfp_oc_short_notm}} **仪表板**中，单击**应用程序**旁边的**新建**。

  + 提供 *MFPStarterAndroid* 作为**应用程序名称**。

  + **选择平台**为 *Android*。

  + 提供 *com.ibm.mfpstarterandroid* 作为**应用程序标识**。

  + 输入 *1.0* 作为**版本**。

  + 单击**注册应用程序**。

## 步骤 4：下载样本应用程序
{: #downloadapp}

  1.  在 {{site.data.keyword.mfp_oc_short_notm}} **仪表板**中，选择**应用程序**下的 **MFPStarterAndroid**。

  + 单击**获取入门模板代码**，然后选择下载 Android 应用程序样本。

## 步骤 5：编辑样本应用程序
{: #editapp}

  1. 将上述步骤中下载的样本 Android 应用程序导入到 Android Studio。

  + 在 Android Studio 的**项目**侧边栏菜单中，选择 **app → java → com.ibm.mfpstarterandroid → ServerConnectActivity.java** 文件。

    * 添加以下导入项
      ```java
      import java.net.URI;
      import java.net.URISyntaxException;
      import android.util.Log;
      ```
      {: codeblock}

    * 粘贴以下代码片段，将调用替换为 `WLAuthorizationManager.getInstance().obtainAccessToken`

        ```java
          WLAuthorizationManager.getInstance().obtainAccessToken("", new WLAccessTokenListener() {
            @Override
            public void onSuccess(AccessToken token) {
                System.out.println("Received the following access token value: " + token);
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        titleLabel.setText("Yay!");
                        connectionStatusLabel.setText("Connected to MobileFirst Server");
                    }
                });

                URI adapterPath = null;
                try {
                    adapterPath = new URI("/adapters/javaAdapter/resource/greet");
                } catch (URISyntaxException e) {
                    e.printStackTrace();
                }

                WLResourceRequest request = new WLResourceRequest(adapterPath, WLResourceRequest.GET);

                request.setQueryParameter("name","world");
                request.send(new WLResponseListener() {
                    @Override
                    public void onSuccess(WLResponse wlResponse) {
                        // Will print "Hello world" in LogCat.
                        Log.i("MobileFirst Quick Start", "Success: " + wlResponse.getResponseText());
                    }

                    @Override
                    public void onFailure(WLFailResponse wlFailResponse) {
                        Log.i("MobileFirst Quick Start", "Failure: " + wlFailResponse.getErrorMsg());
                    }
                });
            }

            @Override
            public void onFailure(WLFailResponse wlFailResponse) {
                System.out.println("Did not receive an access token from server: " + wlFailResponse.getErrorMsg());
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        titleLabel.setText("Bummer...");
                        connectionStatusLabel.setText("Failed to connect to MobileFirst Server");
                    }
                });
            }
        });
        ```
        {: codeblock}  

## 步骤 6：部署适配器
{: #deployadapter}

  1. 下载此[适配器工件](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/javaAdapter.adapter){: download}，并从 {{site.data.keyword.mfp_oc_short_notm}} 使用**操作 → 部署适配器**进行部署。

## 步骤 7：测试应用程序
{: #testapp}

  1. 在 Android Studio 的**项目**侧边栏菜单中，选择 **app → src → main →assets → mfpclient.properties** 文件，然后使用 MobileFirst 服务器的正确值编辑 `protocol`、`host` 和 `port` 属性。

   值通常是 https、*your-server-address* 和 443。
   {: tip}

  2. 单击 Android Studio 中的**运行应用程序**。
     * 您将看到应用程序在设备仿真器上启动。
     * 在启动的应用程序中，单击**对 MobileFirst 服务器执行 Ping 操作**按钮，这将显示`已连接到 MobileFirst 服务器`。
     * 如果应用程序能够连接到 MobileFirst 服务器，将使用部署的 Java 适配器执行资源请求调用。
     * 然后将适配器响应打印到 Android Studio 的 LogCat 视图中。


## 后续步骤
{: #nextsteps}

您可以遵循[快速入门教程 ![外部链接图标](../../icons/launch-glyph.svg "快速入门教程")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/){: new_window} 来使用更多样本应用程序并探索 {{site.data.keyword.mobilefoundation_short}} 的工作方式。“快速入门”提供了一些教程，其中解释了 {{site.data.keyword.mobilefoundation_short}} 针对 iOS、Android、Web、Cordova、Windows 和 Xamarin 应用程序的工作方式。

# 相关链接
{: #rellinks  notoc}

## 相关链接
{: #general notoc}

*	[IBM MobileFirst Platform Foundation V8.0.0 产品文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}
*	[IBM MobileFirst Platform Developer Center ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://mobilefirstplatform.ibmcloud.com){: new_window}
