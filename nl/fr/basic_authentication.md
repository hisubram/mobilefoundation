---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: security, basic authentication, protecting resources, tokens, scopemapping

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


# Authentification et sécurité
{: #basic_authentication}

L'infrastructure de sécurité MobileFirst repose sur le protocole [OAuth 2.0](http://oauth.net/). Selon ce protocole, une ressource peut être protégée par une **portée** qui définit les droits requis pour accéder à la ressource. Pour accéder à une ressource protégée, le client doit fournir un **jeton d'accès** correspondant, qui encapsule la portée de l'autorisation qui est accordée au client.

Le protocole d'autorisation OAuth sépare les rôles du serveur
d'autorisations et du serveur de ressources sur lequel la ressource est hébergée.

* Le serveur d'autorisations gère l'autorisation du client et la génération
du jeton.
* Le serveur de ressources utilise le serveur d'autorisations pour valider le jeton d'accès fourni par le client et vérifier qu'il correspond à la portée de protection de la ressource demandée.

L'infrastructure de sécurité est construite autour d'un serveur d'autorisations qui implémente le protocole OAuth et expose les noeuds finaux OAuth avec lesquels le client interagit pour obtenir les jetons d'accès. Elle fournit les blocs de construction permettant d'implémenter une logique d'autorisation personnalisée au dessus du serveur d'autorisations et du protocole OAuth sous-jacent. Par défaut, le serveur MobileFirst fait également office de **serveur d'autorisations**. Toutefois, vous pouvez configurer un dispositif IBM WebSphere DataPower comme serveur d'autorisations interagissant avec le serveur MobileFirst.

L'application client peut alors utiliser ces jetons pour accéder aux ressources d'un **serveur de ressources**, qui peut être le serveur MobileFirst lui-même ou un serveur externe. Le serveur de ressources vérifie la validité du jeton pour s'assurer que le client peut être autorisé à accéder à la ressource demandée. La séparation entre le serveur de ressources et le serveur d'autorisations applique la sécurité sur les ressources qui ne s'exécutent pas sur le serveur MobileFirst.

Les développeurs d'applications protègent l'accès à leurs ressources en définissant la portée requise pour chaque ressource protégée et en implémentant des contrôles de sécurité et des gestionnaires de demandes d'authentification. L'infrastructure de sécurité côté serveur et l'API côté client gèrent l'échange de messages OAuth et l'interaction avec le serveur d'autorisations de manière transparente, ce qui permet aux développeurs de se concentrer uniquement sur la logique d'autorisation.

## Entités d'autorisation
{: #acs_authorization_entitiesty}

### Jetons d'accès
{: #acs_access_tokens}

Un jeton d'accès MobileFirst est une entité signée numériquement qui décrit les droits d'accès d'un client. Une fois que la demande d'autorisation du client pour une portée spécifique est satisfaite et que le client est authentifié, le noeud final de jeton du serveur d'autorisations envoie au client une réponse HTTP contenant le jeton d'accès demandé.

#### Structure du jeton d'accès

Le jeton d'accès MobileFirst contient les informations suivantes :

* **Client ID** : identificateur unique du client.
* **Scope** : portée pour laquelle le jeton est accordé (voir les portées OAuth). Cette portée n'inclut pas la portée d'application obligatoire.
* **Token-expiration time** : délai au bout duquel le jeton n'est plus valide (expire), en secondes.

#### Expiration du jeton
{: #token-expiration}

Le jeton d'accès accordé reste valide jusqu'à la fin du délai d'expiration. Le délai d'expiration du jeton d'accès a pour valeur le délai d'expiration le plus court parmi les délais d'expiration de tous les contrôles de sécurité dans la portée. Cependant, si la durée jusqu'à la fin du délai d'expiration le plus court est plus longue que le délai d'expiration de jeton maximal de l'application, le délai d'expiration du jeton a pour valeur le délai d'expiration maximal à partir de l'heure en cours. Le délai d'expiration maximal de jeton par défaut (durée de validité) est 3600 secondes (1 heure), mais vous pouvez le configurer en définissant la valeur de la propriété ``maxTokenExpiration``.

##### Configuration du délai d'expiration maximal du jeton d'accès
{: #acs_config-max-access-tokens}

Configurez le délai d'expiration de jeton d'accès maximal de l'application en appliquant l'une des méthodes suivantes :

* Utilisation de MobileFirst Operations Console
    1. Sélectionnez l'onglet **[votre_application]** → **Sécurité**.
    2. Dans la section **Configuration de jeton**, définissez la valeur de votre choix dans la zone **Délai d'expiration de jeton maximal (secondes)** et cliquez sur **Sauvegarder**. Vous pouvez répéter cette procédure à tout moment afin de changer le délai d'expiration de jeton maximal ou sélectionner **Restaurer les valeurs
par défaut** afin de restaurer la valeur par défaut.

* Edition du fichier de configuration de l'application

    1. Depuis une interface de ligne de commande, accédez au dossier racine du projet et exécutez la commande suivante :
      ```bash
      mfpdev app pull
      ```
    2. Ouvrez le fichier de configuration, qui se trouve dans le dossier `[dossier_projet]\mobilefirst`.
    3. Editez le fichier en définissant une propriété `maxTokenExpiration` ayant pour valeur le délai d'expiration de jeton d'accès maximal en secondes :
        ```java
        {
            ...
            "maxTokenExpiration": 7200
        }
        ```
        {: codeblock}
    4. Déployez le fichier JSON de configuration mis à jour en exécutant la commande ``mfpdev app push``.

##### Structure d'une réponse contenant un jeton d'accès
{: #acs_access-tokens-structure}

Une réponse HTTP positive à une demande de jeton d'accès contient un objet JSON comportant le jeton d'accès et des données supplémentaires. Voici un exemple de réponse du serveur d'autorisations contenant un jeton valide :

```json
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-store
Pragma: no-cache
{
    "token_type": "Bearer",
    "expires_in": 3600,
    "access_token": "yI6ICJodHRwOi8vc2VydmVyLmV4YW1",
    "scope": "scopeElement1 scopeElement2"
}
```
{: codeblock}

L'objet JSON d'une réponse comportant un jeton comporte les objets de propriété suivants :

* **token_type** : le type de jeton est toujours "*Bearer*" conformément à la [spécification OAuth 2.0 Bearer Token Usage](https://tools.ietf.org/html/rfc6750).
* **expires_in** : délai d'expiration du jeton d'accès en secondes.
* **access_token** : jeton d'accès généré (les véritables jetons d'accès sont plus longs que ceux présentés dans l'exemple).
* **scope** : portée demandée.

Les informations **expires_in** et **scope** se trouvent également dans le jeton lui-même (**access_token**).

**Remarque** : la structure d'une réponse comportant un jeton d'accès valide est pertinente si vous utilisez la classe `WLAuthorizationManager` de niveau inférieur et gérez l'interaction OAuth entre le client et les serveurs d'autorisations et de ressources vous-même, ou si vous utilisez un client confidentiel. Si vous utilisez la classe
`WLResourceRequest` de niveau supérieur qui encapsule le flux OAuth pour accéder à des ressources protégées, l'infrastructure de sécurité gère automatiquement le traitement des réponses comportant des jetons d'accès. Voir [API de sécurité du client](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_oauth_client_apis.html?view=kc#c_oauth_client_apis) et [Confidential clients](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients/).

### Jetons d'actualisation
{: #acs_refresh_tokens}

Un jeton d'actualisation est un type spécial de jeton que vous pouvez utiliser pour obtenir un nouveau jeton d'accès lorsque celui-ci arrive à expiration. Vous pouvez présenter un jeton d'actualisation valide pour demander un nouveau jeton d'accès. Les jetons d'actualisation sont des jetons dont la durée de vie est longue et qui restent valides plus longtemps que les jetons d'accès.

Les applications doivent utiliser les jetons d'actualisation avec précaution car ceux-ci peuvent permettre à un utilisateur de rester authentifié indéfiniment. Les applications de médias sociaux, d'e-commerce, de consultation de catalogues de produit et d'utilitaires similaires, où le fournisseur d'application n'authentifie pas les utilisateurs régulièrement, peuvent utiliser des jetons d'actualisation. Les applications qui demandent fréquemment une authentification d'utilisateur doivent éviter l'utilisation de jetons d'actualisation.
Jeton d'actualisation MobileFirst

Un jeton d'actualisation MobileFirst est une entité signée numériquement, comme un jeton d'accès, qui décrit les droits d'accès d'un client. Un jeton d'actualisation peut être utilisé pour obtenir un nouveau jeton d'accès d'une portée identique. Une fois que la demande d'autorisation du client pour une portée spécifique est satisfaite et que le client est authentifié, le noeud final de jeton du serveur d'autorisations envoie au client une réponse HTTP contenant le jeton d'accès et le jeton d'actualisation demandés. Lorsque le jeton d'accès expire, le client envoie le jeton d'actualisation au noeud final de jeton du serveur d'autorisations pour obtenir une nouvelle paire de jeton d'accès et de jeton d'actualisation.

#### Structure du jeton d'actualisation
{: #structure_refresh_tokens}

A l'instar du jeton d'accès MobileFirst, le jeton d'actualisation MobileFirst contient les informations suivantes :

* **Client ID** : identificateur unique du client.
* **Scope** : portée pour laquelle le jeton est accordé (voir les portées OAuth). Cette portée n'inclut pas la portée d'application obligatoire.
* **Token-expiration time** : délai au bout duquel le jeton n'est plus valide (expire), en secondes.

##### Expiration du jeton
{: #str-token-expiration}

Le délai d'expiration du jeton d'actualisation est plus long que le délai d'expiration classique d'un jeton d'accès. Le jeton d'accès qui est accordé reste valide jusqu'à la fin du délai d'expiration. Au cours de cette période de validité, un client peut utiliser le jeton d'actualisation pour obtenir une nouvelle paire de jeton d'accès et de jeton d'actualisation. Le délai d'expiration du jeton d'actualisation est un délai fixe de 30 jours. A chaque fois que le client reçoit une nouvelle paire de jeton d'accès et de jeton d'actualisation, le délai d'expiration du jeton d'actualisation est réinitialisé, ce qui donne au client l'impression que le jeton n'expire jamais. Les règles relatives à l'expiration du jeton d'accès restent les mêmes, comme expliqué dans la section **Jeton d'accès**.

##### Activation de la fonction de jeton d'actualisation
{: #acs_enable-refresh-token}

Vous pouvez activer la fonction de jeton d'actualisation en utilisant les propriétés ci-dessous côté client et côté serveur respectivement.

Client-side property (Android):
*File name*: mfpclient.properties
*Property name*: wlEnableRefreshToken
*Property value*: true
For example,
*wlEnableRefreshToken*=true

Client-side property (iOS): 
*File name*: mfpclient.plist
*Property name*: wlEnableRefreshToken
*Property value*: true
For example,
*wlEnableRefreshToken*=true

Server-side property:
*File name*: server.xml
*Property name*: mfp.security.refreshtoken.enabled.apps
*Property value*: application bundle id separated by ‘;’

Exemple :

```xml
<jndiEntry jndiName="mfp/mfp.security.refreshtoken.enabled.apps" value='"com.sample.android.myapp1;com.sample.android.myapp2"'/>
```
{: codeblock}
Utilisez des ID de bundle différents pour différentes plateformes.

##### Structure d'une réponse comportant un jeton d'actualisation
{: #refresh-token-response-structure}

Voici un exemple de réponse comportant un jeton d'actualisation valide du serveur d'autorisations :

```json
    HTTP/1.1 200 OK
    Content-Type: application/json
    Cache-Control: no-store
    Pragma: no-cache
    {
        "token_type": "Bearer",
        "expires_in": 3600,
        "access_token": "yI6ICJodHRwOi8vc2VydmVyLmV4YW1",
        "scope": "scopeElement1 scopeElement2",
        "refresh_token": "yI7ICasdsdJodHRwOi8vc2Vashnneh "
    }
```        
{: codeblock}

Une réponse comportant un jeton d'actualisation contient un objet de propriété supplémentaire `refresh_token` en plus des autres objets de propriété qui sont présentés dans le cadre de la structure d'une réponse comportant un jeton d'accès.

**Remarque** : les jetons d'actualisation ont une durée de vie longue par rapport aux jetons d'accès. Par conséquent, la fonction de jeton d'actualisation doit être utilisée avec précaution. Elle est particulièrement adaptée pour les applications dans lesquelles il n'est pas nécessaire de procéder à l'authentification régulière des utilisateurs.

MobileFirst prend en charge la fonction de jeton d'actualisation sous iOS depuis l'édition CD, mise à jour 3.

#### Contrôles de sécurité
{: #acs_securitychecks}

Un contrôle de sécurité est une entité côté serveur qui implémente la logique de sécurité pour protéger les ressources d'application côté serveur. Un contrôle de sécurité de connexion utilisateur recevant les informations d'identification d'un utilisateur et vérifiant les informations d'identification par rapport à un registre d'utilisateurs en est un exemple simple. Le contrôle de sécurité d'authenticité de l'application MobileFirst prédéfini en est un autre exemple : il valide l'authenticité de l'application mobile et vous protège ainsi contre toute tentative illégale d'accès aux ressources de l'application. Ce contrôle de sécurité peut également être utilisé pour protéger plusieurs ressources.

Un contrôle de sécurité émet généralement des demandes d'authentification de sécurité nécessitant que le client réponde d'une manière spécifique pour passer le contrôle. Cet établissement de liaison se produit dans le cadre du flux d'acquisition de jeton d'accès OAuth. Le client utilise des **gestionnaires de demandes d'authentification** pour gérer les demandes d'authentification à partir de contrôles de sécurité.

##### Contrôles de sécurité intégrés
{: #builtin-sec-checks}

Les contrôles de sécurité prédéfinis suivants sont disponibles :

* Authenticité de l'application
* Connexion unique reposant sur LTPA
* Mise à jour directe

#### Entité Gestionnaire de demandes d'authentification
{: #challengehandler_entity}

Lorsque vous tentez d'accéder à une ressource protégée, le client peut avoir à répondre à une demande d'authentification. Il s'agit d'une question, d'un test de sécurité ou d'une invite du serveur qui permet de garantir que le client est autorisé à accéder à la ressource. Le plus souvent, il s'agit d'une demande de données d'identification, comme un nom d'utilisateur et un mot de passe.

Un gestionnaire de demandes d'authentification est une entité côté client qui implémente la logique de sécurité côté client et l'interaction utilisateur associée.

**Important** : si une demande d'authentification est reçue, elle ne peut pas être ignorée. Vous devez y répondre ou l'annuler. Le fait d'ignorer une demande d'authentification peut entraîner un comportement inattendu.

### Portées
{: #scopes}

Vous pouvez protéger des ressources, comme des adaptateurs, contre tout accès non autorisé en affectant une portée à la ressource.

Une portée est définie sous forme de chaîne composée d'un ou de plusieurs éléments de portée séparés par un espace (“élémentPortée1 élémentPortée2 …”), ou comme valeur null pour appliquer la portée par défaut (RegisteredClient). L'infrastructure de sécurité MobileFirst requiert un jeton d'accès pour toute ressource d'adaptateur, même si une portée n'est pas affectée à la ressource, sauf si vous désactivez la protection des ressources pour la ressource.

#### Eléments de portée
{: #scopeelements}

Un élément de portée peut être :

* Le nom d'un contrôle de sécurité
* Un mot clé arbitraire tel que `access-restricted` ou `deletePrivilege`, qui définit le niveau de sécurité requis pour cette ressource. Ce mot clé est mappé ultérieurement à un contrôle de sécurité.

#### Mappage de la portée
{: #scopemapping}

Par défaut, les **éléments de portée** que vous écrivez dans votre **portée** sont mappés à un **contrôle de sécurité** du même nom. Par exemple, si vous écrivez un contrôle de sécurité dont le nom est `PinCodeAttempts`, vous pouvez utiliser un élément de portée du même nom dans votre portée.

Le mappage de la portée permet de mapper des éléments de portée à des contrôles de sécurité. Lorsque le client demande un élément de portée, cette configuration définit quels sont les contrôles de sécurité qui doivent être appliqués. Par exemple, vous pouvez mapper l'élément de portée `access-restricted` à votre contrôle de sécurité `PinCodeAttempts`.

Le mappage de la portée est utile si vous voulez protéger une ressource différemment selon l'application qui tente d'y accéder. Vous pouvez également mapper une portée à une liste de zéro contrôle de sécurité ou plus.

Exemple : scope = `access-restricted deletePrivilege`

* Dans l'application A
    * `access-restricted` est mappé à `PinCodeAttempts`.
    * `deletePrivilege` est mappé à une chaîne vide.
* Dans l'application B
    * `access-restricted` est mappé à `PinCodeAttempts`.
    * `deletePrivilege` est mappé à `UserLogin`.

Pour mapper votre élément de portée à une chaîne vide, ne sélectionnez pas de contrôle de sécurité dans le menu contextuel **Ajout d'un nouveau mappage d'élément de portée**.

![Mappage de la portée](/images/scope_mapping.png "Ecran d'ajout d'un nouveau mappage d'élément de portée")

Vous pouvez aussi éditer manuellement le fichier JSON de configuration de l'application avec la configuration requise et envoyer les modifications à un serveur MobileFirst.

1. Depuis une **fenêtre de ligne de commande**, accédez au dossier racine du projet et exécutez la commande `mfpdev app pull`.
2. Ouvrez le fichier de configuration, qui se trouve dans le dossier `[dossier_projet]\mobilefirst`.
3. Editez le fichier en définissant une propriété `scopeElementMapping`. Dans cette propriété, définissez des paires de données composées chacune du nom de l'élément de portée que vous avez sélectionné et d'une chaîne de zéro contrôle de sécurité ou plus, séparés par un espace, auxquels l'élément est mappé. Exemple :

```java
     "scopeElementMapping": {
         "UserAuth": "UserAuthentication",
         "SSOUserValidation": "LtpaBasedSSO CredentialsValidation"
     }
```
4. Déployez le fichier JSON de configuration mis à jour en exécutant la commande suivante :
  ```bash
  mfpdev app push
  ```

Vous pouvez également envoyer les configurations mises à jour à des serveurs distants. Voir le tutoriel Using MobileFirst CLI to Manage MobileFirst artifacts.
{: note}

### Protection des ressources
{: #protecting-resources}

Dans le modèle OAuth, une ressource protégée est une ressource qui requiert un jeton d'accès. Vous pouvez utiliser l'infrastructure de sécurité MobileFirst pour protéger les ressources qui sont hébergées dans une instance du serveur MobileFirst et les ressources qui se trouvent sur un serveur externe. Vous protégez une ressource en lui affectant une portée qui définit les droits requis pour l'acquisition d'un jeton d'accès pour la ressource.

Vous pouvez protéger vos ressources de plusieurs façons :

#### Portée d'application obligatoire
{: #mandatoryappscope}

Au niveau de l'application, vous pouvez définir une portée qui s'applique à toutes les ressources utilisées par l'application. L'infrastructure de sécurité exécute ces contrôles (le cas échéant) en plus des contrôles de sécurité de la portée de ressource demandée.

**Remarque** :
   * La portée d'application obligatoire n'est pas appliquée lorsque vous accédez à une ressource non protégée.
   * Le jeton d'accès qui est accordé à la portée de ressource ne contient pas la portée d'application obligatoire.

Dans MobileFirst Operations Console, sélectionnez votre application dans la section **Applications** de la barre latérale de navigation, puis sélectionnez l'onglet **Sécurité**. Sous **Portée d'application obligatoire**, sélectionnez **Ajouter à la portée**.

![Portée d'application obligatoire](/images/mandatory-application-scope.png "Ecran de configuration de la portée d'application obligatoire")

Vous pouvez aussi éditer manuellement le fichier JSON de configuration de l'application avec la configuration requise et envoyer les modifications à un serveur MobileFirst.

1. Depuis une **fenêtre de ligne de commande**, accédez au dossier racine du projet et exécutez la commande `mfpdev app pull`.
2. Ouvrez le fichier de configuration, qui se trouve dans le dossier **dossier_projet\mobilefirst**.
3. Editez le fichier en définissant une propriété `mandatoryScope` et en définissant comme valeur de propriété une chaîne de portée contenant la liste des éléments de portée que vous avez sélectionnés, séparés par un espace. Exemple :

    ```java
        "mandatoryScope": "appAuthenticity PincodeValidation"
    ```
4. Déployez le fichier JSON de configuration mis à jour en exécutant la commande mfpdev app push.

Vous pouvez également envoyer les configurations mises à jour à des serveurs distants.

#### Protection des ressources d'adaptateur
{: #protectadapterres}

Dans votre adaptateur, vous pouvez spécifier la portée de protection pour une méthode Java ou une procédure de ressource JavaScript, ou pour une classe de ressources Java entière. Une portée est définie sous forme de chaîne composée d'un ou de plusieurs éléments de portée séparés par un espace (“élémentPortée1 élémentPortée2 …”), ou comme valeur null pour appliquer la portée par défaut. Pour plus d'informations sur la protection des ressources d'adaptateur, voir [Protection des adaptateurs](/docs/services/mobilefoundation?topic=mobilefoundation-protecting_adapters#protecting_adapters).

### Désactivation de la protection des ressources
{: #disablingresprotection}

Vous pouvez désactiver la protection des ressources MobileFirst par défaut pour une ressource d'adaptateur Java ou JavaScript spécifique ou pour une classe Java entière, comme expliqué dans les sections Java et JavaScript ci-après. Lorsque la protection des ressources est désactivée, l'infrastructure de sécurité MobileFirst ne requiert pas de jeton pour l'accès à la ressource.

#### Désactivation de la protection OAuth des ressources Java
{: #disablejavaresoauthprotection}

Afin de désactiver entièrement la protection OAuth pour une classe ou une méthode de ressources Java, ajoutez l'annotation `@OAuthSecurity` à la déclaration de classe ou de ressource et définissez la valeur `false` pour l'élément `enabled` :

```
@OAuthSecurity(enabled = false)
```

La valeur par défaut de l'élément `enabled` de l'annotation est `true`. Lorsque l'élément `enabled` a pour valeur `false`, l'élément `scope` est ignoré et la ressource ou la classe de ressources n'est pas protégée.

**Remarque** : lorsque vous affectez une portée à une méthode d'une classe non protégée, la méthode est protégée malgré l'annotation de classe, sauf si vous avez défini la valeur `false` pour l'élément `enabled` de l'annotation de ressource.

Examinez les exemples suivants :

Le code suivant désactive la protection des ressources pour une méthode `helloUser` :

```java
    @GET
    @Path("/{username}")
    @OAuthSecurity(enabled = "false")
    public String helloUser(@PathParam("username") String name){
        ...
    }
```

Le code suivant désactive la protection des ressources pour une classe `MyUnprotectedResources` :

```java
    @Path("/users")
    @OAuthSecurity(enabled = "false")
    public class MyUnprotectedResources {
        ...
    }
```

#### Désactivation de la protection OAuth des ressources JavaScript
{: #disablejavascriptresoauthprotection}

Afin de désactiver entièrement la protection OAuth pour une ressource d'adaptateur JavaScript (procédure), dans le fichier **adapter.xml**, définissez la valeur `false` pour l'attribut `secured` de l'élément <procedure> :

```javascript
<procedure name="procedureName" secured="false">
```

Lorsque l'attribut `secured` a pour valeur `false`, l'attribut `scope` est ignoré et la ressource n'est pas protégée.

Examinez l'exemple suivant :

Le code suivant désactive la protection des ressources pour une procédure `userName` :

```javascript
<procedure name="userName" secured="false">
```

### Définition de ressources non protégées
{: #defunprotectedresources}

Une ressource non protégée est une ressource qui ne requiert pas de jeton d'accès. L'infrastructure de sécurité MobileFirst ne gère pas les accès aux ressources non protégées et ne valide pas ni ne vérifie l'identité des clients qui accèdent à ces ressources. Par conséquent, les fonctions telles que la mise à jour directe, le blocage de l'accès d'un appareil ou la désactivation à distance d'une application ne sont pas prises en charge pour les ressources non protégées.

### Protection des ressources externes
{: #protecextresources}

Pour protéger les ressources externes, vous ajoutez un filtre de ressources avec un module de validation de jeton d'accès au serveur de
ressources externe. Le module de validation de jeton utilise le noeud final d'introspection du serveur d'autorisations de l'infrastructure de sécurité pour valider les jetons d'accès MobileFirst avant d'accorder l'accès aux ressources au client OAuth. Vous pouvez utiliser l'API REST MobileFirst pour l'environnement d'exécution MobileFirst afin de créer votre propre module de validation de jeton d'accès pour tout serveur externe. Vous pouvez aussi utiliser l'une des extensions MobileFirst fournies pour protéger les ressources Java externes.
