---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-27"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 料金設定
{: #pricing}

IBM Cloud 上の Mobile Foundation サービスでは、3 つの異なるプラン・オプションが提供されています。 有料プランの料金は、特定の国または地域について[ここ](https://cloud.ibm.com/catalog/services/mobile-foundation)で確認できます。 Mobile Foundation サービス用に以下のプランが用意されています。
* デベロッパー 
* プロフェッショナル 1 アプリケーション
* デバイス当たりのプロフェッショナル

## デベロッパー
{: #developer_plan}

「開発者」プランは無料プランです。 このプランでは、*Liberty for Java* ランタイムに Cloud Foundry アプリケーションとして Mobile Foundation サーバーが作成されます。 *Liberty for Java* の料金は別に請求され、このプランには含まれません。 Mobile Analytics は追加料金なしで提供され、イベントは 6 カ月間保持されます。 このプランでは外部データベースの使用がサポートされておらず、開発およびテストに制限されています。 Mobile Foundation サーバーの「開発者」プランのインスタンスでは開発およびテスト用に任意の数の Mobile アプリケーションを登録できますが、このプランでは接続デバイス数は 1 日当たり 10 台に制限されます。

## プロフェッショナル 1 アプリケーション
{: #prof_1_app}

「プロフェッショナル 1 アプリケーション」を使用すると、ユーザーは、ユーザーやデバイスの数に関係なく、予測可能な料金で Mobile Foundation でモバイル・アプリケーションを実稼働で作成、テスト、実行できます。 Mobile Analytics は追加料金なしで提供され、イベントは 6 カ月間保持されます。 このプランでは、最小 1 GB の 2 ノードから開始して、*Liberty for Java* 上でスケーラブルな環境に IBM Cloud Foundry アプリケーションとして Mobile Foundation サーバーが作成されます。 *Liberty for Java* の料金は別に請求され、このプランには含まれません。 このプランでは、別途作成および請求される、IBM Db2 (**ライト**・プラン以外のプラン) または Compose for PostgreSQL サービスのインスタンスも必要です。

## デバイス当たりのプロフェッショナル
{: #prof_per_device}

「デバイス当たりのプロフェッショナル」プランを使用すると、ユーザーは、Mobile Foundation 上で最大 5 つのモバイル・アプリケーションを実稼働で作成、テスト、実行できます。 Mobile Analytics は追加料金なしで提供され、イベントは 6 カ月間保持されます。 課金は、日々の接続されたクライアント・デバイスの数に基づきます。 このプランは、大規模なデプロイメントと高可用性をサポートします。 このプランでは、別途作成および請求される、IBM DB2 (**ライト**・プラン以外のプラン) または Compose for PostgreSQL サービスのインスタンスが必要です。 このプランでは、最小 1 GB の 2 ノードから開始して、*Liberty for Java* 上に Mobile Foundation サーバーが作成されます。 *Liberty for Java* の料金は別途請求され、このプランの一部として含まれることはありません。
