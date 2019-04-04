---

copyright:
  years: 2018, 2019
lastupdated:  "2019-02-12"

keywords: JSONStore, offline storage, jsonstore error codes

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:pre: .pre}


# JSONStore
{: #jsonstore }
{{site.data.keyword.mobilefoundation_short}} **JSONStore** は、軽量なドキュメント指向のストレージ・システムを提供する、オプションのクライアント・サイド API です。 JSONStore を使用すると、**JSON ドキュメント**を永続的に保管できます。 JSONStore では、アプリケーションを実行しているデバイスがオフラインの時でも、アプリケーションのドキュメントを使用できます。 この永続的に常時使用できるストレージにより、例えば、使用可能なネットワーク接続がデバイスにないときでも、ドキュメントにアクセスできるため便利です。

リレーショナル・データベース用語は開発者にとって馴染み深いため、この資料では、JSONStore の説明においてリレーショナル・データベース用語を使用しています。 ただし、リレーショナル・データベースと JSONStore との間には多くの違いがあります。 例えば、リレーショナル・データベースでのデータの保管に使用される厳格なスキーマは、JSONStore のアプローチとは異なります。 JSONStore では、どのような JSON コンテンツも保管可能で、検索の必要があるコンテンツは索引付けすることができます。

## 主な機能
{: #key-features-jsonstore }
* 効率的な検索のためのデータ索引付け
* 保管データに対するローカルのみの変更を追跡するためのメカニズム
* 多数のユーザーのサポート
* 保管データの AES 256 暗号化 により、セキュリティーと機密性が提供されます。 1 つのデバイスに複数のユーザーが存在する場合、パスワード保護を使用して、ユーザー別に保護をセグメント化することが可能です。

1 つのストアが多数のコレクションを含み、各コレクションが多数のドキュメントを含むことが可能です。 また、複数のストアからなる 1 つの MobileFirst アプリケーションを持つこともできます。 詳しくは、JSONStore の『複数ユーザー・サポート』を参照してください。

## サポート・レベル
{: #support-level-jsonstore }
* JSONStore は、ネイティブ iOS アプリケーションおよび Android アプリケーションでサポートされています (ネイティブ Windows (Universal および UWP) ではサポートされていません)。
* JSONStore は、Cordova iOS、Android、および Windows (Universal および UWP) アプリケーションでサポートされています。


## JSONStore の一般用語
{: #general-jsonstore-terminology }
### ドキュメント
{: #document }
ドキュメントは、JSONStore の基本的なビルディング・ブロックです。

JSONStore ドキュメントは、自動的に生成される ID (`_id`) と JSON データを持つ JSON オブジェクトです。 これは、データベース用語のレコードや行に似ています。 `_id` の値は常に、特定のコレクション内の固有の整数です。 `JSONStoreInstance` クラスの `add`、`replace`、`remove` などの一部の関数では、ドキュメントまたはオブジェクトの配列を取ります。 これらのメソッドは、さまざまなドキュメントまたはオブジェクトに対して一度に操作を実行する際に役立ちます。

**単一のドキュメント**  

```javascript
var doc = { _id: 1, json: {name: 'carlos', age: 99} };
```
{: codeblock}

**ドキュメント配列**

```javascript
var docs = [
  { _id: 1, json: {name: 'carlos', age: 99} },
  { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### コレクション
{: #collection }
JSONStore コレクションは、データベース用語の表に似ています。  
以下のコード・サンプルは、ドキュメントをディスクに保管する方法ではなく、コレクションの概要がどのようなものかを視覚化するのに適した方法です。

```javascript
[
    { _id: 1, json: {name: 'carlos', age: 99} },
    { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### ストア
{: #store }
ストアは、1 つ以上のコレクションからなる永続 JSONStore ファイルです。  
ストアは、データベース用語のリレーショナル・データベースに似ています。 ストアは、JSONStore とも呼ばれます。

### 検索フィールド
{: #search-fields }
検索フィールドは、キーと値のペアです。  
検索フィールドは、検索時間を高速にするために索引付けされたキーであり、データベース用語の列フィールドまたは列属性と似ています。

追加の検索フィールドは、索引付けされているが、保管された JSON データの一部ではないキーです。 これらのフィールドは、(JSON コレクション内の) 値が索引付けされたキーを定義し、検索をより素早く行うために使用できます。

有効なデータ型は、String、Boolean、Number、および Integer です。 これらは単に型のヒントであり、型の妥当性検査はありません。 また、これらの型によって、索引付け可能なフィールドの保管方法が決定されます。 例えば、`{age: 'number'}` は 1 を 1.0 として索引付けし、`{age: 'integer'}` は 1 を 1 として索引付けします。

**検索フィールドおよび追加の検索フィールド**

```javascript
var searchField = {name: 'string', age: 'integer'};
var additionalSearchField = {key: 'string'};
```
{: codeblock}

索引付けが可能なのはオブジェクト内のキーのみです。オブジェクトそのものではありません。 配列はパススルー方式で処理されます。つまり、配列を索引付けることも、配列の特定の索引を索引付けることもできません (arr[n]) が、配列内部のオブジェクトを索引付けることはできます。

**配列内の値の索引付け**

```javascript

var searchFields = {
    'people.name' : 'string', // matches carlos and tim on myObject
    'people.age' : 'integer' // matches 99 and 100 on myObject
};

var myObject = {
    people : [
        {name: 'carlos', age: 99}, 
        {name: 'tim', age: 100}
    ]
};
```
{: codeblock}

### 照会
{: #queries }
照会は、検索フィールドまたは追加の検索フィールドを使用してドキュメントを検索するオブジェクトです。  
これらの例では、name 検索フィールドは型 string であり、age 検索フィールドは型 integer であることを前提としています。

**`carlos` と一致する `name` のドキュメントを検索**

```javascript
var query1 = {name: 'carlos'};
```
{: codeblock}

**`carlos` と一致する `name` で、`99` と一致する `age` のドキュメントを検索**

```javascript
var query2 = {name: 'carlos', age: 99};
```
{: codeblock}

### 照会部分
{: #query-parts }
照会部分は、より高度な検索をビルドするために使用されます。 一部のバージョンの `find` や `count` など、一部の JSONStore 操作は照会部分を使用します。 照会部分内のすべてのものは `AND` ステートメントで結合されますが、照会部分自体は `OR` ステートメントで結合されます。 照会部分のすべてのものが **true** である場合にのみ、検索基準が一致を返します。 複数の照会部分を使用して、1 つ以上の照会部分を満たす一致を検索することができます。

照会部分による検索は、最上位検索フィールドにのみ機能します。 例えば、`name.first` ではなく `name` を検索します。 この振る舞いを回避するには、すべての検索フィールドが最上位である複数のコレクションを使用します。 最上位でない検索フィールドで機能する照会部分操作は `equal`、`notEqual`、`like`、`notLike`、`rightLike`、`notRightLike`、`leftLike`、および `notLeftLike` です。 最上位でない検索フィールドを使用した場合、振る舞いは不確定になります。

## フィーチャー表
{: #features-table }
JSONStore フィーチャーを、その他のデータ・ストレージのテクノロジーおよび形式のフィーチャーと比較します。

JSONStore は、MobileFirst プラグインを使用する Cordova アプリケーション内のデータを保管する JavaScript API、ネイティブ iOS アプリケーション対応の Objective-C API、および Android アプリケーション対応の Java API です。 参考のために、さまざまな JavaScript ストレージ・テクノロジーの比較を次に示します。これにより、JSONStore がこれらのテクノロジーと比べてどのようなものであるかが分かります。

JSONStore は、LocalStorage、Indexed DB、Cordova Storage API、Cordova File API などのテクノロジーと似ています。 以下の表は、JSONStore によって提供されるいくつかのフィーチャーが他のテクノロジーと比べてどうであるかを示しています。 JSONStore フィーチャーは、iOS および Android のデバイスおよびシミュレーターのみで使用可能です。

| 機能                                            | JSONStore      | LocalStorage | IndexedDB | Cordova ストレージ API | Cordova ファイル API |
|----------------------------------------------------|----------------|--------------|-----------|---------------------|------------------|
| Android サポート (Cordova &amp; ネイティブ・アプリケーション)|	     ✔ 	      |      ✔	    |     ✔	     |        ✔	           |         ✔	      |
| iOS サポート (Cordova & ネイティブ・アプリケーション)	     |	     ✔ 	      |      ✔	    |     ✔	     |        ✔	           |         ✔	      |
| Windows 8.1 Universal および Windows 10 UWP (Cordova アプリケーション)          |	     ✔ 	      |      ✔	    |     ✔	     |        -	           |         ✔	      |
| データ暗号化	                                 |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| 最大ストレージ	                                 |使用可能なスペース |    最大 5 MB     |   最大 5 MB 	 | 使用可能なスペース	   | 使用可能なスペース  |
| 信頼性の高いストレージ (注を参照)	                     |	     ✔ 	      |      -	    |     -	     |        ✔	           |         ✔	      |
| ローカルでの変更のトラッキング	                     |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| マルチユーザーのサポート                                 |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| 索引付け	                                         |	     ✔ 	      |      -	    |     ✔	     |        ✔	           |         -	      |
| ストレージのタイプ	                                 | JSON ドキュメント | 鍵と値のペア | JSON ドキュメント | リレーショナル (SQL) | ストリング     |

信頼性の高いストレージ は、以下のイベントのいずれかが発生しない限り、データが削除されないことを意味します。
* アプリケーションがデバイスから除去された。
* データを除去するメソッドのいずれかが呼び出された。
{: note}

## 複数ユーザー・サポート
{: #multiple-user-support }
JSONStore を使用すると、単一の MobileFirst アプリケーションに、さまざまなコレクションからなる多数のストアを作成できます。

init (JavaScript) API または open (ネイティブ iOS および ネイティブ Android) API は、ユーザー名を持つ Options オブジェクトを使用することができます。 異なるストアは、ファイル・システム内では別個のファイルです。 ユーザー名がストアのファイル名として使用されます。 セキュリティーおよびプライバシー上の理由により、これらの個別のストアを異なるパスワードで暗号化することも可能です。 closeAll API を呼び出すと、すべてのコレクションに対するアクセス権が除去されます。 また changePassword API を呼び出して、暗号化されたストアのパスワードを変更することもできます。

ユース・ケースの一例としては、さまざまな従業員が同じ物理デバイス (例えば iPad や Android タブレットなど) と MobileFirst アプリケーションを共有しているケースが考えられます。 複数ユーザーのサポートは、従業員がさまざまなシフトで勤務していて MobileFirst アプリケーションを使用しながらさまざまな顧客のプライベート・データを扱う場合に便利です。

## セキュリティー
{: #security-jsonstore }
ストア内のすべてのコレクションは、暗号化により保護することができます。

ストア内のすべてのコレクションを暗号化するには、パスワードを `init` (JavaScript) API または `open` (ネイティブ iOS および ネイティブ Android) API に渡します。 パスワードを渡さないと、ストアのコレクション内にあるドキュメントはいずれも暗号化されません。

一部のセキュリティー成果物 (ソルト など) は、キーチェーン (iOS)、共有設定 (Android) および資格情報保管ボックス (Windows Universal 8.1 および Windows 10 UWP) に保管されています。 ストアは 256 ビットの Advanced Encryption Standard (AES) 鍵で暗号化されます。 すべての鍵は Password-Based Key Derivation Function 2 (PBKDF2) により強化されています。 アプリケーションのデータ・コレクションを暗号化するよう選択することはできますが、暗号化とプレーン・テキストの形式を切り替えたり、ストア内で複数の形式を混用したりすることはできません。

ストア内でデータを保護する鍵は、指定されるユーザー・パスワードに基づいています。 その鍵が有効期限切れになることはありませんが、changePassword API を呼び出してその鍵を変更することができます。

データ保護鍵 (DPK) は、ストアの内容を暗号化解除するために使用される鍵です。 DPK は、アプリケーションがアンインストールされた場合でも、iOS キーチェーン内に保持されています。 キーチェーン内の鍵と JSONStore がアプリケーションに入れた他のすべてのものの両方を除去するには、destroy API を使用します。 このプロセスは、暗号化された DPK が共有設定に保管され、アプリケーションのアンインストール時に完全にワイプされるため、Android には適用されません。

JSONStore がパスワードを使用してコレクションを初めて開く場合 (つまり開発者がストア内でデータを暗号化したい場合)、JSONStore はランダム・トークンを必要とします。 そのランダム・トークンは、クライアントまたはサーバーから取得することができます。

localKeyGen 鍵が JSONStore API の JavaScript 実装環境にあり、true の値になっている場合、暗号的にセキュアなトークンがローカルに生成されます。 それ以外の場合、トークンはサーバーにアクセスすることによって生成されます。MobileFirst Server との接続が必要になります。 このトークンは、ストアが初回にパスワードを使用して開かれるときにのみ必要です。 ネイティブ実装環境 (Objective-C および Java) では暗号論的に安全なトークンがデフォルトでローカルに生成されます。生成されない場合は、secureRandom オプションを使用してトークンを渡すことができます。

以下のようなトレードオフ関係があります。
* ストアをオフラインで開くことと、クライアントを信頼してランダム・トークンを生成すること (安全性が低い)、または、
* MobileFirst Server にアクセスしてストアを開くこと (接続が必要) と、サーバーを信頼すること (安全性が高い)

### セキュリティー・ユーティリティー
{: #security-utilities }
MobileFirst クライアント・サイド API は、ユーザーのデータを保護するのに役に立つセキュリティー・ユーティリティーをいくつか提供しています。 JSONStore などのフィーチャーは、JSON オブジェクトを保護する場合に優れた能力を発揮します。 ただし、JSONStore コレクションにバイナリー blob を保管することはお勧めできません。

代わりに、バイナリー・データをファイル・システムに保管して、ファイル・パスやその他のメタデータを JSONStore コレクション内に保管します。 イメージなどのファイルを保護したい場合、それを base64 ストリングとしてエンコードし、暗号化して、ディスクに出力を書き込むことができます。

データの暗号化を解除するには、JSONStore コレクション内のメタデータを検索し、暗号化されたデータをディスクから読み取り、保管されたメタデータを使用して暗号化を解除します。

このメタデータには鍵、ソルト、初期設定ベクトル (IV)、ファイルのタイプ、ファイルへのパスなどを含めることができます。

詳しくは、[JSONStore セキュリティー・ユーティリティー](/docs/services/mobilefoundation?topic=mobilefoundation-security_utilities#security_utilities)を参照してください。
{: tip}

### Windows 8.1 Universal および Windows 10 UWP の暗号化
{: #windows-81-universal-and-windows-10-uwp-encryption }
ストア内のすべてのコレクションは、暗号化により保護することができます。

JSONStore は、基礎となるデータベース・テクノロジーとして [SQLCipher ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://sqlcipher.net/){: new_window} を使用します。 SQLCipher は、Zetetic LLC 製作の SQLite ビルドであり、暗号化のレイヤーをデータベースに追加します。

JSONStore は、すべてのプラットフォームで SQLCipher を使用します。 Android および iOS では、Community Edition とも呼ばれる無料オープン・ソース・バージョンの SQLCipher が使用可能です。これは、Mobile Foundation に組み込まれている JSONStore のバージョンに取り込まれています。 SQLCipher の Windows バージョンは、商用ライセンスの下でのみ入手可能であり、Mobile Foundation が直接再配布することはできません。

代わりに、Windows 8 Universal 用の JSONStore に SQLite が基本データベースとして組み込まれています。 これらのプラットフォームのいずれかでデータを暗号化するには、独自のバージョンの SQLCipher を獲得して、Mobile Foundation に組み込まれている SQLite バージョンをスワップアウトする必要があります。

暗号化が必要ない場合、Mobile Foundation 内の SQLite バージョンを使用しても (暗号化を差し引いた状態で) JSONStore が完全に機能します。

#### Windows Universal および Windows UWP 用の SQLite から SQLCipher への置換
{: #replacing-sqlite-with-sqlcipher-for-windows-universal-and-windows-uwp }
1. SQLCipher for Windows Runtime Commercial Edition に同梱されている SQLCipher for Windows Runtime 8.1/10 拡張を実行します。
2. 拡張のインストールが終了したら、作成されたばかりの **sqlite3.dll** ファイルの SQLCipher バージョンを見つけ出します。 x86 用、x64 用、および ARM 用のファイルがそれぞれ 1 つずつ存在します。

   ```bash
   C:\Program Files (x86)\Microsoft SDKs\Windows\v8.1\ExtensionSDKs\SQLCipher.WinRT81\3.0.1\Redist\Retail\<platform>
   ```
   {: codeblock}

3. このファイルをご使用の MobileFirst アプリケーションにコピーして置換します。

   ```bash
   <Worklight project name>\apps\<application name>\windows8\native\buildtarget\<platform>
   ```
   {: codeblock}

## パフォーマンス
{: #performance-jsonstore }
JSONStore のパフォーマンスに影響する可能性のある要因は、以下のとおりです。

### ネットワーク
{: #network-jsonstore }
* すべてのダーティー・ドキュメントを  アダプターに送るなどの操作を実行する前に、ネットワーク接続をチェックしてください。
* ネットワーク経由でクライアントへ送信されるデータ量は、パフォーマンスに大きく影響します。 バックエンド・データベース内のすべてのものをコピーするのではなく、アプリケーションに必要なデータのみを送ってください。
* アダプターを使用している場合、compressResponse フラグを true に設定することを検討してください。 このようにすると、応答が圧縮されます。これにより、一般的に圧縮がない場合に比べて、使用される処理能力が節約され、転送時間が短縮されます。

### メモリー
{: #memory-jsonstore }
* JavaScript API を使用すると、JSONStore ドキュメントは、ネイティブ (Objective-C、Java、または C#) レイヤーと JavaScript レイヤーの間で、ストリングとしてシリアライズされたりデシリアライズされたりします。 発生し得るメモリーの問題を軽減する 1 つの方法は、find API の使用時に制限とオフセットを使用することです。 このようにすると、結果に割り振られるメモリーの容量が制限されるので、ページ編集などを実装できます (X はページ当たりの結果数)。
* 最終的にストリングとしてシリアライズおよびデシリアライズされる長い鍵名を使用する代わりに、それらの長い鍵名を短い名前にマップすること (例えば、` myVeryVeryVerLongKeyName` を `k` または `key` にマップするなど) を検討してください。 長い鍵名をアダプターからクライアントへ送信するときにそれを短い鍵名にマップし、バックエンドにデータを送信して戻すときに元の長い鍵名にマップするのが理想的です。
* ストア内のデータをさまざまなコレクションに分割することを検討してください。 単一のコレクション内に一体構造のドキュメントを保持するのではなく、さまざまなコレクションで小さなドキュメントを保持します。 こうした検討は、データ同士がどの程度密接に関連しているか、および当該データのユースケースに依存します。
* オブジェクトの配列を使用して add API を使用する際には、メモリーの問題が発生する可能性があります。 この問題を軽減するには、一度に指定する JSON オブジェクトを少なくしてこれらのメソッドを呼び出してください。
* JavaScript および Java™ にはガーベッジ・コレクターが備わっていますが、Objective-C には Automatic Reference Counting が備わっています。 これが機能できるようにしてください。ただし完全にこれに依存しないようにしてください。 もう使用されていない参照をヌルに設定し、プロファイル・ツールを使用して、メモリー使用量が予想どおりに減少していることを確認してください。

### CPU
{: #cpu-jsonstore }
* 索引付けを行う add メソッドを呼び出す際、使用される検索フィールドと追加の検索フィールドの数量はパフォーマンスに影響します。 find メソッドの照会に使用する値にのみ索引を付けてください。
* デフォルトでは、JSONStore はそのドキュメントに対するローカルでの変更をトラッキングします。 add、remove、および replace の各 API を使用する際に `markDirty` フラグを **false** に設定すればこの動作を無効にすることができ、数サイクルを節約できます。
* セキュリティーを有効にすると、`init` API または `open` API や、コレクション内のドキュメントを処理する他の操作に、いくらかのオーバーヘッドが加わります。 セキュリティーが本当に必要かどうかを検討してください。 例えば、open API は、暗号化と暗号化解除に使用される暗号鍵を生成しなければならないため、暗号化によりかなり遅くなります。
* `replace` API および `remove` API は、コレクション全体を調べてすべてのオカレンスの置き換えおよび除去を行う必要があるため、コレクション・サイズに依存します。 各レコードを調べる必要があるため、暗号化の使用時にはそれらのレコードをすべて暗号化解除する必要があり、大幅に遅くなります。 このパフォーマンス・ヒットは、大きいコレクションでは顕著です。
* `count` API は、相対的に費用がかかります。 ただし、そのコレクションのカウントを保持する変数を保持することができます。 内容をコレクションに保管したり、コレクションから除去したりするたびに、この変数を更新してください。
* `find` API (`find`、`findAll`、および `findById`) は、すべてのドキュメントを暗号化解除して一致があるかどうかを確認する必要があるため、暗号化によって影響を受けます。 照会ごとの find について、制限が渡された場合は、その結果の制限に達したときに停止するため高速になる可能性があります。 JSONStore は、残りのドキュメントを暗号化解除して他の検索結果が残っているかどうかを確認する必要はありません。

## 並行性
{: #concurrency-jsonstore }
### JavaScript での並行性
{: #javascript-jsonstore }
add や find など、コレクションに対して実行できる操作のほとんどは非同期です。 これらの操作は、正常に完了した場合は解決され、障害が発生した場合は拒否される、jQuery promise を返します。 これらの promise は、成功時と失敗時のコールバックと似ています。

jQuery Deferred は、解決されるか拒否される promise です。 以下の例は、JSONStore に固有なものではなく、それらの使用法全般を理解するのに役立つことを意図したものです。

promise やコールバックの代わりに、JSONStore `success` イベントと `failure` イベントを listen することもできます。 イベント・リスナーに渡された引数に基づくアクションを実行します。

**promise の定義例**

```javascript
var asyncOperation = function () {
  // Assumes that you have jQuery defined via $ in the environment
  var deferred = $.Deferred();

  setTimeout(function() {
    deferred.resolve('Hello');
  }, 1000);

  return deferred.promise();
};
```

**promise の使用例**

```javascript
// The function that is passed to .then is executed after 1000 ms.
asyncOperation.then(function (response) {
  // response = 'Hello'
});
```

**コールバックの定義例**

```javascript
var asyncOperation = function (callback) {
  setTimeout(function() {
    callback('Hello');
  }, 1000);
};
```

**コールバックの使用例**

```javascript
// The function that is passed to asyncOperation is executed after 1000 ms.
asyncOperation(function (response) {
  // response = 'Hello'
});
```

**イベントの例**

```javascript
$(document.body).on('WL/JSONSTORE/SUCCESS', function (evt, data, src, collectionName) {

  // evt - Contains information about the event
  // data - Data that is sent ater the operation (add, find, etc.) finished
  // src - Name of the operation (add, find, push, etc.)
  // collectionName - Name of the collection
});
```

### Objective-C での並行性
{: #objective-c-jsonstore }
JSONStore 用のネイティブ iOS API を使用すると、すべての操作は同期ディスパッチ・キューに追加されます。 この動作により、メイン・スレッドではないスレッド上で、ストアに作用する操作が順番に実行されることが確実になります。 詳しくは、Apple 資料の [Grand Central Dispatch (GCD) ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.apple.com/library/ios/documentation/Performance/Reference/GCD_libdispatch_Ref/Reference/reference.html#//apple_ref/c/func/dispatch_sync){: new_window} を参照してください。

### Java での並行性
{: #java-jsonstore }
JSONStore 用のネイティブ Android API を使用すると、すべての操作がメイン・スレッドで実行されます。 振る舞いを非同期にするには、スレッドを作成するか、スレッド・プールを使用する必要があります。 すべてのストア操作はスレッド・セーフです。

## 分析
{: #analytics-jsonstore }
JSONStore に関する重要な分析情報を収集することができます。

### ファイル情報
{: #file-information }
分析フラグを **true** に設定して JSONStore API が呼び出された場合、ファイル情報はアプリケーション・セッションごとに 1 回収集されます。 アプリケーション・セッションは、アプリケーションがメモリーにロードされたときに始まり、アプリケーションがメモリーから除去されたときに終わるものとして定義されます。 この情報を使用して、アプリケーション内で JSONStore コンテンツが使用しているスペース量を知ることができます。

### パフォーマンス・メトリック
{: #performance-metrics }
パフォーマンス・メトリックは、JSONStore API が操作の開始時刻と終了時刻に関する情報を使用して呼び出されるたびに収集されます。 この情報を使用して、さまざまな操作の所要時間 (ミリ秒) を知ることができます。

### iOS 用の JSONStore の例
{: #ios-example}
```objc
JSONStoreOpenOptions* options = [JSONStoreOpenOptions new];
[options setAnalytics:YES];

[[JSONStore sharedInstance] openCollections:@[...] withOptions:options error:nil];
```

### Android 用の JSONStore の例
{: #android-example }
```java
JSONStoreInitOptions initOptions = new JSONStoreInitOptions();
initOptions.setAnalytics(true);

WLJSONStore.getInstance(...).openCollections(..., initOptions);
```

### JavaScript 用の JSONStore の例
{: #java-script-example }
```javascript
var options = {
  analytics : true
};

WL.JSONStore.init(..., options);
```

## 外部データを使用した作業
{: #working-with-external-data }
**プル**と**プッシュ**という異なる概念で外部データを使用して作業できます。

### プル
{: #pull }
多くのシステムでは、プル という用語を使って、外部ソースからデータを所得することを表します。  
以下の 3 つの重要な要素があります。

#### 外部データ・ソースからのプル
{: #external-data-source-pull }
このソースとしては、データベースや REST API あるいは SOAP API などが考えられますが、他にも多くのものがあります。 要件は、MobileFirst Server からアクセス可能であるか、クライアント・アプリケーションから直接アクセス可能でなければならないということだけです。 このソースではデータが JSON 形式で返されることが理想です。

#### プルでのトランスポート層
{: #transport-layer-pull }
このソースは、データが外部ソースから内部ソース (ストア内部の JSONStore コレクション) にどのように取得されるかを定義します。 代替方法の 1 つがアダプターです。

#### プルでの内部データ・ソース API
{: #internal-data-source-api-pull }
このソースは、コレクションに JSON データを追加するために使用できる JSONStore API です。

内部ストアには、ファイル、入力フィールド、または変数内のハードコーディングされたデータから読み取られるデータを取り込むことができます。 このデータのソースは、ネットワーク通信を必要とする外部ソースに限られる必要はありません。
{: note}

以下のコード例はすべて、JavaScript に類似した疑似コードで書かれています。

トランスポート層の場合、アダプターを使用します。 アダプターを使用することの利点として、XML から JSONへの変換、セキュリティー、フィルタリング、サーバー・サイド・コードとクライアント・サイド・コードの分離などが挙げられます。
{: note}
**外部データ・ソース: バックエンド REST エンドポイント**  
データベースからデータを読み取り、それを JSON オブジェクトの配列として返す REST エンドポイントがあると想定してください。

```javascript
app.get('/people', function (req, res) {

  var people = database.getAll('people');

  res.json(people);
});
```
{: codeblock}

返されるデータは、以下の例のようになります。

```xml
[{id: 0, name: 'carlos', ssn: '111-22-3333'},
 {id: 1, name: 'mike', ssn: '111-44-3333'},
 {id: 2, name: 'dgonz' ssn: '111-55-3333')]
```
{: codeblock}

**トランスポート層: アダプター**  
people と呼ばれるアダプターを作成し、getPeople と呼ばれるプロシージャーを定義していると想定してください。 このプロシージャーは、REST エンドポイントを呼び出し、JSON オブジェクトの配列をクライアントに返します。 ここでさらに操作を行うことができます。例えば、データのサブセットのみをクライアントに返すことができます。

```javascript
function getPeople () {

  var input = {
    method : 'get',
    path : '/people'
  };

  return MFP.Server.invokeHttp(input);
}
```
{: codeblock}

クライアントでは、WLResourceRequest API を使用してデータを取得することができます。 さらに、クライアントから  アダプターにパラメーターをいくつか渡すことができます。 例えば、アダプターを通じてクライアントが外部ソースから新しいデータを取得した最終時刻を持つ日付を渡すことができます。

```javascript
var adapter = 'people';
var procedure = 'getPeople';

var resource = new WLResourceRequest('/adapters' + '/' + adapter + '/' + procedure, WLResourceRequest.GET);
resource.send()
.then(function (responseFromAdapter) {
  // ...
});
```
{: codeblock}

`WLResourceRequest` API に渡すことができる `compressResponse`、`timeout`、または他のパラメーターを活用することもできます。  
{: note}

オプションで、アダプターをスキップし、jQuery.ajax などを使用して、保存したいデータを持つ REST エンドポイントに直接アクセスすることができます。

```javascript
$.ajax({
  type: 'GET',
  url: 'http://example.org/people',
})
.then(function (responseFromEndpoint) {
  // ...
});
```
{: codeblock}

**内部データ・ソース API: JSONStore**
バックエンドから応答を取得したら、JSONStore を使用してそのデータを処理できます。
JSONStore は、ローカルでの変更をトラッキングする手段を提供しています。 これにより、一部の API でドキュメントをダーティーとしてマーク付けできるようになります。 API は、ドキュメントに対して最後に実行された操作、およびドキュメントがダーティーとしてマーク付けされた時点を記録します。 そうすると、この情報を使用して、データ同期などのフィーチャーを実装できます。

change API がデータといくつかのオプションを取ります。

**replaceCriteria**  
これらの検索フィールドは、入力データの一部です。 既にコレクション内部にあるドキュメントを見つけるのに使用されます。 例えば、置き換え基準として以下を選択したとします。

```javascript
['id', 'ssn']
```
{: codeblock}

この場合、以下の配列が入力データとして渡されます。

```javascript
[{id: 1, ssn: '111-22-3333', name: 'Carlos'}]
```
{: codeblock}

さらに `people` コレクションには既に以下のドキュメントで構成されます。

```javascript
{_id: 1,json: {id: 1, ssn: '111-22-3333', name: 'Carlitos'}}
```
{: codeblock}

`change` 操作では、以下の照会と正確に一致するドキュメントが検索されます。

```javascript
{id: 1, ssn: '111-22-3333'}
```
{: codeblock}

次に、`change` 操作が入力データによる置き換えを実行し、コレクションには以下のものが含まれます。

```javascript
{_id: 1, json: {id:1, ssn: '111-22-3333', name: 'Carlos'}}
```
{: codeblock}

名前が `Carlitos` から `Carlos` に変更されました。 複数のドキュメントが置き換え基準に一致した場合、一致するすべてのドキュメントが対応する入力データに置き換えられます。

**addNew**  
置き換え基準に一致するドキュメントがない場合、change API は、このフラグの値を調べます。 このフラグが **true** に設定されている場合、change API は新しいドキュメントを作成して、ストアに追加します。 そうでなれば、さらなるアクションは実行されません。

**markDirty**  
change API が置き換えまたは追加されるドキュメントをダーティーとしてマーク付けするかどうかを決定します。

アダプターからデータの配列が返されます。

```javascript
.then(function (responseFromAdapter) {

  var accessor = WL.JSONStore.get('people');

  var data = responseFromAdapter.responseJSON;

  var changeOptions = {
    replaceCriteria : ['id', 'ssn'],
    addNew : true,
    markDirty : false
  };

  return accessor.change(data, changeOptions);
})

.then(function() {
  // ...
})
```
{: codeblock}

他の API を使用して、保管されたローカル・ドキュメントに対する変更をトラッキングすることができます。 操作が実行されるコレクションに対するアクセサーを常に取得します。

```javascript
var accessor = WL.JSONStore.get('people')
```
{: codeblock}

そうすると、データ (JSON オブジェクトの配列) を追加し、そのデータにダーティーまたは非ダーティーのマークが付けられるようにしたいかどうかを決定することができます。 一般的に、外部ソースから変更を取得する場合は、markDirty フラグを false に設定します。 データをローカルに追加する場合は、このフラグを true に設定します。

```javascript
accessor.add(data, {markDirty: true})
```
{: codeblock}

また、ドキュメントを置き換え、代替物のあるそのドキュメントをダーティーまたは非ダーティーとしてマーク付けすることを選択することができます。

```javascript
accessor.replace(doc, {markDirty: true})
```
{: codeblock}

同様に、ドキュメントを除去し、その除去をダーティーまたは非ダーティーとしてマーク付けすることを選択することができます。 除去されてダーティーのマークが付けられたドキュメントは、find API を使用したときに表示されません。 ただし、コレクションからドキュメントを物理的に除去する `markClean` API を使用するまでは、これらのドキュメントはコレクション内部に残っています。 ドキュメントがダーティーとしてマーク付けされていない場合、そのドキュメントはコレクションから物理的に除去されます。

```javascript
accessor.remove(doc, {markDirty: true})
```
{: codeblock}

### プッシュ
{: #push }
多くのシステムでは、プッシュ という用語を使って、外部ソースにデータを送ることを表します。

以下の 3 つの重要な要素があります。

#### プッシュでの内部データ・ソース API
{: #internal-data-source-api-push }
このソースは、ローカルのみの変更 (ダーティー) を持つドキュメントを返す JSONStore API です。

#### プッシュでのトランスポート層
{: #transport-layer-push }
このソースは、変更を送信するためにどのように外部データ・ソースにアクセスするかを定義します。

#### 外部データ・ソースへのプッシュ
{: #external-data-source-push }
通常、このソースは、データベースや REST または SOAP エンドポイントで (他にもありますが)、クライアントがデータに対して行った更新を受け取るものです。

以下のコード例はすべて、JavaScript に類似した疑似コードで書かれています。

トランスポート層の場合、アダプターを使用します。 アダプターを使用することの利点として、XML から JSONへの変換、セキュリティー、フィルタリング、サーバー・サイド・コードとクライアント・サイド・コードの分離などが挙げられます。
{: note}

**内部データ・ソース API: JSONStore**  
コレクションへのアクセサーを取得したら、`getAllDirty` API を呼び出して、ダーティーとマークされたすべてのドキュメントを取得します。 これらのドキュメントは、トランスポート層を通じて外部データ・ソースに送りたいローカルのみの変更を含むものです。

```javascript
var accessor = WL.JSONStore.get('people');

accessor.getAllDirty()

.then(function (dirtyDocs) {
  // ...
});
```
{: codeblock}

`dirtyDocs` 引数は、以下の例のようになります。

```javascript
[{_id: 1,
  json: {id: 1, ssn: '111-22-3333', name: 'Carlos'},
  _operation: 'add',
  _dirty: '1395774961,12902'}]
```
{: codeblock}

フィールドは以下のとおりです。
* `_id`: JSONStore が使用する内部フィールド。 どのドキュメントにも固有のものが 1 つ割り当てられています。
* `json`: 保管されたデータ。
* `_operation`: ドキュメントに対して最後に実行された操作。 可能な値は add、store、replace、および remove です。
* `_dirty`: ドキュメントがダーティーとマークされた時点を示す数値として保管されているタイム・スタンプ。

**トランスポート層: MobileFirst アダプター**  
ダーティー・ドキュメントを  アダプターに送ることを選択できます。 `updatePeople` プロシージャーによって定義された `people` アダプターがあるものと想定してください。

```javascript
.then(function (dirtyDocs) {
  var adapter = 'people',
  procedure = 'updatePeople';

  var resource = new WLResourceRequest('/adapters/' + adapter + '/' + procedure, WLResourceRequest.GET)
  resource.setQueryParameter('params', [dirtyDocs]);
  return resource.send();
})

.then(function (responseFromAdapter) {
  // ...
})
```
{: codeblock}

`WLResourceRequest` API に渡すことができる `compressResponse`、`timeout`、または他のパラメーターを活用することもできます。
{: note}

MobileFirst Server では、アダプターに以下の例のような `updatePeople` プロシージャーがあります。

```javascript
function updatePeople (dirtyDocs) {

  var input = {
    method : 'post',
    path : '/people',
    body: {
      contentType : 'application/json',
      content : JSON.stringify(dirtyDocs)
    }
  };

  return MFP.Server.invokeHttp(input);
}
```
{: codeblock}

クライアント上で `getAllDirty` API からの出力を中継するのではなく、バックエンドが要求する形式と一致するようにペイロードを更新しなければならないことがあります。 置き換え、除去、および組み込みを別々のバックエンド API 呼び出しに分ける必要があるかもしれません。

オプションで、`dirtyDocs` 配列全体を繰り返し、`_operation` フィールドを確認することができます。 次に、1 つのプロシージャーに置き換え、別のプロシージャーに除去、さらに別のプロシージャーに組み込みを送ることができます。 上記の例は、すべてのダーティー・ドキュメントを一括して  アダプターに送るものです。

```javascript
var len = dirtyDocs.length;
var arrayOfPromises = [];
var adapter = 'people';
var procedure = 'addPerson';
var resource;

while (len--) {

  var currentDirtyDoc = dirtyDocs[len];

  switch (currentDirtyDoc._operation) {

    case 'add':
    case 'store':

    resource = new WLResourceRequest('/adapters/people/addPerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);

      arrayOfPromises.push(resource.send());

    break;

    case 'replace':
    case 'refresh':

    resource = new WLResourceRequest('/adapters/people/replacePerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);


      arrayOfPromises.push(resource.send());

    break;

    case 'remove':
    case 'erase':

    resource = new WLResourceRequest('/adapters/people/removePerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);

      arrayOfPromises.push(resource.send());
  }
}

$.when.apply(this, arrayOfPromises)
.then(function () {
  var len = arguments.length;

  while (len--) {
    // Look at the responses in arguments[len]
  }
});
```
{: codeblock}

オプションで、アダプターをスキップし、REST エンドポイントに直接アクセスすることができます。

```javascript
.then(function (dirtyDocs) {

  return $.ajax({
    type: 'POST',
    url: 'http://example.org/updatePeople',
    data: dirtyDocs
  });
})

.then(function (responseFromEndpoint) {
  // ...
});
```
{: codeblock}

**外部データ・ソース: バックエンド REST エンドポイント**  
バックエンドは、変更を受け入れるか、拒否して、応答を中継してクライアントに戻します。 クライアントは、応答を調べてから、更新されたドキュメントを markClean API に渡すことができます。

```javascript
.then(function (responseFromAdapter) {

  if (responseFromAdapter is successful) {
    WL.JSONStore.get('people').markClean(dirtyDocs);
  }
})

.then(function () {
  // ...
})
```
{: codeblock}

クリーンとマークされたドキュメントは、`getAllDirty` API からの出力には表示されません。

## JSONStore のトラブルシューティング
{: #troubleshooting-jsonstore }

## ヘルプ要求をする際の情報提供
{: #provide-information-when-you-ask-for-help }
提供する情報が不足するリスクを負うよりも、多くの情報を提供することをお勧めします。 以下のリストは、JSONStore の問題に関してサポートを受けるために必要な情報の出発点と考えてください。

* オペレーティング・システムおよびバージョン。 例えば、Windows XP SP3 Virtual Machine や Mac OSX 10.8.3。
* Eclipse のバージョン。 例えば、Eclipse Indigo 3.7 Java EE。
* JDK のバージョン。 例えば、Java SE Runtime Environment (ビルド 1.7)。
* {{ site.data.keys.product }} のバージョン。 例えば、IBM Worklight V5.0.6 Developer Edition。
* iOS のバージョン。 例えば、iOS Simulator 6.1 や iPhone 4S iOS 6.0 (非推奨。『非推奨となった機能および API エレメント』を参照)。
* Android のバージョン。 例えば、Android Emulator 4.1.1 や Samsung Galaxy Android 4.0 API Level 14。
* Windows のバージョン。 例えば、Windows 8、Windows 8.1、Windows Phone 8.1。
* adb のバージョン。 例えば、Android Debug Bridge バージョン 1.0.31。
* ログ (iOS の Xcode 出力や Android の logcat 出力など)。

## 問題の切り分けの試行
{: #try-to-isolate-the-issue }
より正確に問題を報告するために問題を切り分けるには、以下のステップを実行します。

1. エミュレーター (Android) またはシミュレーター (iOS) をリセットし、destroy API を呼び出してクリーン・システムで開始します。
2. サポートされている実稼働環境で稼働するようにします。
    * Android >= 2.3 ARM v7/ARM v8/x86 エミュレーターまたはデバイス
    * iOS >= 6.0 シミュレーターまたはデバイス (非推奨)
    * Windows 8.1/10 ARM/x86/x64 シミュレーターまたはデバイス
3. init API または open API にパスワードを渡さずに、暗号化をオフにします。
4. JSONStore により生成された SQLite データベース・ファイルを調べます。 暗号化はオフにする必要があります。

   * Android エミュレーターの場合:

   ```bash
   $ adb shell
   $ cd /data/data/com.<app-name>/databases/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

   * iOS シミュレーターの場合:

   ```bash
   $ cd ~/Library/Application Support/iPhone Simulator/7.1/Applications/<id>/Documents/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```  

   * Windows 8.1 Universal / Windows 10 UWP シミュレーターの場合:

   ```bash
   $ cd C:\Users\<username>\AppData\Local\Packages\<id>\LocalState\wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

   * **注:** Web ブラウザー (Firefox、Chrome、Safari、Internet Explorer) で稼働する JavaScript 専用実装環境では、SQLite データベースは使用されません。 ファイルは HTML5 LocalStorage に保管されます。
   * `.schema` がある `searchFields` を調べて、`SELECT * FROM <collection-name>;` を使用してデータを選択します。 sqlite3 を終了するには、`.exit` と入力します。 ユーザー名を init メソッドに渡す場合、ファイルは **the-username.sqlite** という名前になります。 ユーザー名を渡さない場合、ファイルはデフォルトで **jsonstore.sqlite** という名前になります。
5. (Android のみ) JSONStore の詳細出力を有効にします。

   ```bash
   adb shell setprop log.tag.jsonstore-core VERBOSE
   adb shell getprop log.tag.jsonstore-core
   ```

6. デバッガーを使用します。

## 一般的な問題
{: #common-issues-jsonstore }
以下に記載する JSONStore の特性を理解しておくと、発生する可能性のある一般的な問題の解決に役立つ場合があります。  

* バイナリー・データを JSONStore に保管するための唯一の方法は、まずそのデータを base64 でエンコードすることです。 JSONStore 内に実際のファイルの代わりに、ファイル名またはパスを保管します。
* {{ site.data.keys.v62_product_full }} V6.2.0 でのみネイティブ・コードから JSONStore データにアクセスできます。
* モバイル・オペレーティング・システムによって定められた制限以外に、JSONStore 内に保管可能なデータ量に制限はありません。
* JSONStore は永続データ・ストレージを提供します。 メモリー内に保管されるだけではありません。
* コレクション名が数字または記号で始まる場合、init API は失敗します。 IBM Worklight V5.0.6.1 以降は、該当するエラー `4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING` を返します。
* 検索フィールドにおいて、number と integer には違いがあります。 タイプが `number` の場合、`1` や `2` などの数値は、`1.0` や `2.0` として保管されます。 タイプが `integer` の場合は `1` および `2` として保管されます。
* アプリケーションが強制的に停止されたり異常終了したりした場合、そのアプリケーションを再始動して `init` API または `open` API を呼び出すと、そのアプリケーションは常にエラー・コード -1 で失敗します。 この問題が発生した場合、まず `closeAll` API を呼び出します。
* JSONStore の JavaScript 実装環境では、コードを順次に呼び出す必要があります。 1 つの操作が完了するのを待ってから、次の操作を呼び出します。
* Cordova アプリケーション用の Android 2.3.x ではトランザクションはサポートされていません。
* 64 ビット・デバイスで JSONStore を使用する場合、エラー`「java.lang.UnsatisfiedLinkError: dlopen が失敗しました。「...」は 64 ビットではなく 32 ビットです (java.lang.UnsatisfiedLinkError: dlopen failed: "..." is 32-bit instead of 64-bit)」`が表示されることがあります。
* このエラーは、Android プロジェクト内に 64 ビットのネイティブ・ライブラリーがあること、およびこれらのライブラリーが使用された場合 JSONStore は現在機能しないことを意味しています。 確認するために、Android プロジェクトの下の **src/main/libs** または **src/main/jniLibs** に移動して、x86_64 フォルダーまたは arm64-v8a フォルダーがあるかどうかを調べます。 ある場合、これらのフォルダーを削除すると、JSONStore は再び機能するようになります。
* 場合 (または環境) によっては、JSONStore プラグインが初期化される前にフローが `wlCommonInit()` に入ります。 これが原因で JSONStore 関連の API 呼び出しは失敗します。 `cordova-plugin-mfp` ブートストラップは、完了時に `wlCommonInit` 関数をトリガーする `WL.Client.init` を自動的に呼び出します。 この初期化プロセスは JSONStore プラグインでは異なります。 JSONStore プラグインには、`WL.Client.init` 呼び出しを_停止_ する方法はありません。 さまざまな環境で、`mfpjsonjslloaded` が完了する前にフローが `wlCommonInit()` に入ることがあります。
`mfpjsonjsloaded` イベントと `mfpjsloaded` イベントの順序を確認するために、開発者はオプションで `WL.CLient.init` を手動で呼び出すことができます。 これによって、プラットフォーム固有のコードを使用する必要はなくなります。

  `WL.CLient.init` の呼び出しを手動で構成するには、以下のステップを実行します。                             

  1. `config.xml` で、`clientCustomInit` プロパティーを **true** に変更します。

  + `index.js` ファイルで、以下のようにします。                                   
    * 次の行をファイルの先頭に追加します。                
      ```javascript
      document.addEventListener('mfpjsonjsloaded', initWL, false);
      ```           
    * `wlCommonInit()` の `WL.JSONStore.init` 呼び出しはそのままにします。                    

    * 次の関数を追加します。  
    ```javascript                                         
    function initWL(){                                                     
        var options = typeof wlInitOptions !== 'undefined' ? wlInitOptions
        : {};                                                                
        WL.Client.init(options);                                           
    }
    ```                                                                     

これは、`mfpjsonjsloaded` イベントを (`wlCommonInit` の外部で) 待機します。これにより、スクリプトがロードされたことを確認してから、`wlCommonInit` をトリガーする `WL.Client.init` を呼び出します。それにより、`WL.JSONStore.init` が呼び出されます。

## ストア内部
{: #store-internals }
JSONStore データの保管方法の例を確認します。

この簡素化された例には、以下の主要な要素があります。

* `_id` は固有 ID です (例えば、AUTO INCREMENT PRIMARY KEY)。
* `json` には、保管されている JSON オブジェクトの正確な表現が含まれています。
* `name` および age は検索フィールドです。
* `key` は追加の検索フィールドです。

| _id | key | name | age | JSON |
|-----|-----|------|-----|------|
| 1   | c   | carlos | 99 | {name: 'carlos', age: 99} |
| 2   | t   | tim   | 100 | {name: 'tim', age: 100} |

`{_id : 1}、{name: 'carlos'}`、`{age: 99}、{key: 'c'}` のいずれかの照会またはこれらの照会の組み合わせを使用して検索を行った場合に返される文書は `{_id: 1, json: {name: 'carlos', age: 99} }` です。

その他の内部 JSONStore フィールドは以下のとおりです。

* `_dirty`: 文書がダーティーとマーク付けされているかどうかを判別します。 このフィールドは、文書に対する変更をトラッキングする上で役立ちます。
* `_deleted`: 文書に削除済みかどうかのマークを付けます。 このフィールドは、コレクションからオブジェクトを削除したり、後でそれらのオブジェクトをバックエンドでの変更のトラッキングに使用したり、これらのオブジェクトを削除するかどうかを決定したりする上で役立ちます。
* `_operation`: 文書で最後に実行される操作 (例えば、置換) を反映するストリング。

## JSONStore エラー
{: #jsonstore-errors }
### JavaScript エラー
{: #javascript-errors }
JSONStore は、エラー・オブジェクトを使用して、障害の原因に関するメッセージを返します。

JSONStore 操作 (例えば、`JSONStoreInstance` クラスの `find` メソッドや `add` メソッド) 中にエラーが発生した場合、エラー・オブジェクトが返されます。 これは障害の原因に関する情報を提供します。

```javascript
var errorObject = {
  src: 'find', // Operation that failed.
  err: -50, // Error code.
  msg: 'PERSISTENT\_STORE\_FAILURE', // Error message.
  col: 'people', // Collection name.
  usr: 'jsonstore', // User name.
  doc: {_id: 1, {name: 'carlos', age: 99}}, // Document that is related to the failure.
  res: {...} // Response from the server.
}
```
{: codeblock}
すべてのキー/値のペアがすべてのエラー・オブジェクトの一部であるとは限りません。 例えば、doc 値は、ある文書 (例えば、`JSONStoreInstance` クラスの `remove` メソッド) がある文書を削除できなかったために操作が失敗した場合にのみ使用可能です。

### Objective-C エラー
{: #objective-c-errors }
失敗する可能性があるすべての API が、NSError オブジェクトへのアドレスを使用するエラー・パラメーターを取ります。 エラーが通知されないようにするには、`nil` で渡すことができます。 操作が失敗した場合、エラーと `userInfo` (存在する場合) を含む NSError がアドレスに取り込まれます。 `userInfo` には追加の詳細 (例えば、障害の原因となった文書) が含まれていることがあります。

```objc
// This NSError points to an error if one occurs.
NSError* error = nil;

// Perform the destroy.
[JSONStore destroyDataAndReturnError:&error];
```

### Java エラー
{: #java-errors }
すべての Java API 呼び出しが、発生したエラーに応じて特定の例外をスローします。 それぞれの例外を個別に処理することも、すべての JSONStore 例外のアンブレラとして `JSONStoreException` を catch することもできます。

```java
try {
  WL.JSONStore.closeAll();
}

catch(JSONStoreException e) {
  // Handle error condition.
}
```
{: codeblock}
### エラー・コードのリスト
{: #list-of-error-codes }
一般的なエラー・コードとその説明のリスト:

|エラー・コード      | 説明 |
|----------------|-------------|
| -100 UNKNOWN_FAILURE | 認識できないエラー。|
| -75 OS\_SECURITY\_FAILURE | このエラー・コードは requireOperatingSystemSecurity フラグに関連しています。このエラーは、destroy API が、オペレーティング・システムのセキュリティー (パスコード・フォールバックによるタッチ ID) によって保護されているセキュリティー・メタデータを削除できない場合や、init API または open API がセキュリティー・メタデータを見つけることができない場合に発生する可能性があります。 また、デバイスがオペレーティング・システムのセキュリティーをサポートしない場合に、オペレーティング・システムのセキュリティーの使用が要求された場合にも発生する可能性があります。 |
| -50 PERSISTENT\_STORE\_NOT\_OPEN | JSONStore はクローズされています。 最初に JSONStore クラスで open メソッドの呼び出しを試行して、ストアへのアクセスを有効にしてください。|
| -48 TRANSACTION\_FAILURE\_DURING\_ROLLBACK | トランザクションのロールバック中に問題が発生しました。|
| -47 TRANSACTION\\_FAILURE\_DURING\_REMOVE\_COLLECTION |トランザクションの進行中に removeCollection を呼び出すことはできません。|
| -46 TRANSACTION\_FAILURE\_DURING\_DESTROY | 進行中のトランザクションがある場合は destroy を呼び出すことはできません。|
| -45 TRANSACTION\_FAILURE\_DURING\_CLOSE\_ALL | 実行中トランザクションがある場合は closeAll を呼び出すことはできません。|
| -44 TRANSACTION\_FAILURE\_DURING\_INIT | 進行中のトランザクションがある場合はストアを初期化できません。|
| -43 TRANSACTION_FAILURE | トランザクションの問題が発生しました。|
| -42 NO\_TRANSACTION\_IN\_PROGRESS | 進行中のトランザクションがない場合は、トランザクションのロールバックをコミットできません。|
| -41 TRANSACTION\_IN\_POGRESS | 別のトランザクションの進行中には新規トランザクションを開始できません。|
| -40 FIPS\_ENABLEMENT\_FAILURE |FIPS に何らかの問題があります。|
| -24 JSON\_STORE\_FILE\_INFO\_ERROR | ファイル・システムからファイル情報の取得中に問題がありました。|
| -23 JSON\_STORE\_REPLACE\_DOCUMENTS\_FAILURE | コレクションでドキュメントの置き換え中に問題がありました。|
| -22 JSON\_STORE\_REMOVE\_WITH\_QUERIES\_FAILURE | コレクションでドキュメントの除去中に問題がありました。|
| -21 JSON\_STORE\_STORE\_DATA\_PROTECTION\_KEY\_FAILURE | データ保護鍵 (DPK) の保管中に問題がありました。|
| -20 JSON\_STORE\_INVALID\_JSON\_STRUCTURE | 入力データの索引付け中に問題がありました。|
| -12 INVALID\_SEARCH\_FIELD\_TYPES | searchFields に渡しているタイプが string、integer、number、または boolean のいずれかであることを確認してください。|
| -11 OPERATION\_FAILED\_ON\_SPECIFIC\_DOCUMENT | 文書の配列に対する操作 (例えば、replace メソッド) は、特定の文書での動作時には失敗することがあります。 失敗した文書が返され、トランザクションはロールバックされます。 Android では、サポートされないアーキテクチャーで JSONStore の使用を試みた場合にも、このエラーが発生します。|
| -10 ACCEPT\_CONDITION\_FAILED | ユーザーが指定した accept 関数が false を返しました。|
| -9 OFFSET\_WITHOUT\_LIMIT | オフセットを使用するには、制限も指定する必要があります。|
| -8 INVALID\_LIMIT\_OR\_OFFSET | 妥当性検査エラー。正整数でなければなりません。|
| -7 INVALID_USERNAME | 妥当性検査エラー ([A から Z]、[a から z]、または [0 から 9] のみでなければなりません)。|
| -6 USERNAME\_MISMATCH\_DETECTED | ログアウトするには、JSONStore ユーザーは最初に closeAll メソッドを呼び出す必要があります。同時に存在できるユーザーは 1 人だけです。|
| -5 DESTROY\_REMOVE\_PERSISTENT\_STORE\_FAILED |ストアのコンテンツを保持するファイルを削除しようとした destroy メソッドで問題が発生しました。|
| -4 DESTROY\_REMOVE\_KEYS\_FAILED | キーチェーン (iOS) または共有ユーザー設定 (Android) をクリアしようとした destroy メソッドで問題が発生しました。|
| -3 INVALID\_KEY\_ON\_PROVISION | 暗号化されたストアに誤ったパスワードが渡されました。|
| -2 PROVISION\_TABLE\_SEARCH\_FIELDS\_MISMATCH | 検索フィールドは動的ではありません。新しい検索フィールドを使用して init メソッドまたは open メソッドを呼び出す前に、destroy メソッドまたは removeCollection メソッドを呼び出さずに検索フィールドを変更することはできません。 このエラーは、検索フィールドの名前またはタイプを変更した場合に発生する可能性があります。 例: {key: 'string'} から {key: 'number'} または {myKey: 'string'} から {theKey: 'string'}。 |
| -1 PERSISTENT\_STORE\_FAILURE | 一般エラー。 ネイティブ・コード内で誤動作が発生しました。init メソッドが呼び出された可能性があります。|
| 0 SUCCESS | 場合によっては、成功を示す 0 が JSONStore ネイティブ・コードから返されることがあります。 |
| 1 BAD\_PARAMETER\_EXPECTED\_INT | 検証エラー。 |
| 2 BAD\_PARAMETER\_EXPECTED\_STRING | 検証エラー。 |
| 3 BAD\_PARAMETER\_EXPECTED\_FUNCTION | 検証エラー。 |
| 4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING | 検証エラー。 |
| 5 BAD\_PARAMETER\_EXPECTED\_OBJECT | 検証エラー。 |
| 6 BAD\_PARAMETER\_EXPECTED\_SIMPLE\_OBJECT | 検証エラー。 |
| 7 BAD\_PARAMETER\_EXPECTED\_DOCUMENT | 検証エラー。 |
| 8 FAILED\_TO\_GET\_UNPUSHED\_DOCUMENTS\_FROM\_DB |ダーティーとマークされたすべてのドキュメントを選択する照会が失敗しました。 SQL 照会の例: SELECT * FROM [collection] WHERE _dirty > 0 |
| 9 NO\_ADAPTER\_LINKED\_TO\_COLLECTION | JSONStoreCollection クラスの push や load メソッドなどの関数を使用するには、init メソッドにアダプターを渡す必要があります。 |
| 10 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ARRAY\_OF\_DOCUMENTS | 検証エラー |
| 11 INVALID\_PASSWORD\_EXPECTED\_ALPHANUMERIC\_STRING\_WITH\_LENGTH\_GREATER\_THAN\_ZERO | 検証エラー |
| 12 ADAPTER_FAILURE | WL.Client.invokeProcedure の呼び出しに関する問題、特にアダプターへの接続に関する問題。 このエラーは、バックエンドの呼び出しを試行するアダプターの障害とは異なります。 |
| 13 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ID | 検証エラー |
| 14 CAN\_NOT\_REPLACE\_DEFAULT\_FUNCTIONS | JSONStoreCollection クラスの enhance メソッドを呼び出して既存の関数 (find および add) を置き換えることは許可されません。 |
| 15 COULD\_NOT\_MARK\_DOCUMENT\_PUSHED | プッシュでドキュメントがアダプターに送信されましたが、JSONStore がドキュメントを「非ダーティー」とマークできませんでした。 |
| 16 COULD\_NOT\_GET\_SECURE\_KEY | パスワードを使用してコレクションを開始するには、{{ site.data.keys.mf_server }} への接続が必要です。これは「セキュア・ランダム・トークン」を返すためです。 IBM  Worklight  V5.0.6 以降では、開発者は、options オブジェクトを介して init メソッドに {localKeyGen: true} を渡すことで、セキュア・ランダム・トークンをローカルで生成できます。 |
| 17 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER | WL.Client.invokeProcedure が失敗時コールバックを呼び出したため、データをロードできませんでした。 |
| 18 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER\_INVALID\_LOAD\_OBJ | init メソッドに渡された load オブジェクトが検証に失格しました。 |
| 19 INVALID\_KEY\_IN\_LOAD\_OBJECT | add メソッドの呼び出し時に load オブジェクトに使用されたキーに問題があります。 |
| 20 UNDEFINED\_PUSH\_OPERATION | ダーティー・ドキュメントをサーバーにプッシュするように定義されているプロシージャーがありません。 例えば、init メソッド (新規文書はダーティーで、操作は「add」) と push メソッド (操作「add」で新規文書を検索) が呼び出されたが、追加プロシージャーを使用する add キーが、コレクションにリンクされたアダプターに見つかりませんでした。 アダプターのリンクは、init メソッド内で行われます。 |
| 21 INVALID\_ADD\_INDEX\_KEY | 追加の検索フィールドで問題が発生しました。 |
| 22 INVALID\_SEARCH\_FIELD | いずれかの検索フィールドが無効です。 渡した検索フィールドが _id、json、_deleted、_operation でないことを確認してください。 |
| 23 ERROR\_CLOSING\_ALL | 一般エラー。 ネイティブ・コードが closeAll メソッドを呼び出したときにエラーが発生しました。 |
| 24 ERROR\_CHANGING\_PASSWORD | パスワードを変更できません。 例えば、渡された古いパスワードが誤っていました。 |
| 25 ERROR\_DURING\_DESTROY | 一般エラー。 ネイティブ・コードが destroy メソッドを呼び出したときにエラーが発生しました。 |
| 26 ERROR\_CLEARING\_COLLECTION | 一般エラー。 ネイティブ・コードが removeCollection メソッドを呼び出したときにエラーが発生しました。|
| 27 INVALID\_PARAMETER\_FOR\_FIND\_BY\_ID | 検証エラー。 |
| 28 INVALID\_SORT\_OBJECT | いずれかの JSON オブジェクトが無効であるため、ソートのために指定された配列が無効です。正しい構文は、各オブジェクトにプロパティーが 1 つだけ含まれる JSON オブジェクトの配列です。 このプロパティーは、ソート基準と、昇順で検索するのか降順で検索するのかを指定してフィールドを検索します。 例: {searchField1 : "ASC"}。 |
| 29 INVALID\_FILTER\_ARRAY | 結果のフィルター処理に指定された配列が無効です。 この配列の正しい構文は、各ストリングが検索フィールドまたは内部 JSONStore フィールドのいずれかになっているストリングの配列です。 詳しくは、『ストア内部』を参照してください。 |
| 30 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_OBJECTS | JSON オブジェクトのみで構成された配列でない場合に、検証エラーが発生します。 |
| 31 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_CLEAN\_DOCUMENTS | 検証エラー。 |
| 32 BAD\_PARAMETER\_WRONG\_SEARCH\_CRITERIA | 検証エラー。 |
