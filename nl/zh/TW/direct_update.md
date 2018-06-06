---

copyright:
  years: 2018
lastupdated:  "2018-05-11"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

#	Cordova 應用程式中的直接更新
{: #direct_update_cordova_apps}

Cordova 應用程式中的 Web 資源（JavaScript、HTML、CSS 或影像檔）可以透過*無線傳輸 (OTA)* 方式，利用「直接更新」來進行更新。使用「直接更新」特性，企業可以確保一般使用者使用其應用程式的最新版本。
若要更新應用程式，應用程式的已更新 Web 資源需要包裝並上傳至「MobileFirst 伺服器」，方法為使用 MobileFirst CLI 或部署已產生的保存檔。然後，「直接更新」即會自動啟動。接著，會強制執行每一位使用者對受保護資源的要求。

Cordova iOS 及 Cordova Android 平台中支援「直接更新」。

基本開發及測試目的，開發人員通常會藉由僅將保存檔上傳至開發伺服器來使用「直接更新」。儘管此處理程序易於實作，但它並不安全。對於此階段，會使用從內嵌的 MobileFirst 自簽憑證中擷取的內部 RSA 金鑰組。

不過，對於即時生產或正式作業前測試階段，建議先實作安全的「直接更新」，再將您的應用程式發佈至應用程式市集。安全「直接更新」需要從實際 CA 簽署伺服器憑證中擷取的 RSA 金鑰組。

* 請小心，不要在發佈應用程式之後，修改金鑰儲存庫配置。在利用新的公開金鑰重新配置應用程式，並重新發佈應用程式之前，無法鑑別已下載的更新項目。如果您未執行上述兩個步驟，則「直接更新」無法在用戶端上進行。
* 「直接更新」只會更新應用程式的 Web 資源。若要更新原生資源，必須將新的應用程式版本提交給各自的應用程式市集。
* 使用「直接更新」特性，並已啟用 Web 資源總和檢查特性時，會使用每一個「直接更新」來建立新的總和檢查基礎。
* 如果已使用修正套件來升級「MobileFirst 伺服器」，則它會繼續適當地提供直接更新項目。不過，如果已上傳最近建置的「直接更新」保存檔（.zip 檔案），則它可能會中止更新至舊版用戶端。原因是此保存檔包含 `cordova-plugin-mfp` 外掛程式的版本。在它將該保存檔提供給行動用戶端之前，伺服器會比較用戶端版本與外掛程式版本。如果這兩個版本相當接近（表示三個最有效位數相同），則「直接更新」即會正常發生。否則，「MobileFirst 伺服器」會無聲自動跳過更新。版本不符的解決方案為下載版本相同的 `cordova-plugin-mfp`，作為原始 Cordova 專案中的外掛程式，並重新產生「直接更新」保存檔。
* 最佳狀況為單一「MobileFirst 伺服器」可用每秒 250 MB 的速率，將資料推送至用戶端。如果需要更高的速率，請考量叢集或 CDN 服務。
{: tip}

## 「直接更新」的運作方式？
{: #working_direct_update}

應用程式 Web 資源起初會與應用程式一起包裝，以確保第一個離線可用性。之後，應用程式會根據每個「MobileFirst 伺服器」要求來檢查更新項目。

>**附註：**執行「直接更新」之後，會在 60 分鐘之後重新檢查。

![直接更新運作方式的圖表](images/internal_function.jpg)

在「直接更新」之後，應用程式不再使用預先包裝的 Web 資源。它會改用從應用程式的沙盤推演中下載的 Web 資源。如果清除了裝置上的應用程式快取，則會再次使用原始包裝的 Web 資源。

>「直接更新」僅適用於特定版本。換言之，針對 2.0 版應用程式產生的更新項目無法套用至相同應用程式的不同版本。

## 建立及部署已更新的 Web 資源
{: #creating_deploying_updates}

已更新的 Web 資源需要包裝並上傳至「MobileFirst 伺服器」。

1. 開啟指令行視窗，並導覽至 Cordova 專案的根目錄。
2. 執行指令：
  ```
  mfpdev app webupdate
  ```
  {: pre}
`mfpdev app webupdate` 指令會將已更新的 Web 資源包裝為 `.zip` 檔案，然後將它上傳至在開發人員工作站中執行的預設「MobileFirst 伺服器」。您可在 `[cordova-project-root-folder]/mobilefirst/` 資料夾中找到包裝的 Web 資源。

**替代步驟：**

* 建置 `.zip` 檔案，並將它上傳至不同的「MobileFirst 伺服器」：`mfpdev app webupdate [server-name] [runtime-name]`。
  例如：
  ```
  mfpdev app webupdate myQAServer MyBankApps
  ```
  {: pre}

* 上傳先前產生的 `.zip` 檔案：`mfpdev app webupdate [server-name] [runtime-name] --file [path-to-packaged-web-resources]`。
  例如：
  ```
  mfpdev app webupdate myQAServer MyBankApps --file mobilefirst/ios/com.mfp.myBankApp-1.0.1.zip
  ```
  {: pre}

* 手動將包裝的 Web 資源上傳至「MobileFirst 伺服器」：
  1. 建置 .zip 檔案，但不上傳：
      ```
      mfpdev app webupdate --build
      ```
      {: pre}
  2. 載入「MobileFirst 作業主控台」，然後按一下應用程式項目。
  3. 按一下**上傳 Web 資源檔案**來上傳包裝的 Web 資源。    
      ![從主控台中上傳「直接更新」.zip 檔案](images/upload-direct-update-package.png)

執行指令 `mfpdev help app webupdate` 來進一步瞭解。
{: tip}

## 使用者體驗
{: #user_experience}

在接收「直接更新」之後，依預設會顯示一個對話框，然後會要求使用者提供啟動更新程序的許可權。在使用者核准之後，會顯示一個進度列對話框，並下載 Web 資源。在完成更新之後，即會自動重新載入應用程式。

![直接更新範例](images/direct-update-flow.png)

## 自訂直接更新使用者介面
{: #customize_du_ui}

您可以自訂呈現給一般使用者的「直接更新使用者介面」。
在 **index.js** 的 `wlCommonInit()` 函數內新增下列程式碼：
```JavaScript
wl_DirectUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext) {
    // Implement custom Direct Update logic
};
```
{: codeblock}

*directUpdateData* 是包含 downloadSize 內容的 JSON 物件，此內容代表要從「MobileFirst 伺服器」下載之更新套件的檔案大小（以位元組為單位）。
*directUpdateContext* 是顯示 .start() 及 .stop() 函數（用來啟動及停止「直接更新」流程）的 JavaScript 物件。

如果「MobileFirst 伺服器」上的 Web 資源比應用程式中的 Web 資源還新，則「直接更新」盤查資料會新增至伺服器回應。每當 MobileFirst 用戶端架構偵測到這個直接更新盤查時，它就會呼叫 `wl_directUpdateChallengeHandler.handleDirectUpdate` 函數。

此函數提供預設「直接更新」設計：在「直接更新」可用時顯示的預設訊息對話框，以及在起始直接更新程序時顯示的預設進度畫面。您可以藉由置換此函數，並實作您自己的邏輯，來實作自訂的「直接更新」使用者介面行為，或自訂「直接更新」對話框。

在下面的程式碼範例中，`handleDirectUpdate` 函數會在「直接更新」對話框中實作自訂訊息。請將這個程式碼新增至 Cordova 專案的 `www/js/index.js` 檔案。
自訂的「直接更新」使用者介面的其他範例：
* 使用協力廠商 JavaScript 架構（例如 jQuery Mobile、Ionic、…）所建立的對話框
* 執行 Cordova 外掛程式的完全原生使用者介面
* 呈現給使用者的替代 HTML，其中具有選項等。

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext) {        
    navigator.notification.confirm(  // Creates a dialog.
        'Custom dialog body text',
        // Handle dialog buttons.
          directUpdateContext.start();
        },
        'Custom dialog title text',
        ['Update']
    );
};
```
{: codeblock}

每當使用者按一下對話框按鈕時，您就可以藉由執行 `directUpdateContext.start()` 方法來啟動「直接更新」程序。這時會顯示預設進度畫面，類似舊版「MobileFirst 伺服器」中的進度畫面。

此方法支援下列類型的呼叫：
* 未指定任何參數時，「MobileFirst 伺服器」會使用預設進度畫面。
* 提供接聽器函數（例如 `directUpdateContext.start(directUpdateCustomListener)`）時，若「直接更新」程序將生命週期事件傳送至接聽器，則此程序會在背景中執行。自訂接聽器必須實作下列方法：

```JavaScript
var  directUpdateCustomListener  = {
    onStart : function ( totalSize ){ },
    onProgress : function ( status , totalSize , completedSize ){ },
    onFinish : function ( status ){ }
};
```
{: codeblock}

接聽器方法根據下列規則在直接更新程序進行期間啟動：
* 使用保留更新檔案大小的 `totalSize` 參數來呼叫 `onStart`。
* 多次呼叫 `onProgress`，狀態為 `DOWNLOAD_IN_PROGRESS`、`totalSize` 及 `completedSize`（到目前為止下載的容量）。
* 呼叫 `onProgress`，狀態為 `UNZIP_IN_PROGRESS`。
* 呼叫 `onFinish`，最終狀態碼為下列其中一個：

| 狀態碼 | 說明 |
|:------------|:------------|
| `SUCCESS` | 直接更新已完成，沒有發生任何錯誤。|
| `CANCELED` | 已取消直接更新（例如，因為已呼叫 `stop()` 方法）。|
| `FAILURE_NETWORK_PROBLEM` | 更新期間網路連線發生問題。|
| `FAILURE_DOWNLOADING` | 未完整地下載檔案。|
| `FAILURE_NOT_ENOUGH_SPACE` | 裝置上沒有足夠空間可用來下載並解壓縮更新檔案。|
| `FAILURE_UNZIPPING` | 解壓縮更新檔案時發生問題。|
| `FAILURE_ALREADY_IN_PROGRESS` | 直接更新已在執行中時，已呼叫啟動方法。|
| `FAILURE_INTEGRITY` | 無法驗證更新檔案的確實性。|
| `FAILURE_UNKNOWN` | 非預期的內部錯誤。|

如果實作自訂直接更新接聽器，則您必須確保在直接更新程序完成時重新載入應用程式，並已呼叫 `onFinish()` 方法。如果直接更新程序無法順利完成，您也必須呼叫 `wl_directUpdateChalengeHandler.submitFailure()`。

下列範例顯示自訂直接更新接聽器的實作：
```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
    //show custom progress dialog
  },
  onProgress: function(status,totalSize,completedSize){
    //update custom progress dialog
  },
  onFinish: function(status){

    if (status == 'SUCCESS'){
      //show success message
      WL.Client.reloadApp();
    }
    else {
      //show custom error message

      //submitFailure must be called is case of error
      wl_directUpdateChallengeHandler.submitFailure();
    }
  }
};

wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  WL.SimpleDialog.show('Update Avalible', 'Press update button to download version 2.0', [{
    text : 'update',
    handler : function() {
      directUpdateContext.start(directUpdateCustomListener);
    }
  }]);
};
```
{: codeblock}

### 情境：執行無使用者介面的直接更新
{: #scenario-running-ui-less-direct-updates }
當應用程式在前景時，{{site.data.keyword.mobilefoundation_short}} 支援無使用者介面的直接更新。

若要執行無使用者介面的直接更新，請實作 `directUpdateCustomListener`。請將空的函數實作提供給 `onStart` 及 `onProgress` 方法。空的實作會導致直接更新程序在背景中執行。

若要完成直接更新程序，必須重新載入應用程式。下列是可用的選項：
* `onFinish` 方法也可以是空的。在此情況下，將在重新啟動應用程式之後套用直接更新。
* 您可以實作一個自訂對話框，通知或要求使用者重新啟動應用程式。（請參閱下列範例。）
* `onFinish` 方法可以藉由呼叫 `WL.Client.reloadApp()` 來強制重新載入應用程式。

以下是 `directUpdateCustomListener` 的範例實作：

```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
  },
  onProgress: function(status,totalSize,completeSize){
  },
  onFinish: function(status){
    WL.SimpleDialog.show('New Update Available', 'Press reload button to update to new version', [ {
      text : WL.ClientMessages.reload,
      handler : WL.Client.reloadApp
    }]);
  }
};
```
{: codeblock}

實作 `wl_directUpdateChallengeHandler.handleDirectUpdate` 函數。將已建立的 `directUpdateCustomListener` 實作當作參數傳遞至函數。請確定已呼叫 `directUpdateContext.start(directUpdateCustomListener`。以下是範例 `wl_directUpdateChallengeHandler.handleDirectUpdate` 實作：

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  directUpdateContext.start(directUpdateCustomListener);
};
```
{: codeblock}

**附註：**將應用程式傳送至背景時，會暫停直接更新程序。

### 情境：處理直接更新失敗
{: #scenario-handling-a-direct-update-failure }
此情境顯示如何處理由於失去連線功能（例如）而可能造成的直接更新失敗。在此情境中，即使處於離線模式，也會阻止使用者使用應用程式。畫面上會顯示一個對話框，提供使用者重試的選項。

建立一個廣域變數，來儲存直接更新環境定義，讓您可在後續當直接更新程序失敗時使用它。例如：

```JavaScript
var savedDirectUpdateContext;
```
{: codeblock}

實作直接更新盤查處理程式。在這裡儲存直接更新環境定義。例如：

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  savedDirectUpdateContext = directUpdateContext; // save direct update context

  var downloadSizeInMB = (directUpdateData.downloadSize / 1048576).toFixed(1).replace(".", WL.App.getDecimalSeparator());
  var directUpdateMsg = WL.Utils.formatString(WL.ClientMessages.directUpdateNotificationMessage, downloadSizeInMB);

  WL.SimpleDialog.show(WL.ClientMessages.directUpdateNotificationTitle, directUpdateMsg, [{
    text : WL.ClientMessages.update,
    handler : function() {
      directUpdateContext.start(directUpdateCustomListener);
    }
  }]);
};
```
{: codeblock}

使用直接更新環境定義，建立一個啟動直接更新程序的函數。例如：

```JavaScript
restartDirectUpdate = function () {
  savedDirectUpdateContext.start(directUpdateCustomListener); // use saved direct update context to restart direct update
};
```
{: codeblock}

實作 `directUpdateCustomListener`。在 `onFinish` 方法中新增狀態檢查。如果狀態開始於 `FAILURE`，請開啟選項為**重試**的僅限制模式對話框。例如：

```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
    alert('onStart: totalSize = ' + totalSize + 'Byte');
  },
  onProgress: function(status,totalSize,completeSize){
    alert('onProgress: status = ' + status + ' completeSize = ' + completeSize + 'Byte');
  },
  onFinish: function(status){
    alert('onFinish: status = ' + status);
    var pos = status.indexOf("FAILURE");
    if (pos > -1) {
      WL.SimpleDialog.show('Update Failed', 'Press try again button', [ {
        text : "Try Again",
        handler : restartDirectUpdate // restart direct update
      }]);
    }
  }
};
```
{: codeblock}

當使用者按一下**重試**按鈕時，應用程式即會重新啟動直接更新程序。

## 差異與完整直接更新
{: #delta-and-full-direct-update }
「差異直接更新」可讓應用程式只下載自前次更新後已變更的檔案，而不是應用程式的整個 Web 資源。這會減少下載時間、保留頻寬，並改善整體使用者體驗。

> <span class="glyphicon glyphicon-exclamation-sign" aria-hidden="true"></span> **重要事項：**只有在用戶端應用程式的 Web 資源比目前部署在伺服器的應用程式落後一個版本時，才能進行**差異更新**。落後目前部署的應用程式多個版本的用戶端應用程式（表示自更新用戶端應用程式後，已至少兩次將應用程式部署至伺服器）會接收**完整更新**（表示下載並更新整個 Web 資源）。


## 範例應用程式
{: #sample-application }
[按一下以下載 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/MobileFirst-Platform-Developer-Center/CustomDirectUpdate/tree/release80) Cordova 專案。  

### 範例用法
{: #sample-usage }
請遵循範例的 README.md 檔案來取得指示。

## CDN 支援
{: #cdn_support}

您可以配置要從 CDN（內容遞送網路）而非「MobileFirst 伺服器」中提供的「直接更新」要求。

### 使用 CDN 的優點
{: #advantages-of-using-a-cdn }
使用 CDN 而非「MobileFirst 伺服器」來提供「直接更新」要求，具有下列優點：

* 移除「MobileFirst 伺服器」中的網路額外需要。
* 從「MobileFirst 伺服器」中提供要求時，增加傳送速率以高於 250 MB/秒限制。
* 確定所有使用者的「直接更新」體驗更一致，不論其地理位置為何。

### 一般需求
{: #general-requirements }
若要從 CDN 中提供「直接更新」要求，請確定您的配置符合下列條件：

* CDN 必須是「MobileFirst 伺服器」前面的反向 Proxy（或者，必要的話，在另一個反向 Proxy 的前面）。
* 從開發環境建置應用程式時，請將目標伺服器設定為 CDN 主機及埠，而非「MobileFirst 伺服器」的主機及埠。例如，執行 MobileFirst CLI 指令 `mfpdev server add` 時，請提供 CDN 主機及埠。
* 在 CDN 管理畫面中，您需要標示下列「直接更新」URL 進行快取，以確定 CDN 會將所有要求傳遞至「MobileFirst 伺服器」，但「直接更新」要求除外。對於「直接更新」要求，CDN 會判定它是否已取得內容。如果已取得，它會返回，而不會移至「MobileFirst 伺服器」；如果未取得，則會移至「MobileFirst 伺服器」、取得「直接更新」保存檔（.zip 檔案），並儲存它，以供該特定 URL 的後續要求使用。對於利用 8.0 版的 {{site.data.keyword.mobilefoundation_short}} 建置的應用程式，「直接更新」URL 為：`PROTOCOL://DOMAIN:PORT/CONTEXT_PATH/api/directupdate/VERSION/CHECKSUM/TYPE`。
所有運行環境要求的 `PROTOCOL://DOMAIN:PORT/CONTEXT_PATH` 字首固定不變。例如：`http://my.cdn.com:9080/mfp/api/directupdate/0.0.1/742914155/full?appId=com.ibm.DirectUpdateTestApp&clientPlatform=android`

