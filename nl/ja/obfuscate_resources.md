---

copyright:
  years: 2018, 2019
lastupdated: "2018-10-16"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}

# リソースの難読化
{: #obfuscate_resources}

難読化とは、実行可能ファイルを、機能は完全に保持したまま、ハッカーの役に立たないファイルに変更するプロセスのことです。自動によるコードの難読化により、プログラムのリバース・エンジニアリングが困難になります。 

難読化を行うと、アプリケーションのリバース・エンジニアリングが非常に困難になるので、企業秘密 (知的財産) の盗難、無許可アクセス、ライセンス制御などの制御の回避を防止し、脆弱性を保護することができます。

コードの難読化は、次に示す多数の手法とアプリケーション・セキュリティー手法から構成されています。

* リネーム難読化 - リネームすることで、メソッドと変数の名前を変更し、逆コンパイルされたソースを判読しにくくします。ただし、アプリケーションの実行は変更されません。.NET、iOS、Java、および Android の難読化機能によって難読化する方法が、最も望ましい方法です。 
* 文字列の暗号化
* 制御フローの難読化
* 指示パターンの変換
* ダミー・コードの挿入
* 改ざん防止、その他。

難読化を行うと、攻撃者がコードを調査してアプリケーションを分析することが非常に難しくなります。難読化のために、さまざまなサード・パーティー製ツールが用意されています。

* Android アプリケーションの難読化について詳しくは、この[ブログ](https://mobilefirstplatform.ibmcloud.com/blog/2016/09/19/mfp-80-obfuscating-android-code-with-proguard/)を参照してください。
    >**注**: MobileFirst Android アプリケーションで Android ProGuard を難読化するためには、構成ファイル (proguard-project.txt) を作成する必要があります。

* Cordova アプリケーションの基礎的な難読化については、[Cordova アプリケーションの暗号化](#encryptingcordovapackage)を参照してください。

## Cordova パッケージの Web リソースの暗号化
{: #encryptingcordovapackage}

``.apk`` パッケージまたは ``.ipa`` パッケージに Web リソースが含まれている間に他のユーザーがその Web リソースを表示および変更するリスクを最小化するために、 MobileFirst CLI mfpdev app webencrypt コマンドまたは mfpwebencrypt フラグを使用して、情報を暗号化できます。 この手順は、破ることが不可能な暗号化を提供するわけではありませんが、基本レベルの難読化を提供します。

**前提条件**:

* Cordova 開発ツールがインストールされている必要があります。 この例では、Apache Cordova CLI を使用します。 他の Cordova 開発ツールを使用する場合は、手順が一部異なります。 手順については、ご使用の Cordova ツールの資料を参照してください。
* MobileFirst CLI がインストールされている必要があります。
* MobileFirst Cordova プラグインがインストールされている必要があります。

この手順を実行する最適のタイミングは、アプリケーションの開発が終わってアプリケーションをデプロイする準備ができたときです。 Web リソース暗号化手順を実行した後に以下のコマンドのいずれかを実行した場合、暗号化されたコンテンツが暗号化解除されます。

* cordova prepare
* cordova build
* cordova run
* cordova emulate
* mfpdev app webupdate
* mfpdev app preview

Web リソースを暗号化した後に上にリストされているコマンドのいずれかを実行した場合、以下の手順を再度実行して、Web リソースを暗号化する必要があります。

1. ターミナル・ウィンドウを開き、暗号化する Cordova アプリケーションのルート・ディレクトリーにナビゲートします。
2. 以下のいずれかのコマンドを入力して、アプリケーションを準備します。
    * ``cordova prepare``
    * ``mfpdev app webupdate``
3. 以下のいずれかの手順を実行して、コンテンツを暗号化します。
    * 以下のコマンドを入力します。``mfpdev app webencrypt``。
        >**ヒント**: ``mfpdev help app webencrypt`` と入力することで、``mfpdev app webencrypt`` コマンドに関する情報を表示できます。
    * Cordova パッケージのビルド時に ``mfpwebencrypt`` フラグを ``cordova compile`` コマンドまたは ``cordova build`` コマンドに追加することで、Cordova パッケージの Web リソースを暗号化することもできます。
       * ``cordova compile -- --mfpwebencrypt`` | ``cordova build -- --mfpwebencrypt``
            **www** フォルダー内のオペレーティング・システム情報が、暗号化コンテンツを含んだ **resources.zip** ファイルに置き換えられます。
            アプリケーションが Android オペレーティング・システム用であり、**resources.zip** ファイルが 1 MB より大きい場合、**resources.zip** ファイルは小さい 768 KB の .zip ファイルに分割され、各ファイルの名前は **resources.zip.nnn** になります。 変数 nnn は、001 から 999 までの数字です。
4. プラットフォーム固有のツールで提供されているエミュレーターによって、暗号化リソースを持つアプリケーションをテストします。 例えば、Android の場合は Android Studio、iOS の場合は Xcode のエミュレーターを使用できます。

>**注**: 暗号化後にアプリケーションをテストするために、以下の Cordova コマンドは使用しないでください。

>* ``cordova run``
>* ``cordova emulate``

>上記のコマンドを使用すると、www フォルダーで暗号化されたコンテンツが更新され、暗号化が解除されたコンテンツとして再度保存されてしまいます。 上記コマンドを使用した場合には、アプリケーションを公開する前に、必ず、前述の手順を再度実行して暗号化してください。
