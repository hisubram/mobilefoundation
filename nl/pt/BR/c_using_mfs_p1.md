---

copyright:
  years: 2016, 2018
lastupdated:  "2018-02-14"

---

#	Usando o plano do Desenvolvedor
{: #using_mobilefoundation_p1}

Após criar a instância de serviço do {{site.data.keyword.mobilefoundation_short}} usando o plano do Desenvolvedor, acesse a página Visão geral no {{site.data.keyword.Bluemix_notm}}. Nessa página, são fornecidos a você tutoriais e vídeos para ajudá-lo na introdução do serviço.

## Iniciando o servidor do MobileFirst
{: #start_mobilefoundation_p1}
* Para iniciar o {{site.data.keyword.mfserver_short_notm}} com configurações padrão, clique em **Iniciar servidor básico**.

  Esta seleção provisiona um {{site.data.keyword.mfserver_long_notm}} com as configurações a seguir:
  *	1 GB de memória. Este tamanho é suficiente para desenvolvimento, atividades de teste leve e cargas de trabalho de produção em pequena escala.

  *	O `username` e a `password` são gerados automaticamente para
você. Você tem acesso a eles quando o servidor está funcionando.

  O processo de fornecimento inicia. Esse processo leva aproximadamente 10 minutos e uma
janela de mensagem indica o progresso dessa operação. Quando completo, um painel é exibido
no qual é possível ver:
    *	O status do servidor que está em execução (estado, tamanho).

    *	A rota do servidor criada para você. Use esta rota em seu aplicativo móvel para se
conectar ao {{site.data.keyword.mfserver_short_notm}}.

    *	Seu `nome do usuário` e `senha` para acessar o {{site.data.keyword.mfp_oc_short_notm}}. A `password` fica oculta. Clique
no ícone **Mostrar senha** para visualizá-lo.

*	Clique em **Ativar console** para ativar o {{site.data.keyword.mfp_oc_short_notm}}.

Com o console, é possível gerenciar os aplicativos móveis e dispositivos móveis, usar o servidor como um backend móvel, enviar notificações push e muito mais.

##  Incluindo o serviço Mobile Analytics
{: #adding_analytics_server_dev}

 Agora é possível monitorar o aplicativo móvel no servidor {{site.data.keyword.mobilefirst}} incluindo um serviço Mobile Analytics na instância de serviço do {{site.data.keyword.mobilefoundation_short}}. O Plano do Desenvolvedor cria o serviço do Mobile Analytics em um grupo de contêiner com um nó único tendo 1 GB de memória.

* Clique em **Incluir Analytics** para incluir o serviço Mobile Analytics na instância de serviço do {{site.data.keyword.mobilefoundation_short}}.

  O processo de fornecimento inicia. Esse processo leva aproximadamente 10 minutos e uma
janela de mensagem indica o progresso dessa operação.  

* Ative o Console do serviço Mobile Analytics por meio do {{site.data.keyword.mfp_oc_short_notm}}.

* A conexão única é ativada entre o {{site.data.keyword.mfserver_short_notm}} e o serviço Mobile Analytics. O serviço Mobile Analytics é configurado com as mesmas chaves LTPA e credenciais do usuário que as do {{site.data.keyword.mfserver_short_notm}}. É possível usar o mesmo `username` e `password` para efetuar login no console do Mobile
Analytics que aqueles usados no {{site.data.keyword.mfp_oc_short_notm}}.

Para obter mais informações sobre o Mobile Analytics, é possível consultar o [MobileFirst Foundation Operational Analytics ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}.

> **Nota:** excluir a instância de serviço do {{site.data.keyword.mobilefoundation_short}} ou recriar o {{site.data.keyword.mfserver_short_notm}} remove a instância de serviço do Mobile Analytics.

##  Excluindo o serviço Mobile Analytics
{: #deleting_analytics_server_dev}

Agora é possível excluir o serviço Mobile Analytics que foi incluído na instância de serviço do {{site.data.keyword.mobilefoundation_short}} por meio do painel de serviço do {{site.data.keyword.mobilefoundation_short}}.

* Clique em **Excluir Analytics** para excluir o serviço Mobile Analytics que foi incluído na instância de serviço do {{site.data.keyword.mobilefoundation_short}}.

 Clicar em **Excluir Analytics** exclui a instância do servidor analítico. O processo de exclusão da instância de análise leva aproximadamente 10 minutos. É possível atualizar a tela para visualizar o status
atualizado. A exclusão da instância de análise reativa o botão **Incluir Analytics**. Se optar por incluir o serviço Mobile Analytics novamente, será possível clicar nesse botão.


## Recriando o servidor do MobileFirst
{: #recreate_mobilefoundation_p1}

*	Clique em **Recriar** para recriar o servidor.

* Esta ação para o servidor existente e exclui os dados. Todos os dados no servidor móvel são perdidos. Uma nova instância do servidor é criada com uma versão atualizada, se disponível. Esta ação
demora alguns minutos para ser concluída.

##	Definindo a configuração avançada
{: #using_mfs_advanced_p1}

Use **Iniciar servidor com a configuração avançada** na página `Visão geral` para criar o servidor com configurações avançadas ou customizadas. Também é possível
atualizar as definições do servidor para customizar a configuração do servidor clicando na
guia **Configuração**. O {{site.data.keyword.mobilefoundation_short}} fornece acesso a algumas configurações avançadas.

*	Na guia **Topologia**, é possível selecionar o tamanho do servidor
e o número de instâncias necessárias. O servidor padrão de 1 GB é suficiente para teste de desenvolvimento e moderado.

  - Selecione o tamanho correto para seu servidor com base em sua necessidade.

* **Nós** exibe o número de nós que são criados. Esse campo não é editável no {{site.data.keyword.mobilefoundation_short}}: Developer. O número de nós <!--in your {{site.data.keyword.IBM_notm}} container group--> é definido por padrão como **1** no plano do Desenvolvedor.

Consulte a documentação do [{{site.data.keyword.mobilefoundation_long}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window} para obter mais detalhes.
