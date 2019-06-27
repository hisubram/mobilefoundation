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

# Notificações Interativas
{: #interactive_notifications}

Com a notificação interativa, quando uma notificação chega, os usuários podem agir sobre a notificação sem abrir o aplicativo. Quando uma notificação interativa chegar, o dispositivo mostrará os botões de ação juntamente com a mensagem de notificação.
{: shortdesc}

As notificações interativas são suportadas em dispositivos com o iOS versão 8 e superior. Se uma notificação interativa for enviada para um dispositivo do iOS com versão anterior à 8, as ações de notificação não serão exibidas.

## Enviando notificação push interativa
{: #sending-interactive-push-notification }

Prepare a notificação e envie a notificação. Para obter mais informações, consulte [Enviando notificações de push](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications).

É possível configurar uma sequência para indicar a categoria da notificação com o objeto de notificação, em **{{ site.data.keyword.mfp_oc_short_notm }} → [seu aplicativo] → Push → Enviar notificações → Configurações customizadas do iOS**. Com base no valor da categoria, os botões de ação de notificação são exibidos. Por exemplo,

![Configurando categorias para notificações interativas do iOS no {{ site.data.keyword.mfp_oc_short_notm }}](images/categories-for-interactive-notifications.png)

## Manipulando notificações push interativas em aplicativos Cordova
{: #handling-interactive-push-notifications-in-cordova-applications }

Para receber notificações interativas, siga estas etapas:

1. No JavaScript principal, defina as categorias registradas para notificação interativa e passe-a para a chamada de registro de dispositivo `MFPPush.registerDevice`.

   ```javascript
   var options = {
        ios: {
            alert: true,
            badge: true,
            sound: true,     
            categories: [{
                //Category identifier, this is used while sending the notification.
                id: "poll",

                //Optional array of actions to show the action buttons along with the message.    
                actions: [ {
                    //Action identifier
                    id: "poll_ok",

                    //Action title to be displayed as part of the notification button.
                    title: "OK",

                    //Optional mode to run the action in foreground or background. 1-primeiro plano. 0-background. O padrão é o primeiro plano.
                    modo: 1,  

                    //Optional property to mark the action button in red color. O padrão é false.
                    destrutivo: falso,

                    //Optional property to set if authentication is required or not before running the action.(Screen lock).
                    //For foreground, this property is always true.
                    authenticationRequired: true
                },
                {
                    id: "poll_nok",
                    title: "NOK",
                    mode: 1,
                    destructive: false,
                    authenticationRequired: true
                }],

                //Optional list of actions that is needed to show in the case alert.
                //If it is not specified, then the first four actions will be shown.
                defaultContextActions: [ 'poll_ok ',' poll_nok ' ],

                // Lista opcional de ações que são necessárias para mostrar no centro de notificação, na tela de bloqueio.
                //If it is not specified, then the first two actions will be shown.
                minimalContextActions: [ 'poll_ok ',' poll_nok ' ]
            }]     
        }
   }
   ```

2. Passe o objeto `options` enquanto registra o dispositivo para notificações push.

   ```javascript
   MFPPush.registerDevice( options, function (successResponse) {
  		navigator.notification.alert("Successfully registered");
  		enableButtons();
   });  
   ```

## Manipulando notificações push interativas em aplicativos iOS nativos
{: #handling-interactive-push-notifications-in-native-ios-applications }

Siga estas etapas para receber notificações interativas:

1. Ative o recurso do aplicativo para executar tarefas em segundo plano no recebimento de notificações remotas. Esta etapa será necessária se algumas das ações forem ativadas no segundo plano.
2. Defina categorias registradas para notificações interativas e passe-as como opções para `MFPPush.registerDevice`.

   ```swift
   //define categories for Interactive Push
   let acceptAction = UIMutableUserNotificationAction()
   acceptAction.identifier = "OK"
   acceptAction.title = "OK"
   acceptAction.activationMode = .Foreground

   let rejetAction = UIMutableUserNotificationAction()
   rejetAction.identifier = "Cancel"
   rejetAction.title = "Cancel"
   rejetAction.activationMode = .Foreground

   let category = UIMutableUserNotificationCategory () category.identifier = "poll" category.setActions( [acceptAction, rejetAction ], forContext: .Default)

   let categories: Set < UIUserNotificationCategory> = [ category ]

   as opções = [ "alert" :true, "badge" :true, "sound" :true, "categories": categories ]

   // Register device
    MFPPush.sharedInstance().registerDevice(options as [NSObject : AnyObject], completionHandler: {(response: WLResponse!, error: NSError!) -> Void in
   ```
