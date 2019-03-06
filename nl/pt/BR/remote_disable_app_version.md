---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-29"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Desativar remotamente uma versão do aplicativo
{: #remotely_disable_an_app_version}

Nesta seção, discutiremos o como desativar o acesso de usuário a uma versão específica de um aplicativo em um sistema operacional de dispositivo móvel específico e como fornecer uma mensagem customizada para o usuário.

É possível usar o Mobile Foundation Operations Console para gerenciar o acesso ao app.

1. Selecione sua versão do aplicativo na seção **Aplicativos** da barra lateral de navegação do console e, em seguida, selecione a guia **Gerenciamento** do aplicativo.
2. Mude o status para **Acesso desativado**.
3. Forneça uma URL para a nova versão do aplicativo (geralmente, no armazenamento de app público ou privado apropriado), no campo **URL da versão mais recente**. 
   Para alguns ambientes, o Application Center fornece uma URL para acessar a visualização de detalhes de uma versão do aplicativo diretamente. Veja [Propriedades do aplicativo](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/appcenter/appcenter-console/#application-properties).
   {: tip}

4. No campo **Mensagem de notificação padrão**, inclua a mensagem de notificação customizada para exibir quando o usuário tentar acessar o aplicativo. A mensagem de amostra a seguir direciona os usuários para fazer upgrade para a versão mais recente:
   `This version is no longer supported. Please upgrade to the latest version.`
5. Na seção **Códigos de idioma suportados**, é possível, opcionalmente, fornecer a mensagem de notificação em outros idiomas.
6. Selecione **Salvar** para aplicar suas mudanças.

Quando um usuário executa um aplicativo que foi desativado remotamente, uma janela de diálogo com a mensagem customizada configurada é exibida. A mensagem é exibida em qualquer interação de aplicativo que requeira acesso a um recurso protegido ou quando o aplicativo tentar obter um token de acesso. Se você forneceu uma URL de upgrade de versão, o diálogo exibirá um botão **Obter nova versão** para fazer upgrade para uma versão mais recente, além do botão **Fechar** padrão. <br/>
Se o usuário fechar a janela de diálogo sem fazer upgrade da versão, eles poderão continuar a trabalhar com os recursos do aplicativo que não estão protegidos. No entanto, qualquer interação de aplicativo que requeira acesso a um recurso protegido faz com que a janela de diálogo seja exibida novamente e que o aplicativo ou usuário não tenha acesso concedido ao recurso.


