---

copyright:
  years: 2016, 2018
lastupdated:  "2018-03-07"

---

#	Usando o plano do Desenvolvedor
{: #using_mobilefoundation_p1}

Após criar a instância de serviço do {{site.data.keyword.mobilefoundation_short}} usando o plano do Desenvolvedor, acesse a página Visão geral no {{site.data.keyword.Bluemix_notm}}. Nessa página, são fornecidos a você tutoriais e vídeos para ajudá-lo na introdução do serviço.

## Trabalhando com o servidor MobileFirst
{: #start_mobilefoundation_p1}
* É possível acessar e trabalhar instantaneamente com o servidor MobileFirst.

  Esta seleção provisiona um {{site.data.keyword.mfserver_long_notm}} com as configurações a seguir:
  *	1 GB de memória. Este tamanho é suficiente para desenvolvimento, atividades de teste leve e cargas de trabalho de produção em pequena escala.

  * Para acessar o servidor MobileFirst usando a CLI, é necessário usar credenciais, que estão disponíveis quando você clica em **Credenciais de serviço** na área de janela de navegação esquerda do console do IBM Cloud.

<!--  The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation. When complete a dashboard is displayed where you can see:
    *	The status of your server that is running (state, size).

    *	The server route created for you. Use this route in your mobile application to connect to the {{site.data.keyword.mfserver_short_notm}}.

    *	Your personal `username` and `password` to access the {{site.data.keyword.mfp_oc_short_notm}}. The `password` is hidden. Click **Show Password** icon to visualize it.

*	Click **Launch Console** to launch the {{site.data.keyword.mfp_oc_short_notm}}.-->

Agora é possível gerenciar seus apps e dispositivos móveis, usar o servidor como um backend móvel, enviar notificações push e fazer muito mais.

## Serviço Mobile Analytics
{: #adding_analytics_server_dev}

O servidor Mobile Analytics está incluído e é pré-configurado com o Mobile Foundation: instância de serviço do plano Developer.

<!-- You can now monitor your mobile application on {{site.data.keyword.mobilefirst}} server by adding a Mobile Analytics service to the {{site.data.keyword.mobilefoundation_short}} service instance. Developer plan creates the Mobile Analytics service in a container group with a single node having 1 GB memory.

* Click **Add Analytics** to add the Mobile Analytics service to the {{site.data.keyword.mobilefoundation_short}} service instance.

  The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation.  -->

* Ative o Console do serviço Mobile Analytics por meio do {{site.data.keyword.mfp_oc_short_notm}}.

* A conexão única é ativada entre o {{site.data.keyword.mfserver_short_notm}} e o serviço Mobile Analytics. O serviço Mobile Analytics é configurado com as mesmas chaves LTPA e credenciais do usuário que as do {{site.data.keyword.mfserver_short_notm}}. É possível usar o mesmo `username` e `password` para efetuar login no console do Mobile
Analytics que aqueles usados no {{site.data.keyword.mfp_oc_short_notm}}.

Para obter mais informações sobre o Mobile Analytics, é possível consultar o [MobileFirst Foundation Operational Analytics ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}.

> **Nota:** a exclusão da instância de serviço do {{site.data.keyword.mobilefoundation_short}} remove a instância de serviço do Mobile Analytics.

<!--##  Deleting Mobile Analytics service
{: #deleting_analytics_server_dev}

You can now delete the Mobile Analytics service that was added to the {{site.data.keyword.mobilefoundation_short}} service instance, from the {{site.data.keyword.mobilefoundation_short}} service dashboard.

* Click **Delete Analytics** to delete the  Mobile Analytics service that was added to the {{site.data.keyword.mobilefoundation_short}} service instance.

 Clicking **Delete Analytics** deletes the analytics server instance. The process of deleting analytics instance takes about 10 minutes. You can refresh the screen to view the updated status. Deletion of analytics instance reenables the **Add Analytics** button. If you choose to add the Mobile Analytics service again, you can click this button.


## Re-creating the MobileFirst server
{: #recreate_mobilefoundation_p1}

*	Click **Recreate** to re-create the server.

* This action stops your existing server and deletes the data. All the data in your mobile server is lost. A new server instance is created with an updated version, if available. This action takes a few minutes to complete.

##	Setting up advanced configuration
{: #using_mfs_advanced_p1}

Use the **Start Server with Advanced Configuration** from the `Overview` page to create the server with advanced or custom settings. You can also update the server settings to customize your server configuration by clicking the **Configuration** tab. {{site.data.keyword.mobilefoundation_short}} gives you access to some advanced settings.

*	From the **Topology** tab, you can select the server size and the number of instances you need. The default 1 GB server is enough for development and moderate testing.

  - Select the correct size for your server based on your need.

* **Nodes** displays the number of nodes that are created. This field is not editable in {{site.data.keyword.mobilefoundation_short}}: Developer. The number of nodes is defaulted to **1** in the Developer plan.-->

Consulte a documentação do [{{site.data.keyword.mobilefoundation_long}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window} para obter mais detalhes.
