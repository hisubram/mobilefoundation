---

copyright:
  years: 2018
lastupdated:  "2018-05-04"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

#	Interface de ligne de commande {{ site.data.keyword.mobilefoundation_short }}
{: #using_mobilefoundation_cli}

{{ site.data.keyword.mobilefoundation_short }} fournit un outil d'interface de ligne de commande pour le développeur **mfpdev** afin de faciliter la gestion du client {{site.data.keyword.mobilefoundation_short}} et des artefacts du serveur.

Toutes les commandes `mfpdev` peuvent être exécutées en mode direct ou interactif. En mode interactif, l'utilisateur est invité à entrer les paramètres requis pour la commande et certaines valeurs par défaut sont utilisées. En mode direct, les paramètres doivent être fournis avec la commande en cours d'exécution.


## Installation de l'interface de ligne de commande {{site.data.keyword.mobilefoundation_short}}
{: #installing_mf_cli}

{{site.data.keyword.mobilefoundation_short}} est disponible en tant que package NPM dans le [registre NPM![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.npmjs.com){: new_window}.

Assurez-vous que **node.js** et **npm** sont installés pour installer les packages NPM. Suivez les instructions d'installation dans [nodejs.org ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://nodejs.org/){: new_window} pour installer **node.js**. Pour confirmer que **node.js** est installé correctement, exécutez la commande suivante :
```
node -v
```
{: codeblock}

> **Remarque :** la version node.js minimale prise en charge est la version **4.2.3**. De plus, avec les packages de noeud et npm en constante évolution, l'interface de ligne de commande {{site.data.keyword.mobilefoundation_short}} peut ne pas être totalement fonctionnelle avec toutes les versions de packages de noeud et npm disponibles, y compris les versions les plus récentes. Assurez-vous que le noeud est en version **6.11.1** et que npm est en version **3.10.10**, pour un fonctionnement correct de l'interface de ligne de commande.

Pour installer l'interface de ligne de commande {{site.data.keyword.mobilefoundation_short}}, exécutez la commande suivante :
```
npm install -g mfpdev-cli
```
{: codeblock}

Si le fichier .zip d'interface de ligne de commande a été téléchargé depuis le centre de téléchargement de MobileFirst Operations Console, utilisez la commande suivante :
```
npm install -g <path-to-mfpdev-cli.tgz>
```
{: codeblock}

Pour installer l'interface de ligne de commande sans les dépendances facultatives, ajoutez l'indicateur `--no-optional` :
```
npm install -g --no-optional path-to-mfpdev-cli.tgz
```
{: codeblock}

Pour confirmer que l'interface de ligne de commande est installée correctement, exécutez la commande suivante :
```
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

```
mfpdev config
```
{: codeblock}

Affiche des informations sur votre environnement, notamment le système d'exploitation, la consommation de mémoire, la version du
noeud et la version de l'interface de ligne de commande. Si le répertoire en cours est une application Cordova, des informations fournies par la commande `cordova info` sont également
affichées.

```
mfpdev info
```
{: codeblock}

Affiche le numéro de version de l'interface de ligne de commande {{site.data.keyword.mobilefoundation_short}} actuellement utilisée.

```
mfpdev -v
```
{: codeblock}

Mode débogage : génère une sortie de débogage.

```
mfpdev [-d|--debug]
```
{: codeblock}

Mode débogage prolixe : génère une sortie de débogage prolixe.

```
mfpdev [-dd|--ddebug]
```
{: codeblock}

Supprime l'utilisation de couleurs dans le résultat de la commande.

```
mfpdev -no-color
```
{: codeblock}

### mfpdev help
{: #mfpdev_help}

Affiche l'aide pour les commandes (mfpdev) de l'interface de ligne de commande MobileFirst. Avec le nom de la commande comme argument, elle affiche un texte d'aide plus spécifique pour chaque type de commande ou chaque commande. Exemple : `mfpdev help server add`.

```
mfpdev help <command name>
```
{: codeblock}


### mfpdev app
{: #mfpdev_app}

Enregistre votre application avec un serveur MobileFirst Server.

```
mfpdev app register
```
{: codeblock}

Pour enregistrer une application sur un serveur et un environnement d'exécution qui ne sont pas ceux par défaut, utilisez :

```
mfpdev app register <server> <runtime>
```
{: codeblock}

Pour la plateforme Windows Cordova, l'argument `-w <platform>` doit être ajouté à la commande. L'argument de plateforme est une liste des plateformes Windows à enregistrer, séparées par des virgules. Les valeurs valides sont windows, windows8 et windowsphone8.

```
mfpdev app register -w windows8
```
{: codeblock}

Permet de spécifier le serveur de back end et le contexte d'exécution à utiliser pour votre application. De plus, pour les
applications Cordova, permet de configurer plusieurs aspects supplémentaires tels que la langue par défaut pour les messages du système ou l'exécution d'un
contrôle de sécurité de total de contrôle. D'autres paramètres de configuration sont inclus pour les applications Cordova.

```
mfpdev app config
```
{: codeblock}

Extrait une configuration d'application existante depuis le serveur.

```
mfpdev app pull
```
{: codeblock}

Envoie une configuration d'application au serveur.

```
mfpdev app push
```
{: codeblock}

Permet de prévisualiser votre application Cordova sans terminal réel présentant le type de plateforme cible. Vous pouvez afficher l'aperçu dans le simulateur de navigateur mobile ou dans votre navigateur Web.

```
mfpdev app preview
```
{: codeblock}

Conditionne les ressources d'application qui se trouvent dans le répertoire www dans un fichier .zip pouvant être utilisé pour le
processus de mise à jour directe.

```
mfpdev app webupdate
```
{: codeblock}

Cette commande conditionne les ressources web mises à jour dans un fichier .zip et les télécharge sur le serveur par défaut MobileFirst Server enregistré. Ce package de ressources web se trouve dans le dossier `[cordova-project-root-folder]/mobilefirst/`.

Pour télécharger les ressources web sur une instance de serveur différente, fournissez le nom du serveur et l'environnement d'exécution en tant qu'élément de la commande :

```
mfpdev app webupdate <server_name> <runtime>
```
{: codeblock}

Vous pouvez utiliser le paramètre –build pour générer le fichier .zip avec le package de ressources web sans le télécharger sur un serveur.
```
mfpdev app webupdate --build
```
{: codeblock}

Pour télécharger un package qui a été généré précédemment, utilisez le paramètre –file :
```
mfpdev app webupdate --file mobilefirst/com.ibm.test-android-1.0.0.zip
```
{: codeblock}

Il existe aussi une option permettant de chiffrer le contenu du package en utilisant le paramètre –encrypt :
```
mfpdev app webupdate --encrypt
```
{: codeblock}


### mfpdev server
{: #mfpdev_server}

Affiche des informations relatives au serveur MobileFirst Server.

```
mfpdev server info
```
{: codeblock}

Ajoute une nouvelle définition de serveur dans votre environnement.

```
mfpdev server add
```
{: codeblock}

Permet d'éditer une définition de serveur.

```
mfpdev server edit
```
{: codeblock}

Pour définir un serveur en tant que serveur par défaut, utilisez :

```
mfpdev server edit <server_name> --setdefault
```
{: codeblock}

Supprime une définition de serveur de votre environnement.

```
mfpdev server remove
```
{: codeblock}

Ouvrez MobileFirst Operations Console.

```
mfpdev server console
```
{: codeblock}

Pour ouvrir la console d'un autre serveur, fournissez le nom du serveur en tant que paramètre pour la commande :

```
mfpdev server console <server-name>
```
{: codeblock}

Annule l'enregistrement des applications et retire les adaptateurs du serveur MobileFirst Server.

```
mfpdev server clean
```
{: codeblock}

### mfpdev adapter
{: #mfpdev_adapter}

Crée un adaptateur.

```
mfpdev adapter create
```
{: codeblock}

Génère un adaptateur.

```
mfpdev adapter build
```
{: codeblock}

Détecte et génère tous les adaptateurs dans le répertoire actuel et ses sous-répertoires.

```
mfpdev adapter build all
```
{: codeblock}

Déploie un adaptateur sur le serveur MobileFirst Server.

```
mfpdev adapter deploy
```
{: codeblock}

Pour déployer sur un serveur différent, utilisez :
```
mfpdev adapter deploy <server_name>
```
{: codeblock}

Détecte tous les adaptateurs dans le répertoire actuel et ses sous-répertoires et les déploie sur le serveur MobileFirst Server.

```
mfpdev adapter deploy all
```
{: codeblock}

Appelle une procédure d'adaptateur sur le serveur MobileFirst Server.

```
mfpdev adapter call
```
{: codeblock}

Extrait une configuration d'adaptateur existante à partir du serveur.
```
mfpdev adapter pull
```
{: codeblock}

Envoie une configuration d'adaptateur au serveur.
```
mfpdev adapter push
```
{: codeblock}

## Mise à jour et désinstallation de l'interface de ligne de commande {{site.data.keyword.mobilefoundation_short}}
{: #update_uninstall_mf_cli}

Pour mettre à jour l'interface de ligne de commande, exécutez la commande :

```
npm update -g mfpdev-cli
```
{: codeblock}

Pour désinstaller l'interface de ligne de commande, exécutez la commande :

```
npm uninstall -g mfpdev-cli
```
{: codeblock}
