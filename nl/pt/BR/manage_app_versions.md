---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-29"

keywords: app versions, disabling apps

subcollection:  mobilefoundation
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

Usando o console do Mobile Foundation Operations, é possível monitorar e gerenciar o acesso a seus recursos. Também é possível gerenciar sua versão específica do aplicativo.

1.  Acesse o console do Mobile Foundation Operations, clique em **Aplicativos**, escolha o aplicativo que você deseja gerenciar, selecione a versão específica do aplicativo de seu interesse, na lista **Versões** exibida.
    ![Gerenciar versão do aplicativo](images/app_version_management.png)

2. Sob a guia **Gerenciamento**, você vê as opções para configurar o status do aplicativo para a versão selecionada do aplicativo. Os estados suportados para uma versão do aplicativo são,
   * Ativo
   * Ativo e notificando
   * Acesso desativado
3. Uma versão do aplicativo pode ser desativada selecionando a opção *Acesso desativado* em **Acesso ao aplicativo > Status **.
4. Também é possível configurar para fazer upload dos recursos da web atualizados para seu aplicativo Cordova na seção **Direct Update**. Os usuários que se conectam ao servidor Mobile Foundation usando essa versão específica do aplicativo são, então, apresentados à opção para atualizar seu aplicativo usando Direct Update.
5. Também é possível executar as ações a seguir na versão do aplicativo selecionada usando as opções a seguir no menu **Ações**:
   *  Excluir versão
   *  Clonar versão
   *  Exportar versão


Para obter mais informações sobre o gerenciamento de dispositivos, consulte [Gerenciando dispositivos](/docs/services/mobilefoundation?topic=mobilefoundation-manage_devices#manage_devices).
Para obter mais informações sobre a desativação remota de uma versão do app, consulte [Desativar remotamente uma versão do app](/docs/services/mobilefoundation?topic=mobilefoundation-remotely_disable_an_app_version#remotely_disable_an_app_version).
{: note}
