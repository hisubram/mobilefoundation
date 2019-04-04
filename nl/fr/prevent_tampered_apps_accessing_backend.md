---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-19"

keywords: mobile foundation security, restrict backend access, tampered apps

subcollection:  mobilefoundation
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

# Empêcher les applications falsifiées d'accéder au back end
{: #prevent_tampered_apps_accessing_backend}

L'authenticité de l'application aide à vérifier la validité de l'application avant d'activer un service, ce qui permet d'empêcher qu'une application falsifiée accède aux services de back end.
{: shortdesc}

Pour sécuriser correctement votre application, activez le contrôle de sécurité de l'authenticité de l'application MobileFirst prédéfini (``appAuthenticity``). Lorsqu'il est activé, ce contrôle valide l'authenticité
de l'application avant de mettre des services à sa disposition. Cette fonction doit être activée pour les applications qui se trouvent dans un environnement de production.

Pour activer l'authenticité de l'application, vous pouvez suivre les instructions à l'écran dans **MobileFirst Operations Console → [votre_application] → Authenticité**, ou vous reporter aux informations ci-dessous.

* **Disponibilité**

    L'authenticité de l'application est disponibles sur toutes les plateformes prises en charge (iOS, Android, Windows 8.1 Universal, Windows 10 UWP) dans les applications Cordova et natives.

* **Limitations**

    L'authenticité de l'application ne prend pas en charge le **Bitcode** sous iOS. Par conséquent, avant d'utiliser l'authenticité de l'application, désactivez le Bitcode dans les propriétés de projet Xcode.

## Flux d'authenticité de l'application
{: #appauthenticityflow}

Le contrôle de sécurité de l'authenticité de l'application est exécuté au cours de l'enregistrement de l'application sur le serveur MobileFirst, qui a lieu lorsqu'une instance de l'application tente pour la première fois de se connecter au serveur. Par défaut, le contrôle d'authenticité n'est pas exécuté plusieurs fois.

Une fois que l'authenticité de l'application est activée et si le client doit apporter des modifications à son application, la version de l'application doit être mise à niveau.

Voir [Configuration de l'authenticité de l'application](#configappauthenticity) pour en savoir plus sur la personnalisation de ce comportement.

### Activation de l'authenticité de l'application
{: #enableappauthenticity}

Pour activer l'authenticité de l'application dans votre application :

1. Ouvrez MobileFirst Operations Console dans le navigateur de votre choix.
2. Sélectionnez votre application dans la barre latérale de navigation et cliquez sur l'élément de menu **Authenticité**.
3. Activez le bouton **Activé/Désactivé** dans la zone **Statut**.

![Activation de l'authenticité de l'application](/images/enable_application_authenticity.png)

Le serveur MobileFirst valide l'authenticité de l'application lors de la première tentative de connexion au serveur. Pour appliquer également cette validation aux ressources protégées, ajoutez le contrôle de sécurité appAuthenticity à la portée de protection.

### Désactivation de l'authenticité de l'application
{: #disableappauthenticity}

Certaines modifications apportées à l'application au cours du développement peuvent entraîner l'échec de la validation de l'authenticité. Par conséquent, il est recommandé de désactiver l'authenticité de l'application au cours du processus de développement. Cette fonction doit être activée pour les applications qui se trouvent dans un environnement de production.

Pour désactiver l'authenticité de l'application, redésactivez le bouton **Activé/Désactivé** dans la zone **Statut**.

## Configuration de l'authenticité de l'application
{: #configappauthenticity}

Par défaut, l'authenticité de l'application est vérifiée uniquement au cours de l'enregistrement du client. Toutefois, à l'instar de tout autre contrôle de sécurité, vous pouvez décider de protéger votre application ou vos ressources avec le contrôle de sécurité ``appAuthenticity`` depuis la console, en suivant les instructions figurant dans la section Protection des ressources.

Vous pouvez configurer le contrôle de sécurité de l'authenticité de l'application avec la propriété suivante :

* ``expirationSec`` : la valeur par défaut est 3600 secondes/1 heure. Définit le délai d'expiration du jeton d'authenticité.

Une fois qu'un contrôle d'authenticité a été effectué, il n'est pas effectué à nouveau tant que le jeton n'est pas arrivé à expiration, conformément à cette valeur.

Pour configurer la propriété ``expirationSec`` :

1. Chargez MobileFirst Operations Console, accédez à **[votre_application] → Sécurité → Configurations du contrôle de sécurité**, et cliquez sur **Nouveau**.
2. Recherchez l'élément de portée ``appAuthenticity``.
3. Définissez une nouvelle valeur en secondes.

    ![Configuration de l'expiration en nombre de secondes](/images/configuring_expirationSec.png)

## Build Time Secret (BTS)
{: #buildtimesecret}

Build Time Secret (BTS) est un **outil facultatif permettant d'améliorer la validation de l'authenticité** pour les applications iOS seulement. Il injecte l'application avec un secret déterminé au moment de la génération, qui est utilisé ultérieurement dans le processus de validation de l'authenticité.

Vous pouvez télécharger l'outil BTS depuis **MobileFirst Operations Console → Centre de téléchargement**.

Pour utiliser l'outil BTS dans Xcode :

1. Dans l'onglet **Build Phases**, cliquez sur le bouton **+** et créez un élément **Run Script Phase**.
2. Copiez le chemin d'accès à l'outil BTS et collez-le dans l'élément Run Script Phase que vous avez créé.
3. Faites glisser l'élément Run Script Phase au-dessus de **Compile sources phase**.

Vous devez utiliser l'outil lors de la génération d'une version de production de l'application.

## Traitement des incidents liés à l'authenticité de l'application
{: #troubleshooting-app-authenticity}

### Réinitialisation
{: #trblreset}

L'algorithme d'authenticité de l'application utilise des données et des métadonnées d'application au cours de sa validation. Le premier appareil à se connecter au serveur après l'activation de l'authenticité de l'application fournit une .empreinte. de l'application contenant certaines de ces données.

Il est possible de réinitialiser cette empreinte en fournissant de nouvelles données à l'algorithme. Cette opération peut être utile au cours du développement (par exemple, après la modification de l'application dans Xcode). Pour réinitialiser l'empreinte, utilisez la commande **reset** depuis l'interface de ligne de commande mfpadm.

Après la réinitialisation de l'empreinte, le contrôle de sécurité appAuthenticity continue de fonctionner comme avant (la réinitialisation est transparente pour l'utilisateur).

### Types de validation
{: #trblvalidationtypes}

Mobile First Platform Foundation fournit l'authenticité de l'application statique et dynamique pour les applications. Selon le type de validation, l'algorithme et les attributs utilisés pour générer les points de départ de l'authenticité de l'application diffèrent. Par défaut, lorsque l'authenticité de l'application est activée, elle utilise l'algorithme de validation dynamique. Les deux types de validation assurent la sécurité de l'application. L'authenticité de l'application dynamique utilise des validations strictes et vérifie l'authenticité de l'application. Pour l'authenticité de l'application statique, nous utilisons un algorithme légèrement souple qui n'applique pas tous les contrôles de validation appliqués par l'authenticité de l'application dynamique.

L'authenticité de l'application dynamique peut être configurée depuis la console MobileFirst. L'algorithme interne génère les données d'authenticité de l'application en fonction des options choisies dans la console. Pour l'authenticité de l'application statique, vous devez utiliser l'interface de ligne de commande mfpadm.

Pour activer l'authenticité de l'application statique et passer d'un type de validation à l'autre, utilisez l'interface de ligne de commande mfpadm :

```bash
mfpadm --url=  --user=  --passwordfile= --secure=false app version [CONTEXTE_EXECUTION] [NOM_APPLICATION] [ENVIRONNEMENT] [VERSION] set authenticity-validation TYPE
```
{: codeblock}

La valeur de TYPE peut être dynamic ou static.
