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

# Notificações Silenciosas
{: #silent_notifications}

Notificações silenciosas são notificações que não exibem alertas ou que, de outra maneira, não perturbem o usuário. Quando uma notificação silenciosa chega, o código de entrega do aplicativo é executado em segundo plano sem trazer o aplicativo para primeiro plano. Atualmente, as notificações silenciosas são suportadas em dispositivos do iOS com a versão 7 em diante. Se a notificação silenciosa for enviada para dispositivos do iOS com a versão menor que 7, a notificação será ignorada se o aplicativo estiver em execução em segundo plano. Se o aplicativo estiver em execução no primeiro plano, então, o método de retorno de chamada de notificação será chamado.
{: shortdesc}

## Enviando notificações push silenciosas
{: #sending-silent-push-notifications }

Prepare a notificação e envie a notificação. Para obter mais informações, consulte [Enviando notificações de push](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications).

Os três tipos de notificações que são suportados para iOS são representados por constantes `DEFAULT`, `SILENT` e `MIXED`. Quando o tipo não é especificado explicitamente, o tipo `DEFAULT` é assumido.

Para notificações de tipo `MIXED`, uma mensagem é exibida no dispositivo enquanto, em segundo plano, o app desperta e processa uma notificação silenciosa. O método de retorno de chamada para as notificações de tipo `MIXED` é chamado duas vezes - uma vez quando a notificação silenciosa atinge o dispositivo e uma vez quando o aplicativo é aberto dando um toque na notificação.

Com base no requisito, escolha o tipo apropriado em **{{ site.data.keyword.mfp_oc_short_notm }} → [seu aplicativo] → Push → Enviar notificações → Configurações customizadas do iOS**.

Se a notificação for silenciosa, as propriedades **alert**, **sound** e **badge** serão ignoradas.
{: note}

![Configurando o tipo de notificação para notificações silenciosas do iOS no {{ site.data.keyword.mfp_oc_short_notm }}](images/notification-type-for-silent-notifications.png)

## Manipulando notificações push silenciosas em aplicativos Cordova
{: #handling-silent-push-notifications-in-cordova-applications }

No método de retorno de chamada de notificação push JavaScript, deve-se executar as etapas a seguir:

1. Verifique o tipo de notificação. Por exemplo,

   ```javascript
   if (props [ 'content-available' ] == 1) {
        // Silent Notification or Mixed Notification. Execute tarefas não da GUI aqui.
   } else { else {
        // Normal notification }
   ```
   {: codeblock}

2. Se a notificação for silenciosa ou combinada, depois de concluir a tarefa de segundo plano, chame a API `WL.Client.Push.backgroundJobDone`.

## Manipulando notificações push silenciosas em aplicativos iOS nativos
{: #handling-silent-push-notifications-in-native-ios-applications }

Deve-se seguir estas etapas para receber notificações silenciosas:

1. Ative o recurso do aplicativo para executar tarefas em segundo plano no recebimento de notificações remotas.
2. Verifique se a notificação é silenciosa ou não verificando se a chave `content-available` está configurada como **1**.
3. Depois de concluir o processamento da notificação, deve-se chamar o bloco no parâmetro do manipulador imediatamente, caso contrário, seu app será finalizado. Seu app tem até 30 segundos para processar a notificação e chamar o bloco de manipulador de conclusão especificado.
