---

copyright:
  years: 2018, 2019
lastupdated:  "2018-11-19"

keywords: command line, cli, mfpdev-cli, cli commands

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

#	Interface de ligne de commande Mobile Foundation
{: #mobile_foundation_cli}

{{ site.data.keyword.mobilefoundation_short }} fournit un outil d'interface de ligne de commande (CLI) pour le développeur **mfpdev** afin de faciliter la gestion du client {{site.data.keyword.mobilefoundation_short}} et des artefacts du serveur.

Toutes les commandes `mfpdev` peuvent être exécutées en mode direct ou interactif. En mode interactif, l'utilisateur est invité à entrer les paramètres requis pour la commande et certaines valeurs par défaut sont utilisées. En mode direct, les paramètres doivent être fournis avec la commande.


## Installation de l'interface de ligne de commande {{site.data.keyword.mobilefoundation_short}}
{: #installing_mf_cli}

{{site.data.keyword.mobilefoundation_short}} est disponible en tant que package NPM dans le [registre NPM![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.npmjs.com){: new_window}.

Vérifiez que **node.js** et **npm** sont installés pour installer les packages NPM. Suivez les instructions d'installation dans [nodejs.org ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://nodejs.org/){: new_window} pour installer **node.js**. Pour confirmer que **node.js** est installé correctement, exécutez la commande suivante :
```bash
node -v
```
{: codeblock}

> **Remarque :** la version node.js minimale prise en charge est la version **4.2.3**. De plus, avec les packages de noeud et npm en constante évolution, l'interface de ligne de commande {{site.data.keyword.mobilefoundation_short}} peut ne pas être totalement fonctionnelle avec toutes les versions de packages de noeud et npm disponibles, y compris les versions les plus récentes. Assurez-vous que le noeud est en version **6.11.1** et que npm est en version **3.10.10**, pour un fonctionnement correct de l'interface de ligne de commande.

> Pour les versions de correctif temporaire de l'interface CLI MobileFirst *8.0.2018100112* et versions ultérieures, vous pouvez utiliser les versions de noeud 8.x ou 10.x.

Pour installer l'interface de ligne de commande {{site.data.keyword.mobilefoundation_short}}, exécutez la commande suivante :
```bash
npm install -g mfpdev-cli
```
{: codeblock}

Si le fichier compressé d'interface CLI (*.zip*) a été téléchargé depuis le centre de téléchargement de MobileFirst Operations Console, utilisez la commande suivante :
```bash
npm install -g <path-to-mfpdev-cli.tgz>
```
{: codeblock}

Pour installer l'interface CLI sans les dépendances facultatives, ajoutez l'indicateur `--no-optional` :
```bash
npm install -g --no-optional path-to-mfpdev-cli.tgz
```
{: codeblock}

Lors de l'installation de l'interface CLI MobileFirst qui utilise le noeud 8, vous pouvez voir quelques-unes des erreurs suivantes dans la fenêtre du terminal :
```text
> node-gyp rebuild

gyp ERR! clean error 
gyp ERR! stack Error: EACCES: permission denied, rmdir 'build'
gyp ERR! System Darwin 18.0.0
gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /usr/local/lib/node_modules/mfpdev-cli/node_modules/bufferutil
gyp ERR! node -v v8.12.0
gyp ERR! node-gyp -v v3.8.0
gyp ERR! not ok 

> utf-8-validate@1.2.2 install /usr/local/lib/node_modules/mfpdev-cli/node_modules/utf-8-validate
> node-gyp rebuild

gyp ERR! clean error 
gyp ERR! stack Error: EACCES: permission denied, rmdir 'build'
gyp ERR! System Darwin 18.0.0
gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /usr/local/lib/node_modules/mfpdev-cli/node_modules/utf-8-validate
gyp ERR! node -v v8.12.0
gyp ERR! node-gyp -v v3.8.0
gyp ERR! not ok 

> fsevents@1.2.4 install /usr/local/lib/node_modules/mfpdev-cli/node_modules/fsevents
> node install
```

Cette erreur est due à un [bogue connu dans node-gyp![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/nodejs/node-gyp/issues/1547){: new_window}. Ces erreurs peuvent être ignorées car elles n'affectent pas le fonctionnement de l'interface CLI de MobileFirst. Ce problème concerne le niveau de correctif temporaire `mfpdev-cli` *8.0.2018100112* et les versions ultérieures. Pour surmonter cette erreur, utilisez l'indicateur `--no-optional` pendant l'installation.

Pour confirmer que l'interface de ligne de commande est installée correctement, exécutez la commande suivante :
```bash
mfpdev
```
{: codeblock}

L'aide de l'interface de ligne de commande est imprimée en sortie.

```
 NAME
     IBM MobileFirst Foundation Command Line Interface (CLI).

 SYNOPSIS
     mfpdev <command> [options]

 DESCRIPTION
     The IBM MobileFirst Foundation Command Line Interface (CLI) is a command-line
     for developing MobileFirst applications. The command-line can be used by itself, or in conjunction  with the IBM MobileFirst Foundation Operations Console. Some functions are available from  the command-line only and not the console.

     For more information and a step-by-step example of using the CLI, see the IBM Knowledge Center for your version of IBM MobileFirst Foundation at
     https://www.ibm.com/support/knowledgecenter.
     ...
     ...
     ...
```
{: screen}

## Liste des commandes d'interface de ligne de commande {{site.data.keyword.mobilefoundation_short}}
{: #list_cli_commands}

<table summary="{{site.data.keyword.mobilefoundation_short}} Commandes d'interface de ligne de commande disposant de liens qui vous proposent plus d'informations sur la commande">
<caption>Tableau 1. Commandes de l'interface de ligne de commande {{site.data.keyword.mobilefoundation_short}}</caption>
 <thead>
 <th colspan="6">Commandes [mfpdev](#mfpdev) générales</th>
 </thead>
 <tbody>
 <tr>
 <td>[app](#mfpdev_app)</td>
 <td>[server](#mfpdev_server)</td>
 <td>[adapter](#mfpdev_adapter)</td>
 <td>[help](#mfpdev_help)</td>
 </tr>
   </tbody>
 </table>

### mfpdev
{: #mfpdev}

Définit vos préférences de configuration pour le type de navigateur d'aperçu, la valeur de délai d'aperçu et la valeur de délai de
serveur pour l'interface de ligne de commande mfpdev.

```bash
mfpdev config
```
{: codeblock}

Affiche des informations sur votre environnement, notamment le système d'exploitation, la consommation de mémoire, la version du
noeud et la version de l'interface de ligne de commande. Si le répertoire en cours est une application Cordova, des informations fournies par la commande `cordova info` sont également affichées.

```bash
mfpdev info
```
{: codeblock}

Affiche le numéro de version de l'interface de ligne de commande {{site.data.keyword.mobilefoundation_short}} actuellement utilisée.

```bash
mfpdev -v
```
{: codeblock}

Mode débogage : génère une sortie de débogage.

```bash
mfpdev [-d|--debug]
```
{: codeblock}

Mode débogage prolixe : génère une sortie de débogage prolixe.

```bash
mfpdev [-dd|--ddebug]
```
{: codeblock}

Supprime l'utilisation de couleurs dans le résultat de la commande.

```bash
mfpdev -no-color
```
{: codeblock}

### mfpdev help
{: #mfpdev_help}

Affiche l'aide pour les commandes (mfpdev) de l'interface de ligne de commande MobileFirst. Avec le nom de la commande comme argument, elle affiche un texte d'aide plus spécifique pour chaque type de commande ou chaque commande. Exemple : `mfpdev help server add`.

```bash
mfpdev help <command name>
```
{: codeblock}


### mfpdev app
{: #mfpdev_app}

Enregistre votre application avec un serveur MobileFirst.

```bash
mfpdev app register
```
{: codeblock}

Pour enregistrer une application sur un serveur et un environnement d'exécution qui ne sont pas ceux par défaut, utilisez la commande suivante : 

```bash
mfpdev app register <server> <runtime>
```
{: codeblock}

Pour la plateforme Windows Cordova, l'argument `-w <platform>` doit être ajouté à la commande. L'argument de plateforme est une liste des plateformes Windows à enregistrer, séparées par des virgules. Les valeurs valides sont windows, windows8 et windowsphone8.

```bash
mfpdev app register -w windows8
```
{: codeblock}

Vous pouvez spécifier le serveur de back end et le contexte d'exécution à utiliser pour votre application. Pour les applications Cordova, cette commande vous permet de configurer plusieurs autres aspects, tels que la langue par défaut pour les messages système et la nécessité de procéder à une vérification de sécurité de total de contrôle. D'autres paramètres de configuration sont inclus pour les applications Cordova.

```bash
mfpdev app config
```
{: codeblock}

Extrait une configuration d'application existante depuis le serveur.

```bash
mfpdev app pull
```
{: codeblock}

Envoie une configuration d'application au serveur.

```bash
mfpdev app push
```
{: codeblock}

Prévisualisez votre application Cordova sans appareil réel présentant le type de plateforme cible. Vous pouvez afficher l'aperçu dans le simulateur de navigateur mobile ou dans votre navigateur Web.

```bash
mfpdev app preview
```
{: codeblock}

Conditionne les ressources d'application qui se trouvent dans le répertoire www dans un fichier compressé (*.zip*) pouvant être utilisé pour le
processus de mise à jour directe.

```bash
mfpdev app webupdate
```
{: codeblock}

Cette commande conditionne les ressources Web mises à jour dans un fichier compressé (*.zip*) et les télécharge sur le serveur MobileFirst par défaut enregistré. Ce package de ressources web se trouve dans le dossier `[cordova-project-root-folder]/mobilefirst/`.

Pour télécharger les ressources web sur une instance de serveur différente, fournissez le nom du serveur et l'environnement d'exécution en tant qu'élément de la commande :

```bash
mfpdev app webupdate <server_name> <runtime>
```
{: codeblock}

Vous pouvez utiliser le paramètre –build pour générer le fichier compressé (*.zip*) avec le package de ressources web sans le télécharger sur un serveur.
```bash
mfpdev app webupdate --build
```
{: codeblock}

Pour télécharger un package qui a été généré précédemment, utilisez le paramètre –file :
```bash
mfpdev app webupdate --file mobilefirst/com.ibm.test-android-1.0.0.zip
```
{: codeblock}

Il existe aussi une option permettant de chiffrer le contenu du package en utilisant le paramètre `–encrypt` :
```bash
mfpdev app webupdate --encrypt
```
{: codeblock}


### mfpdev server
{: #mfpdev_server}

Affiche des informations relatives au serveur MobileFirst.

```bash
mfpdev server info
```
{: codeblock}

Ajoute une définition de serveur à votre environnement.

```bash
mfpdev server add
```
{: codeblock}

Editez une définition de serveur.

```bash
mfpdev server edit
```
{: codeblock}

Pour définir un serveur en tant que serveur par défaut, utilisez la commande suivante : 

```bash
mfpdev server edit <server_name> --setdefault
```
{: codeblock}

Supprime une définition de serveur de votre environnement.

```bash
mfpdev server remove
```
{: codeblock}

Ouvrez MobileFirst Operations Console.

```bash
mfpdev server console
```
{: codeblock}

Pour ouvrir la console d'un autre serveur, fournissez le nom du serveur en tant que paramètre pour la commande :

```bash
mfpdev server console <server-name>
```
{: codeblock}

Annule l'enregistrement des applications et retire les adaptateurs du serveur MobileFirst.

```bash
mfpdev server clean
```
{: codeblock}

### mfpdev adapter
{: #mfpdev_adapter}

Crée un adaptateur.

```bash
mfpdev adapter create
```
{: codeblock}

Génère un adaptateur.

```bash
mfpdev adapter build
```
{: codeblock}

Détecte et génère tous les adaptateurs dans le répertoire actuel et ses sous-répertoires.

```bash
mfpdev adapter build all
```
{: codeblock}

Déploie un adaptateur sur le serveur MobileFirst.

```bash
mfpdev adapter deploy
```
{: codeblock}

Pour déployer sur un serveur différent, utilisez la commande suivante : 
```bash
mfpdev adapter deploy <server_name>
```
{: codeblock}

Détecte tous les adaptateurs dans le répertoire actuel et ses sous-répertoires et les déploie sur le serveur MobileFirst.

```bash
mfpdev adapter deploy all
```
{: codeblock}

Appelle une procédure d'adaptateur sur le serveur MobileFirst.

```bash
mfpdev adapter call
```
{: codeblock}

Extrait une configuration d'adaptateur existante à partir du serveur.
```bash
mfpdev adapter pull
```
{: codeblock}

Envoie une configuration d'adaptateur au serveur.
```bash
mfpdev adapter push
```
{: codeblock}

## Mise à jour et désinstallation de l'interface CLI {{site.data.keyword.mobilefoundation_short}}
{: #update_uninstall_mf_cli}

Pour mettre à jour l'interface de ligne de commande, exécutez la commande :

```bash
npm update -g mfpdev-cli
```
{: codeblock}

Pour désinstaller l'interface de ligne de commande, exécutez la commande :

```bash
npm uninstall -g mfpdev-cli
```
{: codeblock}
