---

copyright:
  years: 2019
lastupdated: "2018-12-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:java: .ph data-hd-programlang='java'}
{:ruby: .ph data-hd-programlang='ruby'}
{:c#: .ph data-hd-programlang='c#'}
{:objectc: .ph data-hd-programlang='Objective C'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:php: .ph data-hd-programlang='PHP'}
{:swift: .ph data-hd-programlang='swift'}

# API du SDK client Javascript
{: #javascript_client_sdk_api}

## API pour applications Cordova / Web(Javascript)

Le tableau suivant répertorie les fonctions que vous pouvez exécuter dans les applications Javascript et la méthode API correspondante.

| Fonction | Description |
|----------|-------------|
| `WL.Client`, `WL.App` | Initialisation et rechargement d'une application, Globalisation des textes d'application | 
| `WLAuthorizationManager` | Obtention de l'ID client et de l'en-tête d'autorisation |
| `WL.Logger` | Impression des messages de journal dans le journal de l'environnement |
| `WL.NativePage` | Commutation de l'écran Web affiché avec une page écrite en mode natif |
| `WLResourceRequest` | Envoi de demandes à des ressources protégées et non protégées | 
| `WL.JSONStore` | API côté client fournissant un système de stockage léger, orienté document | 

## Informations supplémentaires
{: #additional-information }
### Objet options
{: #the-optional-object }
L'objet `options` contient des propriétés communes à toutes les méthodes. Il est utilisé dans tous les appels asynchrones vers le serveur {{ site.data.keys.mf_server }}.

Parfois, il est complété par des propriétés qui ne s'appliquent qu'à des méthodes spécifiques. Ces propriétés supplémentaires sont détaillées dans le cadre de la description des méthodes spécifiques.

Les propriétés communes de l'objet options sont les suivantes :

```javascript
options = {
    onSuccess: successHandler(response),
    onFailure: failureHandlder(response),
    invocationContext: invocation-context
};
```
{: code}
La signification de chaque propriété est la suivante :

| Propriété | Description |
|----------|-------------|
| `onSuccess` | Facultatif. Fonction à appeler à la fin de l'appel asynchrone. La syntaxe de la fonction `onSuccess` est la suivante : `success-handler-function(response)` où `response` est un objet contenant au minimum la propriété suivante : {::nomarkdown}<ul><li><b>invocationContext</b> - Objet <code>invocationContext</code> initialement transmis au serveur {{ site.data.keys.mf_server }} dans l'objet <code>options</code>, ou <code>undefined</code> si aucun objet <code>invocationContext</code> n'a été transmis.</li><li><b>status</b> - Statut de la réponse HTTP</li></ul>{:/} **Remarque :** Pour les méthodes pour lesquelles l'objet `response` contient des propriétés supplémentaires, ces propriétés sont détaillées dans le cadre de la description de la méthode spécifique. |
| `onFailure` | Facultatif. Fonction à appeler lorsque l'appel asynchrone échoue. Ces échecs incluent à la fois les erreurs côté serveur et les erreurs côté client qui se sont produites lors d'appels asynchrones, telles que l'échec de la connexion au serveur ou les appels arrivés à expiration. **Remarque :** La fonction n'est pas appelée pour les erreurs côté client qui arrêtent l'exécution en lançant une exception. La syntaxe de la fonction onFailure est la suivante : `failure-handler-function(response)` où `response` est un objet contenant les propriétés suivantes : {::nomarkdown}<ul><li><b>invocationContext</b> - Objet <code>invocationContext</code> initialement transmis au serveur {{ site.data.keys.mf_server }} dans l'objet <code>options</code>, ou <code>undefined</code> si aucun objet <code>invocationContext</code> n'a été transmis.</li><li><b>errorCode</b> - Chaîne de code d'erreur. Tous les codes d'erreur pouvant être renvoyés sont définis en tant que constantes dans l'objet <code>WL.ErrorCode</code> du fichier <b>worklight.js</b>.</li><li><b>errorMsg</b> - Message d'erreur fourni par le serveur {{ site.data.keys.mf_server }}. Ce message est destiné au développeur uniquement et ne doit pas être affiché à l'utilisateur. Il ne sera pas traduit dans la langue de l'utilisateur.</li><li><b>status</b> - Statut de la réponse HTTP</li></li></ul>{:/} **Remarque :** Pour les méthodes pour lesquelles l'objet `response` contient des propriétés supplémentaires, ces propriétés sont détaillées dans le cadre de la description de la méthode spécifique. |
| `invocationContext` | Facultatif. Objet renvoyé aux gestionnaires de réussite et d'échec. L'objet `invocationContext` sert à préserver le contexte du service asynchrone appelant lors du retour du service. Par exemple, la méthode `invokeProcedure` peut être appelée successivement, à l'aide du même gestionnaire de réussite. Le gestionnaire de réussite doit être en mesure d'identifier quel appel à invokeProcedure est en cours de traitement. Une solution consiste à implémenter l'objet `invocationContext` sous la forme d'un entier et à incrémenter sa valeur d'une unité pour chaque appel de `invokeProcedure`. Lorsqu'il appelle le gestionnaire de réussite, l'infrastructure {{ site.data.keys.product_adj }} lui transmet l'objet `invocationContext` de l'objet options associé à la méthode `invokeProcedure`. La valeur de l'objet `invocationContext` peut être utilisée pour identifier l'appel de `invokeProcedure` auquel les résultats en cours de traitement sont associés. | 

## Objet WL.ClientMessages
{: #the-wlclientmessages-object }
Vous pouvez voir une liste des messages système stockés dans l'objet `WL.ClientMessages` et activer la traduction de ces messages système.

Vous devez remplacer les messages système au niveau JavaScript global, car certaines parties du code ne sont exécutées qu'une fois l'application initialisée.
{: note}

L'objet `WL.ClientMessages` stocke les messages système définis dans le fichier **worklight/messages/messages.json**. Ce fichier se trouve dans le dossier d'environnement des projets que vous avez générés avec {{ site.data.keys.product }}. Pour activer la traduction d'un message système, vous devez remplacer la valeur de ce message dans l'objet `WL.ClientMessages` comme indiqué dans l'exemple de code suivant :

```javascript
WL.ClientMessages.invalidUsernamePassword="The custom user name and password are not valid";
```
{: code}


* [Référence d'API JavaScript](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/#javascript-api-reference)
* [Référence d'API Objective-C (pour Cordova)](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/#objective-c-api-reference-for-cordova)
{: note}
