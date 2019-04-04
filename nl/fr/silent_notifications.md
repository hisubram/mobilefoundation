---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-28"

keywords: push notifications, notification, sending silent notifications

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

# Notifications silencieuses
{: #silent_notifications}

Les notifications silencieuses sont des notifications qui n'affichent pas d'alertes ou ne dérangent pas l'utilisateur. Lorsqu'une notification silencieuse arrive, le code de gestion d'application s'exécute en arrière-plan sans mettre l'application à l'avant-plan. Actuellement, les notifications silencieuses sont prises en charge sur les appareils iOS dotés de la version 7 au minimum. Si la notification silencieuse est envoyée à des appareils iOS dotés d'une version antérieure à la version 7, la notification est ignorée si l'application s'exécute en arrière-plan. Si l'application s'exécute en avant-plan, la méthode de rappel de notification est appelée.
{: shortdesc}

## Envoi de notifications push silencieuses
{: #sending-silent-push-notifications }

Préparez la notification et envoyez-la. Pour plus d'informations, voir [Envoi de notifications push](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications). 

Les trois types de notifications qui sont pris en charge pour iOS sont représentés par les constantes  `DEFAULT`, `SILENT` et `MIXED`. Lorsque le type n'est pas explicitement spécifié, le type `DEFAULT` est supposé.

Pour les notifications de type `MIXED`, un message est affiché sur l'appareil, tandis que, à l'arrière-plan, l'application mobile sort de veille et traite une notification silencieuse. La méthode de rappel pour les notifications de type `MIXED` est appelée deux fois : une fois lorsque la notification silencieuse atteint l'appareil et une fois lorsque l'application est ouverte en tapant sur la notification.

En fonction de vos besoins, choisissez le type approprié sous **{{ site.data.keyword.mfp_oc_short_notm }} → [votre application] → Push → Envoyer des notifications → Paramètres personnalisés iOS**. 

Si la notification est silencieuse, les propriétés **alerte**, **son** et **badge** sont ignorées.
{: note}

![Définition du type de notification pour les notifications silencieuses iOS dans {{ site.data.keyword.mfp_oc_short_notm }}](images/notification-type-for-silent-notifications.png)

## Traitement des notifications push silencieuses dans les applications Cordova
{: #handling-silent-push-notifications-in-cordova-applications }

Dans la méthode de rappel de notification push JavaScript, vous devez suivre les étapes suivantes : 

1. Vérifiez le type de notification. Exemple :

   ```javascript
   if(props['content-available'] == 1) {
        //Notification silencieuse ou notification mixte. Effectuez ici des tâches qui ne sont pas liées à l'interface graphique utilisateur.
   } else {
        //Notification normale
   }
   ```
   {: codeblock}

2. Si la notification est silencieuse ou mixte, une fois le travail en arrière-plan terminé, appelez l'API `WL.Client.Push.backgroundJobDone`. 

## Traitement des notifications push silencieuses dans les applications iOS natives
{: #handling-silent-push-notifications-in-native-ios-applications }

Vous devez procéder comme suit pour recevoir des notifications silencieuses : 

1. Activez la fonction d'application pour effectuer des tâches en arrière-plan lors de la réception des notifications distantes. 
2. Vérifiez si la notification est silencieuse ou non en vous assurant que la clé `content-available` est définie sur **1**.
3. Une fois que vous avez terminé le traitement de la notification, vous devez immédiatement appeler le bloc dans le paramètre du gestionnaire, sous peine de mettre fin
à l'application. Votre application a jusqu'à 30 secondes pour traiter la notification et appeler le bloc du gestionnaire d'achèvement spécifié.
