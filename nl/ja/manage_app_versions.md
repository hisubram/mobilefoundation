---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-29"

keywords: app versions, disabling apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# アプリケーション・バージョンの管理
{: #manage_app_versions}

Mobile Foundation サーバーのユーザーおよび管理者は、Mobile Foundation アプリケーション管理機能によって、アプリケーションへのユーザーおよびデバイスのアクセスを細かく管理できます。

Mobile Foundation サーバーは、モバイル・インフラストラクチャーにアクセスする試みをすべてトラッキングし、アプリケーション、ユーザー、アプリケーションがインストールされるデバイスに関する情報を保管します。 アプリケーション、ユーザー、およびデバイス間のマッピングは、サーバーのモバイル・アプリケーション管理機能の基礎を形成します。

Mobile Foundation Operations Console を使用して、リソースへのアクセスをモニターしたり管理したりできます。特定のアプリケーション・バージョンを管理することもできます。

1.  Mobile Foundation Operations Console に移動し、**「アプリケーション」**をクリックし、管理するアプリケーションを選択し、表示された**「バージョン」**リストから対象の特定のアプリケーション・バージョンを選択します。 ![アプリケーション・バージョンの管理](images/app_version_management.png)

2. **「管理」**タブに、選択したアプリケーション・バージョンのアプリケーション状況を設定するためのオプションが表示されます。 サポートされるアプリケーション・バージョン状況は次のとおりです。
   * アクティブ
   * アクティブ、通知
   * アクセス無効
3. **「アプリケーション・アクセス」 > 「状況」**の下から*「アクセス無効」*オプションを選択することによって、アプリケーション・バージョンを無効にすることができます。
4. **「ダイレクト・アップデート」**セクションで、Cordova アプリケーション用の更新された Web リソースをアップロードするように構成することもできます。 そうすると、この特定のアプリケーション・バージョンを使用して Mobile Foundation サーバーに接続しているユーザーに、ダイレクト・アップデートを使用してアプリケーションを更新するオプションが表示されます。
5. **「アクション」**メニューの以下のオプションを使用して、選択したアプリケーション・バージョンに対して以下のアクションを実行することもできます。
   *  バージョンの削除
   *  バージョンの複製
   *  バージョンのエクスポート


デバイスの管理について詳しくは、[デバイスの管理](/docs/services/mobilefoundation?topic=mobilefoundation-manage_devices#manage_devices)を参照してください。
特定のバージョンのアプリをリモート側で無効にする方法について詳しくは、[アプリケーション・バージョンのリモートでの無効化](/docs/services/mobilefoundation?topic=mobilefoundation-remotely_disable_an_app_version#remotely_disable_an_app_version)を参照してください。
{: note}
