---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-14"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Secure Gateway サービスを使用したオンプレミス・システムへのセキュア接続の確立
{: #integrate_secure_gateway}

エンタープライズ・モバイル・アプリを作成する場合に、アプリを既存の記録システムと統合する必要があることがよくあります。[IBM Cloud](https://cloud.ibm.com/) で [Mobile Foundation サービス](https://cloud.ibm.com/catalog/services/mobile-foundation)と [Secure Gateway サービス](https://cloud.ibm.com/catalog/services/secure-gateway)を使用すれば、モバイル・アプリからオンプレミスのデータ・センターにアクセスし、保管データを利用できます。Secure Gateway サービスは、IBM Cloud と接続先のリモート・システムとの間に、素早く簡単にセキュアな接続を作成し、トンネルを確立します。

このチュートリアルでは、IBM Cloud で実行される Mobile Foundation アダプターから、Secure Gateway サービスを使用して、オンプレミスのデータ・センター内の HTTP エンドポイントにアクセスする方法について説明します。

## 前提条件
{: #prereq}

このチュートリアルを実行するには、お客様の企業のファイアウォールの内側に、記録システムのデータを公開する HTTP エンドポイントが必要になります。代わりに、[このサンプル ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/MobileFirst-Platform-Developer-Center/MFPSecureGatewayIonic/tree/master/NodeJSHTTPProject) `Node.js` プロジェクトを使用して、ローカル環境にテスト・エンドポイントを作成することもできます。

プロジェクト・フォルダーに移動して、HTTP サーバーを実行します。

```bash
npm install
node app.js
```
{: codeblock}

## Secure Gateway サービスとの統合シナリオ
{: #secure_gateway}

以下の図は、このチュートリアルで説明する統合シナリオで使用されているアーキテクチャーを示しています。

![アーキテクチャー図](images/SecureGatewayArchi.png)

## 統合の実装
{: #implementing_integration}

### Secure Gateway サービス・インスタンスの作成
IBM Cloud にログインし、[Secure Gateway サービス](https://cloud.ibm.com/catalog/services/secure-gateway/)のインスタンスを作成します。 

![IBM Cloud](images/SecureGatewayInst.gif)

Secure Gateway サービス・インスタンスを作成したら、以下の手順に従って、IBM Cloud とオンプレミス環境の間に Secure Gateway サービスを構成します。

### ゲートウェイの追加
{: #add_gateway}

Secure Gateway サービス・ダッシュボードで**「ゲートウェイの追加」**をクリックし、任意のゲートウェイ名を指定して新しいゲートウェイを作成します。

![ゲートウェイの追加](images/AcmeAddGateway.gif)


### Secure Gateway クライアントの追加
{: #add_sg_client}

![クライアント 2 の追加](images/AcmeAddClient.gif)

新しいゲートウェイ内で、**「クライアント」**タブから、**「クライアントの接続」**をクリックします。

任意のクライアントを選択し、Secure Gateway クライアントをオンプレミス環境で実行することができます。Secure Gateway クライアントをセットアップする手順が、Secure Gateway コンソールに表示されます。

このチュートリアルでは、Docker コンテナー・オプションを使用して Secure Gateway クライアントを実行することにします。
次の手順に従ってください。
*   まだインストールされていない場合は、Docker をオンプレミスのマシンにインストールします。
*   端末を起動し、サービス・コンソールに表示されたコマンドを使用して、Secure Gateway クライアントをコンテナーで実行します。
    ```bash
    docker run –it ibmcom/secure-gateway-client <gatewayId>
    ```
    {: codeblock}
    上の図に示されているように、`gatewayId` は、コンソール内に表示されています。

### 宛先の追加
{: #add_destination}

![宛先の追加](images/AcmeAddDest.gif)

新しいゲートウェイ内の**「宛先」**タブから、**「宛先の追加」**をクリックします。

Secure Gateway サービスにより、IBM Cloud 環境からオンプレミス・エンドポイントに接続することも、オンプレミス環境からクラウド・リソースに接続することもできます。このシナリオでは、リソースの場所としてオンプレミスを選択し、オンプレミスのリソースのホスト名/IP とポートを指定します。また、宛先名として任意の名前を指定します。

Secure Gateway クライアントからリソースのエンドポイントに要求を転送できるようにするには、リソースをアクセス・リストに追加する必要があります。
リソースをアクセス・リストに追加するには、Secure Gateway クライアント (このシナリオでは Docker ランタイム上でコンテナーとして実行されています) の端末で、次のコマンドを実行します。

```
acl allow <resourceHost>:<resourcePort>
```
{: codeblock}
`resourceHost` および `resourcePort` は、オンプレミスのリソースのエンドポイントの詳細を示しています。

これで、宛先が構成されました。Secure Gateway サービスは、クラウド環境からオンプレミスのリソースにアクセスするために使用できるクラウドのホストとポートの詳細を取り込みます。

![宛先タブ](images/AcmeCloudPopulate.gif)

### Mobile Foundation および Mobile Foundation アダプターによる Secure Gateway サービスの構成
{: #configuration_sg_mfp}

このチュートリアルでは、IBM Cloud の Mobile Foundation サービス・インスタンスを使用して Mobile Foundation サーバーを構成します。IBM Cloud の Mobile Foundation サービスを使用すると、Mobile Foundation サーバーを Cloud Foundry アプリケーションとして Liberty ランタイム上にプロビジョンできます。Mobile Foundation サービスにより、ローカル環境で開発した Mobile Foundation プロジェクトを IBM Cloud で実行することができます。

### IBM Cloud での Mobile Foundation サーバーのセットアップ
{: #mf_server_setup}

IBM Cloud コンソールから、[Mobile Foundation サービス](https://cloud.ibm.com/catalog/services/mobile-foundation)のインスタンスを作成します。

Mobile Foundation サービス・コンソールから、[Mobile Foundation サーバー ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/bluemix/using-mobile-foundation/) を作成します。


### Mobile Foundation Adapter のビルドとデプロイ
{: #deploying_mf_adapter}

このチュートリアルでは、Mobile Foundation アダプターを使用して、Secure Gateway エンドポイントに接続します。Mobile Foundation JavaHTTP アダプターを[ダウンロード ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/MobileFirst-Platform-Developer-Center/Adapters/tree/release80/JavaHTTP) します。

Mobile Foundation Operations コンソールで、[mfpdev-cli](using_cli.html) コマンドを使用して、アダプターをビルドしてデプロイします。
```bash
mfpdev adapter build 
mfpdev adapter deploy
```
{: codeblock}

アダプターをビルドしてデプロイする方法について詳しくは、[こちら ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/) を参照してください。
{: tip}
 
JavaHTTP アダプターのリソース・エンドポイントに、直前のセクションから取得したクラウドのホストおよびポートの詳細を指定します。 

![AdapterConfiguration ](images/AdapterConfiguration.png)

`cap-sg-prd-5.securegateway.appdomain.cloud` および `18946` は、それぞれ Secure Gateway のホストとポートです。
 
これで Mobile Foundation アダプターが構成され、Mobile Foundation サービスが Secure Gateway サービスを使用して企業内のオンプレミス・システムと連携できるようになりました。

### Mobile Foundation サンプル・アプリの作成および登録
{: #registering_sample_app}

Mobile Foundation サンプル・アプリを[こちら](https://github.com/MobileFirst-Platform-Developer-Center/MFPSecureGatewayIonic/)からダウンロードし、`Readme` の説明に従って、Mobile Foundation Operations コンソールでアプリを登録します。

アプリを実行し、ログインのための資格情報を入力し、*「ログイン」*ボタンをクリックします。*「Fetch Acme Writers」*ボタンをクリックすると、Mobile Foundation Operations コンソールでデプロイした JavaHTTP アダプターを使用して Secure Gateway 経由でオンプレミス・エンドポイントを呼び出すことができます。必要なデータをオンプレミス環境から受け取ります。

![アプリでオンプレミス・データを受け取る](images/AcmePublishersApp.gif)

Secure Gateway サービスに複数の宛先を構成し、対応するクラウド・ホストのエンドポイントに接続するための Mobile Foundation アダプターを実装することで、複数のオンプレミス・エンドポイントに接続できます。また、Secure Gateway サービスに追加のセキュリティーを構成して、エンドポイントとの通信が HTTPS およびアプリケーション側のセキュリティーを使用して行われるようにすることもできます。詳しくは、[こちら](https://cloud.ibm.com/docs/services/SecureGateway/index.html)を参照してください。


## 要約
{: #summary}

このチュートリアルを使用すると、IBM Cloud で実行される Mobile Foundation アダプターとオンプレミス HTTP エンドポイントとの間に Secure Gateway サービスを使用してセキュアな接続を確立できます。

