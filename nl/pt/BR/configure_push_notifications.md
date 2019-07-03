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


# Configurar Notificações push
{: #configure_push_notifications}

Para enviar notificações push para dispositivos iOS, Android ou Windows, o {{ site.data.keyword.mfserver_short_notm }} precisa primeiro ser configurado com os detalhes do FCM para Android, um certificado APNS para iOS ou credenciais WNS para o Windows 8.1 Universal/Windows 10 UWP.
{: shortdesc}

As notificações podem então ser enviadas usando as opções a seguir:
* todos os dispositivos (transmissão)
* dispositivos que se registraram em tags específicas
* um único ID de dispositivo,
* Ids do usuário
* apenas dispositivos iOS
* Somente dispositivos Android
* apenas dispositivos Windows
* com base no usuário autenticado.

## Configurando Notificações
{: #setting-up-notifications }

A ativação do suporte a notificações envolve várias etapas de configuração no {{ site.data.keyword.mfserver_short_notm }} e no aplicativo cliente. Continue a leitura para a configuração do lado do servidor ou vá para [Configuração do lado do cliente](#tutorials-to-follow-next).

No lado do servidor, a configuração necessária inclui: configurar o fornecedor necessário (APNS, FCM ou WNS) e mapear o escopo "push.mobileclient".

### Firebase Cloud Messaging
{: #firebase-cloud-messaging }

O Google [descontinuou o GCM](https://developers.google.com/cloud-messaging/faq) e integrou o Cloud Messaging ao Firebase. Se você estiver usando um projeto GCM, assegure-se de [migrar os aplicativos do cliente GCM no Android para o FCM](https://developers.google.com/cloud-messaging/android/android-migrate-fcm).
{: note}

Os dispositivos Android usam o serviço Firebase Cloud Messaging (FCM) para notificações push.

Para configurar o FCM:

1. Visite o  [ Console do Firebase ](https://console.firebase.google.com/?pli=1).
2. Crie um projeto e forneça um nome de projeto.
3. Clique no ícone de "engrenagem" de Configurações e selecione **Configurações do projeto**.
4. Clique na guia **Cloud Messaging** para gerar uma **Chave de API do servidor** e um **ID do emissor** e clique em **Salvar**.

Também é possível configurar o FCM usando a [API de REST para o serviço de Push do {{ site.data.keyword.mobilefirst_notm }}](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_gcm_settings_put.html#Push-GCM-Settings--PUT-) ou a [API de REST para o serviço de administração do {{ site.data.keyword.mobilefirst_notm }}](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_gcm_settings_put.html#restservicesapi).
{: note}

Se a sua organização tem um firewall que restringe o tráfego para ou da Internet, deve-se passar pelas etapas a seguir:  
* Configure o firewall para permitir a conectividade com o FCM a fim de que os apps do cliente FCM recebam mensagens.
* As portas a serem abertas são 5228, 5229 e 5230. O FCM geralmente usa somente 5228, mas às vezes usa 5229 e 5230.
* O FCM não fornece IP específico, portanto, deve-se permitir que seu firewall aceite conexões de saída com todos os endereços IP contidos nos blocos de IP listados no ASN do Google de 15169.
* Assegure-se de que seu firewall aceite conexões de saída de {{ site.data.keyword.mfserver_short_notm }} para fcm.googleapis.com na porta 443.
{: note }

<img class="gifplayer" alt="Image of adding the GCM credentials" src="images/gcm-setup.png"/>

### Serviço de Notificações push da Apple
{: #apple-push-notifications-service }

Os dispositivos iOS usam o Push Notification Service (APNS) da Apple para notificações push.  

Para configurar o APNS:

1. Gerar um certificado de notificação push para desenvolvimento ou produção. Para obter etapas detalhadas, consulte a seção `For iOS` [aqui](https://cloud.ibm.com/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1).
2. No {{ site.data.keyword.mfp_oc_short_notm }} → **[seu aplicativo] → Push → Configurações de Push**, selecione o tipo de certificado e forneça o arquivo e a senha do certificado. Em seguida, clique em  ** Salvar **.

  * Para que as notificações push sejam enviadas, os servidores a seguir devem ser acessíveis de uma instância do {{ site.data.keyword.mfserver_short_notm }},
    * Servidores de ambiente de simulação:
      * gateway.sandbox.push.apple.com:2195
      * feedback.sandbox.push.apple.com:2196
    * Servidores de produção:
      * gateway.push.apple.com:2195
      * Feedback.push.apple.com:2196
      * 1-courier.push.apple.com 5223
  * Durante a fase de desenvolvimento, use o arquivo de certificado de ambiente de simulação apns-certificate-sandbox.p12.
  * Durante a fase de produção, use o arquivo de certificado de produção apns-certificate-production.p12.
    * O certificado de produção do APNS pode ser testado somente quando o aplicativo que o utiliza é enviado com êxito para o Apple App Store.
    {: note }

O MobileFirst não suporta certificados universais.
{: note }

Também é possível configurar APNS usando a [API REST para o serviço {{site.data.keyword.mobilefirst_notm }} Push](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_apns_settings_put.html#Push-APNS-settings--PUT-) ou a [API REST para o {{ site.data.keyword.mobilefirst_notm }}serviço de administração](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_apns_settings_put.html?view=kc).
{: note}

<img class="gifplayer" alt="Image of adding the APNS credentials" src="images/apns-setup.png"/>

### Serviço de Notificações push do Windows
{: #windows-push-notifications-service }

Os dispositivos Windows usam o Windows Push Notifications Service (WNS) para notificações push.  

Para configurar o WNS:

1. Siga as [instruções fornecidas pela Microsoft](https://msdn.microsoft.com/en-in/library/windows/apps/hh465407.aspx) para gerar os valores de **Identificador de segurança do pacote (SID)** e **Segredo do cliente**.
2. No {{ site.data.keyword.mfp_oc_short_notm }} → **[seu aplicativo] → Push → Configurações de Push**, inclua esses valores e clique em **Salvar**.

Também é possível configurar o WNS usando a [API de REST para o serviço de Push do {{ site.data.keyword.mobilefirst_notm }}](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/rest_runtime/r_restapi_push_wns_settings_put.html?view=kc) ou a [API de REST para o serviço de administração do {{ site.data.keyword.mobilefirst_notm }}](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.apiref.doc/apiref/r_restapi_update_wns_settings_put.html?view=kc)

<img class="gifplayer" alt="Image of adding the WNS credentials" src="images/wns-setup.png"/>

### Mapeamento de Escopo
{: #scope-mapping }

Mapeie o elemento do escopo **push.mobileclient** para o aplicativo.

1. Carregue o {{ site.data.keyword.mfp_oc_short_notm }} e navegue para **[seu aplicativo] → Segurança → Mapeamento de elementos do escopo**, clique em **Novo**.
2. Escreva "push.mobileclient" no campo **Elemento do escopo**. Em seguida, clique em  ** Incluir **.

Lista de mais escopos disponíveis:

|**Escopos** | ** Descrição **|
|---|---|
|.l.apps.l | Permissão para ler o recurso de aplicativo. |
|apps.write | Permissão para criar, atualizar, excluir o recurso de aplicativo. |
|gcmConf.read | Permissão para ler as definições de configuração do GCM (Chave de API e SenderId). |
|gcmConf.write | Permissão para atualizar, excluir definições de configuração do GCM. |
|apnsConf.read | Permissão para ler as definições de configuração do APNs. |
|apnsConf.write | Permissão para atualizar, excluir definições de configuração de APNs. |
|devices.read | Permissão para ler dispositivo. |
|devices.write | Permissão para criar, atualizar dispositivo de exclusão. |
|subscriptions.read | Permissão para ler assinaturas. |
|subscriptions.write | Permissão para criar, atualizar, excluir assinaturas. |
|messages.write | Permissão para enviar notificações push. |
|webhooks.read | Permissão para ler notificações de eventos. |
|webhooks.write | Permissão para enviar notificações de eventos.|
|smsConf.read | Permissão para ler definições de configuração SMS.|
|smsConf.write | Permissão para atualizar, excluir definições de configuração de SMS.|
|wnsConf.read | Permissão para ler as definições de configuração do WNS.|
|wnsConf.write | Permissão para atualizar, excluir definições de configuração do WNS.|
{: caption="Tabela 1. Descrições do escopo" caption-side="top"}

<img class="gifplayer" alt="Scope mapping" src="images/scope-mapping.png"/>

### Notificações Autenticadas
{: #authenticated-notifications }

Notificações autenticadas são notificações que são enviadas para um ou mais `userIds`.  

Mapeie o elemento do escopo **push.mobileclient** para a verificação de segurança usada para o aplicativo.  

1. Carregue o {{ site.data.keyword.mfp_oc_short_notm }} e navegue para **[seu aplicativo] → Segurança → Mapeamento de elementos do escopo**, clique em **Novo** ou edite uma entrada de mapeamento de escopo existente.
2. Selecione uma verificação de segurança. Em seguida, clique em  ** Incluir **.

  <img class="gifplayer" alt="Authenticated notifications" src="images/authenticated-notifications.png"/>

## Definindo Tags
{: #defining-tags }
No {{ site.data.keyword.mfp_oc_short_notm }} → **[seu aplicativo] → Push → Tags**, clique em **Novo**.  
Forneça o `Tag Name` e a `Description` apropriados e clique em **Salvar**.

<img class="gifplayer" alt="Adding tags" src="images/adding-tags.png"/>

As assinaturas vinculam um registro de dispositivo e uma tag. Quando um dispositivo tem o registro cancelado de uma tag, todas as assinaturas associadas são automaticamente canceladas do próprio dispositivo. Em um cenário no qual múltiplos usuários de um dispositivo existem, as assinaturas devem ser implementadas em aplicativos móveis com base nos critérios de login do usuário. Por exemplo, a chamada de assinatura é feita depois que um usuário efetua login com êxito em um aplicativo e a chamada de cancelamento de assinatura é feita explicitamente como parte da manipulação da ação de logout.

## Tutorials a seguir
{: #tutorials-to-follow-next }

* [ Enviar Notificações push ](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)

Com o lado do servidor agora configurado, configure o lado do cliente e manipule as notificações recebidas.

* [ Manipulando notificações push em aplicativos clientes ](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
