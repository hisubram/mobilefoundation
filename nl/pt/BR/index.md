---

copyright:
  years: 2016, 2019
lastupdated:  "2018-11-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

# Tutorial de Introdução
{: #gettingstartedtemplate}

O {{site.data.keyword.mobilefoundation_long}} expede a configuração de um ambiente do {{site.data.keyword.mfp_full}} por meio do qual é possível desenvolver, testar e executar apps móveis corporativos. O {{site.data.keyword.mobilefoundation_short}} oferece os diferentes planos de serviços a seguir: Developer, Professional Per Device e Professional 1 Application.
{: shortdesc}

Usando o plano Professional 1 Application, um único aplicativo construído em qualquer um dos sistemas operacionais suportados pode ser gerenciado. Os sistemas operacionais suportados são Android, iOS, Windows ou web móvel. O plano do Desenvolvedor é mais adequado para desenvolvimento e teste. É possível revisar todos os planos disponíveis [aqui](https://console.bluemix.net/catalog/services/mobile-foundation).

Este tutorial de introdução permite criar uma instância de serviço do {{site.data.keyword.mobilefoundation_short}} usando um dos planos suportados. É possível, então, registrar um aplicativo. Faça download e edite o aplicativo registrado, implemente um adaptador e, finalmente, teste o aplicativo.

## Antes de iniciar
{: #prereqs}

Você precisará de uma conta do {{site.data.keyword.Bluemix}} e de uma instância do serviço {{site.data.keyword.mobilefoundation_short}}.

## Etapa 1: criar uma instância do serviço do {{site.data.keyword.mobilefoundation_short}}
{: #step1create}

1. No **catálogo** do {{site.data.keyword.Bluemix_notm}}, selecione [**{{site.data.keyword.mobilefoundation_short}}**](https://{domainName}/catalog/services/mobile-foundation). A tela de configuração de
serviço é aberta.
2. Dê à sua instância de serviço um nome ou use o nome predefinido.
3. Escolha a região, a organização e o espaço em que você gostaria de criar a instância de serviço.
4. Selecione o seu **Plano de precificação** e clique em **Criar**.

## Etapa 2: construir o seu canal móvel
{: #buildmobilechannel}

### Para {{site.data.keyword.mobilefoundation_short}}: plano do Desenvolvedor
{: #buildchanneldevplan}

Após você criar uma instância do {{site.data.keyword.mobilefoundation_short}}: Desenvolvedor, é possível iniciar a construção de seu canal móvel concluindo as etapas a seguir.

* É possível acessar instantaneamente e trabalhar com o Mobile Foundation Server.

  Essa seleção cria um {{site.data.keyword.mfserver_long_notm}} com as configurações a seguir:
  *	1 GB de memória. Este tamanho é suficiente para desenvolvimento, atividades de teste leve e cargas de trabalho de produção em pequena escala.

  * Para acessar o Mobile Foundation Server usando a CLI, você precisará das credenciais, que estão disponíveis ao clicar em **Credenciais de serviço** na área de janela de navegação esquerda do console do IBM Cloud.

### Para o plano {{site.data.keyword.mobilefoundation_short}}: Professional Per Device
{: #buildchannelprofdeviceplan}

Após criar uma instância do serviço {{site.data.keyword.mobilefoundation_short}}: Professional Per Device, é possível iniciar a construção do seu canal de modelo completando as etapas a seguir.

  1.  Conecte-se a um serviço {{site.data.keyword.Db2_on_Cloud_short}} (qualquer plano diferente do plano **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} existente no {{site.data.keyword.Bluemix_notm}}.

      1.  Selecione a `Organization` do {{site.data.keyword.Bluemix_notm}} na qual a instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}} (qualquer plano, exceto o **Lite**) ou do {{site.data.keyword.composeForPostgreSQL}} existe.

      + Selecione o `Space` do {{site.data.keyword.Bluemix_notm}} no qual a instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}} (qualquer plano, exceto o **Lite**) ou do {{site.data.keyword.composeForPostgreSQL}} existe, na lista de espaços disponíveis na `Organization` selecionada.

      + Selecione o {{site.data.keyword.Db2_on_Cloud_short}} (qualquer plano, exceto o **Lite**) ou o `Service Name` e as `Credentials` do {{site.data.keyword.composeForPostgreSQL}} para se conectar à instância de serviço existente do {{site.data.keyword.Db2_on_Cloud_short}} (qualquer plano, exceto o **Lite**) ou do {{site.data.keyword.composeForPostgreSQL}}.

      + Teste a conexão com a instância de serviço selecionada do {{site.data.keyword.Db2_on_Cloud_short}} (qualquer plano, exceto o **Lite**) ou do {{site.data.keyword.composeForPostgreSQL}} clicando em **Testar conexão**.

      + Clique em **Incluir** seguido por **Continuar** na janela pop-up que solicita a confirmação no serviço {{site.data.keyword.Db2_on_Cloud_short}} (qualquer plano diferente do plano **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} selecionado. Essa ação cria as tabelas necessárias na instância de serviço de banco de dados configurada do {{site.data.keyword.Db2_on_Cloud_short}} (qualquer plano, exceto o **Lite**) ou do {{site.data.keyword.composeForPostgreSQL}}.

      Depois de incluir uma conexão do {{site.data.keyword.Db2_on_Cloud_short}} (qualquer plano diferente do plano **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} na instância do {{site.data.keyword.mobilefoundation_short}}, não será possível mudá-la.
      {: note} 
  2.  Crie e inicie o servidor.

      1. Crie uma instância de servidor do {{site.data.keyword.mobilefirst_notm}} com a configuração padrão, clique em **Iniciar servidor básico**.

      + Esta seleção provisiona um {{site.data.keyword.mfserver_long_notm}} com as configurações a seguir:
          - Dois nós com 1 GB de memória cada um. Esse tamanho é bom para desenvolvimento, atividades de teste moderado e cargas de trabalho de produção em pequena escala.

          -	O `username` e a `password` são gerados automaticamente para você. Você tem acesso a eles quando o servidor está funcionando.

          O processo de criação do servidor é iniciado. Esse processo leva aproximadamente 10 minutos e uma
janela de mensagem indica o progresso dessa operação. Quando completo, um painel é exibido
no qual é possível ver:
            -	O status do servidor que está em execução (estado, tamanho).
            -	A rota do servidor é criada para você. Use esta rota em seu aplicativo móvel para se
conectar ao {{site.data.keyword.mfserver_short_notm}}.
            -	Seu `nome do usuário` e `senha` para acessar o {{site.data.keyword.mfp_oc_short_notm}}. A `password` fica oculta. Clique
no ícone **Mostrar senha** para visualizá-lo.

      +	Clique em **Ativar console** para abrir o {{site.data.keyword.mfp_oc_short_notm}}.      

      Para criar uma instância de servidor do {{site.data.keyword.mobilefirst_notm}} com configuração avançada para topologia, segurança e outra configuração do servidor, clique em **Iniciar servidor com configuração avançada**. Consulte [Instalando a configuração avançada](c_using_mfs_p5.html#using_mfs_advanced_p5), para obter mais informações.
      {: tip}

### Para {{site.data.keyword.mobilefoundation_short}}: plano Aplicativo 1 do profissional
{: #buildchannelprof1appplan}

Depois de criar uma instância do serviço {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application, é possível iniciar a construção de seu canal móvel concluindo as etapas a seguir.

  1.  Conecte-se a um serviço {{site.data.keyword.Db2_on_Cloud_long}} (qualquer plano diferente do plano **Lite**) ou {{site.data.keyword.composeForPostgreSQL_full}} existente no {{site.data.keyword.Bluemix_notm}}.

      1.  Selecione a `Organization` do {{site.data.keyword.Bluemix_notm}} na qual a instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}} (qualquer plano, exceto o **Lite**) ou do {{site.data.keyword.composeForPostgreSQL}} existe.

      + Selecione o `Space` do {{site.data.keyword.Bluemix_notm}} no qual a instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}} (qualquer plano, exceto o **Lite**) ou do {{site.data.keyword.composeForPostgreSQL}} existe, na lista de espaços disponíveis na `Organization` selecionada.

      + Selecione o {{site.data.keyword.Db2_on_Cloud_short}} (qualquer plano, exceto o **Lite**) ou o `Service Name` e as `Credentials` do {{site.data.keyword.composeForPostgreSQL}} para se conectar à instância de serviço existente do {{site.data.keyword.Db2_on_Cloud_short}} (qualquer plano, exceto o **Lite**) ou do {{site.data.keyword.composeForPostgreSQL}}.

      + Teste a conexão com a instância de serviço selecionada do {{site.data.keyword.Db2_on_Cloud_short}} (qualquer plano, exceto o **Lite**) ou do {{site.data.keyword.composeForPostgreSQL}} clicando em **Testar conexão**.

      + Clique em **Incluir** seguido por **Continuar** na janela pop-up que solicita a confirmação no serviço {{site.data.keyword.Db2_on_Cloud_short}} ou {{site.data.keyword.composeForPostgreSQL}} selecionado. Essa ação cria as tabelas necessárias na instância de serviço de banco de dados configurada do {{site.data.keyword.Db2_on_Cloud_short}} (qualquer plano, exceto o **Lite**) ou do {{site.data.keyword.composeForPostgreSQL}}.

      Depois de incluir uma conexão do {{site.data.keyword.Db2_on_Cloud_short}} (qualquer plano diferente do plano **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} na instância do {{site.data.keyword.mobilefoundation_short}}, não será possível mudá-la.
      {: note}

  2.  Crie e inicie o servidor.

      1. Crie uma instância de servidor do {{site.data.keyword.mobilefirst_notm}} com a configuração padrão, clique em **Iniciar servidor básico**.

        `A instância do servidor básica inclui um único nó com 1 GB de memória.`

      + O `username` e a `password` são gerados automaticamente para
você. Você tem acesso a eles quando o servidor está funcionando.  

        O processo de criação do servidor é iniciado. Esse processo leva aproximadamente 10 minutos e uma
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
{: note}

## Etapa 3: registrar o seu aplicativo no {{site.data.keyword.mobilefoundation_short}}
{: #registerapp}

Depois de criar e iniciar a instância do servidor Mobile Foundation, é possível realizar as etapas a seguir para registrar um aplicativo Android.

  1.  Chame o {{site.data.keyword.mfp_oc_short_notm}} carregando a URL: `http://<your-server-host>:<server-port>/mfpconsole`. Use o `username` e `password` gerados no momento do fornecimento.

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

  1. Importe o app Android de amostra transferido por download, da etapa anterior, para um Android Studio.

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
     * Você verá o app iniciado em um emulador de dispositivo.
     * Clique em **Executar ping do MobileFirst Server** em seu aplicativo, isso exibirá `Connected to MobileFirst Server`.
     * Se o aplicativo foi capaz de se conectar ao MobileFirst Server, ocorrerá uma chamada de solicitação de recursos usando o adaptador Java implementado.
     * A resposta do adaptador será impressa na visualização LogCat do Android Studio.


## Próximas etapas
{: #nextsteps}

É possível seguir os [Tutoriais de iniciação rápida![Ícone de link externo](../../icons/launch-glyph.svg "Tutoriais de iniciação rápida")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/){: new_window} para trabalhar com mais aplicativos de amostra e explorar o funcionamento do {{site.data.keyword.mobilefoundation_short}}.

A Iniciação rápida tem tutoriais explicando o trabalho de apps {{site.data.keyword.mobilefoundation_short}} para iOS, Android, Web, Cordova, Windows, React Native, Ionic e Xamarin.

# Links relacionados
{: #rellinks  notoc}

## Links relacionados
{: #general notoc}

*	[Documentação do produto IBM MobileFirst Platform Foundation V8.0.0![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}
*	[IBM MobileFirst Platform Developer Center![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mobilefirstplatform.ibmcloud.com){: new_window}
