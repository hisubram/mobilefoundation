---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: update web content in apps, update apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Alternar etapas para atualizar o conteúdo da web no app
{: #alternate_steps_to_update_app_web_content_in_app}

Revise a lista a seguir para obter formas alternativas de atualizar o conteúdo da web em seu app.

* Construa o arquivo `.zip` e faça upload dele em um servidor Mobile Foundation diferente: `mfpdev app webupdate [server-name] [runtime-name]`.
  Por exemplo:
  ```bash
  MyBankApps myQAServer webupdate app mfpdev
  ```

* Faça upload de um arquivo `.zip` gerado anteriormente: `mfpdev app webupdate [server-name] [runtime-name] --file [path-to-packaged-web-resources]`.
  Por exemplo:
  ```bash
  mfpdev app webupdate myQAServer MyBankApps --file mobilefirst/ios/com.mfp.myBankApp-1.0.1.zip
  ```

* Faça upload manualmente dos recursos da web empacotados para o servidor Mobile Foundation:
  1. Construa o arquivo .zip sem fazer seu upload:
      ```bash
      mfpdev app webupdate --build
      ```
      {: pre}
  2. Carregue o Mobile Foundation Operations Console e clique na entrada do aplicativo.
  3. Clique em **Fazer upload de arquivo de recursos da web** para fazer upload dos recursos da web empacotados.    
      ![Fazer upload do arquivo .zip do Direct Update por meio do console](images/upload-direct-update-package.png "Fazer upload do arquivo .zip do Direct Update por meio do console com o botão Fazer upload do arquivo de recursos da web destacado")

Execute o comando `mfpdev help app webupdate` para saber mais.
{: tip}
