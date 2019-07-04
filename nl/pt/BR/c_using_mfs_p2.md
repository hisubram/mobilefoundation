---

copyright:
  years: 2016, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation, mobile analytics, professional plan, configure database

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

#	Configurar usando o plano Professional 1 Application
{: #using_mobilefoundation_p2}

Com o plano Professional 1 Application, os usuários podem criar 1 aplicativo móvel com vários sistemas operacionais de dispositivo móvel.
Depois de criar a instância de serviço {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application, leia o procedimento a seguir para iniciar o serviço.

## Pré-requisitos para plano do Professional 1 Application
{: #prerequisites_p2}

Considere o seguinte antes de configurar a instância de serviço {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application.
* O {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application é suportado somente com os planos do {{site.data.keyword.Db2_on_Cloud_short}} e {{site.data.keyword.composeForPostgreSQL}} {{site.data.keyword.Bluemix_notm}}.

* É necessário acesso às credenciais da instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}} ou {{site.data.keyword.composeForPostgreSQL}} antes de poder definir as configurações de sua instância de serviço do {{site.data.keyword.mobilefoundation_short}}.

A instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}} (qualquer plano diferente do plano **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} pode existir em qualquer `Space` dentro de sua `Organization` do {{site.data.keyword.Bluemix_notm}} ou qualquer outra `Organization` à qual você tem acesso. Assegure-se de que tenha as permissões para acessar o `Space` no qual a instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}} ou do {{site.data.keyword.composeForPostgreSQL}} existe.
{: note}


## Configure a conexão com o banco de dados
{: #configure_dashdb_p2}

###  Primeiras etapas na configuração
{: #firststeps_p2}

Após criar a instância de serviço do {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application, siga o procedimento para iniciar.

### Configurando a conexão com a instância de serviço do DB2 on Cloud
{: #connect_dashdb_p2}

Após a instância de serviço do {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application ser criada, você verá a página *Visão geral*. Aqui, é necessário especificar as informações de conexão para a instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}} (qualquer plano diferente do plano **Lite**) ou do {{site.data.keyword.composeForPostgreSQL}} à qual a instância de serviço do {{site.data.keyword.mobilefoundation_short}} precisa se conectar.

Se você não tiver uma instância do Db2 on Cloud existente, será possível criar uma nova instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}} (qualquer plano diferente do plano **Lite**) ou {{site.data.keyword.composeForPostgreSQL}}.

Siga estas etapas para criar uma nova instância de serviço do Db2:

1. Na página *Visão geral*, selecione a seção **Criar novo serviço**.

+ Selecione `Sim` na opção **Configuração de alta disponibilidade**, se você desejar uma instância de serviço de alta disponível do {{site.data.keyword.Db2_on_Cloud_short}}.

+ Revise os detalhes do plano e clique em **Criar**.

É criada uma nova instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}}, que fornece uma instância dedicada do {{site.data.keyword.Db2_on_Cloud_short}} com 8 GB de RAM e 2 vCPUs, além de 500 GB de armazenamento.

Siga estas etapas para se conectar a uma instância de serviço existente do {{site.data.keyword.Db2_on_Cloud_short}} ou à instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}} que você criou:

1. Selecione a {{site.data.keyword.Bluemix_notm}} `Organização` na qual a instância do serviço {{site.data.keyword.Db2_on_Cloud_short}} existe.

+ Selecione {{site.data.keyword.Bluemix_notm}} `Space` em que a instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}} existe, na lista de espaços disponíveis na
`Organization` selecionada.   
Se você não vir listados a `Organization` e o `Space` nos quais a instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}} existe, verifique se você é um membro dessa `Organization` e desse `Space`. É necessário ter um acesso de função de *Desenvolvedor* para a organização e o espaço. O serviço {{site.data.keyword.mobilefoundation_short}} acessa as credenciais por meio do serviço {{site.data.keyword.Db2_on_Cloud_short}}.
{: note}

+ Selecione {{site.data.keyword.Db2_on_Cloud_short}} `Service name` e `Credentials` para se conectar à instância de serviço existente do {{site.data.keyword.Db2_on_Cloud_short}}.

+  Teste a conexão com a instância de serviço {{site.data.keyword.Db2_on_Cloud_short}} especificada.

