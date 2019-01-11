---

copyright:
  years: 2018
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

# Cordova / Web アプリケーション用の API (Javascript)

以下の表に、Javascript アプリケーションで実行できる関数と、対応する API メソッドを示します。

| 関数 | 説明 |
|----------|-------------|
| `WL.Client`、`WL.App` | アプリケーションの初期化と再ロード、アプリケーション・テキストのグローバル化 |
| `WLAuthorizationManager` | クライアント ID および許可ヘッダーの取得 |
| `WL.Logger` | 環境のログへのログ・メッセージの出力 |
| `WL.NativePage` | 現在表示されている Web ベースの画面とネイティブに書き込まれたページとの切り替え |
| `WLResourceRequest` | 保護リソースおよび無保護リソースへの要求の送信 |
| `WL.JSONStore` | 軽量なドキュメント指向のストレージ・システムを提供するクライアント・サイド API |

## 追加情報
{: #additional-information }
### options オブジェクト
{: #the-optional-object }
`options` オブジェクトには、すべてのメソッドに共有の属性が含まれています。これは、{{ site.data.keys.mf_server }} へのすべての非同期呼び出しで使用されます。

場合によっては、特定のメソッドにのみ適用可能なプロパティーによって拡張されることがあります。これらの追加プロパティーは、特定のメソッドの説明の一部として詳述されます。

options オブジェクトの共通プロパティーは以下のとおりです。

```javascript
options = {
    onSuccess: successHandler(response),
    onFailure: failureHandlder(response),
    invocationContext: invocation-context
};
```
{: code}
各プロパティーの意味は次のとおりです。

| プロパティー | 説明 |
|----------|-------------|
| `onSuccess` | オプション。 非同期呼び出しが正常に完了したときに呼び出される関数。`onSuccess` 関数の構文は、`success-handler-function(response)` です。ここで、`response` は、少なくとも次のプロパティーを含むオブジェクトです。{::nomarkdown}<ul><li><b>invocationContext</b> - 元々 <code>options</code> オブジェクトで {{ site.data.keys.mf_server }} に渡された <code>invocationContext</code> オブジェクト (<code>invocationContext</code> オブジェクトが渡されなかった場合は <code>undefined</code>)。</li><li><b>status</b> - HTTP 応答状況</li></ul>{:/} **注:** `response` オブジェクトに追加プロパティーが含まれているメソッドの場合、これらのプロパティーは特定のメソッドの説明の一部として詳述されます。|
| `onFailure` | オプション。 非同期呼び出しが失敗したときに呼び出される関数。そのような失敗には、サーバー接続障害や呼び出しのタイムアウトなど、非同期呼び出し中に発生したサーバー・サイドとクライアント・サイドの両方のエラーが含まれます。**注:** この関数は、例外をスローすることで実行を停止するクライアント・サイド・エラーの場合には呼び出されません。onFailure 関数の構文は `failure-handler-function(response)` です。ここで、`response` は、次のプロパティーを含むオブジェクトです。{::nomarkdown}<ul><li><b>invocationContext</b> - 元々 <code>options</code> オブジェクトで {{ site.data.keys.mf_server }} に渡された <code>invocationContext</code> オブジェクト (<code>invocationContext</code> オブジェクトが渡されなかった場合は <code>undefined</code>)。</li><li><b>errorCode</b> - エラー・コード・ストリング。返される可能性のあるすべてのエラー・コードが <b>worklight.js</b> ファイル内の <code>WL.ErrorCode</code> オブジェクトに定数として定義されています。</li><li><b>errorMsg</b> - {{ site.data.keys.mf_server }} によって提供されるエラー・メッセージ。このメッセージは開発者専用であり、ユーザーに表示される必要はありません。ユーザーの言語に変換されることはありません。</li><li><b>status</b> - HTTP 応答状況</li></ul>{:/} **注:** `response` オブジェクトに追加プロパティーが含まれているメソッドの場合、これらのプロパティーは特定のメソッドの説明の一部として詳述されます。|
| `invocationContext` | オプション。 成功ハンドラーおよび失敗ハンドラーに返されるオブジェクト。`invocationContext` オブジェクトは、呼び出し側非同期サービスのコンテキストをサービスから戻るときに保持するために使用します。例えば、同じ成功ハンドラーを使用して `invokeProcedure` メソッドが連続的に呼び出される場合があります。成功ハンドラーは、どの invokeProcedure 呼び出しが処理されているのかを識別できる必要があります。1 つの解決策は、`invocationContext` オブジェクトを整数として実装し、`invokeProcedure` の呼び出しごとにその値を 1 つ増やすことです。成功ハンドラーが起動されると、{{ site.data.keys.product_adj }} フレームワークはこのハンドラーに、`invokeProcedure` メソッドに関連付けられた options オブジェクトの `invocationContext` オブジェクトを渡します。`invocationContext` オブジェクトの値を使用して、処理中の結果が関連付けられている `invokeProcedure` 呼び出しを特定できます。|

## WL.ClientMessages オブジェクト
{: #the-wlclientmessages-object }
`WL.ClientMessages` オブジェクトに保管されているシステム・メッセージのリストを表示し、これらのシステム・メッセージの翻訳を有効にすることができます。

コードの一部はアプリケーションが正常に初期化された後に実行されるため、システム・メッセージはグローバル JavaScript レベルでオーバーライドする必要があります。
{: note}

`WL.ClientMessages` オブジェクトは、**worklight/messages/messages.json** ファイルに定義されたシステム・メッセージを保管するオブジェクトです。このファイルは、{{ site.data.keys.product }} で生成したプロジェクトの環境フォルダーに入っています。システム・メッセージの翻訳を有効にするには、以下のコード例に示すように、`WL.ClientMessages` オブジェクト内のメッセージの値をオーバーライドする必要があります。

```javascript
WL.ClientMessages.invalidUsernamePassword="The custom user name and password are not valid";
```
{: code}


* [JavaScript API リファレンス](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/#javascript-api-reference)
* [Objective-C API リファレンス (Cordova 用)](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/#objective-c-api-reference-for-cordova)
{: note}
