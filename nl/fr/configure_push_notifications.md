---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: push notifications, notifications, FCM, GCM, APNS, WNS, authenticate notification

subcollection:  mobilefoundation
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
{:reactnative: .ph data-hd-programlang='React Native'}
{:csharp: .ph data-hd-programlang='csharp'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}
{:xml: .ph data-hd-programlang='xml'}


# Configuration de notifications push
{: #configure_push_notifications}

Pour envoyer des notifications push vers des appareils iOS, Android ou Windows, {{ site.data.keyword.mfserver_short_notm }} doit d'abord être configuré avec les informations FCM pour Android, un certificat APNS pour iOS ou des données d'identification WNS pour Windows 8.1 Universal / Windows 10 UWP.
{: shortdesc}

Les notifications peuvent ensuite être envoyées à l'aide des options suivantes :
* tous les appareils (diffusion),
* appareils qui sont enregistrés sur des étiquettes spécifiques,
* un seul ID appareil,
* plusieurs ID utilisateur,
* uniquement les appareils iOS,
* uniquement les appareils Android,
* uniquement les appareils Windows,
* en fonction de l'utilisateur authentifié.

## Configuration des notifications
{: #setting-up-notifications }

L'activation de la prise en charge des notifications implique plusieurs étapes de configuration, à la fois dans {{ site.data.keyword.mfserver_short_notm }} et dans l'application client. Poursuivez votre lecture pour connaître la procédure de configuration côté serveur, ou passez à la section  [Configuration
côté client](#tutorials-to-follow-next).

Côté serveur, la configuration requise inclut la configuration du fournisseur nécessaire (APNS, FCM ou WNS) et le mappage de la portée "push.mobileclient".

### Firebase Cloud Messaging (FCM)
{: #firebase-cloud-messaging }

Google [a rendu GCM obsolète](https://developers.google.com/cloud-messaging/faq) et a intégré Cloud Messaging à Firebase. Si vous utilisez un projet GCM, vérifiez que vous [migrez les applications client GCM sur Android vers FCM](https://developers.google.com/cloud-messaging/android/android-migrate-fcm).
{: note}

Les appareils Android utilisent le service Firebase Cloud Messaging (FCM) pour les notifications push.

Pour configurer FCM :

1. Consultez la [console Firebase](https://console.firebase.google.com/?pli=1).
2. Créez un projet et indiquez un nom de projet.
3. Cliquez sur l'icône Paramètres en forme de roue dentée et sélectionnez **Project settings**.
4. Cliquez sur l'onglet **Cloud Messaging** pour générer une **clé d'API du serveur** et un **ID d'émetteur**, puis cliquez sur **Save**.

Vous pouvez également configurer FCM à l'aide de l'[API REST pour le service push {{ site.data.keyword.mobilefirst_notm }}](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_gcm_settings_put.html#Push-GCM-Settings--PUT-) ou de l'[API REST pour le service d'administration {{ site.data.keyword.mobilefirst_notm }}](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_gcm_settings_put.html#restservicesapi).
{: note}

Si votre organisation dispose d'un pare-feu qui limite le trafic vers et depuis Internet, vous devez exécuter la procédure suivante :  
* Configurez le pare-feu pour permettre la connectivité avec FCM afin que vos applications client FCM puissent recevoir des messages.
* Les ports à ouvrir sont 5228, 5229 et 5230. FCM utilise généralement uniquement le port 5228, mais il se sert parfois des ports 5229 et 5230.
* FCM ne fournissant pas d'adresse IP spécifique, vous devez permettre à votre pare-feu d'accepter les connexions sortantes vers toutes les adresses IP contenues dans les blocs IP répertoriés dans l'ASN 15169 de Google.
* Assurez-vous que votre pare-feu accepte les connexions sortantes en provenance de {{ site.data.keyword.mfserver_short_notm }} vers fcm.googleapis.com sur le port 443.
{: note }

<img class="gifplayer" alt="Image illustrant l'ajout des données d'identification GCM" src="images/gcm-setup.png"/>

### Apple Push Notification Service (APNS)
{: #apple-push-notifications-service }

Les appareils iOS utilisent Apple Push Notification Service (APNS) pour les notifications push.  

Pour configurer APNS :

1. Générez un certificat de notification push pour le développement ou la production. Pour connaître la procédure détaillée, consultez la section `Pour iOS` [ici](https://cloud.ibm.com/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1).
2. Dans {{ site.data.keyword.mfp_oc_short_notm }} → **[votre application] → Push → Paramètres push**, sélectionnez le type de certificat et indiquez le fichier et le mot de passe du certificat. Cliquez ensuite sur **Sauvegarder**.

  * Pour que des notifications push soient envoyées, les serveurs suivants doivent être accessibles depuis une instance {{ site.data.keyword.mfserver_short_notm }} :
    * Serveurs de bac à sable :
      * gateway.sandbox.push.apple.com:2195
      * feedback.sandbox.push.apple.com:2196
    * Serveurs de production :
      * gateway.push.apple.com:2195
      * Feedback.push.apple.com:2196
      * 1-courier.push.apple.com 5223
  * Lors de la phase de développement, utilisez le fichier certificat de bac à sable apns-certificate-sandbox.p12.
  * Lors de la phase de production, utilisez le fichier certificat de production apns-certificate-production.p12.
    * Le certificat de production APNS peut être testé uniquement lorsque l'application qui l'utilise est soumise à l'App Store Apple.
    {: note }

MobileFirst ne prend pas en charge les certificats Universal.
{: note }

Vous pouvez également configurer APNS à l'aide de l'[API REST pour le service push {{ site.data.keyword.mobilefirst_notm }}](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_apns_settings_put.html#Push-APNS-settings--PUT-) ou l'[API REST pour le service d'administration {{ site.data.keyword.mobilefirst_notm }}](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_apns_settings_put.html?view=kc).
{: note}

<img class="gifplayer" alt="Image illustrant l'ajout des données d'identification APNS" src="images/apns-setup.png"/>

### Windows Push Notification Service (WNS)
{: #windows-push-notifications-service }

Les appareils Windows utilisent Windows Push Notification Service (WNS) pour les notifications push.  

Pour configurer WNS :

1. Suivez les [instructions fournies par Microsoft](https://msdn.microsoft.com/en-in/library/windows/apps/hh465407.aspx) pour générer les valeurs **Identificateur de sécurité de package (SID)** et **Valeur confidentielle du client**.
2. Dans {{ site.data.keyword.mfp_oc_short_notm }} → **[votre application] → Push → Paramètres push**, ajoutez ces valeurs et cliquez
sur **Sauvegarder**.

Vous pouvez également configurer WNS à l'aide de l'[API REST pour le service push {{ site.data.keyword.mobilefirst_notm }}](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_wns_settings_put.html?view=kc) ou l'[API REST pour le service d'administration {{ site.data.keyword.mobilefirst_notm }}](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_wns_settings_put.html?view=kc).

<img class="gifplayer" alt="Image illustrant l'ajout des données d'identification WNS" src="images/wns-setup.png"/>

### Mappage de la portée
{: #scope-mapping }

Mappez l'élément de portée **push.mobileclient** à l'application.

1. Chargez {{ site.data.keyword.mfp_oc_short_notm }} et accédez à **[votre application] → Sécurité → Mappage d'éléments de portée**, puis cliquez sur **Nouveau**.
2. Ecrivez "push.mobileclient" dans la zone **Elément de portée**. Cliquez ensuite sur **Ajouter**.

Liste de portées supplémentaires disponibles :

|**Portées** | **Description**|
|---|---|
|apps.read | Droit de lire la ressource d'application. |
|apps.write | Droit de créer, mettre à jour et supprimer une ressource d'application. |
|gcmConf.read | Droit de lire les paramètres de configuration GCM (clé d'API et ID d'émetteur). |
|gcmConf.write | Droit de mettre à jour et supprimer des paramètres de configuration GCM. |
|apnsConf.read | Droit de lire les paramètres de configuration APNS. |
|apnsConf.write | Droit de mettre à jour et supprimer des paramètres de configuration APNS. |
|devices.read | Droit de lire un appareil. |
|devices.write | Droit de créer, mettre à jour et supprimer un appareil. |
|subscriptions.read | Droit de lire les abonnements. |
|subscriptions.write | Droit de créer, mettre à jour et supprimer des abonnements. |
|messages.write | Droit d'envoyer des notifications push. |
|webhooks.read | Droit de lire des notifications d'événement. |
|webhooks.write | Droit d'envoyer des notifications d'événement.|
|smsConf.read | Droit de lire les paramètres de configuration SMS.|
|smsConf.write | Droit de mettre à jour et supprimer des paramètres de configuration SMS.|
|wnsConf.read | Droit de lire les paramètres de configuration WNS.|
|wnsConf.write | Droit de mettre à jour et supprimer des paramètres de configuration WNS.|
{: caption="Tableau 1. Descriptions de portée" caption-side="top"}

<img class="gifplayer" alt="Mappage de la portée" src="images/scope-mapping.png"/>

### Notifications authentifiées
{: #authenticated-notifications }

Les notifications authentifiées sont des notifications qui sont envoyées à un ou plusieurs `ID utilisateur`.  

Mappez l'élément de portée **push.mobileclient** au contrôle de sécurité utilisé pour l'application.  

1. Chargez {{ site.data.keyword.mfp_oc_short_notm }} et accédez à **[votre application] → Sécurité → Mappage d'éléments de portée**, puis cliquez sur **Nouveau** ou modifiez une entrée de mappage de la portée existante.
2. Sélectionnez un contrôle de sécurité. Cliquez ensuite sur **Ajouter**.

  <img class="gifplayer" alt="Notifications authentifiées" src="images/authenticated-notifications.png"/>

## Définition d'étiquettes
{: #defining-tags }
Dans {{ site.data.keyword.mfp_oc_short_notm }} → **[votre application] → Push → Etiquettes**, cliquez sur **Nouveau**.  
Indiquez les valeurs `Nom de l'étiquette` et `Description` appropriées, puis cliquez sur **Sauvegarder**.

<img class="gifplayer" alt="Ajout d'étiquettes" src="images/adding-tags.png"/>

Les abonnements associent un enregistrement d'appareil à une étiquette. Lorsqu'un appareil est désenregistré d'une étiquette, tous les abonnements associés sont automatiquement désabonnés de l'appareil lui-même. Dans un scénario où il existe plusieurs utilisateurs pour un appareil, les abonnements doivent être mis en oeuvre dans les applications mobiles en fonction des critères de connexion de l'utilisateur. Par exemple, l'appel d'abonnement est effectué après qu'un utilisateur a réussi à se connecter à une application et l'appel de désabonnement est effectué explicitement dans le cadre du traitement de l'action de déconnexion.

## Tutoriels à suivre ensuite
{: #tutorials-to-follow-next }

* [Envoi de notifications push](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)

Maintenant que le côté serveur est configuré, configurez le côté client et traitez les notifications reçues.

* [Traitement de notifications push dans les applications du client](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
