---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: push notifications, sending interactive notification

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

# Notifications interactives
{: #interactive_notifications}

Avec une notification interactive, lorsqu'une notification arrive, les utilisateurs peuvent agir dessus sans ouvrir l'application. Lorsqu'une notification interactive arrive, l'appareil affiche des boutons d'action avec le message de notification.
{: shortdesc}

Les notifications interactives sont prises en charge sur les appareils dotés d'iOS version 8 au minimum. Si une notification interactive est envoyée sur un appareil iOS doté d'une version antérieure à la version 8, les actions de notification ne s'affichent pas.

## Envoi d'une notification push interactive
{: #sending-interactive-push-notification }

Préparez la notification et envoyez-la. Pour plus d'informations, voir [Envoi de notifications push](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications).

Vous pouvez définir une chaîne pour indiquer la catégorie de la notification avec l'objet notification, sous **{{ site.data.keyword.mfp_oc_short_notm }} → [votre application] → Push → Envoyer des notifications → Paramètres personnalisés iOS**. Les boutons d'action de notification s'affichent en fonction de la valeur de la catégorie. Exemple :

![Définition de catégories pour des notifications interactives iOS dans {{ site.data.keyword.mfp_oc_short_notm }}](images/categories-for-interactive-notifications.png)

## Traitement des notifications push interactives dans les applications Cordova
{: #handling-interactive-push-notifications-in-cordova-applications }

Pour recevoir des notifications interactives, procédez comme suit :

1. Dans le fichier JavaScript principal, définissez les catégories enregistrées pour la notification interactive et transmettez-la à l'appel d'enregistrement d'appareil `MFPPush.registerDevice`.

   ```javascript
   var options = {
        ios: {
            alert: true,
            badge: true,
            sound: true,     
            categories: [{
                //Identificateur de catégorie, qui est utilisé lors de l'envoi de la notification.
                id : "poll",

                //Tableau facultatif des actions pour afficher les boutons d'action avec le message.    
                actions: [{
                    //Identificateur de l'action
                    id: "poll_ok",

                    //Titre de l'action à afficher dans le cadre du bouton de notification.
                    title: "OK",

                    //Mode facultatif pour exécuter l'action en avant-plan ou arrière-plan. 1-avant-plan. 0-arrière-plan. La valeur par défaut est l'avant-plan.
                    mode: 1,  

                    //Propriété facultative pour marquer le bouton d'action en rouge. La valeur par défaut est false.
                    destructive: false,

                    //Propriété facultative pour définir si l'authentification est requise ou non avant d'exécuter l'action.(Verrouillage d'écran).
                    //Pour l'avant-plan, cette propriété est toujours définie sur true.
                    authenticationRequired: true
                },
                {
                    id: "poll_nok",
                    title: "NOK",
                    mode: 1,
                    destructive: false,
                    authenticationRequired: true
                }],

                //Liste facultative des actions qui doivent s'afficher dans l'alerte de cas.
                //Si elle n'est pas indiquée, les quatre premières actions s'affichent.
                defaultContextActions: ['poll_ok','poll_nok'],

                //Liste facultative des actions qui doivent s'afficher dans le centre de notification, verrouillage d'écran.
                //Si elle n'est pas indiquée, les deux premières actions s'affichent.
                minimalContextActions: ['poll_ok','poll_nok']
            }]     
        }
   }
   ```

2. Transmettez l'objet `options` lors de l'enregistrement de l'appareil pour les notifications push.

   ```javascript
   MFPPush.registerDevice(options, function(successResponse) {
  		navigator.notification.alert("Successfully registered");
  		enableButtons();
   });  
   ```

## Traitement des notifications push interactives dans les applications iOS natives
{: #handling-interactive-push-notifications-in-native-ios-applications }

Procédez comme suit pour recevoir des notifications interactives :

1. Activez la fonction d'application pour effectuer des tâches en arrière-plan lors de la réception des notifications distantes. Cette étape est requise si certaines des actions sont activées en arrière-plan. 
2. Définissez les catégories enregistrées pour les notifications interactives et transmettez-les sous forme d'options à `MFPPush.registerDevice`.

   ```swift
   //définition de catégories pour les notifications push interactives
   let acceptAction = UIMutableUserNotificationAction()
   acceptAction.identifier = "OK"
   acceptAction.title = "OK"
   acceptAction.activationMode = .Foreground

   let rejetAction = UIMutableUserNotificationAction()
   rejetAction.identifier = "Cancel"
   rejetAction.title = "Cancel"
   rejetAction.activationMode = .Foreground

   let category = UIMutableUserNotificationCategory()
   category.identifier = "poll"
   category.setActions([acceptAction, rejetAction], forContext: .Default)

   let categories:Set<UIUserNotificationCategory> = [category]

   let options = ["alert":true, "badge":true, "sound":true, "categories": categories]

   // Enregistrement d'appareil
    MFPPush.sharedInstance().registerDevice(options as [NSObject : AnyObject], completionHandler: {(response: WLResponse!, error: NSError!) -> Void in
   ```
