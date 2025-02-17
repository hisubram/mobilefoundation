---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile foundation security, MIM attack, certificate pinning

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 中間者攻撃の防止
{: #prevent_man_in_the_middle_attack}


公衆網を使用して通信している場合、情報を安全に送受信することが重要です。 こうした通信を保護するために広く使用されているプロトコルが SSL/TLS です。 SSL/TLS は、デジタル証明書を使用して認証と暗号化を提供します。 これらのプロトコルは、証明書チェーンの検証に依存し、中間者攻撃 (不正な人物が、モバイル・デバイスとバックエンド・システムの間を行き来するすべてのトラフィックを参照して変更できる場合に発生する) などのいくつかの危険な攻撃に対して脆弱です。
{: shortdesc}

[証明書ピン留め](#cert_pinning)および中間者 (MitM) 攻撃の防止により、セキュリティー攻撃を削減します。

この機能はオプションであり、プロフェッショナル・プランでのみ使用できます。
{: note}

証明書ピン留めプロセスは、次のステップで構成されています。

1. [証明書セットアップ](#certsetup)を実行します。
2. 保護された要求を行う前に正しい[証明書ピン留め API](#certpinapi) メソッドを呼び出すように、クライアント・コードを構成します。

SSL ハンドシェーク (サーバーへの最初の要求) の間、Mobile Foundation クライアント SDK は、サーバー証明書の公開鍵が、アプリに保管されている証明書の公開鍵に一致することを検証します。

認証局から購入した証明書を使用する必要があります。 自己署名証明書は**サポートされません**。
{: note}

## 証明書ピン留め
{: #cert_pinning}

証明書チェーンの検証に依存する SSL/TLS などのプロトコルは、中間者攻撃 (不正な人物が、モバイル・デバイスとバックエンド・システムの間を行き来するすべてのトラフィックを参照して変更できる場合に発生する) などのいくつかの危険な攻撃に対して脆弱です。 証明書は、本物であり有効であると信頼するために、トラステッド認証局 (CA) に属しているルート証明書によってデジタル署名されます。 オペレーティング・システムとブラウザーは、トラステッド CA ルート証明書のリストを保守して、CA が発行し署名した証明書を容易に検証できるようにします。

IBM Mobile Foundation では、**証明書ピン留め**を可能にする API が提供されています。 この API は、ネイティブ iOS、ネイティブ Android、およびクロスプラットフォーム Cordova の MobileFirst アプリケーションでサポートされます。

## 証明書ピン留めプロセス
{: #certpinprocess}

証明書ピン留めとは、ホストと予期される公開鍵とを関連付けるプロセスです。 オペレーティング・システムまたはブラウザーによって認識されるトラステッド CA ルート証明書に対応する証明書の代わりに、ユーザーのドメイン・ネーム用の特定の証明書のみを受け入れるように、クライアント・コードを構成することができます。 証明書のコピーがクライアント・アプリケーション内に置かれます。 SSL ハンドシェーク (サーバーへの最初の要求) の間、MobileFirst クライアント SDK は、サーバー証明書の公開鍵が、アプリに保管されている証明書の公開鍵に一致することを検証します。

複数の証明書をクライアント・アプリケーションにピン留めすることもできます。 すべての証明書のコピーをクライアント・アプリケーション内に配置してください。 SSL ハンドシェーク (サーバーへの最初の要求) の間、MobileFirst クライアント SDK は、サーバー証明書の公開鍵が、アプリケーションに保管されているいずれかの証明書の公開鍵に一致することを検証します。
{: note}

* **重要**
    * 一部のモバイル・オペレーティング・システムは、証明書の検証チェック結果をキャッシュすることがあります。 したがって、使用するコードは、保護された要求を行う**前**に、証明書ピン留め API メソッドを呼び出します。 そうしないと、以降の要求で証明書の検証とピン留め検査がスキップされる可能性があります。
    * 証明書ピン留めが行われた後も、関連ホストとの通信にはすべて Mobile Foundation API のみを使用してください。 同じホストとの対話にサード・パーティーの API を使用すると、検証されていない証明書がモバイル・オペレーティング・システムによってキャッシュに入れられるなど、予期しない動作につながるおそれがあります。
    * 証明書ピン留め API メソッドを 2 回目に呼び出すと、前のピン留め操作はオーバーライドされます。

ピン留めプロセスが正常に行われると、保護された要求 SSL/TLS ハンドシェーク時、提供された証明書内の公開鍵を使用して、MobileFirst Server 証明書の保全性が検証されます。 ピン留めプロセスが失敗すると、サーバーへのすべての SSL/TLS 要求はクライアント・アプリケーションによって拒否されます。

## 証明書のセットアップ
{: #certsetup}

認証局から購入した証明書を使用する必要があります。 自己署名証明書は**サポートされません**。 サポートされる環境との互換性のために、**DER** (国際電気通信連合 X.690 規格に定義されている Distinguished Encoding Rules) フォーマットでエンコードされた証明書を使用するようにしてください。

証明書は、MobileFirst Server 内とアプリケーション内の両方に配置する必要があります。 証明書は次のように配置してください。

* MobileFirst Server (WebSphere Application Server、WebSphere Application Server Liberty、または Apache Tomcat) 内: SSL/TLS および証明書の構成方法については、使用している特定のアプリケーション・サーバーの資料を参照してください。
* アプリケーション内で、以下のようにします。
    * ネイティブ iOS の場合、証明書をアプリケーション・**バンドル**に追加します
    * ネイティブ Android の場合、証明書を **assets** フォルダーに入れます
    * Cordova の場合、証明書を **app-name\www\certificates** フォルダーに入れます (フォルダーがまだ存在しない場合は、作成してください)

## 証明書ピン留め API
{: #certpinapi}

証明書ピン留めは次の多重定義された API メソッドからなります。1 つのメソッドにはパラメーター ``certificateFilename`` (``certificateFilename`` は証明書ファイルの名前) があり、2 番目のメソッドにはパラメーター ``certificateFilenames`` (``certificateFilenames`` は証明書ファイルの名前の配列です) があります。

### Android
{: #certpinapiandroid}

* 単一の証明書:

    構文: pinTrustedCertificatePublicKeyFromFile(String certificateFilename);

    例:

    ```java
    WLClient.getInstance().pinTrustedCertificatePublicKey("myCertificate.cer");
    ```

* 複数の証明書:

    構文: pinTrustedCertificatePublicKeyFromFile(String[] certificateFilename);

    例:

    ```java
    String[] certificates={"myCertificate.cer","myCertificate1.cer"};
WLClient.getInstance().pinTrustedCertificatePublicKey(certificates);
    ```

証明書ピン留めメソッドでは、次の 2 つの場合に例外が発生します。

* ファイルが存在しない
* ファイルのフォーマットが正しくない

### iOS
{: #certpinapiios}

* 単一の証明書ピン留め

    構文: pinTrustedCertificatePublicKeyFromFile:(NSString*) certificateFilename;

    証明書ピン留めメソッドでは、次の 2 つの場合に例外が発生します。

    * ファイルが存在しない
    * ファイルのフォーマットが正しくない

* 複数の証明書ピン留め

    構文: pinTrustedCertificatePublicKeyFromFiles:(NSArray*) certificateFilenames;

    証明書ピン留めメソッドでは、次の 2 つの場合に例外が発生します。

    * 証明書ファイルが存在しない
    * 正しいフォーマットの証明書ファイルがない

**Objective-C の場合**:

例: 単一の証明書:

```objectc
[[WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFile:@"myCertificate.cer"];
```

例: 複数の証明書:

```objectc
NSArray *arrayOfCerts = [NSArray arrayWithObjects:@“Cert1”,@“Cert2”,@“Cert3",nil];
[[WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFiles:arrayOfCerts];
```

**Swift の場合**:

例: 単一の証明書:

```swift
WLClient.sharedInstance().pinTrustedCertificatePublicKeyFromFile("myCertificate.cer")
```

例: 複数の証明書:

```swift
let arrayOfCerts : [Any] = ["Cert1", "Cert2”, "Cert3”];
WLClient.sharedInstance().pinTrustedCertificatePublicKey( fromFiles: arrayOfCerts)
```

証明書ピン留めメソッドでは、次の 2 つの場合に例外が発生します。

* ファイルが存在しない
* ファイルのフォーマットが正しくない

### Cordova
{: #certpinapicordova}

* 単一の証明書ピン留め:

    ```javascript
    WL.Client.pinTrustedCertificatePublicKey('myCertificate.cer').then(onSuccess, onFailure);
    ```

* 複数の証明書ピン留め:

    ```javascript
    WL.Client.pinTrustedCertificatePublicKey(['Cert1.cer','Cert2.cer','Cert3.cer']).then(onSuccess, onFailure);
    ```

証明書ピン留めメソッドは、次のように確約を返します。

* 証明書ピン留めメソッドは、ピン留めが成功した場合は `onSuccess` メソッドを呼び出します。
* 証明書ピン留めメソッドは、次の 2 つのケースでは `onFailure` コールバックをトリガーします。
  * ファイルが存在しない。
  * ファイルのフォーマットが正しくない。

その後、証明書がピン留めされていないサーバーに対して、保護された要求が行われると、その特定の要求 (例えば、`obtainAccessToken` または `WLResourceRequest`) の `onFailure` コールバックが呼び出されます。

証明書ピン留め API メソッドについて詳しくは、[API リファレンス](/docs/services/mobilefoundation?topic=mobilefoundation-client_sdks#client_sdks)を参照してください。
{: note}
