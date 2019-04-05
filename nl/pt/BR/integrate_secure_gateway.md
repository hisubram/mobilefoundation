---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-13"

keywords: integration, mobile foundation, secure gateway

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Estabelecendo uma conexão segura com um sistema local usando o serviço Secure Gateway
{: #integrate_secure_gateway}

Ao construir apps móveis corporativos, você deseja muitas vezes integrar os apps a sistemas de registro existentes. Para acessar e aproveitar dados armazenados no data center local por meio de um app móvel, é possível usar o [Mobile Foundation Service](https://cloud.ibm.com/catalog/services/mobile-foundation) com o [serviço Secure Gateway](https://cloud.ibm.com/catalog/services/secure-gateway) no [IBM Cloud](https://cloud.ibm.com/). O serviço Secure Gateway fornece conectividade rápida, fácil e segura e estabelece um túnel entre o IBM Cloud e o sistema remoto ao qual você deseja se conectar.

Este tutorial explica como acessar terminais HTTP em seu data center local por meio de adaptadores do Mobile Foundation em execução no IBM Cloud, usando o serviço Secure Gateway.

## Pré-requisito
{: #prereq_int_sec_gw}

Para concluir este tutorial, você precisará de um terminal HTTP dentro de seu firewall corporativo que exponha os sistemas de dados de registro. Como alternativa, crie um terminal de teste em seu ambiente local usando [este projeto `Node.js` de amostra ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/MobileFirst-Platform-Developer-Center/MFPSecureGatewayIonic/tree/master/NodeJSHTTPProject).

Navegue para a pasta do projeto e execute seu servidor HTTP.

```bash
npm install node app.js
```
{: codeblock}

## Cenário de integração com o serviço do Secure Gateway
{: #secure_gateway}

A imagem a seguir ilustra a arquitetura usada no cenário de integração explicado neste tutorial.

![Architecture Diagram](images/SecureGatewayArchi.png)

## Implementando a integração do Secure Gateway
{: #implementing_sg_integration}

### Criar uma instância de serviço Secure Gateway
Efetue login no IBM Cloud e crie uma instância do [serviço Secure Gateway](https://cloud.ibm.com/catalog/services/secure-gateway/).

![IBM Cloud](images/SecureGatewayInst.gif)

Após a criação da instância de serviço do Secure Gateway, siga as etapas abaixo para configurar o serviço Secure Gateway entre o IBM Cloud e o seu ambiente local.

### Incluir um Gateway
{: #add_gateway}

No painel de serviço do Secure Gateway, clique em **Incluir gateway**, para criar um novo gateway fornecendo qualquer nome de gateway desejado.

![Add Gateway](images/AcmeAddGateway.gif)


### Incluir um cliente Secure Gateway
{: #add_sg_client}

![Add Client2](images/AcmeAddClient.gif)

De dentro de seu novo gateway na guia **Clientes**, clique em **Conectar cliente**.

É possível usar qualquer um dos clientes de sua preferência e executar o cliente Secure Gateway em seu ambiente local. As etapas para configurar o cliente Secure Gateway estão disponíveis no console do Secure Gateway.

Neste tutorial, usaremos a opção de contêiner do Docker para executar o cliente Secure Gateway.
Siga as etapas abaixo:
*   Instale o Docker em sua máquina local, se ele ainda não estiver instalado.
*   Ative um terminal e execute o cliente Secure Gateway em um contêiner usando o comando mostrado no console de serviço.
    ```bash
    docker run –it ibmcom/secure-gateway-client <gatewayId>
    ```
    {: codeblock}
    `gatewayId` pode ser localizado no console, conforme mostrado na imagem acima.

### Incluir um destino
{: #add_destination}

![Add Destination](images/AcmeAddDest.gif)

De dentro de seu novo gateway na guia **Destinos**, clique em **Incluir destino**.

O serviço Secure Gateway permite que você se conecte a um terminal local por meio do ambiente do IBM Cloud ou se conecte de um ambiente local a um recurso de nuvem. Para esse cenário, selecione no local como o local do recurso e forneça o nome do host/IP e a porta do recurso local. Além disso, forneça um nome de destino de sua preferência.

Para permitir que a solicitação seja encaminhada do cliente Secure Gateway para o terminal de recurso, é necessário incluir o recurso na lista de acesso.
Execute o comando a seguir no terminal do cliente Secure Gateway (em execução como contêiner no tempo de execução do Docker, neste cenário) para incluir o recurso na lista de acesso.

```
acl allow <resourceHost>:<resourcePort>
```
{: codeblock}
`resourceHost` e `resourcePort` referem-se aos detalhes do terminal de recurso no local.

O destino está agora configurado. O serviço Secure Gateway preencherá os detalhes do host e da porta da nuvem, que podem ser usados para acessar o recurso local por meio do ambiente de nuvem.

![Destination Tab](images/AcmeCloudPopulate.gif)

### Configurando o serviço Secure Gateway com o Mobile Foundation e o Mobile Foundation Adapter
{: #configuration_sg_mfp}

Neste tutorial, usaremos a instância de serviço do Mobile Foundation no IBM Cloud para configurar o servidor Mobile Foundation. O serviço Mobile Foundation no IBM Cloud ajuda a provisionar o servidor Mobile Foundation no tempo de execução do Liberty como um aplicativo Cloud Foundry. O serviço Mobile Foundation permite usar qualquer projeto do Mobile Foundation desenvolvido em um ambiente local e executá-lo no IBM Cloud.

### Configuração do servidor Mobile Foundation no IBM Cloud
{: #mf_server_setup}

Crie uma instância do [serviço Mobile Foundation](https://cloud.ibm.com/catalog/services/mobile-foundation) por meio do console do IBM Cloud.

No console de serviço do Mobile Foundation, crie o [Servidor Mobile Foundation ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/bluemix/using-mobile-foundation/).


### Construindo e implementando o Mobile Foundation Adapter
{: #deploying_mf_adapter}

Neste tutorial, nos conectaremos ao terminal do Secure Gateway usando um adaptador do Mobile Foundation. [Faça download ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/MobileFirst-Platform-Developer-Center/Adapters/tree/release80/JavaHTTP) do adaptador JavaHTTP do Mobile Foundation.

Construa e implemente o adaptador no console do Mobile Foundation Operations usando os comandos [mfpdev-cli](/docs/services/mobilefoundation?topic=mobilefoundation-mobile_foundation_cli#mobile_foundation_cli).
```bash
mfpdev adapter build 
mfpdev adapter deploy
```
{: codeblock}

Saiba mais sobre a construção e a implementação de adaptadores [aqui ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/).
{: tip}

Forneça os detalhes do host e da porta da nuvem para o terminal de recurso no adaptador JavaHTTP obtidos na seção anterior.

![AdapterConfiguration ](images/AdapterConfiguration.png)

em que `cap-sg-prd-5.securegateway.appdomain.cloud` e `18946` são o host e a porta do Secure Gateway, respectivamente.

Agora o adaptador do Mobile Foundation está configurado e o serviço Mobile Foundation está ativado para trabalhar com um sistema local dentro da empresa usando o serviço Secure Gateway.

### Criando e registrando o app de amostra do Mobile Foundation
{: #registering_sample_app}

Faça download do app de amostra do Mobile Foundation [aqui](https://github.com/MobileFirst-Platform-Developer-Center/MFPSecureGatewayIonic/), siga as instruções no `Readme` e registre o app no console do Mobile Foundation Operations.

Execute o app, forneça credenciais para efetuar login e clique no botão *Login*. Clique no botão *Buscar gravadores da Acme* para chamar seu terminal local por meio do Secure Gateway usando o JavaHTTP Adapter implementado no console do Mobile Foundation Operations. Receba os dados desejados do ambiente local.

![App receive on-premise data](images/AcmePublishersApp.gif)

É possível conectar-se a múltiplos terminais locais, configurando diversos destinos no serviço Secure Gateway e implementando adaptadores do Mobile Foundation para se conectar ao respectivo host de nuvem do terminal. Também é possível configurar o serviço Secure Gateway com segurança adicional para garantir que a comunicação com o terminal aconteça sobre a segurança HTTPS e do lado do aplicativo. É possível localizar os  [ detalhes aqui ](/docs/services/SecureGateway?topic=securegateway-getting-started-with-sg#getting-started-with-sg).


## Resumo
{: #summary_int_sec_gw}

Usando este tutorial, você deve ser capaz de estabelecer uma conexão segura entre os adaptadores do Mobile Foundation em execução no IBM Cloud e um terminal HTTP local, usando o serviço Secure Gateway.
