---

copyright:
  years: 2016, 2018
lastupdated:  "2018-03-05"

---

#	Usando o plano Professional Per Device
{: #using_mobilefoundation_p5}

Com o plano Professional Per Device, os usuários podem construir, testar e executar aplicativos móveis em produção, independentemente do número de usuários ou dispositivos móveis. Esse plano suporta implementações grandes e alta disponibilidade.Após criar a instância de serviço {{site.data.keyword.mobilefoundation_short}}: Professional Per Device, leia o procedimento a seguir para iniciar o serviço.

## Pré-requisitos
{: #prerequisites_p5}

Considere o seguinte antes de configurar a instância de serviço {{site.data.keyword.mobilefoundation_short}}: Professional Per Device.
* {{site.data.keyword.mobilefoundation_short}}: Professional Per Device é suportado somente com os planos do {{site.data.keyword.Db2_on_Cloud_short}}{{site.data.keyword.Bluemix_notm}}.

* É necessário ter acesso às credenciais da instância de serviço {{site.data.keyword.Db2_on_Cloud_short}} antes de poder definir
as configurações de sua instância de serviço {{site.data.keyword.mobilefoundation_short}}.

> **Nota**: a instância de serviço {{site.data.keyword.Db2_on_Cloud_short}} pode existir em qualquer `Espaço` dentro de sua{{site.data.keyword.Bluemix_notm}} `Organização` ou qualquer outra `Organização` à qual você tem acesso. Assegure-se de ter as permissões para acessar o `Espaço` na qual a instância de serviço {{site.data.keyword.Db2_on_Cloud_short}} existe.


## Incluindo a conexão com o banco de dados
{: #configure_dashdb_p5}

###  Primeiras etapas
{: #firststeps_p5}

Após criar a instância de serviço {{site.data.keyword.mobilefoundation_short}}: Professional Per Device, siga o procedimento para iniciar.

### Configurando a conexão com a instância de serviço do DB2 on Cloud
{: #connect_dashdb_p5}

Após a instância de serviço {{site.data.keyword.mobilefoundation_short}}: Professional Per Device ser criada, você verá a página *Visão geral*, na qual será necessário especificar as informações de conexão para a instância de serviço {{site.data.keyword.Db2_on_Cloud_short}} à qual a instância de serviço {{site.data.keyword.mobilefoundation_short}} deverá se conectar.

Também será possível criar uma nova instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}}, se você não tiver um já existente.

Siga estas etapas para criar uma nova instância de serviço do DB2 on Cloud:

1. Na página *Visão geral*, selecione a seção **Criar novo serviço**.

+ Selecione `Sim` na opção **Configuração de alta disponibilidade**, se você desejar uma instância de serviço de alta disponível do {{site.data.keyword.Db2_on_Cloud_short}}.

+ Revise os detalhes do plano e clique em **Criar**.

Uma nova instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}} é criada, o que fornece uma instância dedicada do {{site.data.keyword.Db2_on_Cloud_short}} com 8 GB de RAM e 2 vCPUs, além de 500 GB de armazenamento.

Siga estas etapas para se conectar a uma instância de serviço existente do {{site.data.keyword.Db2_on_Cloud_short}} ou a uma instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}} que você acabou de criar:

1. Selecione a {{site.data.keyword.Bluemix_notm}} `Organização` na qual a instância do serviço {{site.data.keyword.Db2_on_Cloud_short}} existe.

+ Selecione {{site.data.keyword.Bluemix_notm}} `Space` em que a instância de serviço do {{site.data.keyword.Db2_on_Cloud_short}} existe, na lista de espaços disponíveis na `Organization` selecionada.   
> **Nota:** se você não vir listados a `Organização` e o `Espaço` nos quais a instância de serviço {{site.data.keyword.Db2_on_Cloud_short}} existe, então, verifique se você é um membro de tal `Organização` e `Espaço`. É necessário ter acesso a uma função de *Desenvolvedor* para a organização e para o espaço, já que o serviço {{site.data.keyword.mobilefoundation_short}} acessa as credenciais por meio do serviço {{site.data.keyword.Db2_on_Cloud_short}}.

