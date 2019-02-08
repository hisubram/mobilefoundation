---

copyright:
  years: 2018, 2019
lastupdated:  "2018-11-19"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

#	{{ site.data.keyword.mobilefoundation_short }} CLI
{: #using_mobilefoundation_cli}

{{ site.data.keyword.mobilefoundation_short }} には、{{site.data.keyword.mobilefoundation_short}} クライアント成果物およびサーバー成果物を簡単に管理するための、開発者向けのコマンド・ライン・インターフェース (CLI) ツール **mfpdev** が用意されています。

すべての `mfpdev` コマンドは、対話モードまたは直接モードで実行できます。 対話モードでは、コマンドに必要なパラメーターの入力を求めるプロンプトが出され、一部ではデフォルト値が使用されます。 直接モードでは、実行するコマンドと一緒にパラメーターを指定する必要があります。


## {{site.data.keyword.mobilefoundation_short}} CLI のインストール
{: #installing_mf_cli}

{{site.data.keyword.mobilefoundation_short}} は、NPM パッケージとして [NPM レジストリー ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.npmjs.com){: new_window} で入手できます。

NPM パッケージをインストールするため、**node.js** および **npm** がインストール済みであることを確認してください。 **node.js** をインストールするには、[nodejs.org ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://nodejs.org/){: new_window} のインストール手順に従ってください。 **node.js** が正しくインストールされているかどうかを確認するには、以下のコマンドを実行します。
```
node -v
```
{: codeblock}

> **注:** サポートされている node.js の最小バージョンは **4.2.3** です。 また、急速に進化している node と npm のパッケージでは、{{site.data.keyword.mobilefoundation_short}} CLI は、最新バージョンも含めて、node と npm のすべての利用可能なバージョンで完全には機能しない可能性があります。 CLI が適切に機能するように、node がバージョン **6.11.1** であり、npm のバージョンが **3.10.10** であるようにしてください。

> MobileFirst CLI 暫定修正バージョン *8.0.2018100112* 以上では、Node バージョン 8.x または 10.x を使用できます。

{{site.data.keyword.mobilefoundation_short}} CLI をインストールするには、以下のコマンドを実行します。
```
npm install -g mfpdev-cli 
```
{: codeblock}

CLI の圧縮ファイル (*.zip*) を MobileFirst Operations Console のダウンロード・センターからダウンロードした場合は、次のコマンドを使用します。
```
npm install -g <path-to-mfpdev-cli.tgz> 
```
{: codeblock}

オプションの従属関係を含めずに CLI をインストールするには、`--no-optional` フラグを追加して、次のようにします。
```
npm install -g --no-optional path-to-mfpdev-cli.tgz
```
{: codeblock}

Node 8 を使用して MobileFirst CLI をインストールしているときに、端末ウィンドウに以下のエラーのいくつかが表示されることがあります。
```
> node-gyp rebuild

gyp ERR! clean error 
gyp ERR! stack Error: EACCES: permission denied, rmdir 'build'
gyp ERR! System Darwin 18.0.0
gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /usr/local/lib/node_modules/mfpdev-cli/node_modules/bufferutil
gyp ERR! node -v v8.12.0
gyp ERR! node-gyp -v v3.8.0
gyp ERR! not ok 

> utf-8-validate@1.2.2 install /usr/local/lib/node_modules/mfpdev-cli/node_modules/utf-8-validate
> node-gyp rebuild

gyp ERR! clean error 
gyp ERR! stack Error: EACCES: permission denied, rmdir 'build'
gyp ERR! System Darwin 18.0.0
gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /usr/local/lib/node_modules/mfpdev-cli/node_modules/utf-8-validate
gyp ERR! node -v v8.12.0
gyp ERR! node-gyp -v v3.8.0
gyp ERR! not ok 

> fsevents@1.2.4 install /usr/local/lib/node_modules/mfpdev-cli/node_modules/fsevents
> node install
```

このエラーの原因は、[node-gyp の既知のバグ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/nodejs/node-gyp/issues/1547){: new_window} です。 これらのエラーは、MobileFirst CLI の機能に影響を与えないため無視できます。 この問題は `mfpdev-cli` 暫定修正レベル *8.0.2018100112* 以降に適用されます。 このエラーに対処するには、インストール時に `--no-optional` フラグを使用します。

CLI が正しくインストールされていることを確認するには、以下のコマンドを実行します。
```
mfpdev
```
{: codeblock}

CLI のヘルプが出力として表示されます。

```
 NAME
     IBM MobileFirst Foundation Command Line Interface (CLI).

 SYNOPSIS
     mfpdev <command> [options]

 DESCRIPTION
     The IBM MobileFirst Foundation Command Line Interface (CLI) is a command-line
     for developing MobileFirst applications. The command-line can be used by itself, or in conjunction  with the IBM MobileFirst Foundation Operations Console. Some functions are available from  the command-line only and not the console.

     For more information and a step-by-step example of using the CLI, see the IBM Knowledge Center for your version of IBM MobileFirst Foundation at
     https://www.ibm.com/support/knowledgecenter.
     ...
     ...
     ...
```
{: screen}

## {{site.data.keyword.mobilefoundation_short}} CLI コマンドのリスト
{: #list_cli_commands}

<table summary="{{site.data.keyword.mobilefoundation_short}} CLI コマンド。その詳細にリンクされています。">
<caption>表 1. {{site.data.keyword.mobilefoundation_short}} CLI コマンド</caption>
 <thead>
 <th colspan="6">一般的な [mfpdev](#mfpdev) コマンド</th>
 </thead>
 <tbody>
 <tr>
 <td>[app](#mfpdev_app)</td>
 <td>[server](#mfpdev_server)</td>
 <td>[adapter](#mfpdev_adapter)</td>
 <td>[help](#mfpdev_help)</td>
 </tr>
   </tbody>
 </table>

### mfpdev
{: #mfpdev}

mfpdev コマンド・ライン・インターフェースのプレビュー・ブラウザー・タイプ、プレビュー・タイムアウト値、およびサーバー・タイムアウト値の構成設定を指定します。

```
mfpdev config
```
{: codeblock}

オペレーティング・システム、メモリー使用量、ノード・バージョン、コマンド・ライン・インターフェースのバージョンなど、環境に関する情報を表示します。 現行ディレクトリーが Cordova アプリケーションである場合、Cordova の `cordova info` コマンドで提供される情報も表示されます。

```
mfpdev info
```
{: codeblock}

現在使用されている {{site.data.keyword.mobilefoundation_short}} CLI のバージョン番号を表示します。

```
mfpdev -v
```
{: codeblock}

デバッグ・モード: デバッグ出力を生成します。

```
mfpdev [-d|--debug]
```
{: codeblock}

冗長デバッグ・モード: 冗長デバッグ出力を生成します。

```
mfpdev [-dd|--ddebug]
```
{: codeblock}

コマンド出力でのカラーの使用を抑止します。

```
mfpdev -no-color
```
{: codeblock}

### mfpdev help
{: #mfpdev_help}

MobileFirst CLI (mfpdev) コマンドのヘルプを表示します。 引数としてコマンド名を指定すると、各コマンド・タイプまたは各コマンドの詳細なヘルプ・テキストが表示されます。 例えば、`mfpdev help server add` のように入力します。

```
mfpdev help <command name>
```
{: codeblock}


### mfpdev app
{: #mfpdev_app}

アプリケーションを MobileFirst Server に登録します。

```
mfpdev app register
```
{: codeblock}

デフォルトではないサーバーおよびランタイムにアプリケーションを登録するには、以下のようにします。

```
mfpdev app register <server> <runtime>
```
{: codeblock}

Cordova Windows プラットフォームの場合、`-w <platform>` 引数をコマンドに追加する必要があります。 platform 引数は、登録する Windows プラットフォームのコンマ区切りリストです。 有効な値は windows、windows8、および windowsphone8 です。

```
mfpdev app register -w windows8
```
{: codeblock}

アプリケーションで使用するバックエンド・サーバーおよびランタイムを指定できます。 Cordova アプリケーションの場合、このコマンドを使用すると、システム・メッセージのデフォルト言語やチェックサム・セキュリティー検査を実行するかどうかなど、さらにいくつかの側面を構成できます。 Cordova アプリケーションでは、その他の構成パラメーターが含まれます。

```
mfpdev app config
```
{: codeblock}

サーバーから既存のアプリケーション構成を取得します。

```
mfpdev app pull
```
{: codeblock}

アプリケーション構成をサーバーに送信します。

```
mfpdev app push
```
{: codeblock}

ターゲット・プラットフォーム・タイプの実際のデバイスがなくても Cordova アプリケーションをプレビューできます。 モバイル・ブラウザー・シミュレーターか Web ブラウザーのいずれかでプレビューを表示できます。

```
mfpdev app preview
```
{: codeblock}

www ディレクトリーにあるアプリケーション・リソースを、ダイレクト・アップデート・プロセスで使用できる圧縮 (*.zip*) ファイルにパッケージ化します。

```
mfpdev app webupdate
```
{: codeblock}

このコマンドにより、更新された Web リソースが圧縮 (*.zip*) ファイルにパッケージ化され、登録済みのデフォルトの MobileFirst Server にアップロードされます。 パッケージ化された Web リソースは、`[cordova-project-root-folder]/mobilefirst/` フォルダー内にあります。

Web リソースを別のサーバー・インスタンスにアップロードするには、コマンドの一部としてサーバー名とランタイムを指定します。

```
mfpdev app webupdate <server_name> <runtime>
```
{: codeblock}

–build パラメーターを使用すると、パッケージ化された Web リソースが含まれた圧縮 (*.zip*) ファイルを、サーバーにアップロードすることなく生成できます。
```
mfpdev app webupdate --build
```
{: codeblock}

以前にビルド済みのパッケージをアップロードするには、-file パラメーターを使用します。
```
mfpdev app webupdate --file mobilefirst/com.ibm.test-android-1.0.0.zip
```
{: codeblock}

-encrypt パラメーターを使用してパッケージの内容を暗号化するという選択肢もあります。
```
mfpdev app webupdate --encrypt
```
{: codeblock}


### mfpdev server
{: #mfpdev_server}

MobileFirst Server の情報を表示します。

```
mfpdev server info
```
{: codeblock}

サーバー定義を環境に追加します。

```
mfpdev server add
```
{: codeblock}

サーバー定義を編集できます。

```
mfpdev server edit
```
{: codeblock}

デフォルトのサーバーを設定するには、次のコマンドを使用します。

```
mfpdev server edit <server_name> --setdefault
```
{: codeblock}

サーバー定義を環境から削除します。

```
mfpdev server remove
```
{: codeblock}

MobileFirst Operations Console を開きます。

```
mfpdev server console
```
{: codeblock}

別のサーバーのコンソールを開くには、次のように、コマンドのパラメーターとしてサーバー名を指定します。

```
mfpdev server console <server-name>
```
{: codeblock}

アプリケーションを登録抹消し、アダプターを MobileFirst Server から削除します。

```
mfpdev server clean
```
{: codeblock}

### mfpdev adapter
{: #mfpdev_adapter}

アダプターを作成します。

```
mfpdev adapter create
```
{: codeblock}

アダプターをビルドします。

```
mfpdev adapter build
```
{: codeblock}

現行ディレクトリーおよびそのサブディレクトリー内にあるすべてのアダプターを検出してビルドします。

```
mfpdev adapter build all
```
{: codeblock}

アダプターを MobileFirst Server にデプロイします。

```
mfpdev adapter deploy
```
{: codeblock}

別のサーバーにデプロイするには、次のコマンドを使用します。
```
mfpdev adapter deploy <server_name>
```
{: codeblock}

現行ディレクトリーおよびそのサブディレクトリー内にあるすべてのアダプターを検出して、MobileFirst Server にそれらをデプロイします。

```
mfpdev adapter deploy all
```
{: codeblock}

MobileFirst Server でアダプターのプロシージャーを呼び出します。

```
mfpdev adapter call
```
{: codeblock}

サーバーから既存のアダプター構成を取得します。
```
mfpdev adapter pull
```
{: codeblock}

アダプター構成をサーバーに送信します。
```
mfpdev adapter push
```
{: codeblock}

## {{site.data.keyword.mobilefoundation_short}} CLI の更新とアンインストール
{: #update_uninstall_mf_cli}

コマンド・ライン・インターフェースを更新するには、次のコマンドを実行します。

```
npm update -g mfpdev-cli
```
{: codeblock}

コマンド・ライン・インターフェースをアンインストールするには、次のコマンドを実行します。

```
npm uninstall -g mfpdev-cli
```
{: codeblock}
