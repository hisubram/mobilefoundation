---

copyright:
  years: 2018
lastupdated: "2018-11-29"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Gerenciar versões do aplicativo
{: #manage_app_versions}

Os recursos de gerenciamento de aplicativos do Mobile Foundation fornecem aos usuários e administradores do Mobile Foundation Server o controle granular sobre o acesso de usuário e dispositivo a seus aplicativos.

O servidor Mobile Foundation rastreia todas as tentativas de acessar sua infraestrutura móvel e armazena informações sobre o aplicativo, o usuário e o dispositivo no qual o aplicativo está instalado. O mapeamento entre o aplicativo, o usuário e o dispositivo forma a base para os recursos de gerenciamento de aplicativos móveis do servidor.

Usando o Mobile Foundation Operations Console, é possível monitorar e gerenciar o acesso aos seus recursos. Também é possível gerenciar a sua versão de aplicativo específica.

1.  Acesse o Mobile Foundation Operations Console, clique em **Aplicativos**, escolha o aplicativo que deseja gerenciar, selecione a versão específica do aplicativo no qual você está interessado, na lista **Versões** exibida.
   ![Gerenciar versão do aplicativo](images/app_version_management.png)

2. Sob a guia **Gerenciamento**, você verá as opções para configurar o status do aplicativo para a versão do aplicativo selecionada. Os estados suportados para uma versão do aplicativo são:
   * Ativo
   * Ativo e notificando
   * Acesso desativado
3. Uma versão do aplicativo pode ser desativada selecionando a opção *Acesso desativado* em **Acesso ao aplicativo > Status **.
4. Também é possível configurar para fazer upload dos recursos da web atualizados para seu aplicativo Cordova na seção **Direct Update**. Os usuários que se conectam ao servidor Mobile Foundation usando essa versão do aplicativo específica são, então, apresentados à opção para atualizar seu aplicativo usando Direct Update.
5. Também é possível executar as ações a seguir na versão do aplicativo selecionada, usando as opções a seguir no menu **Ações**:
   *  Excluir versão
   *  Clonar versão
   *  Exportar versão


Veja [Gerenciando dispositivos](manage_devices.html) para aprender sobre o gerenciamento de dispositivos. Veja [Desativar remotamente uma versão do aplicativo](remote_disable_app_version.html) para aprender sobre a desativação remota de uma versão específica de um app.
{: note}

