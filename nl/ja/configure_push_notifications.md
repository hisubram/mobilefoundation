---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: push notifications, notifications, FCM, GCM, APNS, WNS, authenticate notification

subcollection:  mobilefoundation
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
{:reactnative: .ph data-hd-programlang='React Native'}
{:csharp: .ph data-hd-programlang='csharp'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}
{:xml: .ph data-hd-programlang='xml'}


# プッシュ通知の構成
{: #configure_push_notifications}

プッシュ通知を iOS、Android、または Windows デバイスに送信するためには、まず、FCM 詳細 (Android)、APNS 証明書 (iOS)、または WNS 資格情報 (Windows 8.1 Universal/Windows 10 UWP) を使用して {{ site.data.keyword.mfserver_short_notm }} を構成する必要があります。
{: shortdesc}

その後、以下のオプションを使用して通知を送信できます。
* すべてのデバイス (ブロードキャスト)
* 特定のタグに登録されたデバイス
* 単一のデバイス ID
* ユーザー ID
* iOS デバイスのみ
* Android デバイスのみ
* Windows デバイスのみ
* 認証済みユーザーに基づく。

## 通知のセットアップ
{: #setting-up-notifications }

通知サポートの有効化では、{{ site.data.keyword.mfserver_short_notm }} とクライアント・アプリケーションの両方でいくつかの構成ステップが必要になります。 この後のサーバー・サイドのセットアップを続けてお読みになるか、[クライアント・サイドのセットアップ](#tutorials-to-follow-next)にお進みください。

サーバー・サイドで必要なセットアップには、必要なベンダー (APNS、FCM、または WNS) の構成と push.mobileclient スコープのマッピングがあります。

### Firebase Cloud Messaging
{: #firebase-cloud-messaging }

Google は [GCM を非推奨](https://developers.google.com/cloud-messaging/faq)とし、Cloud Messaging を Firebase に統合しました。 GCM プロジェクトを使用する場合は、必ず [Android 上の GCM クライアント・アプリケーションを FCM にマイグレーション](https://developers.google.com/cloud-messaging/android/android-migrate-fcm)してください。
{: note}

Android デバイスは、プッシュ通知に Firebase Cloud Messaging (FCM) サービスを使用します。

FCM をセットアップするには、次のようにします。

1. [Firebase コンソール](https://console.firebase.google.com/?pli=1)にアクセスします。
2. プロジェクトを作成し、プロジェクトに名前を付けます。
3. 設定 (歯車) アイコンをクリックし、**「プロジェクトの設定」**を選択します。
4. **「クラウド メッセージング」**タブをクリックして、**「サーバー API キー」**と**「送信者 ID」**を生成し、**「保存」**をクリックします。

[{{ site.data.keyword.mobilefirst_notm }} プッシュ・サービス用 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_gcm_settings_put.html#Push-GCM-Settings--PUT-) または [{{ site.data.keyword.mobilefirst_notm }} 管理サービス用 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_gcm_settings_put.html#restservicesapi) のいずれかを使用して FCM をセットアップすることもできます。
{: note}

お客様の組織にインターネットとの間のトラフィックを制限するファイアウォールが存在する場合は、以下のステップを実行する必要があります。  
* FCM クライアント・アプリケーションがメッセージを受信するために FCM との接続を許可するようにファイアウォールを構成します。
* 開くポートは 5228、5229、および 5230 です。 FCM は通常は 5228 のみを使用しますが、場合によっては 5229 および 5230 を使用することもあります。
* FCM は特定の IP を提供しないため、Google の ASN 15169 にリストされた IP ブロックに含まれるすべての IP アドレスへの発信接続をファイアウォールが受け入れられるようにする必要があります。
* ファイアウォールが {{ site.data.keyword.mfserver_short_notm }} から fcm.googleapis.com への発信接続をポート 443 で受け入れるようにします。
{: note }

<img class="gifplayer" alt="GCM 資格情報を追加するイメージ" src="images/gcm-setup.png"/>

### Apple Push Notifications Service
{: #apple-push-notifications-service }

iOS デバイスは、プッシュ通知に Apple Push Notification Service (APNS) を使用します。  

APNS をセットアップするには、以下のようにします。

1. 開発や実動のためのプッシュ通知証明書を生成します。 詳細な手順については、[こちら](https://cloud.ibm.com/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1)の`「iOS の場合」`セクションを参照してください。
2. {{ site.data.keyword.mfp_oc_short_notm }} →**「 [ご使用のアプリケーション] 」→「プッシュ」→「プッシュ設定」**で、証明書タイプを選択し、証明書のファイルとパスワードを指定します。 次に、**「保存」**をクリックします。

  * プッシュ通知を送信するには、{{ site.data.keyword.mfserver_short_notm }} インスタンスから以下のサーバーにアクセス可能でなければなりません。
    * Sandbox サーバー:
      * gateway.sandbox.push.apple.com:2195
      * feedback.sandbox.push.apple.com:2196
    * 実動サーバー:
      * gateway.push.apple.com:2195
      * Feedback.push.apple.com:2196
      * 1-courier.push.apple.com 5223
  * 開発フェーズでは、apns-certificate-sandbox.p12 サンドボックス証明書ファイルを使用します。
  * 実動フェーズでは、apns-certificate-production.p12 実動証明書ファイルを使用します。
    * APNS 実動証明書は、その証明書を使用するアプリケーションが Apple App Store に正常に送信されたときにのみテストできます。
    {: note }

MobileFirst は Universal 証明書をサポートしていません。
{: note }

[{{ site.data.keyword.mobilefirst_notm }} プッシュ・サービス用 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_apns_settings_put.html#Push-APNS-settings--PUT-) または [{{ site.data.keyword.mobilefirst_notm }} 管理サービス用 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_apns_settings_put.html?view=kc) のいずれかを使用して APNS をセットアップすることもできます。
{: note}

<img class="gifplayer" alt="APNS 資格情報を追加するイメージ" src="images/apns-setup.png"/>

### Windows Push Notifications Service
{: #windows-push-notifications-service }

Windows デバイスは、プッシュ通知に Windows Push Notifications Service (WNS) を使用します。  

WNS をセットアップするには、次のようにします。

1. [Microsoft が提供する指示](https://msdn.microsoft.com/en-in/library/windows/apps/hh465407.aspx)に従って、**「パッケージ セキュリティ ID (SID)」**と**「クライアント シークレット」**の値を生成します。
2. {{ site.data.keyword.mfp_oc_short_notm }} →**「 [ご使用のアプリケーション] 」→「プッシュ」→「プッシュ設定」**で、これらの値を追加し、**「保存」**をクリックします。

[{{ site.data.keyword.mobilefirst_notm }} プッシュ・サービス用 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_wns_settings_put.html?view=kc) または [{{ site.data.keyword.mobilefirst_notm }} 管理サービス用 REST API](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_wns_settings_put.html?view=kc) のいずれかを使用して WNS をセットアップすることもできます。

<img class="gifplayer" alt="WNS 資格情報を追加するイメージ" src="images/wns-setup.png"/>

### スコープ・マッピング
{: #scope-mapping }

**push.mobileclient** スコープ・エレメントをアプリケーションにマップします。

1. {{ site.data.keyword.mfp_oc_short_notm }} をロードし、**「 [ご使用のアプリケーション] 」→「セキュリティー」→「スコープ・エレメントのマッピング」**にナビゲートし、**「新規」**をクリックします。
2. **「スコープ・エレメント」**フィールドに「push.mobileclient」と入力します。 次に、**「追加」**をクリックします。

その他の使用可能なスコープのリストを以下に示します。

|**スコープ** | **説明**|
|---|---|
|apps.read | アプリケーション・リソースの読み取りの許可 |
|apps.write | アプリケーション・リソースの作成、更新、削除の許可 |
|gcmConf.read | GCM 構成設定の読み取りの許可 (API キーおよび SenderId) |
|gcmConf.write | GCM 構成設定の更新、削除の許可 |
|apnsConf.read | APNs 構成設定の読み取りの許可 |
|apnsConf.write | APNs 構成設定の更新、削除の許可 |
|devices.read | デバイスの読み取りの許可 |
|devices.write | デバイスの作成、更新、削除の許可 |
|subscriptions.read | サブスクリプションの読み取りの許可 |
|subscriptions.write | サブスクリプションの作成、更新、削除の許可 |
|messages.write | プッシュ通知の送信の許可 |
|webhooks.read | イベント通知の読み取りの許可 |
|webhooks.write | イベント通知の送信の許可|
|smsConf.read | SMS 構成設定の読み取りの許可|
|smsConf.write | SMS 構成設定の更新、削除の許可|
|wnsConf.read | WNS 構成設定の読み取りの許可|
|wnsConf.write | WNS 構成設定の更新、削除の許可|
{: caption="表 1. スコープの説明" caption-side="top"}

<img class="gifplayer" alt="スコープのマッピング" src="images/scope-mapping.png"/>

### 認証済み通知
{: #authenticated-notifications }

認証済み通知とは、1 つ以上の `userId` に送信される通知です。  

**push.mobileclient** スコープ・エレメントをアプリケーションで使用されるセキュリティー検査にマップします。  

1. {{ site.data.keyword.mfp_oc_short_notm }} をロードし、**「 [ご使用のアプリケーション] 」→「セキュリティー」→「スコープ・エレメントのマッピング」**にナビゲートし、**「新規」**をクリックするか、既存のスコープ・マッピング・エントリーを編集します。
2. セキュリティー検査を選択します。 次に、**「追加」**をクリックします。

  <img class="gifplayer" alt="認証済みの通知" src="images/authenticated-notifications.png"/>

## タグの定義
{: #defining-tags }
{{ site.data.keyword.mfp_oc_short_notm }} →**「 [ご使用のアプリケーション] 」→「プッシュ」→「タグ」**で、**「新規」**をクリックします。  
適切な`「タグ名」`と`「説明」`を入力し、**「保存」**をクリックします。

<img class="gifplayer" alt="タグの追加" src="images/adding-tags.png"/>

サブスクリプションにより、デバイス登録とタグが結び付けられます。 デバイスがタグから登録解除されると、関連付けられたすべてのサブスクリプションが、デバイス自体から自動的にアンサブスクライブされます。 デバイスのユーザーが複数存在するシナリオでは、ユーザー・ログイン基準に基づいて、モバイル・アプリケーションにサブスクリプションを実装する必要があります。 例えば、ユーザーがアプリケーションに正常にログインした後にサブスクライブ呼び出しを行い、ログアウト・アクション処理の一部としてアンサブスクライブ呼び出しを明示的に行います。

## 次に使用するチュートリアル
{: #tutorials-to-follow-next }

* [プッシュ通知の送信](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)

これでサーバー・サイドがセットアップされたので、次はクライアント・サイドをセットアップし、受け取った通知を処理します。

* [クライアント・アプリケーションでのプッシュ通知の処理](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
