---

copyright:
  years: 2018, 2019
lastupdated: "2018-10-16"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}

# Ressources brouillées
{: #obfuscate_resources}

Le brouillage est le processus qui consiste à modifier un fichier exécutable pour qu'il ne puisse plus servir à un pirate informatique tout en restant entièrement opérationnel. Le brouillage de code automatisé rend difficile l'application de l'ingénierie inverse à un programme. 

Grâce au brouillage, il devient beaucoup plus difficile d'appliquer l'ingénierie inverse à une application protégée contre le vol de secret commercial (propriété intellectuelle), tout accès non autorisé, le contournement de l'octroi de licence et d'autres contrôles, et la vulnérabilité.

Le brouillage du code compte de nombreuses techniques différentes, notamment des techniques relatives à la sécurité des applications :

* Brouillage par changement de nom : le changement de nom modifie le nom des méthodes et des variables et rend la source décompilée plus difficile à comprendre pour un individu, sans modifier l'exécution de l'application. Il s'agit de la méthode de brouillage préférée des brouilleurs .NET, iOS, Java et Android. 
* Brouillage des chaînes
* Brouillage des flux de contrôle
* Transformation des modèles d'instruction
* Insertion de code factice
* Anti-piratage, etc.

Le brouillage complique la révision du code et l'analyse de l'application pour les agresseurs informatiques. Divers outils de tiers sont disponibles pour le brouillage.

* Pour le brouillage d'une application android, consultez ce [blogue](https://mobilefirstplatform.ibmcloud.com/blog/2016/09/19/mfp-80-obfuscating-android-code-with-proguard/).
    >**Remarque** : vous devez créer le fichier de configuration (proguard-project.txt) pour le brouillage Android ProGuard avec une application MobileFirst Android.

* Pour le brouillage de base d'une application Cordova, voir [Chiffrement des ressources Web de vos packages Cordova](#encryptingcordovapackage).

## Chiffrement des ressources Web de vos packages Cordova
{: #encryptingcordovapackage}

Afin de limiter le risque d'affichage et de modification de vos ressources Web par un tiers alors qu'elles se trouvent dans le package ``.apk`` ou ``.ipa``, vous pouvez utiliser la commande mfpdev app webencrypt de l'interface de ligne de commande de MobileFirst ou l'indicateur mfpwebencrypt pour chiffrer les informations. Cette procédure n'assure pas un chiffrement impossible à décoder, mais fournit un niveau basique de brouillage.

**Prérequis** :

* Les outils de développement Cordova doivent être installés. Cet exemple utilise l'interface de ligne de commande d'Apache Cordova. Si vous utilisez d'autres outils de développement Cordova, certaines des étapes seront différentes. Reportez-vous à la documentation de l'outil Cordova pour des instructions.
* L'interface de ligne de commande de MobileFirst doit être installée.
* Le plug-in MobileFirst Cordova doit être installé.

Il est préférable de suivre cette procédure une fois le développement de votre application terminé, et lorsque vous êtes prêt à la déployer. Si vous exécutez l'une des commandes suivantes une fois la procédure de chiffrement des ressources Web appliquée, le contenu qui était chiffré est déchiffré :

* cordova prepare
* cordova build
* cordova run
* cordova emulate
* mfpdev app webupdate
* mfpdev app preview

Si vous exécutez l'une des commandes répertoriées après avoir chiffré les ressources Web, vous devez suivre à nouveau la procédure ci-dessous pour chiffrer les ressources Web.

1. Ouvrez une fenêtre de terminal et accédez au répertoire racine de l'application Cordova à chiffrer.
2. Préparez l'application en entrant l'une des commandes suivantes :
    * ``cordova prepare``
    * ``mfpdev app webupdate``
3. Suivez l'une des procédures ci-dessous pour chiffrer le contenu :
    * Entrez la commande suivante : ``mfpdev app webencrypt``.
        >**Astuce** : vous pouvez afficher les informations sur la commande ``mfpdev app webencrypt`` en entrant ``mfpdev help app webencrypt``.
    * Vous pouvez aussi chiffrer les ressources Web de vos packages Cordova en ajoutant l'indicateur ``mfpwebencrypt`` à la commande
``cordova compile`` ou ``cordova build`` lorsque vous générez vos packages.
       * ``cordova compile -- --mfpwebencrypt`` | ``cordova build -- --mfpwebencrypt``
            Les informations de système d'exploitation dans le dossier **www** sont remplacées par un fichier **resources.zip** comportant le contenu chiffré.
            Si votre application a été conçue pour le système d'exploitation Android et que la taille du fichier **resources.zip** est supérieure à 1 Mo, le fichier **resources.zip** est divisé en fichiers plus petits de 768 ko nommés **resources.zip.nnn**. La variable nnn est un nombre compris entre 001 et 999.
4. Testez l'application avec les ressources chiffrées en utilisant l'émulateur fourni avec les outils propres à la plateforme. Par exemple, vous pouvez utiliser l'émulateur dans Android Studio pour Android, ou Xcode pour iOS.

>**Remarque** : n'utilisez pas les commandes Cordova suivantes pour tester l'application après l'avoir chiffrée :

>* ``cordova run``
>* ``cordova emulate``

>Ces commandes actualisent le contenu qui a été chiffré dans le dossier www et les sauvegarde à nouveau sous forme de contenu non chiffré. Si vous les utilisez, pensez à suivre à nouveau la procédure de chiffrement du contenu avant de publier l'application.
