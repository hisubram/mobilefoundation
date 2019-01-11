---

copyright:
  years: 2018
lastupdated: "2018-11-29"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 管理應用程式版本
{: #manage_app_versions}

Mobile Foundation 應用程式管理功能可讓「Mobile Foundation 伺服器」使用者及管理者更精細地控制使用者及裝置對其應用程式的存取。

Mobile Foundation 伺服器會追蹤所有存取行動基礎架構的嘗試，並且儲存應用程式、使用者以及安裝該應用程式之裝置的相關資訊。應用程式、使用者與裝置之間的對映，會形成伺服器行動應用程式管理功能的基礎。

您可以使用「Mobile Foundation 作業」主控台來監視及管理資源的存取，也可以管理特定應用程式版本。

1.  從顯示的**版本**清單中，移至「Mobile Foundation 作業」主控台、按一下**應用程式**、選擇您要管理的應用程式、選取您感興趣的特定應用程式版本。
    ![管理應用程式版本](images/app_version_management.png)

2. 在**管理**標籤下，您會看到針對所選取應用程式版本設定應用程式狀態的選項。支援的應用程式版本狀態為：
   * 作用中
   * 作用中且通知中
   * 已停用存取權
3. 從**應用程式存取 > 狀態**中選取*已停用存取權* 選項，可以停用應用程式版本。
4. 在**直接更新**區段中，您也可以配置成上傳 Cordova 應用程式的已更新 Web 資源。接著，會向使用此特定應用程式版本連接至 Mobile Foundation 伺服器的使用者，呈現使用「直接更新」來更新其應用程式的選項。
5. 您也可以使用**動作**功能表中的下列選項，對選取的應用程式版本執行下列動作：
   *  刪除版本
   *  複製版本
   *  匯出版本


請參閱[管理裝置](manage_devices.html)，以瞭解如何管理裝置。請參閱[遠端停用應用程式版本](remote_disable_app_version.html)，以瞭解遠端停用應用程式的特定版本。
{: note}

