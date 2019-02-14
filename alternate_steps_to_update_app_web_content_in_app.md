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

# Alternate steps to update web content in app
{: #alternate_steps_to_update_app_web_content_in_app}

Listed below are some of the alternate ways to update the web content in your app.

* Build the `.zip` file and upload it to a different Mobile Foundation server:  `mfpdev app webupdate [server-name] [runtime-name]`.
  For example:
  ```bash
  mfpdev app webupdate myQAServer MyBankApps
  ```

* Upload a previously generated `.zip` file: `mfpdev app webupdate [server-name] [runtime-name] --file [path-to-packaged-web-resources]`.
  For example:
  ```bash
  mfpdev app webupdate myQAServer MyBankApps --file mobilefirst/ios/com.mfp.myBankApp-1.0.1.zip
  ```

* Manually upload packaged web resources to the Mobile Foundation server:
  1. Build the .zip file without uploading it:
      ```bash
      mfpdev app webupdate --build
      ```
      {: pre}
  2. Load the Mobile Foundation Operations Console and click the application entry.
  3. Click **Upload Web Resources File** to upload the packaged web resources.    
      ![Upload Direct Update .zip file from the console](images/upload-direct-update-package.png)

Run the command `mfpdev help app webupdate` to learn more.
{: tip}
