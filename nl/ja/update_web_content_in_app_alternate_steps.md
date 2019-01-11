---

copyright:
  years: 2018
lastupdated: "2018-12-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# アプリケーション内の Web コンテンツを更新するための代替手順
{: #alternate_steps_to_update_app_web_content}

以下に、アプリケーション内の Web コンテンツを更新する代替方法をいくつか示します。

* `.zip` ファイルを作成し、コマンド `mfpdev app webupdate [server-name] [runtime-name]` を使用して別の Mobile Foundation サーバーにアップロードします。
  例えば、次のようにします。
  ```bash
  mfpdev app webupdate myQAServer MyBankApps
  ```

* コマンド `mfpdev app webupdate [server-name] [runtime-name] --file [path-to-packaged-web-resources]` を使用して、以前に生成した `.zip` ファイルをアップロードします。
  例えば、次のようにします。
  ```bash
  mfpdev app webupdate myQAServer MyBankApps --file mobilefirst/ios/com.mfp.myBankApp-1.0.1.zip
  ```

* 以下のように、パッケージした Web リソースを Mobile Foundation サーバーに手動でアップロードします。
  1. アップロードは行わずに、.zip ファイルを作成します。
      ```bash
      mfpdev app webupdate --build
      ```
      {: pre}
  2. Mobile Foundation Operations Console をロードし、そのアプリケーション項目をクリックします。
  3. **「Web リソース・ファイルのアップロード (Upload Web Resources File)」**をクリックして、パッケージした Webリソースをアップロードします。    
      ![ダイレクト・アップデートの .zip ファイルをコンソールからアップロード](images/upload-direct-update-package.png)

詳細については、コマンド `mfpdev help app webupdate` を実行してください。
{: tip}
