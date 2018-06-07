---

copyright:
  years: 2018
lastupdated:  "2018-05-11"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

#	Mise à jour directe dans les applications Cordova
{: #direct_update_cordova_apps}

Les ressources Web (JavaScript, HTML, CSS ou fichiers image) des applications Cordova peuvent être mises à jour via *OTA (Over-The-Air)* avec la mise à jour directe. En utilisant le fonction Mise à jour directe, les entreprises peuvent s'assurer que l'utilisateur final se sert de la dernière version de leurs applications. Pour mettre à jour une application, les ressources web mises à jour de l'application doivent être conditionnées et téléchargées sur MobileFirst Server, en utilisant l'interface de ligne de commande MobileFirst ou en déployant un fichier archive généré. La mise à jour directe est activée automatiquement. Elle peut ensuite être appliquée sur chaque demande d'une ressource protégée effectuée par l'utilisateur.

La mise à jour directe est prise en charge par le système d'exploitation Cordova et sur les plateformes Cordova Android.

Aux fins de développement et de test, les développeurs utilisent généralement la mise à jour directe simplement en téléchargeant une archive sur le serveur de développement. Même si ce processus est facile à implémenter, il n'est pas sécurisé. Pour cette phase, une paire de clés RSA interne, extraite depuis un certificat autosigné MobileFirst intégré, est utilisée. 

Toutefois, pour la phase de test de pré-production ou de production, il est conseillé d'implémenter la mise à jour directe sécurisée avant de publier votre application dans le magasin d'applications. La mise à jour directe sécurisée nécessite une paire de clés RSA extraite d'un certificat de serveur réel, signé par une autorité de certification (AC).

* Prenez soin de ne pas modifier la configuration de magasin de clés après la publication de l'application. Les mises à jour téléchargées ne peuvent pas être authentifiées avant la mise en place d'une reconfiguration de l'application avec une nouvelle clé publique et d'une republication de l'application. Si vous n'effectuez pas ces deux étapes, la mise à jour directe échouera sur le client.
* La fonctionnalité Mise à jour directe ne met à jour que les ressources web de l'application. Pour mettre à jour les ressources natives, une nouvelle version d'application doit être soumise au magasin d'applications concerné.
* Lorsque vous utilisez la fonction de mise à jour directe et que la fonction de somme de contrôle des ressources Web est activée, une nouvelle base de somme de contrôle est établie avec chaque mise à jour directe.
* Si MobileFirst Server a été mis à niveau via un groupe de correctifs, il continue à servir correctement les mises à jour directes. Toutefois, si une archive (fichier .zip) de mise à jour directe
générée récemment est téléchargée, elle peut interrompre les mises à jour de clients plus anciens. En effet, cette archive contient la version du plug-in
`cordova-plugin-mfp`. Avant d'envoyer cette archive sur un client mobile, le serveur compare la version du client à la version du
plug-in. Si les deux versions sont assez proches (c'est-à-dire si les trois chiffres les plus significatifs sont identiques), la mise à jour directe a lieu
normalement. Sinon, MobileFirst Server saute silencieusement la mise à jour. En cas de non concordance de version, l'une des solutions consiste à télécharger le plug-in
`cordova-plugin-mfp` avec la même version que dans votre projet Cordova d'origine et à régénérer l'archive de
mise à jour directe.
* Dans des conditions optimales, un serveur MobileFirst Server unique peut envoyer par push des données aux clients à un débit de 250 Mo par seconde. Si des débits plus élevés sont requis, envisagez d'utiliser un cluster
ou un service de réseau CDN.
{: tip}

## Comment opère la fonctionnalité Mise à jour directe ?
{: #working_direct_update}

Les ressources web d'application sont initialement conditionnées dans l'application pour assurer d'abord une disponibilité hors ligne. Ensuite, l'application vérifie les mises à jour à chaque demande effectuée sur le serveur MobileFirst Server.

>**Remarque :** une fois exécutée une mise à jour directe, une vérification est effectuée au bout de 60 minutes.

![Diagramme illustrant le mode de fonctionnement de la mise à jour directe](images/internal_function.jpg)

Après une mise à jour directe, l'application n'utilise plus les ressources web pré-conditionnées. Elle se sert des ressources web téléchargées depuis le bac à sable de l'application. Si le cache de l'application de l'appareil est effacé, les ressources web conditionnées à l'origine sont utilisées à nouveau.

>Une mise à jour directe ne s'applique qu'à une version spécifique. En d'autres termes, les mises à jour générées pour une application de version 2.0 ne peuvent être appliquées sur une version différente de la même application.

## Création et déploiement de ressources web mises à jour
{: #creating_deploying_updates}

Les ressources web mises à jour doivent être conditionnées et téléchargées sur MobileFirst Server.

1. Ouvrez une fenêtre de ligne de commande et accédez à la racine du projet Cordova.
2. Exécutez la commande :
  ```
  mfpdev app webupdate
  ```
  {: pre}
La commande `mfpdev app webupdate` conditionne les ressources web mises à jour dans un fichier `.zip` et le télécharge sur le serveur MobileFirst Server par défaut s'exécutant sur le poste de travail du développeur. Ce package de ressources web se trouve dans le dossier `[cordova-project-root-folder]/mobilefirst/`.

**Autres procédures possibles :**

* Générez le fichier `.zip` et téléchargez-le sur un serveur MobileFirst Server :  `mfpdev app webupdate [server-name] [runtime-name]`.
  Exemple :
  ```
  mfpdev app webupdate myQAServer MyBankApps
  ```
  {: pre}

* Téléchargez un fichier `.zip` généré précédemment : `mfpdev app webupdate [server-name] [runtime-name] --file [path-to-packaged-web-resources]`.
  Exemple :
  ```
  mfpdev app webupdate myQAServer MyBankApps --file mobilefirst/ios/com.mfp.myBankApp-1.0.1.zip
  ```
  {: pre}

* Téléchargez manuellement un package de ressources web sur MobileFirst Server :
  1. Générez le fichier .zip sans le télécharger :
      ```
      mfpdev app webupdate --build
      ```
      {: pre}
  2. Chargez MobileFirst Operations Console et cliquez sur une entrée d'application.
  3. Cliquez sur **Télécharger une archive de ressources Web** pour télécharger le package de ressources web.    
      ![Téléchargement du fichier .zip de mise à jour directe depuis la console](images/upload-direct-update-package.png)

Exécutez la commande `mfpdev help app webupdate` pour en savoir plus.
{: tip}

## Expérience utilisateur
{: #user_experience}

Après réception d'une mise à jour directe, une boîte de dialogue est affichée, par défaut, et un message demande à l'utilisateur l'autorisation de démarrer le processus de mise à jour. Après approbation de l'utilisateur, une boîte de dialogue avec une barre de progression s'affiche et les ressources web sont téléchargées. L'application est automatiquement rechargée une fois la mise à jour terminée.

![Exemple de mise à jour directe](images/direct-update-flow.png)

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

*directUpdateData* est un objet JSON contenant la propriété downloadSize qui représente la taille de fichier (en octets) du package de mise à niveau à télécharger depuis MobileFirst Server.
*directUpdateContext* est un objet JavaScript exposant les fonctions .start() et .stop(), qui démarre et arrête le flux Mise à jour directe.

Si les ressources web sont plus récentes sur le serveur MobileFirst Server que dans l'application, les données de demande d'authentification de mise à jour directe sont ajoutées dans la réponse du serveur. A chaque fois que l'infrastructure MobileFirst côté client détecte cette demande d'authentification de mise à jour directe, elle invoque la fonction `wl_directUpdateChallengeHandler.handleDirectUpdate`.

La fonction fournit un style Mise à jour directe par défaut : elle propose une boîte de dialogue de message par défaut qui est affichée quand une mise à jour directe est disponible ainsi qu'un écran de progression par défaut qui apparaît au lancement du processus de mise à jour directe. Vous pouvez implémenter un comportement d'interface utilisateur Mise à jour directe personnalisé ou personnaliser la boîte de dialogue Mise à jour directe en remplaçant cette fonction et en implémentant votre propre logique.

Dans le code exemple ci-après, une fonction `handleDirectUpdate` implémente un message personnalisé dans la boîte de dialogue Mise à jour directe. Ajoutez ce code dans le fichier `www/js/index.js` du projet Cordova.
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

Vous pouvez démarrer le processus Mise à jour directe en exécutant la méthode `directUpdateContext.start()` chaque fois que l'utilisateur clique sur le bouton dans la boîte de dialogue. L'écran de progression par défaut, qui ressemble à celui des versions précédentes de MobileFirst, s'affiche.

Cette méthode prend en charge les types d'appel suivants :
* Si aucun paramètre n'est spécifié, MobileFirst Server utilise l'écran de progression par défaut.
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

### Scénario : exécution de mises à jour directes sans interface utilisateur
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

**Remarque :** quand l'application est envoyée vers l'arrière-plan, le processus de mise à jour directe est suspendu.

### Scénario : traitement d'un échec de mise à jour directe
{: #scenario-handling-a-direct-update-failure }
Ce scénario montre comment gérer un échec de mise à jour directe qui pourrait être causé, par exemple, par la perte de connectivité. Dans ce scénario, l'utilisateur est empêché d'utiliser l'application mobile y compris en mode hors ligne. Une boîte de dialogue est affichée offrant à l'utilisateur la possibilité d'essayer à nouveau.

Créez une variable globale pour stocker le contexte de mise à jour directe de sorte que vous pouvez l'utiliser ultérieurement lorsque le processus de mise à jour directe échoue. Exemple :

```JavaScript
var savedDirectUpdateContext;
```
{: codeblock}

Implémentez un gestionnaire de demandes d'authentification de mise à jour directe. Enregistrez le contexte de mise à jour directe ici. Exemple :

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

Créez une fonction qui lance le processus de mise à jour directe en utilisant le contexte de la mise à jour directe. Exemple :

```JavaScript
restartDirectUpdate = function () {
  savedDirectUpdateContext.start(directUpdateCustomListener); // use saved direct update context to restart direct update
};
```
{: codeblock}

Implémentez `directUpdateCustomListener`. Ajoutez le contrôle de statut dans la méthode
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

## Mise à jour delta et complète
{: #delta-and-full-direct-update }
Les mises à jour directe delta permettent à une application de ne télécharger que les fichiers qui ont été modifiés depuis la dernière mise à jour au lieu de l'ensemble des ressources web de l'application, ce qui a l'avantage de réduire le temps de téléchargement, de préserver la bande passante et d'améliorer l'expérience utilisateur globale.

> <span class="glyphicon glyphicon-exclamation-sign" aria-hidden="true"></span> **Important :** une **mise à jour delta** n'est possible que si les ressources web de l'application client sont une version derrière l'application qui est actuellement déployée sur le serveur. Les applications client qui sont plusieurs versions en arrière par rapport à l'application déployée actuellement (ce qui signifie que l'application a été déployée sur le serveur au moins deux fois depuis la mise à jour de l'application client), reçoivent une **mise à jour complète** (l'intégralité des ressources web sont dans ce cas déployées et mises à jour).


## Application exemple
{: #sample-application }
[Cliquez pour télécharger ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/MobileFirst-Platform-Developer-Center/CustomDirectUpdate/tree/release80) le projet Cordova.  

### Exemple d'utilisation
{: #sample-usage }
Suivez le fichier README.md de l'exemple pour des instructions.

## Prise en charge CDN
{: #cdn_support}

Vous pouvez configurer des demandes Mise à jour directe afin qu'elles soient servies depuis un réseau CDN (Content Delivery Network) au lieu du serveur MobileFirst Server.

### Avantages de l'utilisation d'un réseau CDN
{: #advantages-of-using-a-cdn }
L'utilisation d'un réseau CDN à la place du serveur MobileFirst Server pour servir des demandes Direct Update offre les avantages suivants :

* Elle retire la charge qui incombait au serveur MobileFirst Server.
* Elle augmente les taux de transfert supérieurs à la limite de 250 Mo/seconde lors du traitement des demandes en provenance d'un serveur MobileFirst.
* Elle assure une expérience de mise à jour directe plus uniforme pour tous les utilisateurs, quel que soit leur emplacement géographique.

### Exigences générales
{: #general-requirements }
Pour mettre à disposition des demandes de mise à jour directe depuis un réseau CDN, vérifiez que votre configuration remplit les conditions suivantes :

* Le réseau CDN doit être un proxy inverse qui se trouve devant le serveur MobileFirst Server (ou devant un autre proxy inverse, si nécessaire).
* Quand vous générez l'application depuis votre environnement de développement, configurez votre serveur cible sur le port et l'hôte CDN au lieu du port et de l'hôte MobileFirst Server. Ainsi, quand vous exécutez la commande d'interface de ligne de commande MobileFirst `mfpdev server add`, fournissez le port et l'hôte CDN.
* Dans le panneau d'administration CDN, vous devez marquer les URL de mise à jour directe ci-après pour une mise en cache afin de vous assurer que le réseau CDN passe toutes les demandes au serveur MobileFirst Server, sauf pour les demandes Mise à jour directe. Pour celles-ci, le réseau CDN détermine s'il a obtenu le contenu. Si c'est le cas, il le renvoie sans passer par MobileFirst Server ; dans le cas contraire, il accède au serveur MobileFirst Server, obtient l'archive Mise à jour directe (fichier .zip) et la stocke pour les demandes suivantes relatives à cette URL spécifique. Pour les applications qui sont générées avec la version v8.0 de {{site.data.keyword.mobilefoundation_short}}, l'URL Mise à jour directe est : `PROTOCOL://DOMAIN:PORT/CONTEXT_PATH/api/directupdate/VERSION/CHECKSUM/TYPE`.
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
qui met en cache l'archive de mise à jour directe. Les tâches suivantes sont effectuées par l'administrateur réseau, l'administrateur MobileFirst et l'administrateur Akamai :

#### Administrateur réseau
{: #network-administrator }
Crée un autre domaine dans le système de noms de domaine pour votre serveur MobileFirst Server. Ainsi, si votre domaine de serveur
est `yourcompany.com`, vous devez créer un domaine supplémentaire, par exemple `cdn.yourcompany.com`.
Dans le système de noms de domaine pour votre nouveau domaine `cdn.yourcompany.com`, définissez un nom `CNAME` dans le nom de domaine qui est fourni par Akamai. Exemple : `yourcompany.com.akamai.net`.

#### Administrateur MobileFirst
{: #mobilefirst-administrator }
Définit le nouveau domaine `cdn.yourcompany.com` comme URL de serveur MobileFirst Server pour les applications MobileFirst. Ainsi, pour la tâche de générateur Ant, la propriété est :
```xml
<property name="wl.server" value="http://cdn.yourcompany.com/${contextPath}/"/>
```
{: codeblock}

#### Administrateur Akamai
{: #akamai-administrator }
1. Ouvrez le gestionnaire de propriétés Akamai et définissez la propriété **host name** sur la valeur du nouveau domaine.

    ![Définition de la propriété sur la valeur du nouveau domaine](images/direct_update_cdn_3.jpg)

2. Sur l'onglet Default Rule, configurez l'hôte et le port MobileFirst Server d'origine et définissez la valeur **Custom Forward Host Header** sur le domaine nouvellement créé.

    ![Définition de la valeur Custom Forward Host Header sur le domaine nouvellement créé](images/direct_update_cdn_4.jpg)

3. Dans la liste **Caching Option**, sélectionnez **No
Store**.

    ![Dans la liste Caching Option list, sélectionnez No Store](images/direct_update_cdn_5.jpg)

4. Dans l'onglet **Static Content configuration**, configurez les critères de mise en correspondance en fonction de l'URL de mise à jour directe de l'application. Ainsi, créez une condition qui stipule `If Path matches one of direct_update_URL`.

    ![Configuration des critères de mise en correspondance en fonction de l'URL de mise à jour directe de l'application](images/direct_update_cdn_6.jpg)

5. Configurez le comportement de clé de cache pour utiliser tous les paramètres de demande dans la clé de cache (nécessaire pour mettre en cache
différentes archives de mise à jour directe pour différentes applications ou versions). Par exemple,
dans la liste **Behavior**, sélectionnez `Include all parameters (preserve
order from request)`.

    ![Configurez le comportement de clé de cache pour utiliser tous les paramètres de demande dans la clé de cache](images/direct_update_cdn_8.jpg)

6. Définissez des valeurs similaires aux valeurs ci-après afin de configurer le comportement de mise en cache pour mettre en cache l'adresse URL de mise à
jour directe et pour définir la durée de vie.

      ![Définition des valeurs pour configurer le comportement de mise en cache](images/direct_update_cdn_7.jpg)

| Zone | Valeur |
|:------|:------|
| Caching Option | Cache |
| Force Revaluation of Stale Objects | Mise à disposition périmée si validation impossible |
| Max-Age | 3 minutes |


## Mise à jour directe sécurisée
{: #secure-dc }

Désactivée par défaut, la mise à jour directe sécurisée empêche que l'attaque d'un pirate tiers altère les ressources web qui sont transmises, depuis le serveur MobileFirst Server (ou à partir d'un réseau CDN) vers l'application client.

**Pour activer l'authenticité de mise à jour directe :**  
En utilisant l'outil de votre choix, extrayez la clé publique depuis le magasin de clés MobileFirst Server et convertissez-le en base64.  
La valeur produite doit ensuite être utilisée comme indiqué ci-dessous :

1. Ouvrez une fenêtre de **ligne de commande** et accédez à la racine du projet Cordova.
2. Exécutez la commande `mfpdev app config` et sélectionnez l'option relative à la clé publique d'authenticité de mise à jour directe.
3. Fournissez la clé publique et confirmez.

Toutes les transmissions par mise à jour directe aux applications client seront protégées par l'authenticité de mise à jour directe.

Pour que la mise à jour directe puisse fonctionner, un fichier de magasin de clés défini par l'utilisateur doit être déployé sur MobileFirst Server et une copie de la clé publique correspondante doit être incluse dans l'application client déployée.

Cette rubrique explique comment lier une clé publique à de nouvelles applications client et à des applications client existantes qui ont été mises à
niveau. Pour plus d'informations sur la configuration du magasin de clés dans MobileFirst Server, voir [Configuring the MobileFirst Server keystore ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/configuring-the-mobilefirst-server-keystore/){: new_window}

Le serveur fournit un magasin de clés intégré qui peut être utilisé pour tester la mise à jour directe sécurisée pour les phases de développement.

>**Remarque :** une fois que vous avez lié la clé publique à l'application client puis procédé à une régénération, vous n'avez pas besoin de la télécharger à nouveau sur le serveur {{ site.data.keys.mf_server }}. Toutefois, si vous avez déjà publié l'application sur le marché sans la clé publique, vous devez la republier.

A des fins de développement, la clé publique factice par défaut suivante est fournie avec MobileFirst :

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

** Important :** n'utilisez pas la clé publique en production.

### Génération et déploiement du magasin de clés
{: #generating-and-deploying-the-keystore }
De nombreux outils sont disponibles pour la génération de certificats et l'extraction de clés publiques depuis un magasin de clés. L'exemple ci-dessous
illustre les procédures à suivre avec l'utilitaire de clé du kit Java Development Kit et OpenSSL.

1. Procédez à l'extraction de la clé publique depuis le fichier de magasin de clés qui est déployé sur le serveur {{ site.data.keys.mf_server }}.  
   >**Remarque :** la clé publique doit être codée en base64.

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

   > **Remarque :** l'outil keytool ne peut pas extraire seul les clés publiques au format Base64.

2. Effectuez l'une des procédures suivantes :
    * Copiez le texte généré, sans les marqueurs `BEGIN PUBLIC KEY` et `END PUBLIC KEY`, dans le fichier de propriétés mfpclient de l'application, immédiatement après `wlSecureDirectUpdatePublicKey`.
    * Dans l'invite de commande, émettez la commande suivante : `mfpdev app config direct_update_authenticity_public_key <public_key>`

    Pour `<public_key>`, collez le texte résultant de l'étape 1, sans les marqueurs `BEGIN PUBLIC KEY` et `END PUBLIC KEY`.

3. Exécutez la commande cordova build afin de sauvegarder la clé publique dans l'application.
