---

copyright:
  years: 2019
lastupdated: "2018-12-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:java: .ph data-hd-programlang='java'}
{:ruby: .ph data-hd-programlang='ruby'}
{:c#: .ph data-hd-programlang='c#'}
{:objectc: .ph data-hd-programlang='Objective C'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:php: .ph data-hd-programlang='PHP'}
{:swift: .ph data-hd-programlang='swift'}

# Javascript 用戶端 SDK API
{: #javascript_client_sdk_api}

## 適用於 Cordova/Web 應用程式的 API (Javascript)

下表列出您可以在 Javascript 應用程式中執行的函數，以及對應的 API 方法。

| 函數 | 說明 |
|----------|-------------|
| `WL.Client`、`WL.App` | 起始設定及重新載入應用程式，將應用程式文字全球化 | 
| `WLAuthorizationManager` | 取得用戶端 ID 及授權標頭 |
| `WL.Logger` | 將日誌訊息列印至該環境的日誌 |
| `WL.NativePage` | 使用原生撰寫的頁面來切換目前顯示的 Web 型畫面 |
| `WLResourceRequest` | 將要求傳送至受保護及未受保護的資源 | 
| `WL.JSONStore` | 提供輕量型文件導向儲存空間系統的用戶端 API | 

## 其他資訊
{: #additional-information }
### options 物件
{: #the-optional-object }
`options` 物件包含所有方法通用的內容。它用於所有非同步 {{ site.data.keys.mf_server }} 呼叫。

有時，它會透過只適用於特定方法的內容予以擴增。這些其他內容詳述為特定方法之說明的一部分。

options 物件的一般內容如下：

```javascript
options = {
    onSuccess: successHandler(response),
    onFailure: failureHandlder(response),
    invocationContext: invocation-context
};
```
{: code}
每個內容的意義如下：

| 內容 | 說明 |
|----------|-------------|
| `onSuccess` | 選用。要在順利完成非同步呼叫時呼叫的函數。`onSuccess` 函數的語法如下：`success-handler-function(response)`，其中 `response` 是最少包含下列內容的物件：{::nomarkdown}<ul><li><b>invocationContext</b> - <code>options</code> 物件中原始傳遞至 {{ site.data.keys.mf_server }} 的 <code>invocationContext</code> 物件，或者，如果未傳遞任何 <code>invocationContext</code> 物件，則為 <code>undefined</code>。</li><li><b>status</b> - HTTP 回應狀態</li></ul>{:/} **附註：**針對 `response` 物件包含其他內容的方法，這些內容詳述為特定方法之說明的一部分。|
| `onFailure` | 選用。要在非同步呼叫失敗時呼叫的函數。這類失敗包括伺服器端錯誤，以及非同步呼叫期間所發生的用戶端錯誤（例如伺服器連線失敗或逾時呼叫）。**附註：**針對藉由擲出異常狀況來停止執行的用戶端錯誤，不會呼叫此函數。onFailure 函數的語法如下：`failure-handler-function(response)`，其中 `response` 是包含下列內容的物件：{::nomarkdown}<ul><li><b>invocationContext</b> - <code>options</code> 物件中原始傳遞至 {{ site.data.keys.mf_server }} 的 <code>invocationContext</code> 物件，或者，如果未傳遞任何 <code>invocationContext</code> 物件，則為 <code>undefined</code>。</li><li><b>errorCode</b> - 錯誤碼字串。所有可傳回的錯誤碼都定義為 <b>worklight.js</b> 檔案內 <code>WL.ErrorCode</code> 物件中的常數。</li><li><b>errorMsg</b> - {{ site.data.keys.mf_server }} 所提供的錯誤訊息。此訊息僅供開發人員使用，不應向使用者顯示。它不會被翻譯為使用者的語言。</li><li><b>status</b> - HTTP 回應狀態</li></li></ul>{:/} **附註：**針對 `response` 物件包含其他內容的方法，這些內容詳述為特定方法之說明的一部分。|
| `invocationContext` | 選用。傳回給成功及失敗處理程式的物件。使用 `invocationContext` 物件，保留呼叫端非同步服務從服務傳回時的環境定義。例如，可能會使用相同的成功處理程式來連續呼叫 `invokeProcedure` 方法。成功處理程式必須能夠識別正在處理的 invokeProcedure 呼叫。其中一個解決方案是將 `invocationContext` 物件實作為整數，並針對每個 `invokeProcedure` 呼叫將其值加 1。當它呼叫成功處理程式時，{{ site.data.keys.product_adj }} 架構會將與 `invokeProcedure` 方法相關聯之 options 物件的 `invocationContext` 物件傳遞給它。`invocationContext` 物件的值可以用來識別與正在處理的結果相關聯的 `invokeProcedure` 呼叫。| 

## WL.ClientMessages 物件
{: #the-wlclientmessages-object }
您可以看到 `WL.ClientMessages` 物件中所儲存的系統訊息清單，並啟用這些系統訊息的翻譯。

您必須置換廣域 JavaScript 層次的系統訊息，因為程式碼的某些部分只有在順利起始設定應用程式之後才會執行。
{: note}

`WL.ClientMessages` 物件是用來儲存 **worklight/messages/messages.json** 檔案中所定義系統訊息的物件。此檔案位於您使用 {{ site.data.keys.product }} 所產生之專案的環境資料夾中。若要啟用系統訊息的翻譯，您必須置換 `WL.ClientMessages` 物件中此訊息的值，如下列程式碼範例所示：

```javascript
WL.ClientMessages.invalidUsernamePassword="The custom user name and password are not valid";
```
{: code}


* [JavaScript API 參考資料](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/#javascript-api-reference)
* [Objective-C API 參考資料（適用於 Cordova）](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/#objective-c-api-reference-for-cordova)
{: note}
