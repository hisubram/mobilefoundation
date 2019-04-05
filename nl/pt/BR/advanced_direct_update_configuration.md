---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-14"

keywords: Direct Update, CDN support, secure direct update

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Configuração avançada do Direct Update
{: #advanced_direct_update_configuration}

Aqui estão descritas algumas das maneiras avançadas nas quais é possível configurar e trabalhar com o recurso Direct Update.

## Customizando a UI da Atualização Direta
{: #customize_du_ui}

A UI da atualização direta apresentada ao usuário final pode ser customizada.
Inclua o seguinte na função `wlCommonInit()` em **index.js**:
```JavaScript
Wl_DirectUpdateChallengeHandler.handleDirectUpdate = function (directUpdateData, directUpdateContext) {
    // Implement custom Direct Update logic
};
```
{: codeblock}

*directUpdateData* é um objeto JSON contendo a propriedade downloadSize que representa o tamanho do arquivo (em bytes) do pacote de atualização a ser transferido por download do servidor Mobile Foundation.
*directUpdateContext* é um objeto JavaScript que expõe as funções .start() e .stop(), que iniciam e param o fluxo da atualização direta.

Se os recursos da web forem mais novos no servidor Mobile Foundation que no aplicativo, os dados de desafio de Direct Update serão incluídos na resposta do servidor. Sempre que a estrutura do lado do cliente do Mobile Foundation detecta esse desafio de atualização direta, ela chama a função `wl_directUpdateChallengeHandler.handleDirectUpdate`.

A função fornece um design da atualização direta padrão: um diálogo de mensagem padrão que é exibido quando uma atualização direta está disponível e uma tela de progresso padrão que é exibida quando o processo de atualização direta é iniciado. É possível implementar o comportamento customizado da interface com o usuário do Direct Update ou customizar a caixa de diálogo Direct Update, substituindo essa função e implementando sua própria lógica.

No código de exemplo abaixo, uma função `handleDirectUpdate` implementa uma mensagem personalizada na caixa de diálogo da atualização direta. Inclua esse código no arquivo `www/js/index.js` do projeto Cordova.
Exemplos adicionais para uma UI customizada atualização direta:
* Um diálogo que é criado usando uma estrutura de JavaScript de terceiros (como Dojo ou jQuery Mobile, Ionic,...)
* UI completamente nativa ao executar um plug-in Cordova
* Um HTML alternativo que é apresentado para o usuário com opções e assim por diante.

```JavaScript
Wl_directUpdateChallengeHandler.handleDirectUpdate = function (directUpdateData, directUpdateContext) {        
    Navigator.notification.confirm (// Cria um diálogo.
        'Custom dialog body text',
        // Handle dialog buttons.
          directUpdateContext.start();
        },
        'Custom dialog title text',
        ['Update']
    );
};
```
{: codeblock}

É possível iniciar o processo da atualização direta executando o método `directUpdateContext.start()` sempre que o usuário clicar no botão de diálogo. A tela de progresso padrão, que se assemelha àquela em versões anteriores do servidor do Mobile Foundation, é mostrada.

Esse método suporta os seguintes tipos de chamada:
* Quando nenhum parâmetro for especificado, o servidor Mobile Foundation usará a tela de progresso padrão.
* Quando uma função do listener como `directUpdateContext.start(directUpdateCustomListener)` é fornecida, o processo da atualização direta é executado em segundo plano enquanto o processo envia eventos de ciclo de vida para o listener. O listener customizado deve implementar os métodos a seguir:

```JavaScript
Var directUpdateCustomListener = {
    onStart : function ( totalSize ){ },
    onProgress : function ( status , totalSize , completedSize ){ },
    onFinish : function ( status ){ }
};
```
{: codeblock}

Os métodos do listener são iniciados durante o processo de atualização direta de acordo com as regras a seguir:
* `onStart` é chamado com o parâmetro `totalsize` que retém o tamanho do arquivo de atualização.
* `onProgress` é chamado várias vezes com o status `DOWNLOAD_IN_PROGRESS`, `totalsize` e `completedSize` (o volume que é transferido por download até o momento).
* `onProgress` é chamado com o status `UNZIP_IN_PROGRESS`.
* `onFinish` é chamado com um dos seguintes códigos de status final:

| Código de Status | Descrição |
|:------------|:------------|
| `SUCCESS` | Atualização Direta terminou sem erros. |
| `CANCELED` | A atualização direta foi cancelada (por exemplo, porque o método `stop()` foi chamado). |
| `FAILURE_NETWORK_PROBLEM` | Houve um problema com uma conexão de rede durante a atualização. |
| `FAILURE_DOWNLOADING` | O arquivo não foi transferido completamente. |
| `FAILURE_NOT_ENOUGH_SPACE` | Não há espaço suficiente no dispositivo para fazer download e descompactar o arquivo de atualização. |
| `FAILURE_UNZIPPING` | Houve um problema ao descompactar o arquivo de atualização. |
| `FAILURE_ALREADY_IN_PROGRESS` | O método de início foi chamado enquanto a atualização direta já estava em execução. |
| `FAILURE_INTEGRITY` | A autenticidade do arquivo de atualização não pode ser verificada. |
| `FAILURE_UNKNOWN` | Erro interno inesperado. |

Se você implementar um listener de atualização direta customizada, deverá assegurar que o aplicativo seja recarregado quando o processo de atualização direta for concluído e o método `onFinish()` tiver sido chamado. Também deve-se chamar `wl_directUpdateChalengeHandler.submitFailure()` se o processo de atualização direta falhar ao concluir com sucesso.

O exemplo a seguir mostra uma implementação de um listener de atualização direta customizada:
```JavaScript
Var directUpdateCustomListener = {
  onStart: function(totalSize){
    //show custom progress dialog
  },
  onProgress: function(status,totalSize,completedSize){
    //update custom progress dialog
  },
  onFinish: function(status){

    if (status == 'SUCCESS'){
      //show success message
      WL.Client.reloadApp();
    }
    else {
      Mensagem de erro customizada // show

      //submitFailure must be called is case of error
      wl_directUpdateChallengeHandler.submitFailure();
    }
  }
};

Wl_directUpdateChallengeHandler.handleDirectUpdate = function (directUpdateData, directUpdateContext) {

  WL.SimpleDialog.show('Update Avalible', 'Press update button to download version 2.0', [{
    text : 'update',
    handler : function() {
      DirectUpdateContext.start (directUpdateCustomListener); }
  }]);
};
```
{: codeblock}

## Executando atualizações diretas sem UI
{: #scenario-running-ui-less-direct-updates }
O {{site.data.keyword.mobilefoundation_short}} suporta atualização direta sem UI quando o aplicativo está no primeiro plano.

Para executar atualizações diretas sem UI, implemente `directUpdateCustomListener`. Forneça as implementações de função vazias para os métodos `onStart` e `onProgress`. As implementações vazias fazem com que o processo de atualização direta execute em segundo plano.

Para concluir o processo de atualização direta, o aplicativo deve ser recarregado. As opções a seguir estão disponíveis:
* O método `onFinish` pode ser vazio também. Nesse caso, a atualização direta será aplicada após a reinicialização do aplicativo.
* É possível implementar um diálogo customizado que informa ou requer que o usuário reinicie o aplicativo. (Consulte o exemplo a seguir.)
* O método `onFinish` pode impor um recarregamento do aplicativo chamando `WL.Client.reloadApp()`.

Aqui está um exemplo de implementação do `directUpdateCustomListener`:

```JavaScript
Var directUpdateCustomListener = {
  OnStart: function (totalsize) {
  },
  onProgress: function(status,totalSize,completeSize){
  },
  onFinish: function(status){
    WL.SimpleDialog.show('New Update Available', 'Press reload button to update to new version', [ {
      text : WL.ClientMessages.reload,
      handler : WL.Client.reloadApp
    }]);
  }
};
```
{: codeblock}

Implemente o `wl_directUpdateChallengeHandler.handleDirectUpdate` da função. Passe a implementação
`directUpdateCustomListener` que você criou como um parâmetro para a função. Certifique-se de que `directUpdateContext.start (directUpdateCustomListener`) é chamado. Aqui está um exemplo `wl_directUpdateChallengeHandler.handleDirectUpdate` implementação:

```JavaScript
Wl_directUpdateChallengeHandler.handleDirectUpdate = function (directUpdateData, directUpdateContext) {

  DirectUpdateContext.start (directUpdateCustomListener); };
```
{: codeblock}

Quando o aplicativo é enviado para o segundo plano, o processo de atualização direta é suspenso.
{: note}

## Manipulando uma falha de atualização direta
{: #scenario-handling-a-direct-update-failure }
Esta seção demonstra como manipular uma falha de atualização direta que pode ocorrer, por exemplo, por perda de conectividade. Nesse cenário, o usuário é impedido de usar o aplicativo mesmo no modo off-line. Um diálogo é exibido oferecendo ao usuário a opção para tentar novamente.

1.  Crie uma variável global para armazenar o contexto da atualização direta para poder usá-lo posteriormente quando o processo de atualização direta falhar. Por exemplo:
    ```JavaScript
    var savedDirectUpdateContext;
    ```
    {: codeblock}

2.  Implemente um manipulador de desafios atualização direta. Salve o contexto atualização direta aqui. Por exemplo:
    ```JavaScript
    wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

      savedDirectUpdateContext = directUpdateContext; // save direct update context

      Var downloadSizeInMB = (directUpdateData.downloadSize / 1048576) .toFixed (1) .replace (".", WL.App.getDecimalSeparator());
  var directUpdateMsg = WL.Utils.formatString(WL.ClientMessages.directUpdateNotificationMessage, downloadSizeInMB);

      WL.SimpleDialog.show (WL.ClientMessages.directUpdateNotificationTitle, directUpdateMsg, [ {
        text : WL.ClientMessages.update,
    handler : function() {
          DirectUpdateContext.start (directUpdateCustomListener); }
      }]);
    };
    ```
    {: codeblock}

3.  Crie uma função que inicia o processo de atualização direta usando o contexto de atualização direta. Por exemplo:
    ```JavaScript
    restartDirectUpdate = function () {
      savedDirectUpdateContext.start(directUpdateCustomListener); // use saved direct update context to restart direct update
};
    ```
    {: codeblock}

4.  Implemente o `directUpdateCustomListener`. Inclua a verificação de status no método `onFinish`. Se o status for iniciado com `FAILURE`, abra um diálogo somente modal com a opção **Tentar novamente**. Por exemplo:
    ```JavaScript
    var directUpdateCustomListener = {
      onStart: function(totalSize){
    alert('onStart: totalSize = ' + totalSize + 'Byte');
  },
  onProgress: function(status,totalSize,completeSize){
    alert('onProgress: status = ' + status + ' completeSize = ' + completeSize + 'Byte');
  },
  onFinish: function(status){
    alert('onFinish: status = ' + status);
    var pos = status.indexOf("FAILURE");
    if (pos > -1) {
          WL.SimpleDialog.show('Update Failed', 'Press try again button', [ {
            text : "Try Again",
        handler : restartDirectUpdate // restart direct update
      }]);
    }
      }
    };
    ```
    {: codeblock}

    Quando o usuário clica no botão **Tentar novamente**, o aplicativo reinicia o processo de atualização direta.
    {: note}

## Delta e Full Direct Update
{: #delta-and-full-direct-update }
Atualizações diretas delta permitem que um aplicativo faça download apenas dos arquivos que foram mudados desde a última atualização, em vez dos recursos da web inteiros do aplicativo. Isso reduz o tempo de download, preserva a largura da banda e
melhora a experiência geral do usuário.

Uma **atualização delta** será possível somente se os recursos da web do aplicativo cliente estiverem uma versão atrás do aplicativo que está atualmente implementado no servidor. Os aplicativos clientes que sejam de mais de uma versão
anterior à do aplicativo atualmente implementado (significando que o aplicativo foi implementado no servidor pelo menos duas vezes desde que o
aplicativo cliente foi atualizado), recebem uma **atualização completa** (o que significa que os recursos da web
inteiros são transferidos por download e atualizados).
{: note}

Veja a amostra de Direct Update para o app Cordova na seção **Amostras**. Esse aplicativo demonstra como criar um diálogo customizado de Direct Update em vez do diálogo fornecido por padrão.  

## Suporte CDN
{: #cdn_support}

É possível configurar solicitações de Direct Update para serem entregues por meio de um CDN (rede de entrega de conteúdo) em vez do servidor Mobile Foundation.

O uso de um CDN em vez do servidor Mobile Foundation para entregar solicitações de Direct Update tem as vantagens a seguir:

* Remove as sobrecargas de rede do servidor Mobile Foundation.
* Aumenta as taxas de transferência mais altas do que o limite de 250 MB/segundos ao entregar solicitações de um servidor Mobile Foundation.
* Assegura uma experiência mais uniforme da atualização direta para todos os usuários, independentemente da sua localização geográfica.

### Requisitos gerais
{: #general-requirements }
Para atender às solicitações da atualização direta de um CDN, assegure-se de que a sua configuração esteja em conformidade com as seguintes condições:

* O CDN deve ser um proxy reverso na frente do servidor Mobile Foundation (ou na frente de outro proxy reverso, se necessário).
* Ao construir o aplicativo por meio de seu ambiente de desenvolvimento, configure o seu servidor de destino para o host e a porta do CDN em vez de o host e a porta do servidor Mobile Foundation. Por exemplo, ao executar o comando da CLI do Mobile Foundation `mfpdev server add`, forneça o host e a porta do CDN.
* No painel de administração do CDN, você precisa marcar as URLs de Direct Update a seguir para armazenamento em cache para assegurar que o CDN passe todas as solicitações para o servidor Mobile Foundation, exceto as solicitações de Direct Update. Para solicitações da atualização direta, o CDN determina se ele obteve o conteúdo. Se tiver, ele retornará sem acessar o servidor Mobile Foundation; se não, ele acessará o servidor Mobile Foundation, obterá o archive de Direct Update (arquivo .zip) e o armazenará para as próximas solicitações para essa URL específica. Para aplicativos que são construídos com a v8.0 de {{site.data.keyword.mobilefoundation_short}}, a URL da atualização direta é: `PROTOCOL://DOMAIN:PORT/CONTEXT_PATH/api/directupdate/VERSION/CHECKSUM/TYPE`.
O prefixo `PROTOCOL://DOMAIN:PORT/CONTEXT_PATH` é constante para todas as solicitações de tempo de execução. Por
exemplo: `http://my.cdn.com:9080/mfp/api/directupdate/0.0.1/742914155/full?appId=com.ibm.DirectUpdateTestApp&clientPlatform=android`

No exemplo, há parâmetros de solicitação adicionais que também fazem parte da solicitação.

* O CDN deve permitir o armazenamento em cache dos parâmetros de solicitação. Dois diferentes archives da atualização direta podem diferir apenas pelos parâmetros de solicitação.
* O CDN deve suportar TTL na resposta da atualização direta. O suporte é necessário para suportar várias atualizações diretas para a mesma versão.
* O CDN não deve mudar ou remover os cabeçalhos HTTP que são usados no protocolo do cliente do servidor.

### Exemplo de configuração CDN
{: #example-cdn-configuration }
Esse exemplo é baseado no uso de uma configuração de CDN do Akamai que armazena em cache o archive da atualização direta. As tarefas a seguir são concluídas pelo administrador da rede, pelo administrador do Mobile Foundation e pelo administrador do Akamai:

#### Administrador de rede
{: #network-administrator }
Crie outro domínio no DNS para o servidor Mobile Foundation. Por exemplo, se o seu servidor de domínio for
`yourcompany.com`, será necessário criar um domínio adicional, como
`cdn.yourcompany.com`.
No DNS para o novo domínio `cdn.yourcompany.com`, configure um `CNAME` para o nome de
domínio que é fornecido pelo Akamai. Por exemplo, `yourcompany.com.akamai.net`.

#### Administrador do Mobile Foundation
{: #mobilefoundation-administrator }
Configure o novo domínio `cdn.yourcompany.com` como a URL do servidor Mobile Foundation para os aplicativos do Mobile Foundation. Por exemplo, para a tarefa do construtor Ant, a propriedade é:
```xml
<property name="wl.server" value="http://cdn.yourcompany.com/${contextPath}/"/>
```
{: codeblock}

#### Administrador Akamai
{: #akamai-administrator }
1. Abra o gerente de propriedade do Akamai e configure a propriedade **host name** para o valor do novo
domínio.

    ![Configure o nome de host da propriedade para o valor do novo domínio](images/direct_update_cdn_3.jpg)

2. Na guia Regra padrão, configure o host e a porta do servidor Mobile Foundation original e configure o valor **Cabeçalho do host de encaminhamento customizado** para o domínio recém-criado.

    ![Configure o valor de Cabeçalho do host de encaminhamentocustomizado para o domínio recém-criado](images/direct_update_cdn_4.jpg)


3. Na lista **Opção de armazenamento em cache**, selecione **Sem armazenamento**.

    ![Na lista Opção de armazenamento em cache, selecione Sem armazenamento](images/direct_update_cdn_5.jpg)

4. Na guia **Configuração do conteúdo estático**, configure os critérios de correspondência de acordo com a URL de atualização direta do aplicativo. Por exemplo, crie uma condição que declare `If Path matches one of direct_update_URL`.

    ![Configure os critérios de correspondência de acordo com a URL atualização direta do aplicativo](images/direct_update_cdn_6.jpg)

5. Configure o comportamento de chave de cache para usar todos os parâmetros de solicitação na chave de cache (deve-se fazer isso para armazenar em cache diferentes archives da atualização direta para diferentes aplicativos ou versões). Por exemplo, na lista **Comportamento**, selecione `Include all parameters (preserve order from request)`.

    ![Configure o comportamento de chave de cache para usar todos os parâmetros de solicitação na chave de cache](images/direct_update_cdn_8.jpg)

6. Configure valores semelhantes aos seguintes para configurar o comportamento de armazenamento em cache para armazenar em cache a URL da atualização direta e para configurar o TTL.

      ![Defina valores para configurar o comportamento do armazenamento em cache](images/direct_update_cdn_7.jpg)

| Campos | Valor |
|:------|:------|
| Opção de Armazenamento em Cache | Cache |
| Forçar Revalorização de Objetos Antigos | Atender o antigo se for impossível validar |
| Max-Age | 3 minutes |

## Seguro Direct Update
{: #secure-dc }

Desativado por padrão, o Direct Update seguro evita que um invasor de terceiros mude os recursos da Web que são transmitidos do servidor Mobile Foundation (ou de uma Content Delivery Network (CDN)) para o aplicativo cliente.

**Para ativar a autenticidade Atualização Direta:**  
Usando uma ferramenta preferencial, extraia a chave pública do keystore do servidor Mobile Foundation e converta-a em base64.  
O valor produzido deve então ser usado como instruído abaixo:

1. Abra uma janela de **Linha de comandos** e navegue até a raiz do projeto Cordova.
2. Execute o comando: `mfpdev app config` e selecione a opção **Chave pública de autenticidade da atualização direta**.
3. Forneça a chave pública e confirmar.

Quaisquer futuras entregas da atualização direta para os aplicativos clientes serão protegidas pela autenticidade dele.

Para que o Direct Update seguro funcione, um arquivo keystore definido pelo usuário deve ser implementado no servidor Mobile Foundation e uma cópia da chave pública correspondente deve ser incluída no aplicativo cliente implementado.

Esse tópico descreve como ligar uma chave pública a aplicativos clientes novos e existentes que
foram atualizados. Para obter mais informações sobre como configurar o keystore no servidor Mobile Foundation, veja [Configurando o keystore do servidor Mobile Foundation ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/configuring-the-mobilefirst-server-keystore/){: new_window}

O servidor fornece um keystore integrado que pode ser usado para testar a atualização direta segura para as fases de desenvolvimento.

Depois de ligar a chave pública ao aplicativo cliente e reconstruí-lo, não é necessário fazer upload dele novamente para o {{ site.data.keys.mf_server }}. No entanto, se você publicou anteriormente o aplicativo para o mercado sem a chave pública, deverá publicá-lo novamente.
{: note}

Para propósitos de desenvolvimento, a chave pública simulada padrão a seguir é fornecida com o servidor Mobile Foundation:

```xml
-----BEGIN PUBLIC KEY-----
MIIDPjCCAiagAwIBAgIEUD3/bjANBgkqhkiG9w0BAQsFADBgMQswCQYDVQQGEwJJTDELMAkGA1UECBMCSUwxETA
PBgNVBAcTCFNoZWZheWltMQwwCgYDVQQKEwNJQk0xEjAQBgNVBAsTCVdvcmtsaWdodDEPMA0GA1UEAxMGV0wgRG
V2MCAXDTEyMDgyOTExMzkyNloYDzQ3NTAwNzI3MTEzOTI2WjBgMQswCQYDVQQGEwJJTDELMAkGA1UECBMCSUwxE
TAPBgNVBAcTCFNoZWZheWltMQwwCgYDVQQKEwNJQk0xEjAQBgNVBAsTCVdvcmtsaWdodDEPMA0GA1UEAxMGV0wg
RGV2MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzQN3vEB2/of7KAvuvyoIt0T7cjaSTjnOBm0N3+q
zx++dh92KpNJXj/a3o4YbwJXkJ7jU8ykjCYvjXRf0hme+HGhiIVwxJo54iqh76skDS5m7DaseFdndZUJ4p7NFVw
I5ixA36ZArSZ/Pn/ej56/RRjBeRI7AEGXUSGojBUPA6J6DYkwaXQRew9l+Q1kj4dTigyKL5Os0vNFaQyYu+bT2E
vnOixQ0DXm94IqmHZamZKbZLrWcOEfuAsSjKYOdMSM9jkCiHaKcj7fpEZhUxRRs7joKs1Ri4ihs6JeUvMEiG4gK
l9V3FP/Huy0pfkL0F8xMHgaQ4c/lxS/s3PV0OEg+7wIDAQABMA0GCSqGSIb3DQEBCwUAA4IBAQAgEhhqRl2Rgkt
MJeqOCRcT3uyr4XDK3hmuhEaE0nOvLHi61PoLKnDUNryWUicK/W+tUP9jkN5xRckdzG6TJ/HPySmZ7Adr6QRFu+
xcIMY+/S8j4PHLXBjoqgtUMhkt7S2/thN/VA6mwZpw4Ol0Pa2hyT2TkhQoYYkRwYCk9pxmuBCoH/eCWpSxquNny
RwrY25x0YzccXUaMI8L3/3hzq3mW40YIMiEdpiD5HqjUDpzN1funHNQdsxEIMYsWmGAwOdV5slFzyrH+ErUYUFA
pdGIdLtkrhzbqHFwXE0v3dt+lnLf21wRPIqYHaEu+EB/A4dLO6hm+IjBeu/No7H7TBFm
-----END PUBLIC KEY-----
```
{: codeblock}

Não use a chave pública para propósitos de produção.
{: note}

### Gerando e Implementando o keystore
{: #generating-and-deploying-the-keystore }
Há muitas ferramentas disponíveis para gerar certificados e extrair chaves públicas de um keystore. O exemplo a seguir demonstra os
procedimentos com o utilitário keytool JDK e o openSSL.

1. Extraia a chave pública do arquivo keystore que é implementado no {{ site.data.keys.mf_server }}.  
   A chave pública deve ser codificada em Base64.
   {: note}

   Por exemplo, suponha que o nome do alias seja `mfp-server` e que o arquivo keystore seja
**keystore.jks**.  
   Para gerar um certificado, emita o seguinte comando:

   ```bash
   keytool -export -alias mfp-server -file certfile.cert
   -keystore keystore.jks -storepass keypassword
   ```
   {: codeblock}

   Um arquivo de certificado é gerado.  
   Emita o seguinte comando para extrair a chave pública:

   ```bash
   openssl x509 -inform der -in certfile.cert -pubkey -noout
   ```
   {: codeblock}

   O keytool sozinho não pode extrair chaves públicas no formato Base64.
   {: note}

2. Execute um dos seguintes procedimentos:
    * Copie o texto resultante, sem os marcadores `BEGIN PUBLIC KEY` e `END PUBLIC KEY`
no arquivo de propriedade mfpclient do aplicativo, imediatamente após `wlSecureDirectUpdatePublicKey`.
    * No prompt de comandos, emita o seguinte comando: `mfpdev app config direct_update_authenticity_public_key <public_key>`

    Para `<public_key>`, cole o texto que resulta da Etapa 1, sem os marcadores `BEGIN PUBLIC
KEY` e `END PUBLIC KEY`.

3. Execute o comando de construção cordova para salvar a chave pública no aplicativo.
