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

# Mise à jour directe
{: #direct_update}

Les ressources Web (fichiers JavaScript, HTML, CSS ou images) des applications Cordova ou Ionic peuvent être mises à jour en mode OTA (Over The Air) avec **Mise à jour directe** sans que les utilisateurs doivent passer par la mise à jour de l'App Store ou du Play Store. A l'aide de la fonctionnalité Mise à jour directe, les entreprises peuvent s'assurer que les utilisateurs finaux utilisent la dernière version de leurs applications. Pour mettre à jour une application, les ressources Web mises à jour de l'application doivent être regroupées et téléchargées dans votre instance Mobile Foundation à l'aide de l'interface CLI de Mobile Foundation ou en déployant un fichier d'archive généré. La mise à jour directe est activée automatiquement. Elle peut ensuite être appliquée sur chaque demande d'une ressource protégée effectuée par l'utilisateur.

![Diagramme illustrant le mode de fonctionnement de la mise à jour directe](images/internal_function.jpg)

La mise à jour directe est prise en charge par le système d'exploitation Cordova et sur les plateformes Cordova Android.
{: note}

Pour la phase de test de pré-production ou de production, il est conseillé d'implémenter la mise à jour directe sécurisée avant de publier votre application dans le magasin d'applications. La mise à jour directe sécurisée nécessite une paire de clés RSA extraite d'un certificat serveur réel, signé par une autorité de certification (AC).

1. Prenez soin de ne pas modifier la configuration de magasin de clés après la publication de l'application. Les mises à jour téléchargées ne peuvent pas être authentifiées avant la mise en place d'une reconfiguration de l'application avec une nouvelle clé publique et d'une republication de l'application. Si vous n'effectuez pas ces deux étapes, la mise à jour directe échouera sur le client.
2. La fonctionnalité Mise à jour directe ne met à jour que les ressources web de l'application. Pour mettre à jour les ressources natives, une nouvelle version d'application doit être soumise au magasin d'applications concerné.
3. Lorsque vous utilisez la fonction de mise à jour directe et que la fonction de somme de contrôle des ressources Web est activée, une nouvelle base de somme de contrôle est établie avec chaque mise à jour directe.
4. Si le serveur Mobile Foundation a été mis à niveau, il continue à servir correctement les mises à jour directes. Toutefois, si une archive (fichier .zip) de mise à jour directe
générée récemment est téléchargée, elle peut interrompre les mises à jour de clients plus anciens. En effet, cette archive contient la version du plug-in
`cordova-plugin-mfp`. Avant d'envoyer cette archive sur un client mobile, le serveur compare la version du client à la version du
plug-in. Si les deux versions sont assez proches (c'est-à-dire si les trois chiffres les plus significatifs sont identiques), la mise à jour directe a lieu
normalement. Sinon, le serveur Mobile Foundation saute silencieusement la mise à jour. En cas de non concordance de version, l'une des solutions consiste à télécharger le plug-in `cordova-plugin-mfp` avec la même version que dans votre projet Cordova d'origine et à régénérer l'archive de mise à jour directe.
{: tip}

Après une mise à jour directe, l'application n'utilise plus les ressources web pré-conditionnées. Elle se sert des ressources web téléchargées depuis le bac à sable de l'application. Si le cache de l'application de l'appareil est effacé, les ressources web conditionnées à l'origine sont utilisées à nouveau.

La Mise à jour directe des ressources Web ne s'applique qu'à une version spécifique de l'application. Par exemple, les mises à jour générées pour une application version 2.0 ne sont pas livrées à la version 2.1 de la même application.
{: note}

## Création et déploiement de ressources web mises à jour
{: #creating_deploying_updates}

Une fois que vous avez modifié vos ressources Web, vous pouvez les regrouper et les télécharger dans votre instance Mobile Foundation à l'aide de l'interface CLI de Mobile Foundation.

1.  La fonctionnalité Mise à jour directe ne met à jour que les ressources web de l'application. Pour mettre à jour les ressources natives, une nouvelle version d'application doit être soumise au magasin d'applications concerné.
2. Le service Mobile Foundation arrête la mise à jour directe des clients si la partie native de l'application est très différente de celle téléchargée dans le service Mobile Foundation. Cela peut se produire lorsque le plug-in *cordova-plugin-mfp* est mis à jour avec une version plus récente. Avant d'envoyer l'archive sur un client mobile, le serveur compare la version du client à la version du plug-in. Si les deux versions sont assez proches (c'est-à-dire si les trois chiffres les plus significatifs du numéro de version sont identiques), la mise à jour directe a lieu normalement. Sinon, le serveur Mobile Foundation saute silencieusement la mise à jour. En cas de non concordance de version, l'une des solutions consiste à télécharger le plug-in *cordova-plugin-mfp* avec la même version que dans votre projet Cordova d'origine et à régénérer l'archive de mise à jour directe.
{: tip}

1. Ouvrez une fenêtre de ligne de commande et accédez à la racine du projet Cordova.
2. Exécutez la commande :
  ```bash
  mfpdev app webupdate
  ```
  La commande `mfpdev app webupdate` conditionne les ressources web mises à jour dans un fichier `.zip` et le télécharge sur le serveur Mobile Foundation par défaut s'exécutant sur le poste de travail du développeur. Ce package de ressources web se trouve dans le dossier `[cordova-project-root-folder]/mobilefirst/`.

Pour découvrir d'autres étapes pour mettre à jour le contenu Web dans votre application, voir [ici](/docs/services/mobilefoundation?topic=mobilefoundation-alternate_steps_to_update_app_web_content_in_app#alternate_steps_to_update_app_web_content_in_app).

## Configuration avancée de la mise à jour directe
{: #direct_update_advanced_config}

Pour consulter les rubriques sur la configuration avancée de la mise à jour directe, voir [ici](/docs/services/mobilefoundation?topic=mobilefoundation-advanced_direct_update_configuration#advanced_direct_update_configuration).
