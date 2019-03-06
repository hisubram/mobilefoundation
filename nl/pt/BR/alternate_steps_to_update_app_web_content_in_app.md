---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-14"

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

Abaixo estão listadas algumas das maneiras alternativas de atualizar o conteúdo da web em seu app.

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
      ![Faça upload do arquivo .zip da atualização direta do console](images/upload-direct-update-package.png)

Execute o comando `mfpdev help app webupdate` para saber mais.
{: tip}
