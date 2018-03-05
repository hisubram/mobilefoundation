---

copyright:
  years: 2016, 2018
lastupdated:  "2018-02-14"

---

#	使用 Professional Per Capacity 方案
{: #using_mobilefoundation_p4}

使用 Professional Per Capacity 方案，使用者可以建立任意數量的行動應用程式，並使用多個行動作業系統。建立 {{site.data.keyword.mobilefoundation_short}}: Professional Per Capacity 服務實例之後，請閱讀下列程序，以開始使用該服務。

## 必要條件
{: #prerequisites_p4}

在配置 {{site.data.keyword.mobilefoundation_short}}: Professional Per Capacity 服務實例之前，請考量下列各項。
* 只有 {{site.data.keyword.Db2_on_Cloud_short}} {{site.data.keyword.Bluemix_notm}} 方案才支援 {{site.data.keyword.mobilefoundation_short}}: Professional Per Capacity。

* 您應該具有 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例認證的存取權，才能配置 {{site.data.keyword.mobilefoundation_short}} 服務實例的設定。

> **附註**：{{site.data.keyword.Db2_on_Cloud_short}} 服務實例可存在於 {{site.data.keyword.Bluemix_notm}} `Organization` 的任何 `Space` 中或您具有存取權的任何其他 `Organization` 中。請確定您具有存取 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例所存在的 `Space` 的許可權。


## 新增資料庫連線
{: #configure_dashdb_p4}

###  首要步驟
{: #firststeps_p4}

建立 {{site.data.keyword.mobilefoundation_short}}: Professional Per Capacity 服務實例之後，請遵循程序以開始使用。

### 設定 Db2 on Cloud 服務實例的連線
{: #connect_dashdb_p4}

在建立 {{site.data.keyword.mobilefoundation_short}}: Professional Per Capacity 服務實例之後，就會看到*概觀* 頁面，您將需要在這裡為 {{site.data.keyword.mobilefoundation_short}} 服務實例應該連接至其中的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例指定連線資訊。

如果尚未有 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例，也可以建立新的。

請遵循下列步驟，建立新的 Db2 on Cloud 服務實例：

1. 在*概觀* 頁面中，選取**建立新服務**區段。

+ 如果您想要高可用的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例，請在**高可用性配置**選項上選取`是`。

+ 檢閱方案詳細資料，然後按一下**建立**。

即會建立新的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例，其會提供具有 8GB RAM 及 2 個 vCPU，以及 500 GB 儲存空間的專用 {{site.data.keyword.Db2_on_Cloud_short}}實例。

請遵循下列步驟，來連接至現有的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例或您剛建立的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例。

1. 選取 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例所存在的 {{site.data.keyword.Bluemix_notm}} `Organization`。

+ 從所選取 `Organization` 中可用的空間清單，選取 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例所存在的 {{site.data.keyword.Bluemix_notm}} `Space`。   
> **附註：**如果您未看見，請列出 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例所存在的 `Organization` 和 `Space`，然後檢查您是否為該 `Organization` 和 `Space` 的成員。您需要對組織及空間具有*開發人員* 角色存取權，因為 {{site.data.keyword.mobilefoundation_short}} 服務會從 {{site.data.keyword.Db2_on_Cloud_short}} 服務存取認證。

+ 選取 {{site.data.keyword.Db2_on_Cloud_short}} `Service Name` 和 `Credentials`，以連接至現有的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例。

+  測試所指定的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例的連線。

+  按一下**新增**。此動作會在配置的 {{site.data.keyword.Db2_on_Cloud_short}} 資料庫服務實例中建立必要的表格。

幾秒過後，您就可以存取 `Overview` 頁面，而此頁面提供指導教學及視訊，協助您開始使用 {{site.data.keyword.mobilefoundation_short}} 服務。

> **附註**：您無法變更配置來供 {{site.data.keyword.mobilefoundation_short}} 服務實例使用的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例。不過，您能夠在多個 {{site.data.keyword.mobilefoundation_short}} 服務實例之間使用相同的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例，因為每一個 {{site.data.keyword.mobilefoundation_short}} 服務實例都會在所選取的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例中建立自己的綱目。

## 啟動 MobileFirst 伺服器
{: #start_mobilefoundation_p4}

* 若要以預設值啟動 {{site.data.keyword.mfserver_short_notm}}，請按一下**啟動基本伺服器**。

* 此選項會使用下列設定佈建 {{site.data.keyword.mfserver_long_notm}}：
    -  2 個節點，各具有 1 GB 記憶體。此大小適合進行開發、控管測試活動及小規模正式作業工作負載。

    -	自動產生 `username` 及 `password`。您可以在伺服器啟動並執行時存取它們。

會啟動伺服器的佈建處理程序。此處理程序大約需要 10 分鐘的時間，並且會出現一個訊息視窗，指出此作業的進度。完成時，會顯示一個儀表板，您可以在其中查看：

  -	執行中伺服器的狀態（狀態、大小）。

  -	為您建立伺服器路徑。您可以使用行動應用程式中的這個路徑來連接至 {{site.data.keyword.mfserver_short_notm}}。

  -	用來存取 {{site.data.keyword.mfp_oc_short_notm}} 的 `username` 和 `password`。會隱藏 `password`。按一下**顯示密碼**圖示，即可進行視覺化。

*	按一下**啟動主控台**，以啟動 {{site.data.keyword.mfp_oc_short_notm}}。


<!--This console runs inside the container.--> 使用這個主控台，您可以管理行動應用程式、配接器及行動裝置，使用伺服器作為行動後端、傳送推送通知，以及執行其他作業。

##  新增 Mobile Analytics 服務
{: #adding_analytics_server_p4}

 您現在可以在 {{site.data.keyword.mobilefirst}} 伺服器上監視您的行動應用程式，方法是將 Mobile Analytics 服務實例新增至 {{site.data.keyword.mobilefoundation_short}} 實例。

 <!--The Professional plan creates the Mobile Analytics service in a container group, the user can customize the configuration by selecting the number of container nodes in the container group.

 Users can also attach volumes to the containers to persist data. The volume once selected cannot be changed. 20 GB is the default file share space available to the user. If the user needs additional storage space to persist analytics data, he is required to buy additional file share and create a volume using this file share. He can then select this new volume while deploying the analytics server.

 For more information on adding volumes to {{site.data.keyword.containerlong}}, refer to [Storing persistent data in a volume by using the {{site.data.keyword.Bluemix_notm}} Dashboard ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.ng.bluemix.net/docs/containers/container_volumes_ov.html#container_volumes_ui.html){: new_window}.-->

* 按一下**新增分析**，以建立 Mobile Analytics 服務實例，並將其新增至 {{site.data.keyword.mobilefoundation_short}} 實例。

<!--* You can choose the Mobile Analytics service configuration, the minimum supported configuration for the Analytics server is 2 nodes with 1 GB memory each, you can choose to create an Analytics server up to a maximum configuration of 32 nodes with 16 GB memory each.-->

即會啟動佈建處理程序。此處理程序需要數分鐘的時間，並且會出現一個進度指示器，顯示此作業的進度。  

* 從 {{site.data.keyword.mfp_oc_short_notm}} 啟動 Mobile Analytics 服務主控台。

* 已啟用 {{site.data.keyword.mfserver_short_notm}} 與 Mobile Analytics 服務之間的單一登入。使用相同的 LTPA 金鑰及使用者認證將 Mobile Analytics 服務配置為 {{site.data.keyword.mfserver_short_notm}} 伺服器。您可以使用與用來登入 {{site.data.keyword.mfp_oc_short_notm}} 相同的 `username` 及 `password` 來登入 Mobile Analytics 主控台。

如需 Mobile Analytics 的相關資訊，您可以參閱 [MobileFirst Foundation Operational Analytics ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}。

> **附註：**刪除 {{site.data.keyword.mobilefoundation_short}} 服務實例會移除 Mobile Analytics 服務實例。

##  刪除 Mobile Analytics 服務
{: #deleting_analytics_server_p4}

現在您可以從 {{site.data.keyword.mobilefoundation_short}} 服務儀表板中刪除已新增至 {{site.data.keyword.mobilefoundation_short}} 服務實例的 Mobile Analytics 服務。

* 按一下**刪除分析**，刪除已新增至 {{site.data.keyword.mobilefoundation_short}} 服務實例的 Mobile Analytics 服務。

 按一下**刪除分析**會刪除分析伺服器實例。刪除分析實例的處理程序需要大約 10 分鐘。您可以重新整理畫面來檢視更新的狀態。刪除分析實例會重新啟用**新增分析**按鈕。如果您選擇重新新增 Mobile Analytics 服務，則可以按一下此按鈕。

## 重建 MobileFirst 伺服器
{: #recreate_mobilefoundation_p4}

*	按一下**重建**，以重建伺服器。

* 此動作會停止現有的伺服器，並刪除資料。隨即會使用更新的版本（如果有的話）建立新的伺服器實例。這個動作需要數分鐘的時間才能完成。

> **附註**：來自舊版伺服器實例的資料（包括應用程式及配接器的相關資訊）會持續保存在已配置的 {{site.data.keyword.Db2_on_Cloud_short}} 服務實例。這項資料可用來重建您的伺服器。

##	設定進階配置
{: #using_mfs_advanced_p4}

使用 `Overview` 頁面中的**使用進階配置啟動伺服器**頁面，以使用進階或自訂設定來建立伺服器。您也可以更新伺服器設定來自訂伺服器配置，方法是按一下**設定**標籤。{{site.data.keyword.mobilefoundation_short}} 可讓您存取一些進階設定。

*	從**拓蹼**標籤中，您可以視需要選取伺服器大小和實例數目。預設 1 GB 伺服器就足以進行開發及輕量型測試。
  - 根據您的需求，選取正確的伺服器大小。

  - **實例**會顯示已建立的實例數目。

      <!--- {{site.data.keyword.mobilefirst}} server farm can be created by configuring the number of nodes here. The minimum supported configuration is 2 nodes with 1 GB memory each and the maximum supported configuration is 32 nodes with 16 GB memory each.-->

如需詳細資料，請參閱 [{{site.data.keyword.mobilefoundation_long}} 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}。