+ Selecione {{site.data.keyword.Db2_on_Cloud_short}} `Service name` e `Credentials` para se conectar à instância de serviço existente do {{site.data.keyword.Db2_on_Cloud_short}}.

+  Teste a conexão com a instância de serviço {{site.data.keyword.Db2_on_Cloud_short}} especificada.

+  Clique em **Incluir**. Essa ação cria as tabelas necessárias na instância de serviço de banco de dados {{site.data.keyword.Db2_on_Cloud_short}} configurada.

Em alguns segundos, é possível acessar a página `Overview` que fornece tutoriais e vídeos para ajudar a iniciar o serviço {{site.data.keyword.mobilefoundation_short}}.

> **Nota**: não é possível mudar a instância de serviço {{site.data.keyword.Db2_on_Cloud_short}} que está configurada para ser usada por sua instância de serviço {{site.data.keyword.mobilefoundation_short}}. No entanto, é possível usar a mesma instância de serviço {{site.data.keyword.Db2_on_Cloud_short}} em múltiplas instâncias de serviço {{site.data.keyword.mobilefoundation_short}}, uma vez que cada instância de serviço {{site.data.keyword.mobilefoundation_short}} cria seu próprio esquema na instância de serviço {{site.data.keyword.Db2_on_Cloud_short}} selecionada.

