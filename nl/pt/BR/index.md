---

copyright:
  years: 2016, 2018
lastupdated:  "2018-03-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:tip: .tip}

# Tutorial de Introdução
{: #gettingstartedtemplate}

O {{site.data.keyword.mobilefoundation_long}} expede a configuração de um ambiente do {{site.data.keyword.mfp_full}} e com esse uso é possível desenvolver, testar e operar apps móveis corporativos. O {{site.data.keyword.mobilefoundation_short}} está disponível em diferentes planos de serviços: Developer, Professional Per Device e Professional 1 Application.
{:shortdesc}

Usando o plano Professional 1 Application, um único aplicativo desenvolvido em qualquer ou todas as plataformas operacionais suportadas, tais como Android, iOS, Windows ou web móvel, pode ser
gerenciado. O plano do Desenvolvedor é mais adequado para desenvolvimento e teste. É possível revisar todos os planos disponíveis [aqui](https://console.bluemix.net/catalog/services/mobile-foundation).

## Antes de iniciar
{: #prereqs}

Será necessária uma conta do {{site.data.keyword.Bluemix}} e uma instância do serviço do {{site.data.keyword.mobilefoundation_short}}.

## Etapa 1: criar uma instância do serviço do {{site.data.keyword.mobilefoundation_short}}
{: #step1create}

1. No **Catálogo** do {{site.data.keyword.Bluemix_notm}}, selecione **{{site.data.keyword.mobilefoundation_short}}**. A tela de configuração de
serviço é aberta.
2. Dê à sua instância de serviço um nome ou use o nome predefinido.
3. Escolha a região, a organização e o espaço em que você gostaria de criar a instância de serviço.
4. Selecione o seu **Plano de precificação** e clique em **Criar**.

## Etapa 2: construir o seu canal móvel
{: #buildmobilechannel}

### Para {{site.data.keyword.mobilefoundation_short}}: plano do Desenvolvedor
{: #buildchanneldevplan}

Após você criar uma instância do {{site.data.keyword.mobilefoundation_short}}: Desenvolvedor, é possível iniciar a construção de seu canal móvel concluindo as etapas a seguir.

* É possível acessar e trabalhar instantaneamente com o servidor MobileFirst.

  Esta seleção provisiona um {{site.data.keyword.mfserver_long_notm}} com as configurações a seguir:
  *	1 GB de memória. Este tamanho é suficiente para desenvolvimento, atividades de teste leve e cargas de trabalho de produção em pequena escala.

  * Para acessar o servidor MobileFirst usando a CLI, é necessário usar credenciais, que estão disponíveis quando você clica em **Credenciais de serviço** na área de janela de navegação esquerda do console do IBM Cloud.

### Para o plano {{site.data.keyword.mobilefoundation_short}}: Professional Per Device
{: #buildchannelprofdeviceplan}

Após criar uma instância do serviço {{site.data.keyword.mobilefoundation_short}}: Professional Per Device, é possível iniciar a construção do seu canal de modelo completando as etapas a seguir.

  1.  Conecte-se a um serviço existente do {{site.data.keyword.Db2_on_Cloud_short}} no {{site.data.keyword.Bluemix_notm}}.

      1.  Selecione a {{site.data.keyword.Bluemix_notm}} `Organização` na qual a instância do serviço {{site.data.keyword.Db2_on_Cloud_short}} existe.

      + Selecione {{site.data.keyword.Bluemix_notm}} `Space` em que a instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}} existe, na lista de espaços disponíveis na
`Organization` selecionada.

      + Selecione {{site.data.keyword.Db2_on_Cloud_short}} `Service name` e `Credentials` para se conectar à instância de serviço existente do {{site.data.keyword.Db2_on_Cloud_short}}.

      + Teste a conexão com a instância de serviço selecionada do {{site.data.keyword.Db2_on_Cloud_short}} clicando em **Testar conexão**.

      + Clique em **Incluir**, seguido de **Continuar**, na janela pop-up que solicita confirmação no serviço {{site.data.keyword.Db2_on_Cloud_short}} selecionado. Essa ação cria as tabelas necessárias na instância de serviço de banco de dados {{site.data.keyword.Db2_on_Cloud_short}} configurada.

      > **Nota:** após incluir uma conexão do
{{site.data.keyword.Db2_on_Cloud_short}} na instância do
{{site.data.keyword.mobilefoundation_short}}, não será possível
mudá-la.

  2.  Crie e inicie o servidor.

      1. Crie uma instância de servidor do {{site.data.keyword.mobilefirst_notm}} com a configuração padrão, clique em **Iniciar servidor básico**.

      + Esta seleção provisiona um {{site.data.keyword.mfserver_long_notm}} com as configurações a seguir:
          -  Dois nós com 1 GB de memória cada. Esse tamanho é bom para desenvolvimento, atividades de teste moderado e cargas de trabalho de produção em pequena escala.

          -	O `username` e a `password` são gerados automaticamente para você. Você tem acesso a eles quando o servidor está funcionando.

          O processo de fornecimento do servidor inicia. Esse processo leva aproximadamente 10 minutos e uma
janela de mensagem indica o progresso dessa operação. Quando completo, um painel é exibido
no qual é possível ver:
            -	O status do servidor que está em execução (estado, tamanho).
            -	A rota do servidor é criada para você. Use esta rota em seu aplicativo móvel para se
conectar ao {{site.data.keyword.mfserver_short_notm}}.
            -	Seu `nome do usuário` e `senha` para acessar o {{site.data.keyword.mfp_oc_short_notm}}. A `password` fica oculta. Clique
no ícone **Mostrar senha** para visualizá-lo.

      +	Clique em **Ativar console** para abrir o {{site.data.keyword.mfp_oc_short_notm}}.      

      Para criar uma instância de servidor do {{site.data.keyword.mobilefirst_notm}} com configuração avançada para topologia, segurança e outra configuração do servidor, clique em **Iniciar servidor com configuração avançada**. Consulte [Instalando a configuração avançada](c_using_mfs_p4.html#using_mfs_advanced_p4), para obter mais informações.
      {: tip}

### Para {{site.data.keyword.mobilefoundation_short}}: plano Aplicativo 1 do profissional
{: #buildchannelprof1appplan}

Depois de criar uma instância do serviço {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application, é possível iniciar a construção de seu canal móvel concluindo as etapas a seguir.

  1.  Conecte-se a um serviço existente do {{site.data.keyword.Db2_on_Cloud_long}} no {{site.data.keyword.Bluemix_notm}}.

      1.  Selecione a {{site.data.keyword.Bluemix_notm}} `Organização` na qual a instância do serviço {{site.data.keyword.Db2_on_Cloud_short}} existe.

      + Selecione {{site.data.keyword.Bluemix_notm}} `Space` em que a instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}} existe, na lista de espaços disponíveis na
`Organization` selecionada.

      + Selecione {{site.data.keyword.Db2_on_Cloud_short}} `Service name` e `Credentials` para se conectar à instância de serviço existente do {{site.data.keyword.Db2_on_Cloud_short}}.

      + Teste a conexão com a instância de serviço selecionada do {{site.data.keyword.Db2_on_Cloud_short}} clicando em **Testar conexão**.

      + Clique em **Incluir**, seguido de **Continuar**, na janela pop-up que solicita confirmação no serviço {{site.data.keyword.Db2_on_Cloud_short}} selecionado. Essa ação cria as tabelas necessárias na instância de serviço de banco de dados {{site.data.keyword.Db2_on_Cloud_short}} configurada.

      > **Nota:** após incluir uma conexão do
{{site.data.keyword.Db2_on_Cloud_short}} na instância do
{{site.data.keyword.mobilefoundation_short}}, não será possível
mudá-la.

  2.  Crie e inicie o servidor.

      1. Crie uma instância de servidor do {{site.data.keyword.mobilefirst_notm}} com a configuração padrão, clique em **Iniciar servidor básico**.

        `A instância do servidor básica inclui um único nó com 1 GB de memória.`

      + O `username` e a `password` são gerados automaticamente para
você. Você tem acesso a eles quando o servidor está funcionando.  

        O processo de fornecimento do servidor inicia. Esse processo leva aproximadamente 10 minutos e uma
janela de mensagem indica o progresso dessa operação. Quando completo, um painel é exibido
no qual é possível ver:
          -	O status do servidor que está em execução (estado, tamanho).
          -	A rota do servidor é criada para você. Use esta rota em seu aplicativo móvel para se
conectar ao {{site.data.keyword.mfserver_short_notm}}.
          -	Seu `nome do usuário` e `senha` para acessar o {{site.data.keyword.mfp_oc_short_notm}}. A `password` fica oculta. Clique
no ícone **Mostrar senha** para visualizá-lo.

      +  Clique em **Ativar console** para abrir o {{site.data.keyword.mfp_oc_short_notm}}.  

      Para criar uma instância de servidor do {{site.data.keyword.mobilefirst_notm}} com configuração avançada para topologia, segurança e outra configuração do servidor, clique em **Iniciar servidor com configuração avançada**. Consulte [Instalando a configuração avançada](c_using_mfs_p2.html#using_mfs_advanced_p2), para obter mais informações.
      {: tip}

Acesse [Usando o serviço do Mobile Foundation para configurar o MobileFirst Server ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/bluemix/using-mobile-foundation/){: new_window} para saber mais sobre a introdução ao {{site.data.keyword.mobilefoundation_short}}.
{: tip}

## Etapa 3: registrar o seu aplicativo no {{site.data.keyword.mobilefoundation_short}}
{: #registerapp}

Após criar e iniciar a sua instância do servidor do Mobile Foundation, é possível seguir as etapas abaixo para registrar um aplicativo Android.

  1.  Ative o {{site.data.keyword.mfp_oc_short_notm}} carregando a URL: http://your-server-host:server-port/mfpconsole. Use o `username` e `password` gerados no momento do fornecimento.

  + No {{site.data.keyword.mfp_oc_short_notm}} **Painel**, clique em **Novo** próximo a **Aplicativos**.

  + Forneça o *MFPStarterAndroid* como o **Nome do aplicativo**.

  + **Escolha a plataforma** para ser *Android*.

  + Forneça *com.ibm.mfpstarterandroid* como o **Identificador do aplicativo**.

  + Insira *1.0* como a **Versão**.

  + Clique em **Registrar aplicativo**.

## Etapa 4: faça download do aplicativo de amostra
{: #downloadapp}

  1.  No {{site.data.keyword.mfp_oc_short_notm}} **Painel**, selecione **MFPStarterAndroid** sob **Aplicativos**.

  + Clique em **Obter código de início** e selecione para fazer download da amostra do aplicativo Android.

## Etapa 5: editar o aplicativo de amostra
{: #editapp}

  1. Importe o app Android de amostra transferido por download da etapa acima para o Android Studio.

  + No menu da barra lateral **Projeto** no Android Studio, selecione o arquivo **app → java → com.ibm.mfpstarterandroid → ServerConnectActivity.java**.

    * Inclua as importações a seguir
      ```java
      import java.net.URI;
      import java.net.URISyntaxException;
      import android.util.Log;
      ```
      {: codeblock}

    * Cole o fragmento de código a seguir, substituindo a chamada para `WLAuthorizationManager.getInstance().obtainAccessToken`

        ```java
          WLAuthorizationManager.getInstance().obtainAccessToken("", new WLAccessTokenListener() {
            @Override
            public void onSuccess(AccessToken token) {
                System.out.println("Received the following access token value: " + token);
                runOnUiThread(new Runnable() {
                    @Override public void run() {
                        titleLabel.setText("Yay!");
                        connectionStatusLabel.setText("Connected to MobileFirst Server");
                    }
                });

                URI adapterPath = null;
                try {
                    adapterPath = new URI("/adapters/javaAdapter/resource/greet");
                } catch (URISyntaxException e) {
                    e.printStackTrace();
                }

                WLResourceRequest request = new WLResourceRequest(adapterPath, WLResourceRequest.GET);

                request.setQueryParameter("name","world");
                request.send(new WLResponseListener() {
                    @Override
                    public void onSuccess(WLResponse wlResponse) {
                        // Will print "Hello world" in LogCat.
                        Log.i("MobileFirst Quick Start", "Success: " + wlResponse.getResponseText());
                    }

                    @Override public void onFailure(WLFailResponse wlFailResponse) {
                        Log.i("MobileFirst Quick Start", "Failure: " + wlFailResponse.getErrorMsg());
                    }
                });
            }

            @Override public void onFailure(WLFailResponse wlFailResponse) {
                System.out.println("Did not receive an access token from server: " + wlFailResponse.getErrorMsg());
                runOnUiThread(new Runnable() {
                    @Override public void run() {
                        titleLabel.setText("Bummer...");
                        connectionStatusLabel.setText("Failed to connect to MobileFirst Server");
                    }
                });
            }
        });
        ```
        {: codeblock}  

## Etapa 6: implementar um adaptador
{: #deployadapter}

  1. Faça download deste [artefato do adaptador](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/javaAdapter.adapter){: download} e implemente-o por meio do {{site.data.keyword.mfp_oc_short_notm}} usando **Ações → Implementar adaptador**.

## Etapa 7: testar o aplicativo
{: #testapp}

  1. No Android Studio, no menu da barra lateral **Projeto**, selecione o arquivo **app → src → principais →ativos → mfpclient.properties** e edite as propriedades `protocol`, `host` e `port` com os valores corretos para o seu MobileFirst Server.

   Os valores são normalmente https, *o endereço do seu servidor* e 443.
   {: tip}

  2. Clique em **Executar app** no Android Studio.
     * Você verá o app ativado em um emulador de dispositivo.
     * Clique no botão **Ping MobileFirst Server** em seu aplicativo ativado. Isso exibirá `Connected to MobileFirst Server`.
     * Se o aplicativo foi capaz de se conectar ao MobileFirst Server, ocorrerá uma chamada de solicitação de recursos usando o adaptador Java implementado.
     * A resposta do adaptador será impressa na visualização LogCat do Android Studio.


## Próximas etapas
{: #nextsteps}

É possível seguir os [Tutoriais de iniciação rápida![Ícone de link externo](../../icons/launch-glyph.svg "Tutoriais de iniciação rápida")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/){: new_window} para trabalhar com mais aplicativos de amostra e explorar o funcionamento do {{site.data.keyword.mobilefoundation_short}}. A Iniciação Rápida tem tutoriais que explicam o funcionamento do {{site.data.keyword.mobilefoundation_short}} para apps iOS, Android, Web, Cordova, Windows e Xamarin.

# Links relacionados
{: #rellinks  notoc}

## Links relacionados
{: #general notoc}

*	[Documentação do produto IBM MobileFirst Platform Foundation V8.0.0![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}
*	[IBM MobileFirst Platform Developer Center![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mobilefirstplatform.ibmcloud.com){: new_window}
