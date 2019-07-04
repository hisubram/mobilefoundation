---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile foundation faq, updates to mobile foundation, custom domain

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:faq: data-hd-content-type='faq'}
{:note: .note}

# Foires aux questions
{: #mfp-faq}

Cette FAQ fournit les réponses aux questions courantes sur le service {{site.data.keyword.mobilefoundation_long}}.
{: shortdesc}

## Comment saurai-je que des mises à jour du service {{site.data.keyword.mobilefoundation_short}} sont disponibles ?
{: #maintupdates_mf}
{: faq}

{{site.data.keyword.mobilefoundation_short}} crée un serveur {{site.data.keyword.mfserver_short_notm}}. Les mises à jour du serveur {{site.data.keyword.mobilefoundation_short}} sont notifiées aux utilisateurs. Une notification apparaît dans le tableau de bord de l'instance de service. L'utilisateur peut choisir d'appliquer la mise à jour dans {{site.data.keyword.mobilefoundation_short}} lors d'une fenêtre de maintenance dont il décide de la date d'exécution. Vous pouvez choisir de mettre à jour le serveur {{site.data.keyword.mobilefoundation_short}} quand cela vous arrange.

La mise à jour du service {{site.data.keyword.mobilefoundation_short}} est mise à disposition quand l'un des composants suivants est mis à jour.

* {{site.data.keyword.mfserver_short_notm}}.
* Version Liberty sous-jacente
* Version Java Developer Kit sous-jacente

## Comment appliquer les mises à jour au service {{site.data.keyword.mobilefoundation_short}} ?
{: #apply_update_mf}
{: faq}

La mise à jour dans {{site.data.keyword.mobilefoundation_short}} peut être appliquée en cliquant sur la commande **Recréer**.
Lors de l'application de la mise à jour, la version du serveur, comme affichée dans {{site.data.keyword.mfp_oc_short_notm}}, est modifiée pour indiquer la version de mise à jour du serveur.

* Les utilisateurs ne peuvent pas appliquer leurs propres correctifs et mises à jour à leur instance de service {{site.data.keyword.mobilefoundation_short}}.
* Voir [Utilisation du plan Professionnel par appareil](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p5#recreate_mobilefoundation_p5) et [Utilisation du plan Professionnel, 1 application](/docs/services/mobilefoundation?topic=mobilefoundation-using_mobilefoundation_p2#recreate_mobilefoundation_p2) pour comprendre la différence de comportement d'un plan à un autre quand vous cliquez sur **Recréer**.
{: note}

## Comment configurer un domaine personnalisé pour mon instance de serveur {{site.data.keyword.mobilefoundation_short}} ?
{: #configcustomdomain}
{: faq}

{{site.data.keyword.mobilefoundation_short}} met à disposition un serveur {{site.data.keyword.mfserver_short_notm}}, qui est accessible en utilisant une URL dont les noms de domaine reposent sur la **région** {{site.data.keyword.Bluemix_notm}}. Vous pouvez également configurer votre propre domaine personnalisé.

L'URL ou la route est créée avec les noms de domaine par défaut, d'après la `région` {{site.data.keyword.Bluemix_notm}}.

  |Domaine |  Région  |    
  |:----- | :----- |    
  |`mybluemix.net` | Sud des Etats-Unis |    
  |`eu-gb.mybluemix.net` | Royaume-Uni  |
  |`au-syd.mybluemix.net` | Sydney  |   
  |`eu-de.mybluemix.net` | Francfort |   
  {: caption="Tableau 1. Noms de domaine de l'application basés sur la région dans {{site.data.keyword.Bluemix_notm}}" caption-side="top"}

Pour utiliser votre propre domaine, vous devez configurer un domaine personnalisé en procédant comme suit :

1.	Créez une instance {{site.data.keyword.mfserver_short_notm}} en créant l'instance de service {{site.data.keyword.mobilefoundation_short}} en sélectionnant l'un des plans pris en charge.

+ Ajoutez dans votre `Organisation` {{site.data.keyword.Bluemix_notm}} le domaine personnalisé que vous souhaitez utiliser. Accédez à **Gérer les organisations > Domaines > Ajouter un domaine** pour ajouter votre propre domaine.

+ Configurez une route permettant au serveur d'utiliser votre domaine personnalisé.

+ Accédez au fournisseur DNS de votre domaine et ajoutez une entrée CNAME, qui achemine le trafic de votre domaine vers la route {{site.data.keyword.Bluemix_notm}} par défaut, dans laquelle le serveur s'exécute.

+ Si vous voulez configurer `https` pour votre domaine personnalisé, transférez le certificat SSL de votre domaine dans {{site.data.keyword.Bluemix_notm}}. Pour télécharger le certificat SSL, accédez à **Gérer les organisations > Domaines**, sélectionnez le domaine personnalisé pour lequel le certificat SSL doit être configuré et cliquez sur **Télécharger le certificat** pour transférer le certificat SSL de votre domaine. Pour plus d'informations, consultez [cet article ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/bluemix/2014/09/28/ssl-certificates-bluemix-custom-domains/){: new_window}.
