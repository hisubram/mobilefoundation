---

copyright:
  years: 2018
lastupdated: "2018-12-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Pasos alternativos para actualizar el contenido web de la app
{: #alternate_steps_to_update_app_web_content}

A continuación, se lista algunas de las formas alternativas de actualizar el contenido web en la app.

* Cree el archivo `.zip` y cárguelo en un servidor de Mobile Foundation distinto: `mfpdev app webupdate [server-name] [runtime-name]`.
  Por ejemplo:
  ```bash
  mfpdev app webupdate myQAServer MyBankApps
  ```

* Cargue un archivo `.zip` generado de forma previa: `mfpdev app webupdate [server-name] [runtime-name] --file [path-to-packaged-web-resources]`.
  Por ejemplo:
  ```bash
  mfpdev app webupdate myQAServer MyBankApps --file mobilefirst/ios/com.mfp.myBankApp-1.0.1.zip
  ```

* Cargue manualmente recursos web empaquetados en el servidor de Mobile Foundation:
  1. Compile el archivo .zip sin cargarlo:
      ```bash
      mfpdev app webupdate --build
      ```
      {: pre}
  2. Cargue la consola de operaciones de Mobile Foundation y pulse la entrada de aplicación.
  3. Pulse en **Subir archivo de recursos web** para subir los recursos web empaquetados.    
      ![Cargar el archivo .zip de Direct Update desde la consola](images/upload-direct-update-package.png)

Ejecute el mandato `mfpdev help app webupdate` para obtener más información.
{: tip}