## Iniciando o servidor do MobileFirst
{: #start_mobilefoundation_p5}

* Para iniciar o {{site.data.keyword.mfserver_short_notm}}, com as configurações padrão, clique em **Iniciar servidor básico**.

* Esta seleção provisiona um {{site.data.keyword.mfserver_long_notm}} com as configurações a seguir:
    -  Dois nós com 1 GB de memória cada. Esse tamanho é bom para desenvolvimento, atividades de teste moderado e cargas de trabalho de produção em pequena escala.

    -	O `username` e a `password` são gerados automaticamente para você. Você tem acesso a eles quando o servidor está funcionando.

O processo de fornecimento do servidor inicia. Esse processo leva aproximadamente 10 minutos e uma janela de mensagem indica o progresso dessa operação. Quando completo, um painel é exibido no qual é possível ver:

  -	O status do servidor que está em execução (estado, tamanho).

  -	A rota do servidor é criada para você. Use esta rota em seu aplicativo móvel para se conectar ao {{site.data.keyword.mfserver_short_notm}}.

  -	Seu `nome do usuário` e `senha` para acessar o {{site.data.keyword.mfp_oc_short_notm}}. A `password` fica oculta. Clique no ícone **Mostrar senha** para visualizá-lo.

*	Clique em **Ativar console** para abrir o {{site.data.keyword.mfp_oc_short_notm}}.


Com o console, é possível gerenciar seus aplicativos móveis, adaptadores e dispositivos móveis, usar o servidor como um backend móvel, enviar notificações push e muito mais.


##  Incluindo o serviço Mobile Analytics
{: #adding_analytics_server_p5}

 Agora é possível monitorar o seu aplicativo móvel no servidor do {{site.data.keyword.mobilefirst}} incluindo uma instância de serviço do Mobile Analytics na instância do {{site.data.keyword.mobilefoundation_short}}.

 <!--The Professional plan creates the Mobile Analytics service in a container group, the user can customize the configuration by selecting the number of container nodes in the container group.

 Users can also attach volumes to the containers to persist data. The volume once selected cannot be changed. 20 GB is the default file share space available to the user. If the user needs additional storage space to persist analytics data, he is required to buy additional file share and create a volume using this file share. He can then select this new volume while deploying the analytics server.

 For more information on adding volumes to {{site.data.keyword.containerlong}}, refer to [Storing persistent data in a volume by using the {{site.data.keyword.Bluemix_notm}} Dashboard ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.ng.bluemix.net/docs/containers/container_volumes_ov.html#container_volumes_ui.html){: new_window}.-->

* Clique em **Incluir Analytics** para criar e incluir uma instância de serviço do Mobile Analytics na instância do {{site.data.keyword.mobilefoundation_short}}.

<!--* You can choose the Mobile Analytics service configuration, the minimum supported configuration for the Analytics server is 2 nodes with 1 GB memory each, you can choose to create an Analytics server up to a maximum configuration of 32 nodes with 16 GB memory each.-->

O processo de fornecimento inicia. Esse processo leva alguns minutos e um indicador de progresso exibe o progresso dessa operação.  

* Ative o Console do serviço Mobile Analytics por meio do {{site.data.keyword.mfp_oc_short_notm}}.

* A conexão única é ativada entre o {{site.data.keyword.mfserver_short_notm}} e o serviço Mobile Analytics. O serviço Mobile Analytics é configurado com as mesmas chaves LTPA e credenciais do usuário que as do servidor {{site.data.keyword.mfserver_short_notm}}. É possível usar o mesmo `username` e `password` para efetuar login no console do Mobile Analytics que aqueles usados para efetuar login no {{site.data.keyword.mfp_oc_short_notm}}.

Para obter mais informações sobre o Mobile Analytics, é possível consultar o [MobileFirst Foundation Operational Analytics ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}.

> **Nota:** a exclusão da instância de serviço do {{site.data.keyword.mobilefoundation_short}} remove a instância de serviço do Mobile Analytics.

##  Excluindo o serviço Mobile Analytics
{: #deleting_analytics_server_p5}

Agora é possível excluir o serviço do Mobile Analytics que foi incluído na instância de serviço do {{site.data.keyword.mobilefoundation_short}}, por meio do painel de serviço do {{site.data.keyword.mobilefoundation_short}}.

* Clique em **Excluir Analytics** para excluir o serviço Mobile Analytics que foi incluído na instância de serviço do {{site.data.keyword.mobilefoundation_short}}.

 Clicar em **Excluir Analytics** exclui a instância do servidor analítico. O processo de exclusão da instância de análise leva aproximadamente 10 minutos. É possível atualizar a tela para visualizar o status atualizado. A exclusão da instância de análise reativa o botão **Incluir Analytics**. Se optar por incluir o serviço Mobile Analytics novamente, será possível clicar nesse botão.

## Recriando o servidor do MobileFirst
{: #recreate_mobilefoundation_p5}

*	Clique em **Recriar** para recriar o servidor.

* Esta ação para o servidor existente e exclui os dados. Uma nova instância do servidor é criada com uma versão atualizada, se disponível. Esta ação demora alguns minutos para ser concluída.

> **Nota**: os dados de sua instância de servidor anterior, incluindo informações sobre os apps e adaptadores, são persistidos na instância de serviço configurada do {{site.data.keyword.Db2_on_Cloud_short}}. Esses dados são usados para recriar seu servidor.

##	Definindo a configuração avançada
{: #using_mfs_advanced_p5}

Use **Iniciar servidor com a configuração avançada** na página `Visão geral` para criar o servidor com configurações avançadas ou customizadas. Também é possível atualizar as configurações do servidor para customizar a configuração do servidor clicando na guia **Configurações**. O {{site.data.keyword.mobilefoundation_short}} fornece acesso a algumas configurações avançadas.

*	Na guia **Topologia**, é possível selecionar o tamanho do servidor
e o número de instâncias do servidor com base em sua necessidade. O servidor padrão de 1 GB é suficiente para teste de desenvolvimento e leve.
  - Selecione o tamanho correto para seu servidor com base em sua necessidade.

  - **Instâncias** exibe o número de instâncias que são criadas.

      <!--- {{site.data.keyword.mobilefirst}} server farm can be created by configuring the number of nodes here. The minimum supported configuration is 2 nodes with 1 GB memory each and the maximum supported configuration is 32 nodes with 16 GB memory each.-->

Consulte a documentação do [{{site.data.keyword.mobilefoundation_long}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window} para obter mais detalhes.
