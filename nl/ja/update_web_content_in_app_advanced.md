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

# ダイレクト・アップデートの拡張構成
{: #advanced_direct_update_configuration}

ここでは、ダイレクト・アップデート機能を詳細に構成して操作する方法をいくつか示します。

## ダイレクト・アップデートの UI のカスタマイズ
{: #customize_du_ui}

エンド・ユーザーに表示されるダイレクト・アップデートの UI はカスタマイズ可能です。
**index.js** 内の `wlCommonInit()` 関数の中に以下を追加します。
```JavaScript
wl_DirectUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext) {
    // ダイレクト・アップデートのカスタム・ロジックを実装
};
```
{: codeblock}

*directUpdateData* は、Mobile Foundation サーバーからダウンロードする更新パッケージのファイル・サイズ (バイト単位) を表す downloadSize プロパティーが含まれている JSON オブジェクトです。
*directUpdateContext* は、ダイレクト・アップデート・フローを開始および停止する .start() 関数と .stop() 関数を公開する JavaScript オブジェクトです。

アプリケーション内の Web リソースより Mobile Foundation サーバー上の Web リソースの方が新しい場合は、ダイレクト・アップデート・チャレンジ・データがサーバー応答に追加されます。 Mobile Foundation クライアント・サイド・フレームワークは、このダイレクト・アップデート・チャレンジを検出するたびに `wl_directUpdateChallengeHandler.handleDirectUpdate` 関数を呼び出します。

この関数は、ダイレクト・アップデートのデフォルトのデザインとして、ダイレクト・アップデートが利用可能であるときに表示されるデフォルトのメッセージ・ダイアログと、ダイレクト・アップデート・プロセスが開始されたときに表示されるデフォルトの進行状況画面を提供します。 この関数をオーバーライドして独自のロジックを実装することにより、ダイレクト・アップデート・ユーザー・インターフェースのカスタム動作を実装したり、ダイレクト・アップデート・ダイアログ・ボックスをカスタマイズしたりすることができます。

下記のコード例では、`handleDirectUpdate` 関数でダイレクト・アップデート・ダイアログにカスタム・メッセージを実装します。 Cordova プロジェクトの  `www/js/index.js` ファイルにこのコードを追加します。
カスタマイズされたダイレクト・アップデート UI のその他の例:
* サード・パーティーの JavaScript フレームワーク (Dojo や jQuery Mobile、Ionic など) を使用して作成したダイアログ
* Cordova プラグインを実行することによる完全にネイティブな UI
* オプションなどを使用してユーザーに表示される代替の HTML

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext) {        
    navigator.notification.confirm(  // ダイアログの作成。
        'Custom dialog body text',
        // ダイアログのボタンを処理。
          directUpdateContext.start();
        },
        'Custom dialog title text',
        ['Update']
    );
};
```
{: codeblock}

ユーザーがダイアログ・ボタンをクリックするたびに `directUpdateContext.start()` メソッドを実行することによって、ダイレクト・アップデート・プロセスを開始することができます。 デフォルトの進行状況画面 (Mobile Foundation サーバーの以前のバージョンのものと同じ) が表示されます。

このメソッドは、次のタイプの呼び出しをサポートします。
* パラメーターが指定されていないと、Mobile Foundation サーバーはデフォルトの進行状況画面を使用します。
* `directUpdateContext.start(directUpdateCustomListener)` などのリスナー関数が指定されると、ダイレクト・アップデート・プロセスは、リスナーへのライフサイクル・イベントの送信中にバックグラウンドで実行されます。 カスタム・リスナーは以下のメソッドを実装する必要があります。

```JavaScript
var  directUpdateCustomListener  = {
    onStart : function ( totalSize ){ },
    onProgress : function ( status , totalSize , completedSize ){ },
    onFinish : function ( status ){ }
};
```
{: codeblock}

リスナー・メソッドは、以下のルールに従って、ダイレクト・アップデート・プロセス時に開始されます。
* `onStart` は、更新ファイルのサイズを保持する `totalSize` パラメーターで呼び出されます。
* `onProgress` は、状況 `DOWNLOAD_IN_PROGRESS`、`totalSize`、および `completedSize` (これまでにダウンロードされたボリューム) で複数回呼び出されます。
* `onProgress` は、状況 `UNZIP_IN_PROGRESS` で呼び出されます。
* `onFinish` は、次のいずれかの最終状況コードで呼び出されます。

| 状況コード | 説明 |
|:------------|:------------|
| `SUCCESS` | ダイレクト・アップデートがエラーなしで終了しました。 |
| `CANCELED` | ダイレクト・アップデートが取り消されました (例えば、`stop()` メソッドが呼び出されたため)。 |
| `FAILURE_NETWORK_PROBLEM` | 更新時にネットワーク接続に関する問題がありました。 |
| `FAILURE_DOWNLOADING` | ファイルが完全にダウンロードされませんでした。 |
| `FAILURE_NOT_ENOUGH_SPACE` | 更新ファイルをダウンロードして解凍するだけの十分なスペースがデバイスにありません。 |
| `FAILURE_UNZIPPING` | 更新ファイルの解凍中に問題がありました。 |
| `FAILURE_ALREADY_IN_PROGRESS` | ダイレクト・アップデートが既に実行しているときに start メソッドが呼び出されました。 |
| `FAILURE_INTEGRITY` | 更新ファイルの認証性を検証できません。 |
| `FAILURE_UNKNOWN` | 予期しない内部エラー。 |

カスタム・ダイレクト・アップデート・リスナーを実装する場合は、ダイレクト・アップデート・プロセスが完了して `onFinish()` メソッドが呼び出されたときにアプリケーションが再ロードされるようにする必要があります。 また、ダイレクト・アップデート・プロセスが正常に完了できなかった場合には `wl_directUpdateChalengeHandler.submitFailure()` を呼び出す必要があります。

次の例は、カスタム・ダイレクト・アップデート・リスナーの実装を示しています。
```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
    //カスタムの進行状況表示ダイアログの表示
  },
  onProgress: function(status,totalSize,completedSize){
    //カスタムの進行状況表示ダイアログの更新
  },
  onFinish: function(status){

    if (status == 'SUCCESS'){
      //成功メッセージの表示
      WL.Client.reloadApp();
    }
    else {
      //カスタムのエラー・メッセージの表示

      //エラーの場合は、submitFailure を呼び出す必要がある
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

## UI なしのダイレクト・アップデートの実行
{: #scenario-running-ui-less-direct-updates }
{{site.data.keyword.mobilefoundation_short}} は、アプリケーションがフォアグラウンドにあるとき、UI なしのダイレクト・アップデートをサポートします。

UI なしのダイレクト・アップデートを実行するには、`directUpdateCustomListener` を実装します。 空の関数実装を `onStart` および `onProgress` メソッドに提供します。 空の実装のためにダイレクト・アップデート・プロセスはバックグラウンドで実行されます。

ダイレクト・アップデート・プロセスを完了するためには、アプリケーションを再ロードする必要があります。 使用可能なオプションは次のとおりです。
* `onFinish` メソッドも空にすることができます。 この場合は、アプリケーションの再始動後にダイレクト・アップデートが適用されます。
* アプリケーションの再始動をユーザーに通知したり要求したりするカスタム・ダイアログを実装することができます。 (以下の例を参照してください。)
* `onFinish` メソッドは、`WL.Client.reloadApp()` を呼び出すことによってアプリケーションの再ロードを強制することができます。

`directUpdateCustomListener` の実装例を次に示します。

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

`wl_directUpdateChallengeHandler.handleDirectUpdate` 関数を実装します。 作成した `directUpdateCustomListener` 実装をパラメーターとしてこの関数に渡します。 必ず `directUpdateContext.start(directUpdateCustomListener`) が呼び出されるようにします。 `wl_directUpdateChallengeHandler.handleDirectUpdate` 実装の例を次に示します。

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  directUpdateContext.start(directUpdateCustomListener);
};
```
{: codeblock}

アプリケーションがバックグラウンドに送られると、ダイレクト・アップデート・プロセスは中断します。
{: note}

## ダイレクト・アップデート障害の処理
{: #scenario-handling-a-direct-update-failure }
このセクションでは、接続が失われたことなどによって起こる可能性があるダイレクト・アップデート障害を処理する方法を示します。このシナリオでは、ユーザーは、オフライン・モードでもアプリケーションを使用できなくなっています。 ユーザーに再試行オプションを提供するダイアログが表示されます。

1.  ダイレクト・アップデート・コンテキストを格納するグローバル変数を作成して、その後ダイレクト・アップデート・プロセスが失敗したときにそのダイレクト・アップデート・コンテキストを使用できるようにします。 例えば、次のとおりです。
    ```JavaScript
    var savedDirectUpdateContext;
    ```
    {: codeblock}

2.  ダイレクト・アップデート・チャレンジ・ハンドラーを実装します。 ダイレクト・アップデート・コンテキストをここに保存します。 例えば、次のとおりです。
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

3.  ダイレクト・アップデート・コンテキストを使用してダイレクト・アップデート・プロセスを開始する関数を作成します。 例えば、次のとおりです。
    ```JavaScript
    restartDirectUpdate = function () {
      savedDirectUpdateContext.start(directUpdateCustomListener); // 保存したダイレクト・アップデート・コンテキストを使用してダイレクト・アップデート・プロセスを再始動
};
    ```
    {: codeblock}

4.  `directUpdateCustomListener` を実装します。 `onFinish` メソッドに状況検査を追加します。 状況が`「FAILURE」`で始まる場合は、**「Try Again」**オプションを持つモーダル専用ダイアログを開きます。 例えば、次のとおりです。
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
        handler : restartDirectUpdate // ダイレクト・アップデートを再始動
      }]);
    }
      }
    };
    ```
    {: codeblock}

    ユーザーが**「Try Again」**ボタンをクリックすると、アプリケーションはダイレクト・アップデート・プロセスを再始動します。
    {: note}

## 差分ダイレクト・アップデートおよび完全ダイレクト・アップデート
{: #delta-and-full-direct-update }
差分ダイレクト・アップデートを使用して、アプリケーションは、その Web リソース全体ではなく、最後の更新以降に変更されたファイルのみをダウンロードできます。 これは、ダウンロード時間の削減、帯域幅の節約、さらにユーザー・エクスペリエンス全体の向上につながります。

**差分更新**が可能なのは、クライアント・アプリケーションの Web リソースが、サーバーに現在デプロイされているアプリケーションの 1 つ前のバージョンである場合に限られます。 現在デプロイされているアプリケーションより複数バージョン前のクライアント・アプリケーション (クライアント・アプリケーションが更新された後、最低 2 回はアプリケーションがサーバーにデプロイされていることを意味します) は、**フル・アップデート**を受け取ります (Web リソース全体がダウンロードされ、更新されることを意味します)。
{: note}

**Samples** セクションから、Cordova アプリケーション用のダイレクト・アップデート・サンプルを参照してください。このアプリケーションは、デフォルトで提供されるダイアログではなく、カスタムのダイレクト・アップデート・ダイアログを作成する方法を示しています。  

## CDN のサポート
{: #cdn_support}

CDN (コンテンツ配信ネットワーク) から (Mobile Foundation サーバーからではなく) ダイレクト・アップデート要求に応えるようにダイレクト・アップデート要求を構成することができます。

Mobile Foundation サーバーではなく CDN を使用してダイレクト・アップデート要求に対応することには以下のような利点があります。

* Mobile Foundation サーバーからネットワーク・オーバーヘッドを除去します。
* Mobile Foundation サーバーからの要求処理時に 250 MB/秒制限よりも転送速度が速くなります。
* ユーザーの地理的な居場所に関係なく、すべてのユーザーに対して、より均質なダイレクト・アップデート体験を保証します。

### 一般要件
{: #general-requirements }
CDN からのダイレクト・アップデート要求に応じるには、構成が以下の条件に従うようにしてください。

* CDN は、Mobile Foundation サーバーの前にあるリバース・プロキシー (必要ならば別のリバース・プロキシーの前にあるリバース・プロキシー) でなければなりません。
* 開発環境からアプリケーションをビルドする際は、ターゲット・サーバーを Mobile Foundation サーバーのホストとポートではなく CDN のホストとポートにセットアップします。 例えば、Mobile Foundation CLI コマンド `mfpdev server add` を実行している場合は、CDN のホストとポートを指定します。
* CDN 管理パネルで、次のダイレクト・アップデート URL にキャッシングのためのマークを付けて、CDN がダイレクト・アップデート要求以外のすべての要求を Mobile Foundation サーバーに渡すようにする必要があります。 ダイレクト・アップデート要求の場合は、コンテンツを取得したかどうかを CDN が調べます。 取得していた場合は、Mobile Foundation サーバーにアクセスせずに要求を返します。取得していない場合は、Mobile Foundation サーバーにアクセスしてダイレクト・アップデート・アーカイブ (.zip ファイル) を取得し、その特定の URL に対する次回以降の要求に備えてそのファイルを保管します。 {{site.data.keyword.mobilefoundation_short}} の v8.0 でビルドされたアプリケーションの場合、ダイレクト・アップデート URL は、`PROTOCOL://DOMAIN:PORT/CONTEXT_PATH/api/directupdate/VERSION/CHECKSUM/TYPE` です。
接頭部 `PROTOCOL://DOMAIN:PORT/CONTEXT_PATH` は、すべてのランタイム要求で一定です。 例えば、`http://my.cdn.com:9080/mfp/api/directupdate/0.0.1/742914155/full?appId=com.ibm.DirectUpdateTestApp&clientPlatform=android` などです。

この例では、要求の一部でもある、追加の要求パラメーターがあります。

* CDN は、要求パラメーターのキャッシングを許可する必要があります。 2 つの異なるダイレクト・アップデートのアーカイブは、要求パラメーターしか違わないことがあります。
* CDN はダイレクト・アップデート応答の TTL をサポートしなければなりません。 このサポートは、同じバージョンに対する複数のダイレクト・アップデートをサポートするために必要です。
* CDN は、 サーバー・クライアント・プロトコルで使用される HTTP ヘッダーを変更することも削除することもできません。

### CDN 構成の例
{: #example-cdn-configuration }
この例では、ダイレクト・アップデートのアーカイブをキャッシュに入れる Akamai CDN 構成を基にしています。 以下のタスクは、ネットワーク管理者、Mobile Foundation 管理者、および Akamai 管理者が行うものです。

#### ネットワーク管理者
{: #network-administrator }
Mobile Foundation サーバーの DNS に別のドメインを作成します。 例えば、サーバー・ドメインが `yourcompany.com` である場合は、`cdn.yourcompany.com` などの追加ドメインを作成する必要があります。
DNS 内で、新しい `cdn.yourcompany.com` ドメインに対して、`CNAME` を Akamai 提供のドメイン名に設定します。 例えば、`yourcompany.com.akamai.net` などです。

#### Mobile Foundation 管理者
{: #mobilefoundation-administrator }
新しい `cdn.yourcompany.com` ドメインを Mobile Foundation アプリケーションの Mobile Foundation サーバーの URL として設定します。 例えば、Ant ビルダー・タスクの場合であれは、プロパティーは次のとおりです。
```xml
<property name="wl.server" value="http://cdn.yourcompany.com/${contextPath}/"/>
```
{: codeblock}

#### Akamai 管理者
{: #akamai-administrator }
1. Akamai プロパティー・マネージャーを開き、**ホスト名**プロパティーを新しいドメインの値に設定します。

    ![ホスト名のプロパティーを新しいドメインの値に設定](images/direct_update_cdn_3.jpg)

2. 「デフォルト・ルール」タブで、元の Mobile Foundation サーバーのホストおよびポートを構成し、**「カスタム・フォワード・ホスト・ヘッダー」**値を新しく作成されたドメインに設定します。

    ![「カスタム・フォワード・ホスト・ヘッダー」の値を新しく作成されたドメインに設定](images/direct_update_cdn_4.jpg)

3. **「キャッシュ・オプション」**リストから、**「保管しない」**を選択します。

    ![「キャッシュ・オプション」リストから「保管しない」を選択](images/direct_update_cdn_5.jpg)

4. **「静的コンテンツ構成 (Static Content configuration)」**タブから、アプリケーションのダイレクト・アップデート URL に従って一致基準を構成します。 例えば、`If Path matches one of direct_update_URL` を明示する条件を作成します。

    ![アプリケーションのダイレクト・アップデート URL に従って一致基準を構成](images/direct_update_cdn_6.jpg)

5. キャッシュ・キー内のすべての要求パラメーターを使用するようにキャッシュ・キー動作を構成します (異なるアプリケーションまたはバージョンごとに異なるダイレクト・アップデート・アーカイブをキャッシュに入れるためには、このようにする必要があります)。 例えば、**「動作」**リストから、`「すべてのパラメーターを組み込む (要求からの順序を保持する)(Include all parameters (preserve order from request))」`を選択します。

    ![キャッシュ・キー内のすべての要求パラメーターを使用するようにキャッシュ・キー動作を構成](images/direct_update_cdn_8.jpg)

6. 以下のような値を設定して、ダイレクト・アップデート URL をキャッシュに入れ、TTL を設定するようにキャッシング動作を構成します。

      ![キャッシング動作の構成値を設定](images/direct_update_cdn_7.jpg)

| フィールド | 値 |
|:------|:------|
| キャッシュ・オプション | キャッシュ |
| 古いオブジェクトの再評価を強制 | 検証できない場合古いオブジェクトとして提供 |
| Max-Age | 3 minutes |

## セキュアなダイレクト・アップデート
{: #secure-dc }

セキュアなダイレクト・アップデートは、デフォルトでは無効になっていますが、Mobile Foundation サーバーから (またはコンテンツ配信ネットワーク (CDN) から) クライアント・アプリケーションに送信される Web リソースが第三者のアタッカーによって変更されるのを防止します。

**ダイレクト・アップデートの認証性を有効にするには、以下のようにします。**  
お好きなツールを使用して、Mobile Foundation サーバーの鍵ストアから公開鍵を取り出し、それを base64 に変換します。  
その後、生成された値を以下に示すように使用してください。

1. **コマンド・ライン**・ウィンドウを開き、Cordova プロジェクトのルートに移動します。
2. コマンド `mfpdev app config` を実行し、**「ダイレクト・アップデートの認証性公開鍵 (Direct update authenticity public key)」**のオプションを選択します。
3. 公開鍵を指定し、確認します。

これ以降のクライアント・アプリケーションへのダイレクト・アップデートの配信は、ダイレクト・アップデート認証性によって保護されます。

セキュアなダイレクト・アップデートが機能するためには、ユーザー定義の鍵ストア・ファイルが Mobile Foundation サーバーにデプロイされている必要があり、マッチングする公開鍵のコピーが、デプロイされたクライアント・アプリケーションに組み込まれている必要があります。

このトピックでは、新規クライアント・アプリケーションとアップグレードされた既存のクライアント・アプリケーションに公開鍵をバインドする方法を説明します。 Mobile Foundation サーバーでの鍵ストアの構成について詳しくは、[Configuring the MobileFirst Server Keystore ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/configuring-the-mobilefirst-server-keystore/){: new_window} を参照してください。

サーバーは、開発フェーズのセキュアなダイレクト・アップデートをテストするために使用できる組み込み鍵ストアを提供します。

公開鍵をクライアント・アプリケーションにバインドして再ビルドした後、
それを {{ site.data.keys.mf_server }}
に再度アップロードする必要はありません。 ただし、以前にアプリケーションを公開鍵なしでマーケットに公開している場合は、リパブリッシュする必要があります。
{: note}

開発目的の場合、以下のデフォルトのダミー公開鍵は、Mobile Foundation サーバーで提供されます。

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

実稼働目的で公開鍵を使用しないでください。
{: note}

### 鍵ストアの生成およびデプロイ
{: #generating-and-deploying-the-keystore }
証明書を生成して鍵ストアから公開鍵を抽出するために、複数のツールを利用できます。 以下の例では、JDK 鍵ツール・ユーティリティーと OpenSSL を使用した手順を説明します。

1. {{ site.data.keys.mf_server }} にデプロイされる鍵ストア・ファイルから公開鍵を抽出します。  
   公開鍵は Base64 でエンコードされる必要があります。
   {: note}

   例えば、エイリアス名が `mfp-server` で、鍵ストア・ファイルが **keystore.jks** であるとします。  
   証明書を生成するには、次のコマンドを実行します。

   ```bash
   keytool -export -alias mfp-server -file certfile.cert
   -keystore keystore.jks -storepass keypassword
   ```
   {: codeblock}

   証明書ファイルが生成されます。  
   次のコマンドを実行して公開鍵を抽出します。

   ```bash
   openssl x509 -inform der -in certfile.cert -pubkey -noout
   ```
   {: codeblock}

   鍵ツール単独で公開鍵を Base64 形式で抽出することはできません。
   {: note}

2. 以下の手順のいずれかを実行します。
    * 結果のテキスト (`BEGIN PUBLIC KEY` マーカーおよび `END PUBLIC KEY` マーカーを除いた部分) をアプリケーションの mfpclient プロパティー・ファイル内の `wlSecureDirectUpdatePublicKey` の直後にコピーします。
    * コマンド・プロンプトから、次のコマンドを実行します。`mfpdev app config direct_update_authenticity_public_key <public_key>`

    `<public_key>` には、手順 1 の結果として得られたテキスト (`BEGIN PUBLIC KEY` マーカーおよび `END PUBLIC KEY` マーカーを除いた部分) を貼り付けます。

3. cordova build コマンドを実行して、アプリケーションに公開鍵を保存します。
