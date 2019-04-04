---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-14"

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

# Autres étapes pour mettre à jour le contenu Web dans une application
{: #alternate_steps_to_update_app_web_content_in_app}

La liste ci-dessous répertorie certaines des méthodes alternatives pour mettre à jour le contenu Web de votre application.

* Générez le fichier `.zip` et téléchargez-le sur un autre serveur Mobile Foundation : `mfpdev app webupdate [server-name] [runtime-name]`.
  Exemple :
  ```bash
  mfpdev app webupdate myQAServer MyBankApps
  ```

* Téléchargez un fichier `.zip` généré précédemment : `mfpdev app webupdate [server-name] [runtime-name] --file [path-to-packaged-web-resources]`.
  Exemple :
  ```bash
  mfpdev app webupdate myQAServer MyBankApps --file mobilefirst/ios/com.mfp.myBankApp-1.0.1.zip
  ```

* Téléchargez manuellement un package de ressources web sur le serveur Mobile Foundation :
  1. Générez le fichier .zip sans le télécharger :
      ```bash
      mfpdev app webupdate --build
      ```
      {: pre}
  2. Chargez la console Mobile Foundation Operations et cliquez sur l'entrée d'application.
  3. Cliquez sur **Télécharger une archive de ressources Web** pour télécharger le package de ressources web.    
      ![Téléchargement du fichier .zip de mise à jour directe depuis la console](images/upload-direct-update-package.png)

Exécutez la commande `mfpdev help app webupdate` pour en savoir plus.
{: tip}
