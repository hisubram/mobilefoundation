---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: push notifications, notifications, sending notification, HTTP/2

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


# プッシュ通知の送信
{: #send_push_notifications}

プッシュ通知は、{{ site.data.keyword.mfp_oc_short_notm }} から送信することも、REST API を使用して送信することもできます。
{: shortdesc}

* {{ site.data.keyword.mfp_oc_short_notm }} では、タグ通知およびブロードキャスト通知の 2 つのタイプの通知を送信できます。
* REST API では、タグ通知、ブロードキャスト通知、および認証済み通知のすべての形式の通知を送信できます。

## {{ site.data.keyword.mfp_oc_short_notm }} からのプッシュ通知の送信
{: #sending-push-notification-from-mobilefirst-operations-console }

単一デバイス ID、単一または複数のユーザー ID、iOS デバイスのみか Android デバイスのみ、またはタグにサブスクライブしているデバイスに対して通知を送信できます。

### タグ通知
{: #tag-notifications }

タグ通知は、特定のタグにサブスクライブしているすべてのデバイスをターゲットとする通知メッセージです。 タグはユーザーが関心のあるトピックを表し、選択した関心に従って通知を受けられる機能を提供します。

{{ site.data.keyword.mfp_oc_short_notm }} →**「 [ご使用のアプリケーション] 」→「プッシュ」→「通知の送信」**タブで、**「送信先」**タブから**「タグ別のデバイス」**を選択し、**「通知テキスト」**を入力します。 次に、**「送信」**をクリックします。

<img class="gifplayer" alt="タグ別の送信" src="images/sending-by-tag.png"/>

### ブロードキャスト通知
{: #broadcast-notifications }

ブロードキャスト通知は、サブスクライブされているすべてのデバイスを宛先とするタグ・プッシュ通知の一形態です。 ブロードキャスト通知は、デフォルトでは、予約済みの `Push.all` タグ (あらゆるデバイスで自動作成される) へのサブスクリプションによって、プッシュ対応のすべての {{ site.data.keyword.mobilefirst_notm }} アプリケーションに対して使用可能になります。 `Push.all` タグは、プログラマチックにアンサブスクライブできます。

{{ site.data.keyword.mfp_oc_short_notm }} →**「 [ご使用のアプリケーション] 」→「プッシュ」→「通知の送信」**タブで、**「送信先」**タブから**「すべて」**を選択し、**「通知テキスト」**を入力します。 次に、**「送信」**をクリックします。

<img class="gifplayer" alt="すべてに送信" src="images/sending-to-all.png"/>

## REST API の使用によるプッシュ通知の送信
{: #sending-push-notifications-using-rest-apis }

REST API を使用する場合は、タグ通知、ブロードキャスト通知、および認証済み通知のすべての形式の通知を送信できます。

通知を送信するために、POST を使用して REST エンドポイントへの要求が行われます (`imfpush/v1/apps/<application-identifier>/messages`)。  
以下は URL の例です。

```
https://myserver.com:443/imfpush/v1/apps/com.sample.PinCodeSwift/messages
```

すべてのプッシュ通知 REST API を確認するには、ユーザー資料の [REST API ランタイム・サービス](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/c_restapi_runtime.html)のトピックを参照してください。

### 通知ペイロード
{: #notification-payload }

要求には、以下のペイロード・プロパティーを含めることができます。

|ペイロード・プロパティー| 定義
|--- | ---
|message | 送信されるアラート・メッセージ |
|settings | 通知のさまざまな属性の設定。|
|target | ターゲットのセットで使用できるのは、コンシューマー ID、デバイス、プラットフォーム、またはタグです。 ターゲットのうちの 1 つのみを設定できます。|
|deviceIds | デバイス ID によって表されるデバイスの配列。 これらの ID を持つデバイスが通知を受け取ります。 これはユニキャスト通知です。|
|notificationType | メッセージの送信に使用されるチャネル (プッシュまたは SMS) を示す整数値。 許可される値は 1 (プッシュのみの場合)、2 (SMS のみの場合)、3 (プッシュと SMS 両方の場合) です |
|platforms | デバイス・プラットフォームの配列。 これらのプラットフォームを実行しているデバイスが通知を受け取ります。 サポートされる値は、A (Apple/iOS)、G (Google/Android)、M (Microsoft/Windows) です。|
|tagNames | tagNames として指定されたタグの配列。 これらのタグにサブスクライブされているデバイスが通知を受け取ります。 タグ・ベース通知にはこのタイプのターゲットを使用します。|
|userIds | 通知の送信先とする、ユーザー ID によって表されるユーザーの配列。 これはユニキャスト通知です。|
|phoneNumber | デバイスを登録し、通知を受け取るために使用される電話番号。 これはユニキャスト通知です。|
{: caption="表 1. ペイロード・プロパティー" caption-side="top"}

**プッシュ通知ペイロード JSON サンプル**

```json
{
    "message" : {
    "alert" : "Test message",
  },
  "settings" : {
    "apns" : {
      "badge" : 1,
      "iosActionKey" : "Ok",
      "payload" : "",
      "sound" : "song.mp3",
      "type" : "SILENT",
    },
    "gcm" : {
      "delayWhileIdle" : ,
      "payload" : "",
      "sound" : "song.mp3",
      "timeToLive" : ,
    },
  },
  "target" : {
    // The following list is for demonstration purposes only - per the documentation only 1 target is allowed to be used at a time.
    "deviceIds" : [ "MyDeviceId1", ... ],
    "platforms" : [ "A,G", ... ],
    "tagNames" : [ "Gold", ... ],
    "userIds" : [ "MyUserId", ... ],
  },
}
```

**SMS 通知ペイロード JSON サンプル**

```json
{
  "message" : {
    "alert": "Hello World from an SMS message"
  },
  "notificationType":3,
   "target" : {
     "deviceIds" : ["38cc1c62-03bb-36d8-be8e-af165e671cf4"]
   }
}
```

## 通知の送信
{: #sending-the-notification }

通知は、いろいろなツールを使用して送信できます。 テスト目的では、**Postman** が使用されます。以下のステップでセットアップについて説明します。

1. [機密クライアントを構成します](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients/)。
  REST API 経由でプッシュ通知を送信する場合、スペースで区切られたスコープ・エレメント `messages.write` と `push.application.<applicationId>` を使用します。
  <img class="gifplayer" alt="機密クライアントの構成" src="images/push-confidential-client.png"/>
2. [アクセス・トークンを作成します](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients#obtaining-an-access-token)。  
3. **http://localhost:9080/imfpush/v1/apps/com.sample.PushNotificationsAndroid/messages** への **POST** 要求を行います。
  - リモート {{ site.data.keyword.mobilefirst_notm }} を使用している場合、`hostname` と `port` の値を実際の値で置き換えてください。
  - アプリケーション ID 値を実際の値で更新します。
4. ヘッダーを設定します。
    - `**Authorization**: Bearer eyJhbGciOiJSUzI1NiIsImp ...`
    - **「Bearer」**の後に続く値をステップ (1) で入手したアクセス・トークンの値で置き換えます。
    ![Authorization ヘッダー](images/postman_authorization_header.png "Authorization ヘッダー")
5. 本体を設定します。
  - [通知ペイロード](#notification-payload)の説明に従ってプロパティーを更新します。
  - 例えば、**userIds** 属性が指定された **target** プロパティーを追加すると、特定の登録済みユーザーに通知を送信できます。
    ```json
    {
         "message" : {
             "alert" : "Hello World!"
        }
    }
    ```

    ![Authorization 本体](images/postman_json.png "Authorization 本体")

    **「送信」**ボタンをクリックした後、デバイスは通知を受け取ります。

    ![サンプル・アプリケーションのイメージ](images/notifications-app.png "モバイル・デバイス上のプッシュ通知")

## 通知のカスタマイズ
{: #customizing-notifications }

通知メッセージを送信する前に、以下の通知属性をカスタマイズすることもできます。  

{{ site.data.keyword.mfp_oc_short_notm }} →**「 [ご使用のアプリケーション] 」→「プッシュ」→「タグ」→「通知の送信」**タブで、**「iOS カスタム設定」/「Android カスタム設定」**セクションを展開し、通知属性を変更します。

### Android
{: #android }

* 通知音、FCM ストレージ内に通知を保管する期間、カスタム・ペイロードなど。
* 通知のタイトルを変更する場合は、Android プロジェクトの **strings.xml** ファイル内に `push_notification_tile` を追加します。

### iOS
{: #ios }

* 通知音、カスタム・ペイロード、アクション・キーのタイトル、通知タイプ、バッジの数値。

  ![プッシュ通知のカスタマイズ](images/customizing-push-notifications.png "「プッシュの送信」タブが選択された MobileFirst Operations Console の「プッシュ (Push)」ページ")

## APNs プッシュ通知の HTTP/2 サポート
{: #http2-support-for-apns-push-notifications}

Apple Push Notification service (APNs) は、HTTP/2 ネットワーク・プロトコルに基づく新しい API をサポートします。 HTTP/2 のサポートには、以下に示すような多くの利点があります。

* メッセージの長さが 2 KB から 4 KB に増加し、通知に余分なコンテンツを追加できるようになりました。
* クライアントとサーバー間の複数の接続が不要になるため、スループットが向上します。
* Universal Push Notification クライアント SSL 証明書をサポートします。

>{{ site.data.keyword.mobilefirst_notm }} のプッシュ通知で、レガシーの TCP ソケット・ベースの通知と HTTP/2 ベースの APNs プッシュ通知がサポートされるようになりました。

### HTTP/2 の有効化
{: #enabling-http2}

HTTP/2 ベースの通知は、JNDI プロパティーを使用して有効にすることができます。
```xml
<jndiEntry jndiName="imfpush/mfp.push.apns.http2.enabled" value= "true"/>
```
{: codeblock}

JNDI プロパティーを追加すると、TCP ソケット・ベースの既存の通知は使用されず、HTTP/2 ベースの通知のみが有効になります。
{: note}

### HTTP/2 の プロキシー・サポート
{: #proxy-support-for-http2}

HTTP/2 ベースの通知は、HTTP プロキシー経由で送信できます。 プロキシー経由での通知のルーティングを有効にするには、[ここ](#proxy-support)を参照してください。

## プロキシー・サポート
{: #proxy-support }
プロキシー設定を使用して、通知が Android デバイスおよび iOS デバイスに送信される際に経由するオプションのプロキシーを設定できます。 プロキシーの設定には、`push.apns.proxy.*` 構成プロパティーと `push.gcm.proxy.*` 構成プロパティーを使用できます。 詳しくは、[{{ site.data.keyword.mfserver_short_notm }} プッシュ・サービスの JNDI プロパティーのリスト](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/installation-configuration/production/server-configuration/#list-of-jndi-properties-for-mobilefirst-server-push-service)を参照してください。

## 次のステップ
{: #next-tutorial-to-follow }
これでサーバー・サイドがセットアップされたので、次は、以下のチュートリアルを使用してクライアント・サイドをセットアップし、受け取った通知を処理します。

* [クライアント・アプリケーションでのプッシュ通知の処理](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
