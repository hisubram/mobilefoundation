---

copyright:
  years: 2016, 2019
lastupdated:  "2019-02-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

# 入門チュートリアル
{: #getting-started-tutorial}

{{site.data.keyword.mobilefoundation_long}} は、エンタープライズ・モバイル・アプリケーションの開発、テスト、および実行に使用できる {{site.data.keyword.mfp_full}} 環境のセットアップを迅速に行えるようにします。 {{site.data.keyword.mobilefoundation_short}} には、「開発者」、「デバイス当たりのプロフェッショナル」、「プロフェッショナル 1 アプリケーション」というサービス・プランが用意されています。
{: shortdesc}

「プロフェッショナル 1 アプリケーション」プランを使用すると、サポートされる任意のオペレーティング・システム上にビルドされた単一アプリケーションを管理できます。 サポートされるオペレーティング・システムは、Android、iOS、Windows、またはモバイル Web です。 「開発者」プランは、開発とテストに最適です。 すべての使用可能なプランは [ここ](https://cloud.ibm.com/catalog/services/mobile-foundation) で確認できます。

この入門チュートリアルでは、サポートされるプランのいずれかを使用して {{site.data.keyword.mobilefoundation_short}} サービス・インスタンスを作成できます。 その後、アプリケーションを登録できます。 登録されたアプリケーションをダウンロードして編集し、アダプターをデプロイし、最後にアプリケーションをテストします。

## 始めに
{: #prereqs-gs}

{{site.data.keyword.Bluemix}} アカウントと、{{site.data.keyword.mobilefoundation_short}} サービスのインスタンスが必要になります。

## ステップ 1: {{site.data.keyword.mobilefoundation_short}} サービスのインスタンスの作成
{: #step1create}

1. {{site.data.keyword.Bluemix_notm}} ** カタログ**で、[**{{site.data.keyword.mobilefoundation_short}}**](https://cloud.ibm.com/catalog/services/mobile-foundation) を選択します。 サービス構成画面が開きます。
2. サービス・インスタンスに名前を付けます。または、事前設定された名前を使用します。
3. サービス・インスタンスを作成する地域、組織、およびスペースを選択します。
4. **「価格プラン」**を選択し、**「作成」**をクリックします。

## ステップ 2: モバイル・チャネルの作成
{: #buildmobilechannel}

### {{site.data.keyword.mobilefoundation_short}}: 「開発者」プランの場合
{: #buildchanneldevplan}

{{site.data.keyword.mobilefoundation_short}}: 「開発者」のインスタンスを作成した後、以下のステップを実行することでモバイル・チャネルの作成を開始できます。

* Mobile Foundation Server に即座にアクセスして作業を行うことができます。

  この選択により、以下の設定で {{site.data.keyword.mfserver_long_notm}} が作成されます。
  *	1 GB のメモリー。 開発アクティビティー、簡単なテスト・アクティビティー、および小規模な実動ワークロードには、このサイズで十分です。

  * CLI を使用して Mobile Foundation Server にアクセスするには、IBM Cloud コンソールの左側のナビゲーション・ペインで**「サービス資格情報」**をクリックして表示される、資格情報が必要です。

### 「{{site.data.keyword.mobilefoundation_short}}: デバイス当たりのプロフェッショナル」プランの場合
{: #buildchannelprofdeviceplan}

「{{site.data.keyword.mobilefoundation_short}}: デバイス当たりのプロフェッショナル」サービスのインスタンスの作成後に、以下のステップを実行することでモバイル・チャネルの作成を開始できます。

  1.  {{site.data.keyword.Bluemix_notm}} 上の既存の {{site.data.keyword.Db2_on_Cloud_short}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL}} サービスに接続します。

      1.  {{site.data.keyword.Db2_on_Cloud_short}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL}} サービス・インスタンスが存在する {{site.data.keyword.Bluemix_notm}} `組織` を選択します。

      + 選択した`組織`で使用可能なスペースのリストから、{{site.data.keyword.Db2_on_Cloud_short}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL}} サービス・インスタンスが存在する {{site.data.keyword.Bluemix_notm}} `スペース` を選択します。

      + 既存の {{site.data.keyword.Db2_on_Cloud_short}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL}} サービス・インスタンスに接続するための {{site.data.keyword.Db2_on_Cloud_short}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL}} `サービス名` および`資格情報` を選択します。

      + **「接続のテスト」**をクリックして、選択した {{site.data.keyword.Db2_on_Cloud_short}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL}} サービス・インスタンスへの接続をテストします。

      + **「追加」**をクリックした後、選択した {{site.data.keyword.Db2_on_Cloud_short}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL}} サービスについて確認を求めるポップアップ・ウィンドウで**「続行」**をクリックします。 このアクションにより、構成された {{site.data.keyword.Db2_on_Cloud_short}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL}} データベース・サービス・インスタンスで必要な表が作成されます。

      {{site.data.keyword.Db2_on_Cloud_short}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL}} 接続を {{site.data.keyword.mobilefoundation_short}} インスタンスに追加した後は、それを変更することはできません。
      {: note} 
  2.  サーバーを作成して始動します。

      1. デフォルト構成を使用して {{site.data.keyword.mobilefirst_notm}} サーバー・インスタンスを作成します。**「基本サーバーの始動」**をクリックします。

      + この選択により、以下の設定で {{site.data.keyword.mfserver_long_notm}} がプロビジョンされます。
          - 2 個のノード (それぞれ 1 GB のメモリーを装備)。 開発アクティビティー、中程度のテスト・アクティビティー、および小規模な実動ワークロードには、このサイズが適しています。

          -	`ユーザー名`と`パスワード`は、自動的に生成されます。 サーバーの稼働中にこれらにアクセスできます。

          サーバーを作成するプロセスが開始されます。 このプロセスには約 10 分かかり、メッセージ・ウィンドウにはこの操作の進行が示されます。 完了すると、以下のことを確認できるダッシュボードが表示されます。
            -	実行中のサーバーの状況 (状態、サイズ)。
            -	サーバーの経路が作成されます。 {{site.data.keyword.mfserver_short_notm}} に接続するには、モバイル・アプリケーションでこの経路を使用します。
            -	{{site.data.keyword.mfp_oc_short_notm}} にアクセスするための個人の`ユーザー名` と`パスワード`。 `パスワード` は非表示です。 表示するには**「パスワードの表示」**アイコンをクリックします。

      +	**「コンソールの起動」**をクリックして {{site.data.keyword.mfp_oc_short_notm}} を開きます。      

      トポロジー、セキュリティー、およびその他のサーバー構成について拡張構成を使用して {{site.data.keyword.mobilefirst_notm}} サーバー・インスタンスを作成するには、**「拡張構成を使用したサーバーの始動 (Start Server with Advanced Configuration)」**をクリックします。 詳しくは、[拡張構成のセットアップ](/docs/services/mobilefoundation?topic=mobilefoundation-c_using_mfs_p5#using_mfs_advanced_p5)を参照してください。
      {: tip}

### {{site.data.keyword.mobilefoundation_short}}: 「1 つの商用アプリケーション」プランの場合
{: #buildchannelprof1appplan}

「{{site.data.keyword.mobilefoundation_short}}: プロフェッショナル 1 アプリケーション」サービスのインスタンスの作成後に、以下のステップを実行することでモバイル・チャネルの作成を開始できます。

  1.  {{site.data.keyword.Bluemix_notm}} 上の既存の {{site.data.keyword.Db2_on_Cloud_long}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL_full}} サービスに接続します。

      1.  {{site.data.keyword.Db2_on_Cloud_short}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL}} サービス・インスタンスが存在する {{site.data.keyword.Bluemix_notm}} `組織` を選択します。

      + 選択した`組織`で使用可能なスペースのリストから、{{site.data.keyword.Db2_on_Cloud_short}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL}} サービス・インスタンスが存在する {{site.data.keyword.Bluemix_notm}} `スペース` を選択します。

      + 既存の {{site.data.keyword.Db2_on_Cloud_short}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL}} サービス・インスタンスに接続するための {{site.data.keyword.Db2_on_Cloud_short}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL}} `サービス名` および`資格情報` を選択します。

      + **「接続のテスト」**をクリックして、選択した {{site.data.keyword.Db2_on_Cloud_short}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL}} サービス・インスタンスへの接続をテストします。

      + **「追加」**をクリックした後、選択した {{site.data.keyword.Db2_on_Cloud_short}} または {{site.data.keyword.composeForPostgreSQL}} サービスについて確認を求めるポップアップ・ウィンドウで**「続行」**をクリックします。 このアクションにより、構成された {{site.data.keyword.Db2_on_Cloud_short}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL}} データベース・サービス・インスタンスで必要な表が作成されます。

      {{site.data.keyword.Db2_on_Cloud_short}} (**ライト**・プラン以外のプラン) または {{site.data.keyword.composeForPostgreSQL}} 接続を {{site.data.keyword.mobilefoundation_short}} インスタンスに追加した後は、それを変更することはできません。
      {: note}

  2.  サーバーを作成して始動します。

      1. デフォルト構成を使用して {{site.data.keyword.mobilefirst_notm}} サーバー・インスタンスを作成します。**「基本サーバーの始動」**をクリックします。

        `基本サーバー・インスタンスには、単一のノードと 1 GB のメモリーが含まれています。`

      + `ユーザー名` と `パスワード` は自動的に生成されます。 サーバーの稼働中にこれらにアクセスできます。  

        サーバーを作成するプロセスが開始されます。 このプロセスには約 10 分かかり、メッセージ・ウィンドウにはこの操作の進行が示されます。 完了すると、以下のことを確認できるダッシュボードが表示されます。
          -	実行中のサーバーの状況 (状態、サイズ)。
          -	サーバーの経路が作成されます。 {{site.data.keyword.mfserver_short_notm}} に接続するには、モバイル・アプリケーションでこの経路を使用します。
          -	{{site.data.keyword.mfp_oc_short_notm}} にアクセスするための個人の`ユーザー名` と`パスワード`。 `パスワード` は非表示です。 表示するには**「パスワードの表示」**アイコンをクリックします。

      +  **「コンソールの起動」**をクリックして {{site.data.keyword.mfp_oc_short_notm}} を開きます。  

      トポロジー、セキュリティー、およびその他のサーバー構成について拡張構成を使用して {{site.data.keyword.mobilefirst_notm}} サーバー・インスタンスを作成するには、**「拡張構成を使用したサーバーの始動 (Start Server with Advanced Configuration)」**をクリックします。 詳しくは、[拡張構成のセットアップ](/docs/services/mobilefoundation?topic=mobilefoundation-c_using_mfs_p2#using_mfs_advanced_p2)を参照してください。
      {: tip}

{{site.data.keyword.mobilefoundation_short}} の使用の開始について詳しくは、[Using the Mobile Foundation service to set up MobileFirst Server (Mobile Foundation サービスを使用した、MobileFirst Server のセットアップ)![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/bluemix/using-mobile-foundation/){: new_window}を参照してください。
{: note}

## ステップ 3: アプリケーションの {{site.data.keyword.mobilefoundation_short}} への登録
{: #registerapp}

Mobile Foundation サーバー・インスタンスを作成して始動した後、次の手順を実行して Android アプリケーションを登録できます。

  1.  URL: `http://<your-server-host>:<server-port>/mfpconsole` をロードして、{{site.data.keyword.mfp_oc_short_notm}} を呼び出します。 プロビジョニング時に生成された`ユーザー名`と`パスワード`を使用します。

  + {{site.data.keyword.mfp_oc_short_notm}} **「ダッシュボード」**で、**「アプリケーション」**の横にある**「新規」**をクリックします。

  + **「アプリケーション名」**として「*MFPStarterAndroid*」を指定します。

  + **「プラットフォームの選択」**は「*Android*」にします。

  + **「アプリケーション ID」**として「*com.ibm.mfpstarterandroid*」を指定します。

  + **「バージョン」**として「*1.0*」を入力します。

  + **「アプリケーションの登録」**をクリックします。

## ステップ 4: サンプル・アプリケーションのダウンロード
{: #downloadapp}

  1.  {{site.data.keyword.mfp_oc_short_notm}} **「ダッシュボード」**で、**「アプリケーション」**の下の**「MFPStarterAndroid」**を選択します。

  + **「スターター・コードの取得」**をクリックし、Android アプリケーション・サンプルをダウンロードします。

## ステップ 5: サンプル・アプリケーションの編集
{: #editapp}

  1. 前のステップでダウンロードした Android アプリケーション・サンプルを、Android Studio にインポートします。

  + Android Studio の**「プロジェクト」**サイドバー・メニューから、**「app」 → 「java」 → 「com.ibm.mfpstarterandroid」 → 「ServerConnectActivity.java」**ファイルを選択します。

    * 以下の import を追加します。
      ```java
      import java.net.URI;
      import java.net.URISyntaxException;
      import android.util.Log;
      ```
      {: codeblock}

    * 以下のコード・スニペットを貼り付けて、`WLAuthorizationManager.getInstance().obtainAccessToken` への呼び出しを置換します。

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

## ステップ 6: アダプターのデプロイ
{: #deployadapter}

  1. この[アダプター成果物](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/javaAdapter.adapter){: download}をダウンロードし、それを、**「アクション」 → 「アダプターのデプロイ」**を使用して {{site.data.keyword.mfp_oc_short_notm}} からデプロイします。

## ステップ 7: アプリケーションのテスト
{: #testapp}

  1. Android Studio で、**「プロジェクト」**サイドバー・メニューから、**「app」 → 「src」 → 「main」 → 「assets」 → 「mfpclient.properties」**ファイルを選択し、MobileFirst Server の正しい値で、プロパティー `protocol`、`host`、`port` を編集します。

   値は通常、https、*your-server-address*、および 443 です。
   {: tip}

  2. Android Studio で、**「アプリの実行 (Run App)」**をクリックします。
     * デバイス・エミュレーターにアプリが開始したことが示されます。
     * アプリケーションの**「MobileFirst Server の ping (Ping MobileFirst Server)」**をクリックします。「`Connected to MobileFirst Server`」が表示されます。
     * アプリケーションが MobileFirst Server に接続できた場合は、デプロイした Java アダプターを使用してリソース要求呼び出しが行われます。
     * その後、アダプターの応答は、Android Studio の LogCat ビューに出力されます。


## 次のステップ
{: #nextsteps-gs}

[Quick Start チュートリアル ![外部リンク・アイコン](../../icons/launch-glyph.svg "Quick Start チュートリアル")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/){: new_window} に従って、さらに多くのサンプル・アプリケーションを操作したり {{site.data.keyword.mobilefoundation_short}} の機能を検討したりすることができます。

Quick Start には、iOS、Android、Web、Cordova、Windows、React Native、Ionic、Xamarin の各アプリの {{site.data.keyword.mobilefoundation_short}} の動作を説明するチュートリアルがあります。
