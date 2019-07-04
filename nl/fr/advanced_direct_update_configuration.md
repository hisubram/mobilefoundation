---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: Direct Update, CDN support, secure direct update

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Configuration avancée de la mise à jour directe
{: #advanced_direct_update_configuration}

Vous trouverez ci-dessous une description des moyens avancés permettant de configurer et d'utiliser la fonction de Mise à jour directe.

## Personnalisation de l'interface utilisateur Mise à jour directe
{: #customize_du_ui}

L'interface utilisateur Mise à jour directe présentée à l'utilisateur final peut être personnalisée.
Ajoutez le code suivant à la fonction `wlCommonInit()` dans **index.js** :
```JavaScript
wl_DirectUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext) {
    // Implement custom Direct Update logic
};
```
{: codeblock}

*directUpdateData* est un objet JSON contenant la propriété downloadSize qui représente la taille de fichier (en octets) du package de mise à niveau à télécharger depuis le serveur Mobile Foundation.
*directUpdateContext* est un objet JavaScript exposant les fonctions .start() et .stop(), qui démarre et arrête le flux Mise à jour directe.

Si les ressources web sont plus récentes sur le serveur Mobile Foundation que dans l'application, les données de demande d'authentification de mise à jour directe sont ajoutées dans la réponse du serveur. Chaque fois que l'infrastructure côté client Mobile Foundation détecte cette demande d'authentification de mise à jour directe, elle appelle la fonction `wl_directUpdateChallengeHandler.handleDirectUpdate`.

La fonction fournit un style Mise à jour directe par défaut : elle propose une boîte de dialogue de message par défaut qui est affichée quand une mise à jour directe est disponible ainsi qu'un écran de progression par défaut qui apparaît au lancement du processus de mise à jour directe. Vous pouvez implémenter un comportement d'interface utilisateur Mise à jour directe personnalisé ou personnaliser la boîte de dialogue Mise à jour directe en remplaçant cette fonction et en implémentant votre propre logique.

Dans l'exemple de code suivant, une fonction `handleDirectUpdate` implémente un message personnalisé dans la boîte de dialogue Mise à jour directe. Ajoutez ce code dans le fichier `www/js/index.js` du projet Cordova.
Autres exemples d'interface utilisateur Mise à jour directe personnalisée :
* Boîte de dialogue qui est créée en utilisant une infrastructure JavaScript tiers (comme Dojo ou jQuery Mobile, Ionic, …)
* Interface utilisateur entièrement native en exécutant un plug-in Cordova
* Autre HTML qui est présenté à l'utilisateur avec des options et autres.

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext) {        
    navigator.notification.confirm(  // Creates a dialog.
        'Custom dialog body text',
        // Handle dialog buttons.
          directUpdateContext.start();
        },
        'Custom dialog title text',
        ['Update']
    );
};
```
{: codeblock}

Vous pouvez démarrer le processus Mise à jour directe en exécutant la méthode `directUpdateContext.start()` chaque fois que l'utilisateur clique sur le bouton dans la boîte de dialogue. L'écran de progression par défaut qui ressemble à celui des versions précédentes du serveur Mobile Foundation s'affiche.

Cette méthode prend en charge les types d'appel suivants :
* Si aucun paramètre n'est spécifié, le serveur Mobile Foundation utilise l'écran de progression par défaut.
* Quand une fonction de programme d'écoute telle `directUpdateContext.start(directUpdateCustomListener)` est fournie, le processus Mise à jour directe s'exécute en arrière-plan tandis que le processus envoie des événements de cycle de vie au programme d'écoute. L'écouteur personnalisé doit implémenter les méthodes suivantes :

```JavaScript
var directUpdateCustomListener = {
    onStart : function ( totalSize ){ },
    onProgress : function ( status , totalSize , completedSize ){ },
    onFinish : function ( status ){ }
};
```
{: codeblock}

Les méthodes d'écouteur sont démarrées au cours du processus de mise à jour directe selon les règles suivantes :
* `onStart` est appelé avec le paramètre `totalSize` qui contient la taille du fichier de mise à jour.
* `onProgress` est appelé plusieurs fois avec le statut
`DOWNLOAD_IN_PROGRESS`, `totalSize`, et
`completedSize` (le volume qui est téléchargé jusqu'à présent).
* `onProgress` est appelé avec le statut `UNZIP_IN_PROGRESS`.
* `onFinish` est appelé avec l'un des codes de statut finaux suivants :

| Code de statut | Description |
|:------------|:------------|
| `SUCCESS` | Mise à jour directe terminée sans erreur. |
| `CANCELED` | La mise à jour directe a été annulée (par exemple, parce que la méthode `stop()` a été appelée). |
| `FAILURE_NETWORK_PROBLEM` | Un problème est survenu lors d'une connexion réseau au cours de la mise à jour. |
| `FAILURE_DOWNLOADING` | Le fichier n'a pas été téléchargé complètement. |
| `FAILURE_NOT_ENOUGH_SPACE` | Il n'y a pas suffisamment d'espace sur l'appareil pour télécharger et décompresser le fichier de mise à jour. |
| `FAILURE_UNZIPPING` | Un problème est survenu lors de la décompression du fichier de mise à jour. |
| `FAILURE_ALREADY_IN_PROGRESS` | La méthode de démarrage a été appelée alors que la mise à jour directe était déjà en cours d'exécution. |
| `FAILURE_INTEGRITY` | L'authenticité du fichier de mise à jour ne peut pas être vérifiée. |
| `FAILURE_UNKNOWN` | Erreur interne inattendue. |
{: caption="Tableau 1. Codes de statut" caption-side="top"}

Si vous implémentez un programme d'écoute de mise à jour directe personnalisée, vous devez vous assurer que l'application mobile est rechargée lorsque le processus de mise à jour directe est terminé et la méthode `onFinish()` a été appelée. Vous devez également appeler `wl_directUpdateChalengeHandler.submitFailure()` si le processus de mise à jour directe a échoué.

L'exemple suivant montre l'implémentation d'un programme d'écoute de mise à jour directe personnalisée :
```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
    //show custom progress dialog
  },
  onProgress: function(status,totalSize,completedSize){
    //update custom progress dialog
  },
  onFinish: function(status){

    if (status == 'SUCCESS'){
      //show success message
      WL.Client.reloadApp();
    }
    else {
      //show custom error message

      //submitFailure must be called is case of error
      wl_directUpdateChallengeHandler.submitFailure();
    }
  }
};

wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  WL.SimpleDialog.show('Update Avalible', 'Press update button to download version 2.0', [{
    text : 'update',
    handler : function() {
      directUpdateContext.start(directUpdateCustomListener);
    }
  }]);
};
```
{: codeblock}

## Exécution de mises à jour directes sans interface utilisateur
{: #scenario-running-ui-less-direct-updates }
{{site.data.keyword.mobilefoundation_short}}
prend en charge la mise à jour directe sans interface utilisateur lorsque l'application est au premier plan.

Pour exécuter des mises à jour directes sans interface utilisateur, implémentez `directUpdateCustomListener`. Indiquez les implémentations de fonctions vides dans les méthodes `onStart` et `onProgress`. Les implémentations vides causent l'exécution du processus de mise à jour directe en arrière-plan.

Pour terminer le processus de mise à jour directe, l'application doit être rechargée. Les options suivantes sont disponibles :
* La méthode `onFinish` peut aussi être vide. Dans ce cas, la mise à jour directe s'applique après le redémarrage de l'application.
* Vous pouvez implémenter une boîte de dialogue personnalisée qui informe ou impose à l'utilisateur de redémarrer l'application. (Voir l'exemple suivant.)
* La méthode `onFinish` peut appliquer un rechargement de l'application en appelant
`WL.Client.reloadApp()`.

Voici un exemple d'implémentation de `directUpdateCustomListener` :

```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
  },
  onProgress: function(status,totalSize,completeSize){
  },
  onFinish: function(status){
    WL.SimpleDialog.show('New Update Available', 'Press reload button to update to new version', [ {
      text : WL.ClientMessages.reload,
      handler : WL.Client.reloadApp
    }]);
  }
};
```
{: codeblock}

Implémentez la fonction `wl_directUpdateChallengeHandler.handleDirectUpdate`. Transmettez l'implémentation de `directUpdateCustomListener` que vous avez créée en tant que paramètre à la fonction. Assurez-vous que `directUpdateContext.start(directUpdateCustomListener`) est appelé. Voici un exemple d'implémentation de `wl_directUpdateChallengeHandler.handleDirectUpdate` :

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  directUpdateContext.start(directUpdateCustomListener);
};
```
{: codeblock}

Lorsque l'application est transmise à l'arrière-plan, le processus de mise à jour directe est suspendu.
{: note}

## Traitement d'un échec de mise à jour directe
{: #scenario-handling-a-direct-update-failure }
Cette section montre comment gérer un échec de mise à jour directe qui pourrait se produire en raison, par exemple, de la perte de connectivité. Dans ce scénario, l'utilisateur est empêché d'utiliser l'application mobile y compris en mode hors ligne. Une boîte de dialogue est affichée offrant à l'utilisateur la possibilité d'essayer à nouveau.

1.  Créez une variable globale pour stocker le contexte de mise à jour directe de sorte que vous pouvez l'utiliser ultérieurement lorsque le processus de mise à jour directe échoue. Exemple :
    ```JavaScript
    var savedDirectUpdateContext;
    ```
    {: codeblock}

2.  Implémentez un gestionnaire de demandes d'authentification de mise à jour directe. Enregistrez le contexte de mise à jour directe ici. Exemple :
    ```JavaScript
    wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

      savedDirectUpdateContext = directUpdateContext; // save direct update context

      var downloadSizeInMB = (directUpdateData.downloadSize / 1048576).toFixed(1).replace(".", WL.App.getDecimalSeparator());
  var directUpdateMsg = WL.Utils.formatString(WL.ClientMessages.directUpdateNotificationMessage, downloadSizeInMB);

      WL.SimpleDialog.show(WL.ClientMessages.directUpdateNotificationTitle, directUpdateMsg, [{
        text : WL.ClientMessages.update,
    handler : function() {
          directUpdateContext.start(directUpdateCustomListener);
    }
      }]);
    };
    ```
    {: codeblock}

3.  Créez une fonction qui lance le processus de mise à jour directe en utilisant le contexte de la mise à jour directe. Exemple :
    ```JavaScript
    restartDirectUpdate = function () {
      savedDirectUpdateContext.start(directUpdateCustomListener); // use saved direct update context to restart direct update
};
    ```
    {: codeblock}

4.  Implémentez `directUpdateCustomListener`. Ajoutez le contrôle de statut dans la méthode
`onFinish`. Si le statut commence par `FAILURE`, ouvrez une boîte de dialogue modale uniquement avec l'option **Try Again**. Exemple :
    ```JavaScript
    var directUpdateCustomListener = {
      onStart: function(totalSize){
    alert('onStart: totalSize = ' + totalSize + 'Byte');
  },
  onProgress: function(status,totalSize,completeSize){
    alert('onProgress: status = ' + status + ' completeSize = ' + completeSize + 'Byte');
  },
  onFinish: function(status){
    alert('onFinish: status = ' + status);
    var pos = status.indexOf("FAILURE");
    if (pos > -1) {
          WL.SimpleDialog.show('Update Failed', 'Press try again button', [ {
            text : "Try Again",
        handler : restartDirectUpdate // restart direct update
      }]);
    }
      }
    };
    ```
    {: codeblock}

    Lorsque l'utilisateur clique sur le bouton **Réessayez**, l'application redémarre le processus de mise à jour directe.
    {: note}

## Mise à jour delta et complète
{: #delta-and-full-direct-update }
Les mises à jour directe delta permettent à une application de ne télécharger que les fichiers qui ont été modifiés depuis la dernière mise à jour au lieu de l'ensemble des ressources web de l'application, ce qui a l'avantage de réduire le temps de téléchargement, de préserver la bande passante et d'améliorer l'expérience utilisateur globale.

Une **mise à jour delta** n'est possible que si les ressources web de l'application client sont une version derrière l'application qui est actuellement déployée sur le serveur. Les applications client qui sont plusieurs versions en arrière par rapport à l'application déployée actuellement (ce qui signifie que l'application a été déployée sur le serveur au moins deux fois depuis la mise à jour de l'application client), reçoivent une **mise à jour complète** (l'intégralité des ressources web sont dans ce cas déployées et mises à jour).
{: note}

Voir l'exemple Mise à jour directe pour l'application Cordova dans la section **Exemples**. Cette application montre comment créer une boîte de dialogue Mise à jour directe personnalisée à la place de la boîte de dialogue fournie par défaut.  

## Prise en charge CDN
{: #cdn_support}

Vous pouvez configurer des demandes Mise à jour directe afin qu'elles soient servies depuis un réseau CDN (Content Delivery Network) au lieu du serveur Mobile Foundation.

L'utilisation d'un réseau CDN à la place du serveur Mobile Foundation pour servir des demandes Mise à jour directe offre les avantages suivants :

* Elle retire la charge qui incombait au serveur Mobile Foundation.
* Elle augmente les taux de transfert supérieurs à la limite de 250 Mo/seconde lors du traitement des demandes en provenance d'un serveur Mobile Foundation.
* Elle assure une expérience de mise à jour directe plus uniforme pour tous les utilisateurs, quel que soit leur emplacement géographique.

### Exigences générales
{: #general-requirements }
Pour mettre à disposition des demandes de mise à jour directe depuis un réseau CDN, vérifiez que votre configuration remplit les conditions suivantes :

* Le réseau CDN doit être un proxy inverse qui se trouve devant le serveur Mobile Foundation (ou devant un autre proxy inverse, si nécessaire).
* Quand vous générez l'application depuis votre environnement de développement, configurez votre serveur cible sur le port et l'hôte CDN au lieu du port et de l'hôte du serveur Mobile Foundation. Ainsi, quand vous exécutez la commande d'interface de ligne de commande Mobile Foundation `mfpdev server add`, fournissez le port et l'hôte CDN.
* Dans le panneau d'administration CDN, vous devez marquer les URL de mise à jour directe ci-après pour une mise en cache afin de vous assurer que le réseau CDN transmet toutes les demandes au serveur Mobile Foundation, à l'exception des demandes de Mise à jour directe. Pour celles-ci, le réseau CDN détermine s'il a obtenu le contenu. Si tel est le cas, il le renvoie sans passer par le serveur Mobile Foundation ; dans le cas contraire, il accède au serveur Mobile Foundation, obtient l'archive Mise à jour directe (fichier .zip) et la stocke pour les demandes suivantes relatives à cette URL spécifique. Pour les applications qui sont générées avec la version v8.0 de {{site.data.keyword.mobilefoundation_short}}, l'URL Mise à jour directe est : `PROTOCOL://DOMAIN:PORT/CONTEXT_PATH/api/directupdate/VERSION/CHECKSUM/TYPE`.
Le préfixe `PROTOCOL://DOMAIN:PORT/CONTEXT_PATH` est constant pour toutes les demandes d'exécution. Exemple : `http://my.cdn.com:9080/mfp/api/directupdate/0.0.1/742914155/full?appId=com.ibm.DirectUpdateTestApp&clientPlatform=android`

Dans
l'exemple, des paramètres de demande supplémentaires, qui font partie de la demande, ont été ajoutés.

* Le réseau CDN doit autoriser la mise en cache des paramètres de demande. Deux archives de mise à jour directe ne peuvent
être différentes que parce qu'elles comportent des paramètres de demande différents.
* Le réseau CDN doit prendre en charge la durée de vie dans la réponse de mise à jour directe. Cette prise en charge est
nécessaire pour que plusieurs mises à jour directes puissent être effectuées pour la même version.
* Le réseau CDN ne doit pas changer ou retirer les en-têtes HTTP qui sont utilisés dans le protocole serveur-client.

### Configuration CDN exemple
{: #example-cdn-configuration }
Cet exemple repose sur l'utilisation d'une configuration de réseau CDN Akamai
qui met en cache l'archive de mise à jour directe. Les tâches suivantes sont effectuées par l'administrateur réseau, l'administrateur Mobile Foundation et l'administrateur Akamai :

#### Administrateur réseau
{: #network-administrator }
Crée un autre domaine dans le système de noms de domaine pour votre serveur Mobile Foundation. Ainsi, si votre domaine de serveur
est `yourcompany.com`, vous devez créer un domaine supplémentaire, par exemple `cdn.yourcompany.com`.
Dans le système de noms de domaine pour votre nouveau domaine `cdn.yourcompany.com`, définissez un nom `CNAME` dans le nom de domaine qui est fourni par Akamai. Exemple : `yourcompany.com.akamai.net`.

#### Administrateur Mobile Foundation
{: #mobilefoundation-administrator }
Définit le nouveau domaine `cdn.yourcompany.com` comme URL du serveur Mobile Foundation pour les applications Mobile Foundation. Ainsi, pour la tâche de générateur Ant, la propriété est :
```xml
<property name="wl.server" value="http://cdn.yourcompany.com/${contextPath}/"/>
```
{: codeblock}

#### Administrateur Akamai
{: #akamai-administrator }
1. Ouvrez le gestionnaire de propriétés Akamai et définissez la propriété **host name** sur la valeur du nouveau domaine.

    ![Définition de la propriété host name sur la valeur du nouveau domaine](images/direct_update_cdn_3.jpg "Définition de la propriété host name sur la valeur du nouveau domaine")

2. Sur l'onglet Default Rule, configurez l'hôte et le port d'origine du serveur Mobile Foundation et définissez la valeur **Custom Forward Host Header** sur le nouveau domaine.

    ![Définition de la valeur Custom Forward Host Header sur le domaine nouvellement créé](images/direct_update_cdn_4.jpg "Définition de la valeur Custom Forward Host Header sur le domaine nouvellement créé")

3. Dans la liste **Caching Option**, sélectionnez **No
Store**.

    ![Dans la liste Caching Option, sélectionnez No Store](images/direct_update_cdn_5.jpg "Dans la liste Caching Option, sélectionnez No Store")

4. Dans l'onglet **Static Content configuration**, configurez les critères de mise en correspondance en fonction de l'URL de mise à jour directe de l'application. Ainsi, créez une condition qui stipule `If Path matches one of direct_update_URL`.

    ![Configuration des critères de mise en correspondance en fonction de l'URL de mise à jour directe de l'application](images/direct_update_cdn_6.jpg "Configuration des critères de mise en correspondance en fonction de l'URL de mise à jour directe de l'application")

5. Configurez le comportement de clé de cache pour utiliser tous les paramètres de demande dans la clé de cache (nécessaire pour mettre en cache
différentes archives de mise à jour directe pour différentes applications ou versions). Par exemple,
dans la liste **Behavior**, sélectionnez `Include all parameters (preserve
order from request)`.

    ![Configuration du comportement de la clé de cache de manière à utiliser tous les paramètres de demande dans la clé de cache](images/direct_update_cdn_8.jpg "Configuration du comportement de la clé de cache de manière à utiliser tous les paramètres de demande dans la clé de cache")

6. Définissez des valeurs similaires aux valeurs ci-après afin de configurer le comportement de mise en cache pour mettre en cache l'adresse URL de mise à
jour directe et pour définir la durée de vie.

      ![Définition des valeurs de configuration du comportement de mise en cache](images/direct_update_cdn_7.jpg "Définition des valeurs de configuration du comportement de mise en cache")

| Zone | Valeur |
|:------|:------|
| Caching Option | Cache |
| Force Revaluation of Stale Objects | Mise à disposition périmée si validation impossible |
| Max-Age | 3 minutes |
{: caption="Tableau 2. Zones et valeurs pour la configuration du comportement de mise en cache" caption-side="top"}

## Mise à jour directe sécurisée
{: #secure-dc }

Désactivée par défaut, la mise à jour directe sécurisée empêche que l'attaque d'un pirate tiers altère les ressources web qui sont transmises, depuis le serveur Mobile Foundation (ou à partir d'un réseau CDN) vers l'application client.

### Activation de l'authenticité de mise à jour directe
{: #enable-direct-update-authenticity}
En utilisant l'outil de votre choix, extrayez la clé publique depuis le magasin de clés du serveur Mobile Foundation et convertissez-le en base64.  
La valeur produite doit ensuite être utilisée comme indiqué dans les étapes suivantes :

1. Ouvrez une fenêtre de **ligne de commande** et accédez à la racine du projet Cordova.
2. Exécutez la commande `mfpdev app config` et sélectionnez l'option relative à la clé publique d'authenticité de mise à jour directe.
3. Fournissez la clé publique et confirmez.

Toutes les transmissions par mise à jour directe aux applications client seront protégées par l'authenticité de mise à jour directe.

Pour que la mise à jour directe puisse fonctionner, un fichier de magasin de clés défini par l'utilisateur doit être déployé sur le serveur Mobile Foundation et une copie de la clé publique correspondante doit être incluse dans l'application client déployée.

Cette rubrique explique comment lier une clé publique à de nouvelles applications client et à des applications client existantes qui ont été mises à
niveau. Pour plus d'informations sur la configuration du magasin de clés dans le serveur Mobile Foundation, voir [Configuring the Mobile Foundation server keystore![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/configuring-the-mobilefirst-server-keystore/){: new_window}

Le serveur fournit un magasin de clés intégré qui peut être utilisé pour tester la mise à jour directe sécurisée pour les phases de développement.

Une fois que vous avez lié la clé publique à l'application client et que vous avez régénéré l'application, vous n'avez pas besoin de la télécharger à nouveau sur le serveur {{ site.data.keys.mf_server }}. Toutefois, si vous avez déjà publié l'application sur le marché sans la clé publique, vous devez la republier.
{: note}

A des fins de développement, la clé publique factice par défaut suivante est fournie avec le serveur Mobile Foundation :

```xml
-----BEGIN PUBLIC KEY-----
MIIDPjCCAiagAwIBAgIEUD3/bjANBgkqhkiG9w0BAQsFADBgMQswCQYDVQQGEwJJTDELMAkGA1UECBMCSUwxETA
PBgNVBAcTCFNoZWZheWltMQwwCgYDVQQKEwNJQk0xEjAQBgNVBAsTCVdvcmtsaWdodDEPMA0GA1UEAxMGV0wgRG
V2MCAXDTEyMDgyOTExMzkyNloYDzQ3NTAwNzI3MTEzOTI2WjBgMQswCQYDVQQGEwJJTDELMAkGA1UECBMCSUwxE
TAPBgNVBAcTCFNoZWZheWltMQwwCgYDVQQKEwNJQk0xEjAQBgNVBAsTCVdvcmtsaWdodDEPMA0GA1UEAxMGV0wg
RGV2MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzQN3vEB2/of7KAvuvyoIt0T7cjaSTjnOBm0N3+q
zx++dh92KpNJXj/a3o4YbwJXkJ7jU8ykjCYvjXRf0hme+HGhiIVwxJo54iqh76skDS5m7DaseFdndZUJ4p7NFVw
I5ixA36ZArSZ/Pn/ej56/RRjBeRI7AEGXUSGojBUPA6J6DYkwaXQRew9l+Q1kj4dTigyKL5Os0vNFaQyYu+bT2E
vnOixQ0DXm94IqmHZamZKbZLrWcOEfuAsSjKYOdMSM9jkCiHaKcj7fpEZhUxRRs7joKs1Ri4ihs6JeUvMEiG4gK
l9V3FP/Huy0pfkL0F8xMHgaQ4c/lxS/s3PV0OEg+7wIDAQABMA0GCSqGSIb3DQEBCwUAA4IBAQAgEhhqRl2Rgkt
MJeqOCRcT3uyr4XDK3hmuhEaE0nOvLHi61PoLKnDUNryWUicK/W+tUP9jkN5xRckdzG6TJ/HPySmZ7Adr6QRFu+
xcIMY+/S8j4PHLXBjoqgtUMhkt7S2/thN/VA6mwZpw4Ol0Pa2hyT2TkhQoYYkRwYCk9pxmuBCoH/eCWpSxquNny
RwrY25x0YzccXUaMI8L3/3hzq3mW40YIMiEdpiD5HqjUDpzN1funHNQdsxEIMYsWmGAwOdV5slFzyrH+ErUYUFA
pdGIdLtkrhzbqHFwXE0v3dt+lnLf21wRPIqYHaEu+EB/A4dLO6hm+IjBeu/No7H7TBFm
-----END PUBLIC KEY-----
```
{: codeblock}

N'utilisez pas la clé publique à des fins de production.
{: note}

### Génération et déploiement du magasin de clés
{: #generating-and-deploying-the-keystore }
De nombreux outils sont disponibles pour la génération de certificats et l'extraction de clés publiques depuis un magasin de clés. L'exemple ci-dessous
illustre les procédures à suivre avec l'utilitaire de clé du kit Java Development Kit et OpenSSL.

1. Procédez à l'extraction de la clé publique depuis le fichier de magasin de clés qui est déployé sur le serveur {{ site.data.keys.mf_server }}.  
   La clé publique doit être codée en Base64.
   {: note}

   Par exemple, supposez que le nom d'alias est `mfp-server` et que le fichier de magasin de clés est **keystore.jks**.  
   Pour générer un certificat, émettez la commande suivante :

   ```bash
   keytool -export -alias mfp-server -file certfile.cert
   -keystore keystore.jks -storepass keypassword
   ```
   {: codeblock}

   Un fichier certificat est généré.  
   Emettez la commande suivante afin d'extraire la clé publique :

   ```bash
   openssl x509 -inform der -in certfile.cert -pubkey -noout
   ```
   {: codeblock}

   keytool seul ne peut pas extraire les clés publiques au format Base64.
   {: note}

2. Effectuez l'une des procédures suivantes :
    * Copiez le texte généré, sans les marqueurs `BEGIN PUBLIC KEY` et `END PUBLIC KEY`, dans le fichier de propriétés mfpclient de l'application, immédiatement après `wlSecureDirectUpdatePublicKey`.
    * A l'invite de commande, entrez le commande suivante : `mfpdev app config direct_update_authenticity_public_key <public_key>`

    Pour `<public_key>`, collez le texte résultant de l'étape 1, sans les marqueurs `BEGIN PUBLIC KEY` et `END PUBLIC KEY`.

3. Exécutez la commande cordova build afin de sauvegarder la clé publique dans l'application.
