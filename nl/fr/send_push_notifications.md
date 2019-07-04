---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: push notifications, notifications, sending notification, HTTP/2

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


# Envoi de notifications push
{: #send_push_notifications}

Les notifications push peuvent être envoyées soit à partir de {{ site.data.keyword.mfp_oc_short_notm }}, soit via des API REST.
{: shortdesc}

* {{ site.data.keyword.mfp_oc_short_notm }} permet d'envoyer deux types de notifications : étiquette et diffusion.
* Les API REST permettent d'envoyer toutes les formes de notifications : étiquette, diffusion et authentifiées.

## Envoi de notifications push à partir de {{ site.data.keyword.mfp_oc_short_notm }}
{: #sending-push-notification-from-mobilefirst-operations-console }

Les notifications peuvent être envoyées à un seul ID appareil, un ou plusieurs ID utilisateur, uniquement des appareils iOS ou Android, ou à des appareils abonnés à des étiquettes.

### Notifications d'étiquette
{: #tag-notifications }

Les notifications d'étiquette sont des messages de notification qui ciblent tous les appareils abonnés à une étiquette spécifique. Les étiquettes représentent des sujets présentant un intérêt pour l'utilisateur et permettent de recevoir des notifications en fonction de l'intérêt choisi.

Sur l'onglet {{ site.data.keyword.mfp_oc_short_notm }} → **[votre application] → Push → Envoyer des notifications**, sélectionnez **Appareils par étiquette** sur l'onglet **Envoyer à** et indiquez le **Texte de la notification**. Cliquez ensuite sur **Envoyer**.

<img class="gifplayer" alt="Envoi par étiquette" src="images/sending-by-tag.png"/>

### Notifications de diffusion
{: #broadcast-notifications }

Les notifications de diffusion constituent une forme de notifications push d'étiquette qui ciblent tous les appareils abonnés. Elles sont activées par défaut pour toutes les applications
{{ site.data.keyword.mobilefirst_notm }} activées pour la notification push par le biais d'un abonnement à une étiquette `Push.all` réservée (créée automatiquement pour chaque appareil). Le désabonnement de l'étiquette `Push.all` peut s'effectuer à l'aide d'un programme.

Sur l'onglet {{ site.data.keyword.mfp_oc_short_notm }} → **[votre application] → Push → Envoyer des notifications**, sélectionnez **Tout**
sur l'onglet **Envoyer à** et indiquez le **Texte de la notification**. Cliquez ensuite sur **Envoyer**.

<img class="gifplayer" alt="Envoyer à tous" src="images/sending-to-all.png"/>

## Envoi de notifications push à l'aide d'API REST
{: #sending-push-notifications-using-rest-apis }

Les API REST permettent d'envoyer toutes les formes de notifications : notifications d'étiquette et de diffusion, et notifications authentifiées.

Pour envoyer une notification, une demande est transmise à l'aide de POST au noeud final REST : `imfpush/v1/apps/<application-identifier>/messages`.  
Voici un exemple d'URL :

```
https://myserver.com:443/imfpush/v1/apps/com.sample.PinCodeSwift/messages
```

Pour passer en revue toutes les API REST des notifications push, consultez la
[rubrique Services d'exécution d'API REST](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/c_restapi_runtime.html) dans la
documentation utilisateur.

### Contenu de la notification
{: #notification-payload }

La demande peut contenir les propriétés de contenu suivantes :

|Propriétés de contenu| Définition
|--- | ---
|message | Message d'alerte à envoyer |
|settings | Les paramètres sont les différents attributs de la notification. |
|target | Les ensemble de cibles peuvent être des ID consommateur, des appareils, des plateformes ou des étiquettes. Vous ne pouvez définir qu'une seule cible. |
|deviceIds | Tableau des appareils représentés par les identificateurs d'appareil. Les appareils portant ces ID reçoivent la notification. Il s'agit d'une notification unicast. |
|notificationType | Valeur entière permettant d'indiquer le canal (Push ou SMS) utilisé pour envoyer un message. Les valeurs autorisées sont 1 (par Push uniquement), 2 (par SMS uniquement) et 3 (par Push et SMS) |
|platforms | Tableau des plateformes d'appareil. Les appareils fonctionnant sur ces plateformes reçoivent la notification. Les valeurs prises en charge sont A (Apple/iOS), G (Google/Android) et M (Microsoft/Windows). |
|tagNames | Tableau des étiquettes spécifiées sous la forme tagNames. Les appareils qui sont abonnés à ces étiquettes reçoivent la notification. Utilisez ce type de cible pour les notifications basées sur des étiquettes. |
|userIds | Tableau d'utilisateurs représentés par leurs ID utilisateur pour envoyer la notification. Il s'agit d'une notification unicast. |
|phoneNumber | Numéro de téléphone qui est utilisé pour enregistrer l'appareil et recevoir des notifications. Il s'agit d'une notification unicast. |
{: caption="Tableau 1. Propriétés de contenu" caption-side="top"}

**Exemple JSON de contenu de notifications push**

```json
{
    "message" : {
    "alert" : "Test message",
  },
  "settings" : {
    "apns" : {
      "badge" : 1,
      "iosActionKey" : "Ok",
      "payload" : "",
      "sound" : "song.mp3",
      "type" : "SILENT",
    },
    "gcm" : {
      "delayWhileIdle" : ,
      "payload" : "",
      "sound" : "song.mp3",
      "timeToLive" : ,
    },
  },
  "target" : {
    // The following list is for demonstration purposes only - per the documentation only 1 target is allowed to be used at a time.
    "deviceIds" : [ "MyDeviceId1", ... ],
    "platforms" : [ "A,G", ... ],
    "tagNames" : [ "Gold", ... ],
    "userIds" : [ "MyUserId", ... ],
  },
}
```

**Exemple JSON de contenu de notifications par SMS**

```json
{
  "message": {
    "alert": "Hello World from an SMS message"
  },
  "notificationType":3,
   "target" : {
     "deviceIds" : ["38cc1c62-03bb-36d8-be8e-af165e671cf4"]
   }
}
```

## Envoi de la notification
{: #sending-the-notification }

La notification peut être envoyée à l'aide de différents outils. **Postman** est utilisé à des fins de test. La procédure suivante décrit la configuration.

1. [Configurez un client confidentiel](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients/).
  L'envoi d'une notification push via l'API REST utilise les éléments de portée séparés par un espace `messages.write` et `push.application.<applicationId>.`
  <img class="gifplayer" alt="Configuration d'un client confidentiel" src="images/push-confidential-client.png"/>
2. [Créez un jeton d'accès](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients#obtaining-an-access-token).  
3. Envoyez une demande **POST** à **http://localhost:9080/imfpush/v1/apps/com.sample.PushNotificationsAndroid/messages**
  - Si vous utilisez {{ site.data.keyword.mobilefirst_notm }} à distance, remplacez les valeurs `hostname` et `port` par les vôtres.
  - Mettez à jour la valeur de l'identificateur d'application en indiquant la vôtre.
4. Définissez un en-tête :
    - `**Authorization**: Bearer eyJhbGciOiJSUzI1NiIsImp ...`
    - Remplacez la valeur située après **Bearer** par celle de votre jeton d'accès obtenue à l'étape (1).
    ![En-tête d'autorisation](images/postman_authorization_header.png "En-tête d'autorisation")
5. Définissez un corps :
  - Mettez à jour ses propriétés comme décrit dans [Contenu de la notification](#notification-payload).
  - Par exemple, en ajoutant la propriété **target** avec l'attribut **userIds**, vous pouvez envoyer une notification à des utilisateurs enregistrés spécifiques.
    ```json
    {
         "message" : {
             "alert" : "Hello World!"
         }
    }
    ```

    ![Corps de l'autorisation](images/postman_json.png "Corps de l'autorisation")

    Une fois que vous avez cliqué sur le bouton **Envoyer**, l'appareil reçoit une notification :

    ![Image d'un modèle d'application](images/notifications-app.png "Notification push sur un appareil mobile")

## Personnalisation des notifications
{: #customizing-notifications }

Avant d'envoyer le message de notification, vous pouvez également personnaliser les attributs de notification suivants.  

Sur l'onglet {{ site.data.keyword.mfp_oc_short_notm }} → **[votre application] → Push → Etiquettes → Envoyer des notifications**, développez la section **Paramètres personnalisés iOS/Android** pour modifier les attributs de la notification.

### Android
{: #android }

* Son de la notification, durée de conservation d'une notification dans le stockage FCM, contenu personnalisé, etc.
* Si vous souhaitez modifier le titre de la notification, ajoutez `push_notification_title` dans le fichier **strings.xml** du projet Android.

### iOS
{: #ios }

* Son de la notification, contenu personnalisé, titre de la clé d'action, type de notification et numéro de badge.

  ![Personnalisation des notifications push](images/customizing-push-notifications.png "Page Mobile First Operations Console Push avec onglet Send Push sélectionné")

## Prise en charge du protocole HTTP/2 pour les notifications push du service APNS
{: #http2-support-for-apns-push-notifications}

Apple Push Notification Service (APNS) prend en charge une nouvelle API basée sur le protocole de réseau HTTP/2. La prise en charge du protocole HTTP/2 offre de nombreux avantages, notamment les suivants :

* La longueur du message a été augmentée de 2 à 4 ko, ce qui permet d'ajouter un contenu supplémentaire aux notifications.
* Elimine la nécessité d'effectuer plusieurs connexions entre le client et le serveur, ce qui permet d'améliorer le débit.
* Prise en charge du certificat SSL client de notification push universel.

>Les notifications push dans {{ site.data.keyword.mobilefirst_notm }} prennent désormais en charge les notifications push APNS basées sur HTTP/2 avec les notifications existantes
basées sur un socket TCP.

### Activation du protocole HTTP/2
{: #enabling-http2}

Les notifications basées sur HTTP/2 peuvent être activées à l'aide d'une propriété JNDI.
```xml
<jndiEntry jndiName="imfpush/mfp.push.apns.http2.enabled" value= "true"/>
```
{: codeblock}

Si la propriété JNDI est ajoutée, les notifications existantes basées sur un socket TCP ne sont pas utilisées et seules les notifications basées sur HTTP/2 sont activées.
{: note}

### Prise en charge de proxy pour HTTP/2
{: #proxy-support-for-http2}

Les notifications basées sur HTTP/2 peuvent être envoyées via un proxy HTTP. Pour activer le routage des notifications via un proxy, voir [ici](#proxy-support).

## Prise en charge de proxy
{: #proxy-support }
Vous pouvez utiliser les paramètres de proxy pour définir le proxy facultatif via lequel les notifications sont envoyées à des appareils Android et iOS. Vous pouvez définir le proxy à l'aide des propriétés de configuration `push.apns.proxy.*` et `push.gcm.proxy.*`. Pour plus d'informations, voir [Liste des propriétés JNDI pour le service push {{ site.data.keyword.mfserver_short_notm }}](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/installation-configuration/production/server-configuration/#list-of-jndi-properties-for-mobilefirst-server-push-service).

## Etapes suivantes
{: #next-tutorial-to-follow }
Maintenant que le côté serveur est configuré, configurez le côté client et traitez les notifications reçues en utilisant le tutoriel suivant.

* [Traitement des notifications push dans les applications client](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
