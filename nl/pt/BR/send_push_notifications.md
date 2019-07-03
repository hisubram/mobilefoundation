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


# Enviar Notificações Push
{: #send_push_notifications}

As notificações push podem ser enviadas por meio do {{ site.data.keyword.mfp_oc_short_notm }} ou por meio de APIs de REST.
{: shortdesc}

* Com o {{ site.data.keyword.mfp_oc_short_notm }}, dois tipos de notificações podem ser enviados: tag e transmissão.
* Com as APIs de REST, todas as formas de notificações podem ser enviadas: tag, transmissão e autenticadas.

## Enviando Notificações Push de {{ site.data.keyword.mfp_oc_short_notm }}
{: #sending-push-notification-from-mobilefirst-operations-console }

As notificações podem ser enviadas para um único ID do dispositivo, um único ou vários IDs do usuário, somente dispositivos iOS ou somente dispositivos Android ou para dispositivos inscritos em tags.

### Notificações de tag
{: #tag-notifications }

Notificações de identificação são mensagens de notificação destinadas a todos os
dispositivos que assinaram uma identificação específica. As tags representam tópicos de interesse para o usuário e fornece a capacidade de receber notificações de acordo com o interesse escolhido.

No {{ site.data.keyword.mfp_oc_short_notm }} → **[seu aplicativo] → Push → guia Enviar notificações**, selecione **Dispositivos por tags** na guia **Enviar para** e forneça o **Texto de notificação**. Em seguida, clique em  ** Enviar **.

<img class="gifplayer" alt="Sending by tag" src="images/sending-by-tag.png"/>

### Notificações de Transmissão
{: #broadcast-notifications }

Notificações de transmissão são uma forma de notificações push de tag que são destinadas a todos os dispositivos inscritos. As notificações de transmissão são ativadas por padrão para qualquer aplicativo {{ site.data.keyword.mobilefirst_notm }} ativado por push por uma assinatura para uma tag `Push.all` reservada (criada automaticamente para cada dispositivo). A tag `Push.all` pode ter a assinatura cancelada programaticamente.

No {{ site.data.keyword.mfp_oc_short_notm }} → **[seu aplicativo] → Push → guia Enviar notificações**, selecione **Todos** na guia **Enviar para** e forneça o **Texto de notificação**. Em seguida, clique em  ** Enviar **.

<img class="gifplayer" alt="Sending to all" src="images/sending-to-all.png"/>

## Enviando notificações push usando APIs de REST
{: #sending-push-notifications-using-rest-apis }

Quando você usa as APIs REST para enviar notificações, todas as formas de notificações podem ser enviadas: tags e notificações de transmissão e notificações autenticadas.

Para enviar uma notificação, uma solicitação é feita usando POST para o terminal REST: `imfpush/v1/apps/<application-identifier>/messages`.  
A seguir está uma URL de exemplo,

```
https://myserver.com: 443/imfpush/v1/apps/com.sample.PinCodeSwift/messages
```

Para revisar todas as APIs de REST do Push Notifications, consulte o [tópico Serviços de tempo de execução da API de REST](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/c_restapi_runtime.html) na documentação do usuário.

### Carga útil da notificação
{: #notification-payload }

A solicitação pode conter as propriedades de carga útil a seguir:

|Propriedades da Carga Ú| Definição
|--- | ---
|mensagem | A mensagem de alerta a ser enviada |
|configurações | As configurações são os diferentes atributos da notificação. |
|alvo | O conjunto de destinos pode ser IDs de consumidor, dispositivos, plataformas ou tags. Somente um dos destinos pode ser configurado. |
|deviceIds | Uma matriz dos dispositivos representada pelos identificadores de dispositivo. Os dispositivos com esses IDs recebem a notificação. Esta é uma notificação unicast. |
|notificationType | O valor de número inteiro para indicar o canal (Push ou SMS) usado para enviar mensagem. Os valores permitidos são 1 (somente para Push), 2 (somente para SMS) e 3 (para Push e SMS) |
|plataformas | Uma matriz de plataformas de dispositivo. Os dispositivos em execução nessas plataformas recebem a notificação. Os valores suportados são A (Apple/iOS), G (Google/Android) e M (Microsoft/Windows). |
|TagNames | Uma matriz de tags que são especificadas como tagNames. Os dispositivos que estão inscritos nessas tags recebem a notificação. Use esse tipo de destino para notificações baseadas em tag. |
|userIds | Uma matriz de usuários representada por seus userIds para enviar a notificação. Esta é uma notificação unicast. |
|phoneNumber | O número do telefone que é usado para registrar o dispositivo e receber notificações. Esta é uma notificação unicast. |
{: caption="Tabela 1. Propriedades de carga útil" caption-side="top"}

** Exemplo de Push Notifications Payload JSON **

```json
{
    "message": {
    "alert" : "Test message",
  },
  "settings" : {
    "apns": {
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
    "deviceIds": [ "MyDeviceId1",... ],
    "platforms" : [ "A,G", ... ],
    "tagNames" : [ "Gold", ... ],
    "userIds" : [ "MyUserId", ... ],
  },
}
```

**Exemplo JSON de Carga Útil de Notificação SMS**

```json
{
  "message": {
    "alert": "Hello World from an SMS message"
  },
  "notificationType":3,
   "target" : {
     "deviceIds": [ "38cc1c62-03bb-36d8-be8e-af165e671cf4" ]
   }
}
```

## Enviando a notificação
{: #sending-the-notification }

A notificação pode ser enviada usando diferentes ferramentas. Para propósitos de teste, **Postman** é usado, as etapas a seguir descrevem a configuração,

1. [ Configurar um Cliente Confidencial ](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients/).
  O envio de uma Notificação push por meio da API de REST usa elementos de escopo separados por espaço `messages.write` e `push.application.<applicationId>.`
  <img class="gifplayer" alt="Configure a confidential client" src="images/push-confidential-client.png"/>
2. [ Crie um token de acesso ](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients#obtaining-an-access-token).  
3. Faça uma solicitação  ** POST **  para  ** http://localhost: 9080/imfpush/v1/apps/com.sample.PushNotificationsAndroid/messages **
  - Se estiver usando um {{ site.data.keyword.mobilefirst_notm }} remoto, substitua os valores `hostname` e `port` pelos seus próprios.
  - Atualize o valor do identificador do aplicativo com o seu próprio.
4. Configure um Cabeçalho:
    - ` ** Autorização**: Bearer eyJhbGciOiJSUzI1NiIsImp ... `
    - Substitua o valor após **Portador** pelo valor do seu token de acesso da etapa (1). ![Cabeçalho de autorização](images/postman_authorization_header.png "Cabeçalho de autorização")
5. Configure um Corpo:
  - Atualize suas propriedades conforme descrito em [Carga útil de notificação](#notification-payload).
  - Por exemplo, ao incluir a propriedade **target** com o atributo **userIds**, é possível enviar uma notificação para usuários registrados específicos.
    ```json {
         "message": {
             "alert" : "Hello World!"
         }
    }
    ```

    ![Corpo de autorização](images/postman_json.png "Corpo de autorização")

    Depois que você clica no botão **Enviar**, o dispositivo recebe uma notificação:

    ![Imagem do aplicativo de amostra](images/notifications-app.png "Notificação push em um dispositivo móvel")

## Customizando Notificações
{: #customizing-notifications }

Antes de enviar a mensagem de notificação, é possível também customizar os atributos de notificação a seguir.  

No {{ site.data.keyword.mfp_oc_short_notm }} → **[seu aplicativo] → Push → Tags → guia Enviar notificações**, expanda a seção **Configurações customizadas do iOS/Android** para mudar os atributos de notificação.

### Android
{: #android }

* Som de notificação, por quanto tempo uma notificação pode ser armazenada no armazenamento do FCM, carga útil customizada e mais.
* Se você desejar mudar o título de notificação, inclua `push_notification_tile` no arquivo **strings.xml** do projeto Android.

### iOS
{: #ios }

* Som de notificação, carga útil customizada, título da tecla de ação, tipo de notificação e número do badge.

  ![Customizando notificações push](images/customizing-push-notifications.png "Página de push do Operations Console do Mobile First com a guia Enviar push selecionada")

## Suporte HTTP/2 para Notificações push de APNs
{: #http2-support-for-apns-push-notifications}

O Apple Push Notification service (APNs) suporta uma nova API baseada no protocolo de rede HTTP/2. O suporte para HTTP/2 fornece muitos benefícios, incluindo os seguintes,

* O comprimento da mensagem que é aumentado de 2 KB para 4 KB, que permite incluir conteúdo extra nas notificações.
* Elimina a necessidade de várias conexões entre o cliente e o servidor, o que melhora o rendimento.
* Suporte ao Certificado SSL do Universal Push Notification Client.

>O Push Notifications no {{ site.data.keyword.mobilefirst_notm }} suporta agora o Push Notifications de APNs baseados em HTTP/2 juntamente com as notificações baseadas em Soquete TCP anterior.

### Ativando HTTP/2
{: #enabling-http2}

As notificações baseadas em HTTP/2 podem ser ativadas usando uma Propriedade JNDI.
```xml
< jndiEntry jndiName="imfpush/mfp.push.apns.http2.enabled "value=" true " />
```
{: codeblock}

Se a propriedade JNDI for incluída, as notificações baseadas em Soquete TCP anteriores não serão usadas e somente as notificações baseadas em HTTP/2 serão ativadas.
{: note}

### Suporte ao Proxy para HTTP/2
{: #proxy-support-for-http2}

As notificações baseadas em HTTP/2 podem ser enviadas por meio de um Proxy HTTP. Para ativar o roteamento das notificações por meio de um proxy, consulte [aqui](#proxy-support).

## Suporte ao Proxy
{: #proxy-support }
É possível usar configurações de proxy para configurar o proxy opcional por meio do qual as notificações são enviadas para dispositivos Android e iOS. É possível configurar o proxy usando as propriedades de configuração `push.apns.proxy.*` e `push.gcm.proxy.*`. Para obter mais informações, consulte [Lista de propriedades JNDI para o serviço de push do {{ site.data.keyword.mfserver_short_notm }}](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/installation-configuration/production/server-configuration/#list-of-jndi-properties-for-mobilefirst-server-push-service).

## Próximas etapas
{: #next-tutorial-to-follow }
Com o lado do servidor agora configurado, configure o lado do cliente e manipule as notificações recebidas usando o tutorial a seguir.

* [ Manipulando notificações push em aplicativos clientes ](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
