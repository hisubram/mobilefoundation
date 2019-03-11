---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-14"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 更新應用程式中 Web 內容的替代步驟
{: #alternate_steps_to_update_app_web_content_in_app}

以下列出更新應用程式中 Web 內容的部分替代方式。

* 建置 `.zip` 檔案，並將它上傳至不同的 Mobile Foundation 伺服器：`mfpdev app webupdate [server-name] [runtime-name]`。
  例如：
  ```bash
  mfpdev app webupdate myQAServer MyBankApps
  ```

* 上傳先前產生的 `.zip` 檔案：`mfpdev app webupdate [server-name] [runtime-name] --file [path-to-packaged-web-resources]`。
  例如：
  ```bash
  mfpdev app webupdate myQAServer MyBankApps --file mobilefirst/ios/com.mfp.myBankApp-1.0.1.zip
  ```

* 手動將已包裝的 Web 資源上傳至 Mobile Foundation 伺服器：
  1. 建置 .zip 檔案，但不上傳：
      ```bash
      mfpdev app webupdate --build
      ```
      {: pre}
  2. 載入「Mobile Foundation 作業主控台」，然後按一下應用程式項目。
  3. 按一下**上傳 Web 資源檔案**，上傳已包裝的 Web 資源。    
      ![從主控台中上傳「直接更新」.zip 檔案](images/upload-direct-update-package.png)

執行指令 `mfpdev help app webupdate` 來進一步瞭解。
{: tip}