在範例中，有其他也是要求一部分的要求參數。

* CDN 必須容許要求參數的快取。兩個不同的「直接更新」保存檔可能僅要求參數不同而已。
* CDN 必須支援「直接更新」回應上的 TTL。需要此支援，才能支援相同版本的多個直接更新。
* CDN 不得變更或移除用於伺服器端通訊協定的 HTTP 標題。

### 範例 CDN 配置
{: #example-cdn-configuration }
此範例的基礎是使用 Akamai CDN 配置，此配置會快取「直接更新」保存檔。下列作業是由網路管理者、MobileFirst 管理者及 Akamai 管理者完成：

#### 網路管理者
{: #network-administrator }
在 DNS 中為您的「MobileFirst 伺服器」建立另一個網域。例如，如果您的伺服器網域為 `yourcompany.com`，則您需要建立一個額外網域，例如 `cdn.yourcompany.com`。
在新的 `cdn.yourcompany.com` 網域的 DNS 中，將 `CNAME` 設為 Akamai 所提供的網域名稱。例如，`yourcompany.com.akamai.net`。

#### MobileFirst 管理者
{: #mobilefirst-administrator }
將新的 `cdn.yourcompany.com` 網域設為 MobileFirst 應用程式的「MobileFirst 伺服器」URL。例如，若為 Ant 建置器作業，內容是：
```xml
<property name="wl.server" value="http://cdn.yourcompany.com/${contextPath}/"/>
```
{: codeblock}

#### Akamai 管理者
{: #akamai-administrator }
1. 開啟 Akamai 內容管理程式，並將**主機名稱**內容設為新網域的值。

    ![將主機名稱內容設為新網域的值](images/direct_update_cdn_3.jpg)

2. 在「預設規則」標籤上，配置原始「MobileFirst 伺服器」主機及埠，並將**自訂轉遞主機標頭**值設為剛才建立的網域。

    ![將「自訂轉遞主機標頭」值設為剛才建立的網域](images/direct_update_cdn_4.jpg)

3. 從**快取選項**清單中，選取**無儲存**。

    ![從「快取選項」中，選取「無儲存」](images/direct_update_cdn_5.jpg)

4. 從**靜態內容配置**標籤中，根據應用程式的「直接更新」URL 配置比對準則。例如，建立一個條件，陳述 `If Path matches one of direct_update_URL`。

    ![根據應用程式的「直接更新」URL 配置比對準則](images/direct_update_cdn_6.jpg)

5. 配置快取索引鍵行為，以使用快取索引鍵中的所有要求參數（您必須這樣做，才能快取不同應用程式或版本的不同「直接更新」保存檔）。例如，從**行為**清單中，選取`包括所有參數（保留來自要求的順序）`。

    ![將快取索引鍵行為配置為使用快取索引鍵中的所有要求參數](images/direct_update_cdn_8.jpg)

6. 設定與下列值類似的值，將快取行為配置為快取「直接更新」URL 並設定 TTL。

      ![將值設為配置快取行為](images/direct_update_cdn_7.jpg)

| 欄位 | 值 |
|:------|:------|
| 快取選項 | 快取 |
| 強制重新評估過時物件 | 如果無法驗證，則提供過時物件 |
| 經歷時間上限 | 3 分鐘 |


## 安全直接更新
{: #secure-dc }

預設為已停用，「安全直接更新」可防止協力廠商攻擊者變更從「MobileFirst 伺服器」（或從「內容遞送網路 (CDN)」）傳送至用戶端應用程式的 Web 資源。

**若要啟用「直接更新」確實性，請執行下列動作：**  
使用偏好的工具，從「MobileFirst 伺服器」金鑰儲存庫中擷取公開金鑰，並將它轉換為 base64。  
然後，應該依照以下指示使用所產生的值：

1. 開啟**指令行**視窗，並導覽至 Cordova 專案的根目錄。
2. 執行指令：`mfpdev app config`，並選取**直接更新確實性公開金鑰**選項。
3. 提供公開金鑰並確認。

用戶端應用程式的所有未來「直接更新」交付將受到「直接更新」確實性保護。

若要使安全「直接更新」能夠運作，必須在「MobileFirst 伺服器」中部署使用者定義的金鑰儲存庫檔，並在已部署的用戶端應用程式中包括相符公開金鑰的副本。

本主題說明如何將公開金鑰連結至新的用戶端應用程式，以及已升級的現有用戶端應用程式。如需在「MobileFirst 伺服器」中配置金鑰儲存庫的相關資訊，請參閱[配置「MobileFirst 伺服器」金鑰儲存庫 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/configuring-the-mobilefirst-server-keystore/){: new_window}

伺服器提供內建的金鑰儲存庫，可在開發階段用於測試安全「直接更新」。

>**附註：**在將公開金鑰連結至用戶端應用程式並重建之後，您不需要重新將它上傳至 {{ site.data.keys.mf_server }}。不過，如果先前已將應用程式發佈至市場，但沒有公開金鑰，則必須予以重新發佈。

基於開發目的，「MobileFirst 伺服器」隨附有下列預設虛擬公開金鑰：

```xml
-----BEGIN PUBLIC KEY-----
MIIDPjCCAiagAwIBAgIEUD3/bjANBgkqhkiG9w0BAQsFADBgMQswCQYDVQQGEwJJTDELMAkGA1UECBMCSUwxETA
PBgNVBAcTCFNoZWZheWltMQwwCgYDVQQKEwNJQk0xEjAQBgNVBAsTCVdvcmtsaWdodDEPMA0GA1UEAxMGV0wgRG
V2MCAXDTEyMDgyOTExMzkyNloYDzQ3NTAwNzI3MTEzOTI2WjBgMQswCQYDVQQGEwJJTDELMAkGA1UECBMCSUwxE
TAPBgNVBAcTCFNoZWZheWltMQwwCgYDVQQKEwNJQk0xEjAQBgNVBAsTCVdvcmtsaWdodDEPMA0GA1UEAxMGV0wg
RGV2MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzQN3vEB2/of7KAvuvyoIt0T7cjaSTjnOBm0N3+q
zx++dh92KpNJXj/a3o4YbwJXkJ7jU8ykjCYvjXRf0hme+HGhiIVwxJo54iqh76skDS5m7DaseFdndZUJ4p7NFVw
I5ixA36ZArSZ/Pn/ej56/RRjBeRI7AEGXUSGojBUPA6J6DYkwaXQRew9l+Q1kj4dTigyKL5Os0vNFaQyYu+bT2E
vnOixQ0DXm94IqmHZamZKbZLrWcOEfuAsSjKYOdMSM9jkCiHaKcj7fpEZhUxRRs7joKs1Ri4ihs6JeUvMEiG4gK
l9V3FP/Huy0pfkL0F8xMHgaQ4c/lxS/s3PV0OEg+7wIDAQABMA0GCSqGSIb3DQEBCwUAA4IBAQAgEhhqRl2Rgkt
MJeqOCRcT3uyr4XDK3hmuhEaE0nOvLHi61PoLKnDUNryWUicK/W+tUP9jkN5xRckdzG6TJ/HPySmZ7Adr6QRFu+
xcIMY+/S8j4PHLXBjoqgtUMhkt7S2/thN/VA6mwZpw4Ol0Pa2hyT2TkhQoYYkRwYCk9pxmuBCoH/eCWpSxquNny
RwrY25x0YzccXUaMI8L3/3hzq3mW40YIMiEdpiD5HqjUDpzN1funHNQdsxEIMYsWmGAwOdV5slFzyrH+ErUYUFA
pdGIdLtkrhzbqHFwXE0v3dt+lnLf21wRPIqYHaEu+EB/A4dLO6hm+IjBeu/No7H7TBFm
-----END PUBLIC KEY-----
```
{: codeblock}

**重要事項：**請不要將公開金鑰用於正式作業目的。

### 產生並部署金鑰儲存庫
{: #generating-and-deploying-the-keystore }
您可以利用多種工具來產生憑證，並從金鑰儲存庫中擷取公開金鑰。下列範例示範使用 JDK 金鑰工具公用程式及 openSSL 的程序。

1. 從 {{ site.data.keys.mf_server }} 中部署的金鑰儲存庫檔擷取公開金鑰。  
   >**附註：**公開金鑰必須是 Base64 編碼。

   例如，假設別名為 `mfp-server`，金鑰儲存庫檔為 **keystore.jks**。  
   若要產生憑證，請發出下列指令：

   ```bash
   keytool -export -alias mfp-server -file certfile.cert
   -keystore keystore.jks -storepass keypassword
   ```
   {: codeblock}

   即會產生憑證檔案。  
   發出下列指令來擷取公開金鑰：

   ```bash
   openssl x509 -inform der -in certfile.cert -pubkey -noout
   ```
   {: codeblock}

   > **附註：**金鑰工具無法單獨擷取 Base64 格式的公開金鑰。

2. 執行下列其中一項程序：
    * 將產生的文字（沒有 `BEGIN PUBLIC KEY` 及 `END PUBLIC KEY` 標記）複製到應用程式的 mfpclient 內容檔，緊跟在 `wlSecureDirectUpdatePublicKey` 後面。
    * 從命令提示字元中，發出下列指令：`mfpdev app config direct_update_authenticity_public_key <public_key>`

    對於 `<public_key>`，貼上「步驟 1」中產生的文字（沒有 `BEGIN PUBLIC KEY` 及 `END PUBLIC KEY` 標記）。

3. 執行 cordova 建置指令，在應用程式中儲存公開金鑰。
