---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-26"

keywords: Push Notifications, notifications, unicast notifications, tag notifications, interactive notifications, silent notifications, configure DataPower

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

# Notificações push
{: #push_notifications}

Notificações é a capacidade de um dispositivo móvel receber mensagens que são "enviadas por push" de um servidor. As notificações são recebidas, independentemente se o aplicativo está em execução no primeiro plano ou no segundo plano.  
{: shortdesc}

O {{ site.data.keyword.IBM_notm }} {{ site.data.keyword.mobilefoundation_short }} fornece um conjunto unificado de métodos de API para enviar notificações push para aplicativos iOS, Android, Windows 8.1 Universal, Windows 10 UWP e Cordova (iOS, Android). As notificações são enviadas do {{ site.data.keyword.mfserver_short_notm }} para a infraestrutura do fornecedor (Apple, Google, Microsoft, Gateways SMS) e de lá para os dispositivos relevantes. O mecanismo de notificação unificado torna todo o processo de comunicação com os usuários e os dispositivos transparente para o desenvolvedor.

## Suporte ao dispositivo
{: #device-support }
A notificação push é suportada para as plataformas a seguir no {{ site.data.keyword.mobilefoundation_short }}:

* iOS 8.x ou mais recente
* Android 4.x ou mais recente
* Windows 8.1, Windows 10

## Notificações Push
{: #push-notifications-forms }
As notificações podem tomar várias formas:

* **Alerta** (iOS, Android, Windows) - uma mensagem de texto de pop-up
* **Som** (iOS, Android, Windows) - um arquivo de som que é reproduzido quando uma notificação é recebida
* **Badge** (iOS), Bloco (Windows) - uma representação gráfica que permite um texto ou uma imagem curta
* **Banner** (iOS), Aviso (Windows) - uma mensagem de texto de pop-up que desaparece na parte superior da exibição do dispositivo
* **Interativo** (iOS 8 e superior) - botões de ação dentro do banner de uma notificação recebida
* **Silencioso** (iOS 8 e superior) - enviar notificações sem perturbar o usuário

### Tipos de Notificação Push
{: #push-notification-types }

* **Notificações de Tag**
{: #tag-notifications }

    Notificações de identificação são mensagens de notificação destinadas a todos os dispositivos que assinaram uma identificação específica.  

    As notificações baseadas em identificação permitem a segmentação de notificações com base em áreas ou tópicos de assunto. Os destinatários da notificação podem optar por receber notificações somente se for sobre um assunto ou um tópico de interesse. Portanto, a notificação baseada em identificação fornece um meio de segmentar destinatários. Com esse recurso, é possível definir tags e enviar ou receber mensagens por tags. Uma mensagem é destinada somente aos dispositivos que assinaram uma identificação.

* **Notificações de Transmissão**
{: #broadcast-notifications }

    As notificações de transmissão são uma forma de notificações push de tag que são destinadas a todos os dispositivos assinados e são ativadas por padrão para qualquer aplicativo {{ site.data.keyword.mobilefirst_notm }} ativado por push por uma assinatura para uma tag `Push.all` reservada (criada automaticamente para cada dispositivo). As notificações de transmissão podem ser desativadas cancelando a assinatura da tag `Push.all` reservada.

* **Notificações unicast**
{:# unicast-notifications }

    As Notificações unicast ou Notificações autenticadas pelo usuário são protegidas com OAuth. Essas mensagens de notificação são destinadas a um dispositivo específico ou a IDs do usuário. O ID do usuário na assinatura do usuário pode vir do contexto de segurança subjacente.

* **Notificações interativas**
{: #interactive-notifications-overview }

    Com a notificação interativa, quando uma notificação chega, os usuários podem agir sem abrir o aplicativo. Quando uma notificação interativa chegar, o dispositivo mostrará os botões de ação juntamente com a mensagem de notificação. Atualmente, as notificações interativas são suportadas em dispositivos com o iOS versão 8 em diante. Se uma notificação interativa for enviada para um dispositivo do iOS com versão anterior à 8, as ações de notificação não serão exibidas.

    > Saiba como tratar de [Notificações interativas](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications).

* **Notificações Silenciosas**
{: #silent-notifications-overview }

    Notificações silenciosas são notificações que não exibem alertas ou que, de outra maneira, não perturbem o usuário. Quando uma notificação silenciosa chega, o código de entrega do aplicativo é executado em segundo plano sem trazer o aplicativo para primeiro plano. Atualmente, as notificações silenciosas são suportadas em dispositivos do iOS com a versão 7 em diante. Se a notificação silenciosa for enviada para dispositivos do iOS com a versão menor que 7, a notificação será ignorada se o aplicativo estiver em execução em segundo plano. Se o aplicativo estiver em execução no primeiro plano, então, o método de retorno de chamada de notificação será chamado.

    > Saiba como manipular  [ Notificações Silenciosas ](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications).

    As notificações unicast não contêm nenhuma tag na carga útil. A mensagem de notificação pode ter como destino múltiplos dispositivos ou usuários especificando múltiplos IDs do dispositivo ou IDs do usuário, respectivamente, no bloco de destino da API de mensagem POST.
    {: note}

## Configurações de proxy
{: #proxy-settings }

Use as configurações de proxy para configurar o proxy opcional por meio do qual as notificações são enviadas para APNS e FCM. É possível configurar o proxy usando as propriedades de configuração **push.apns.proxy.*** e **push.gcm.proxy.***. Para obter mais informações, consulte [Lista de propriedades JNDI para o serviço de push do {{ site.data.keyword.mfserver_short_notm }}](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/installation-configuration/production/server-configuration/#list-of-jndi-properties-for-mobilefirst-server-push-service).

O WNS não possui suporte de proxy.
{: note}

### Usando o WebSphere DataPower como um terminal de notificação push
{: #proxy-settings-datapower-endpoint }

É possível configurar o DataPower para aceitar solicitações de notificação do {{ site.data.keyword.mfserver_short_notm }} e redirecioná-lo para o FCM e o WNS.

Os APNs não são suportados.
{: note}

#### Configurando o {{ site.data.keyword.mfserver_short_notm }}
{: #proxy-settings-datapower-1 }

Em `server.xml`, configure a propriedade JNDI a seguir.
```xml
<jndiEntry jndiName="imfpush/mfp.push.dp.endpoint" value = '"https://host"' />
<jndiEntry jndiName="imfpush/mfp.push.dp.gcm.port" value = '"port"' />
<jndiEntry jndiName="imfpush/mfp.push.dp.wns.port" value = '"port"' />
```
{: codeblock}

em que `host` é o nome do host do DataPower e `port` é o número da porta na qual o HTTPS Front Side Handler está configurado para FCM e WNS.

#### Configurando o DataPower
{: #proxy-settings-datapower-2 }

1. Efetue login no dispositivo DataPower.
2. Navegue para **Serviços** > **Gateway multiprotocolo** > **Novo gateway multiprotocolo**.
3. Forneça um nome com o qual é possível identificar a configuração.
4. Selecione o Gerenciador de XML, a Política de gateway multiprotocolo como padrão e a Política de regravação de URL como nenhuma.
5. Selecione o botão de opções **back-end estático** e selecione qualquer uma das opções a seguir para **Configurar a URL de back-end padrão**:
    - Para FCM, `https://gcm-http.googleapis.com`
    - Para WNS, `https://hk2.notify.windows.com`
6. Selecione o Tipo de resposta, o Tipo de solicitação como passagem.

#### Gerando um certificado
{: #proxy-settings-datapower-3 }

Para gerar certificado, escolha qualquer uma das opções a seguir:

- Para FCM,
	1. Na linha de comandos, emita `Openssl` para obter os certificados do FCM.
	2. Execute o seguinte comando:
		```
		openssl s_client -connect gcm-http.googleapis.com:443
		```
    {: codeblock}

	3. Copie os conteúdos de *-----BEGIN CERTIFICATE----- até -----END CERTIFICATE-----* e salve-os em um arquivo com a extensão `.pem`.

- Para WNS,
	1. Na linha de comandos, use `Openssl` para obter os certificados do WNS.
	2. Execute o seguinte comando:
		```
		openssl s_client -connect https://hk2.notify.windows.com:443
		```
    {: codeblock}
	3. Copie os conteúdos de *-----BEGIN CERTIFICATE----- até -----END CERTIFICATE-----* e salve-os em um arquivo com a extensão `.pem`.

#### Configurações do lado traseiro
{: #proxy-settings-datapower-4 }

- Para FCM e WNS,
    1. Crie um Certificado Crypto.

        a. Navegue para **Objetos** > **Configuração de criptografia** e clique em **Certificado de criptografia**.

        b. Forneça um nome com o qual é possível identificar o certificado crypto.

        c. Clique em **Fazer upload** para fazer upload do certificado do FCM gerado.

        d. Configure **Alias de senha** para nenhum.

        e. Clique em **Gerar chave**.

        ![Configure Crypto certificate](images/bck_1.gif)

    2. Crie uma Credencial de Validação de Criptografia.

        a. Navegue para **Objetos** > **Configuração de criptografia** e clique em **Credencial de validação de criptografia**.

        b. Forneça um nome exclusivo.

        c. Para Certificados, selecione o Certificado de criptografia criado na etapa anterior - etapa 1.

        d. Para **Modo de validação de certificado**, selecione Corresponder certificado exato ou emissor imediato.

        e. Clique em **Aplicar**.

        ![Configure Crypto validation credentials](images/bck_2.gif)

    3. Crie uma Credencial de Validação de Criptografia:

        a. Navegue para **Objetos** > **Configuração de criptografia** e clique em **Perfil de criptografia**.

        b. Clique em **Incluir**.

        c. Forneça um nome exclusivo.

        d. Para **Credenciais de validação**, selecione a credencial de validação criada na etapa anterior - etapa 2 no menu suspenso, configure Credenciais de identificação como **nenhuma**.

        e. Clique em **Aplicar**.

        ![Configure Crypto profile](images/bck_3.gif)

    4. Crie um Perfil Proxy SSL:

        a. Navegue para **Objetos** > **Configuração de criptografia** > **Perfil proxy SSL**.

        b. Escolha uma das opções a seguir:

            - Para SMS, selecione **Perfil proxy SSL** como nenhum.

            - Para FCM e WNS com uma URL de back-end seguro (HTTPS), conclua as etapas a seguir:

                i. Clique em **Incluir**.

                ii. Forneça um nome com o qual é possível identificar o perfil proxy ssl posteriormente.

                iii. Selecione **Direção de SSL** como **Encaminhar** na lista suspensa.

                iv. Para Perfil de criptografia de encaminhamento (cliente), selecione o perfil de criptografia criado na etapa 3.

                v. Clique em **Aplicar**.

        ![Configure SSL Proxy profile](images/bck_4.gif)

    5. Na janela Gateway multiprotocolo, sob **Configurações do lado de trás**, selecione **Perfil proxy** como o **Tipo de cliente SSL** e selecione o Perfil proxy SSL criado na etapa 4.

        ![Configure SSL Proxy profile](images/bck_5.gif)

#### Configurações de Frontside
{: #proxy-settings-datapower-5 }

- Para FCM e WNS:

    1. Crie um par chave/certificado com o valor de Nome Comum (CN) como o nome do host do DataPower:

        a. Navegue para **Administração** > **Diversos** e clique em **Ferramentas de criptografia**.

        b. Insira o nome do host do DataPower como o valor para o Nome Comum (CN).

        c. Selecione **Exportar chave privada** se você planeja exportar a chave privada posteriormente e clique em **Gerar chave**.

        ![Creating a key-certificate pair](images/frnt_1.gif)

    2. Crie uma Credencial de Identificação de Criptografia:

        a. Navegue para **Objetos** > **Configuração de criptografia** e clique em **Credenciais de identificação de criptografia**.

        b. Clique em **Incluir**.

        c. Forneça um nome exclusivo.

        d.  Para a Chave de criptografia e o Certificado, selecione a chave e o certificado gerados na etapa anterior - etapa 1 na caixa de listagem.

        e. Clique em **Aplicar**.

        ![Creating a Crypto Identification Credential](images/frnt_2.gif)

    3. Crie um Perfil de Criptografia:

        a. Navegue para **Objetos** > **Configuração de criptografia** e clique em **Perfil de criptografia**.

        b. Clique em **Incluir**.

        c. Forneça um nome exclusivo.

        d. Para Credenciais de identificação, selecione a credencial de identificação criada na etapa anterior - etapa 2 na caixa de listagem. Configure as credenciais de Validação para nenhum.

        e. Clique em **Aplicar**.

        ![Configure Crypto Profile](images/frnt_3.gif)

    4. Crie um Perfil Proxy SSL:

        a. Navegue para **Objetos** > **Configuração de criptografia** > **Perfil proxy SSL**.

        b. Clique em **Incluir**.

        c. Forneça um nome exclusivo.

        d. Selecione Direção de SSL como **Reversa** na caixa de listagem.

        e. Para o Perfil de criptografia reversa (servidor), selecione o perfil de criptografia criado na etapa anterior - etapa 3.  

        f. Clique em **Aplicar**.

        ![Configure SSL Proxy Profile](images/frnt_4.gif)

    5. Crie um Manipulador Frontal HTTPS:

        a. Navegue para **Objetos** > **Manipuladores de protocolo** > **HTTPS Front Side Handler**.

        b. Clique em **Incluir**.

        c. Forneça um nome exclusivo.

        d. Para **Endereço IP local**, selecione o alias correto ou deixe-o no valor padrão (0.0.0.0).

        e. Forneça uma porta disponível.

        f. Para **Métodos e versões permitidos**, selecione HTTP 1.0, HTTP 1.1, método POST, método GET, URL com ?, URL com #, URL com ..

        g. Selecione **Perfil proxy** como o tipo de servidor SSL.

        h. Para o perfil proxy SSL (descontinuado), selecione o perfil proxy ssl que é criado na etapa anterior da etapa 4.

        i. Clique em **Aplicar**.

        ![Configure HTTPS Front Side Handler](images/frnt_5.gif)

    6. Na página Configurar gateway multiprotocolo, em **Configurações do lado frontal**, selecione o HTTPS Front Side Handler como **Protocolo do lado frontal**, criado na etapa 5, e clique em **Aplicar**.

        ![General configuration](images/frnt_6.gif)

    O certificado que está sendo usado pelo DataPower em Configurações do lado frontal é um autoassinado. As conexões com o DataPower falham, a menos que o certificado seja incluído no keystore do JRE usado pelo {{ site.data.keyword.mobilefirst_notm }}.

## Próximas etapas
{: #next-steps }

Siga a configuração necessária do lado do servidor e do lado do cliente para enviar e receber notificações push:

* [Configurar Notificações de Push](/docs/services/mobilefoundation?topic=mobilefoundation-configure_push_notifications#configure_push_notifications)
* [Enviar Notificações de Push](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)
* [Manipulando notificações push em aplicativos clientes](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
* [Notificações Silenciosas](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [Notificações interativas](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
