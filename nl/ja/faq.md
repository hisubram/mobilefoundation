---

copyright:
  years: 2018
lastupdated:  "2018-02-09"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}


# FAQ

この FAQ では、{{site.data.keyword.mobilefoundation_long}} サービスに関する、よくある質問の答えを記載しています。
{: shortdesc}

## {{site.data.keyword.mobilefoundation_short}} サービスの更新が使用可能になったことは、どのようにして分かりますか。
{: #maintupdates_mf}

{{site.data.keyword.mobilefoundation_short}} は {{site.data.keyword.mfserver_short_notm}} をプロビジョンします。 {{site.data.keyword.mobilefoundation_short}} サーバーに対する更新は、ユーザーに通知されます。 通知は、サービス・インスタンス・ダッシュボードに表示されます。 ユーザーは、自分で決定した保守時間帯に {{site.data.keyword.mobilefoundation_short}} に更新を適用することを選択できます。 都合のよいときに {{site.data.keyword.mobilefoundation_short}} サーバーを更新することができます。

以下のいずれかのコンポーネントが更新されたときに、{{site.data.keyword.mobilefoundation_short}} サービス更新が利用可能になります。

* {{site.data.keyword.mfserver_short_notm}}.
* 基盤となる Liberty のバージョン。
* 基盤となる Java Developer Kit のバージョン。

## {{site.data.keyword.mobilefoundation_short}} サービスに更新を適用するにはどうすればいいですか。
{: #apply_update_mf}

{{site.data.keyword.mobilefoundation_short}} に対する更新は、**「再作成」**をクリックして適用できます。
更新を適用すると、{{site.data.keyword.mfp_oc_short_notm}} に表示されるサーバーのバージョンが変更され、サーバーの更新バージョンを示します。

> **注:**
>  * ユーザーが自分の {{site.data.keyword.mobilefoundation_short}} サービス・インスタンスに独自のフィックスや更新を適用することはできません。
>  * *「再作成」** をクリックしたときのプランによる動作の違いについては、[「開発者」プランでのサーバーの再作成](c_using_mfs_p1.html#recreate_mobilefoundation_p1)、[「開発者専門」プランでのサーバーの再作成](c_using_mfs_p3.html#recreate_mobilefoundation_p3)、[「デバイス当たりのプロフェッショナル」プランでのサーバーの再作成 ](c_using_mfs_p4.html#recreate_mobilefoundation_p5)、および[「プロフェッショナル 1 アプリケーション」プランでのサーバーの再作成](c_using_mfs_p2.html#recreate_mobilefoundation_p2)を参照してください。*
>

## {{site.data.keyword.mobilefoundation_short}} サーバー・インスタンスのカスタム・ドメインを構成するにはどうすればいいですか。
{: #configcustomdomain}

{{site.data.keyword.mobilefoundation_short}} は、{{site.data.keyword.Bluemix_notm}} **地域**に基づいたドメイン名を含む URL を使用してアクセス可能な、{{site.data.keyword.mfserver_short_notm}} をプロビジョンします。 独自のカスタム・ドメインを構成することも可能です。

この URL や経路は、{{site.data.keyword.Bluemix_notm}} `地域`に基づいたデフォルト・ドメイン名を使用して作成されます。

  |ドメイン |  地域  |    
  |:----- | :----- |    
  |`mybluemix.net` | 米国南部 |    
  |`eu-gb.mybluemix.net` | 英国  |
  |`au-syd.mybluemix.net` | シドニー  |   
  |`eu-de.mybluemix.net` | フランクフルト |   
  {: caption="表 1. {{site.data.keyword.Bluemix_notm}} の地域に基づいたアプリケーション・ドメイン名" caption-side="top"}

独自ドメインを使用できるようにするには、以下のステップを実行してカスタム・ドメインを構成する必要があります。

1.	いずれかのサポートされるプランを選択して、{{site.data.keyword.mobilefoundation_short}} サービス・インスタンスを作成することで、{{site.data.keyword.mfserver_short_notm}} インスタンスを作成します。

+ 使用するカスタム・ドメインを {{site.data.keyword.Bluemix_notm}} `組織` に追加します。 **「組織の管理」>「ドメイン」>「ドメインの追加」**と進み、独自のドメインを追加します。

+ サーバーがカスタム・ドメインを使用するように<!--container group-->経路をセットアップします。

+ ドメインの DNS プロバイダーに進み、サーバーが実行されているデフォルトの {{site.data.keyword.Bluemix_notm}} の経路にドメインからトラフィックをルーティングする CNAME エントリーを追加します。<!--container group-->

+ カスタム・ドメインに `https` を構成する場合は、{{site.data.keyword.Bluemix_notm}} でドメインの SSL 証明書をアップロードします。 これを行うには、**「組織の管理」>「ドメイン」**と進み、SSL 証明書を構成するカスタム・ドメインを選択し、**「証明書のアップロード」**をクリックしてドメインの SSL 証明書をアップロードします。 詳しくは、[この投稿 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/bluemix/2014/09/28/ssl-certificates-bluemix-custom-domains/){: new_window} を参照してください。