+  Clique em **Incluir**. Essa ação cria as tabelas necessárias na instância de serviço de banco de dados {{site.data.keyword.Db2_on_Cloud_short}} configurada.

Em alguns segundos, é possível acessar a página `Overview` que fornece tutoriais e vídeos para ajudar a iniciar o serviço {{site.data.keyword.mobilefoundation_short}}.

Não é possível mudar a instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}} que está configurada para ser usada por sua instância de serviço do {{site.data.keyword.mobilefoundation_short}}. No entanto, é possível usar a mesma instância de serviço {{site.data.keyword.Db2_on_Cloud_short}} em múltiplas instâncias de serviço {{site.data.keyword.mobilefoundation_short}}, uma vez que cada instância de serviço {{site.data.keyword.mobilefoundation_short}} cria seu próprio esquema na instância de serviço {{site.data.keyword.Db2_on_Cloud_short}} selecionada.
{: note}

## Iniciando o servidor MobileFirst criado usando o plano Professional 1 Application
{: #start_mobilefoundation_p2}

* Para iniciar o {{site.data.keyword.mfserver_short_notm}}, com as configurações padrão, clique em **Iniciar servidor básico**.

* Essa seleção cria um {{site.data.keyword.mfserver_long_notm}} com as configurações a seguir:
    -  1 GB de memória. Esse tamanho é suficiente para desenvolvimento, atividades de teste moderado e cargas de trabalho de produção em pequena escala.

    -	O `username` e a `password` são gerados automaticamente para
você. Você tem acesso a eles quando o servidor está funcionando.

    O processo de criação do servidor é iniciado. Esse processo leva aproximadamente 10 minutos e uma
janela de mensagem indica o progresso dessa operação. Quando completo, um painel é exibido
no qual é possível ver:

      -	O status do servidor que está em execução (estado, tamanho).

      -	A rota do servidor é criada para você. Use esta rota em seu aplicativo móvel para se
conectar ao {{site.data.keyword.mfserver_short_notm}}.

      -	Seu `nome do usuário` e `senha` para acessar o {{site.data.keyword.mfp_oc_short_notm}}. A `password` fica oculta. Clique
no ícone **Mostrar senha** para visualizá-lo.

*	Clique em **Ativar console** para abrir o {{site.data.keyword.mfp_oc_short_notm}}.

Com o console, é possível
gerenciar seus aplicativos móveis, adaptadores e dispositivos móveis, usar seu servidor como um
backend móvel, enviar notificações push e muito mais.

## Recriando o servidor MobileFirst quando você usa o plano do Professional 1 Application
{: #recreate_mobilefoundation_p2}

*	Clique em **Recriar** para recriar o servidor.

* Esta ação para o servidor existente e exclui os dados. Uma nova instância do servidor é criada com uma versão atualizada, se disponível. Esta ação
demora alguns minutos para ser concluída.

Os dados de sua instância do servidor prévia que incluem informações sobre os apps e adaptadores são persistidos na instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}} configurada. Esses dados são usados para recriar seu servidor.
{: note}

##	Definindo a configuração avançada no plano do Professional 1 Application
{: #using_mfs_advanced_p2}

Use **Iniciar servidor com a configuração avançada** na página `Visão geral` para criar o servidor com configurações avançadas ou customizadas. Também é possível
atualizar as definições do servidor para customizar a configuração do servidor clicando na
guia **Configuração**. O {{site.data.keyword.mobilefoundation_short}} fornece acesso a algumas configurações avançadas.

*	Na guia **Topologia**, é possível selecionar o tamanho do servidor
e o número de instâncias do servidor com base em sua necessidade. O servidor padrão de 1 GB é suficiente para teste de desenvolvimento e leve.
  - Selecione o tamanho correto para seu servidor com base em sua necessidade.

  - **Instâncias** exibe o número de nós criados.

## Mobile Analytics no plano do Professional 1 Application
{: #mobile_analytics_p2}

O servidor Mobile Analytics está incluído e é pré-configurado com o Mobile Foundation: instância de serviço do plano Developer.

* Ativar o Mobile Analytics Console por meio do {{site.data.keyword.mfp_oc_short_notm}}.

Para obter mais informações sobre o Mobile Analytics, veja [aqui](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app){: new_window}.
