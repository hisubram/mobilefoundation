---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-19"

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
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:xml: .ph data-hd-programlang='xml'}

# Authentification à un niveau de sécurité supérieur
{: #step_up_authentication}

Les ressources peuvent être protégées par divers contrôles de sécurité. Dans ce cas, le serveur MobileFirst envoie toutes les demandes d'authentification pertinentes simultanément à l'application.

Un contrôle de sécurité peut également dépendre d'un autre contrôle de sécurité. Par conséquent, il est essentiel de pouvoir contrôler le moment auquel les demandes d'authentification sont envoyées.

Par exemple, ce tutoriel décrit une application qui possède deux ressources protégées par un nom d'utilisateur et un mot de passe, où la deuxième ressource requiert également un code PIN.

## Référence à un contrôle de sécurité
{: #referencing-a-security-check}

Créez deux contrôles de sécurité : `StepUpPinCode` et `StepUpUserLogin`. 

Dans cet exemple, `StepUpPinCode` dépend de `StepUpUserLogin`. L'utilisateur doit être invité à entrer un code PIN une fois qu'il s'est connecté à `StepUpUserLogin`. A cette fin, `StepUpPinCode` doit pouvoir référencer la classe `StepUpUserLogin`.

L'infrastructure MobileFirst fournit une annotation permettant d'injecter une référence.

Dans votre classe `StepUpPinCode`, au niveau de la classe, ajoutez :

```java
@SecurityCheckReference
private transient StepUpUserLogin userLogin;
```
{: codeblock}

>**Important** : les deux implémentations de contrôle de sécurité doivent être regroupées dans un même adaptateur.
{.important}

Pour résoudre cette référence, l'infrastructure recherche un contrôle de sécurité avec la classe appropriée et injecte sa référence dans le contrôle de sécurité dépendant.

Si une même classe contient plusieurs contrôles de sécurité, l'annotation comporte un paramètre de nom facultatif que vous pouvez utiliser pour spécifier le nom unique du contrôle référencé.

## Automate
{: #state-machine}

Tous les classes qui étendent `CredentialsValidationSecurityCheck` (qui inclut `StepUpPinCode` et `StepUpUserLogin`) héritent d'un automate simple. A tout moment, le contrôle de sécurité peut se trouver dans l'un des états suivants :

* `STATE_ATTEMPTING` : une demande d'authentification a été envoyée et le contrôle de sécurité attend la réponse du client. Dans cet état, le nombre de tentatives est vérifié.
* `STATE_SUCCESS` : les données d'identification ont été validées.
* `STATE_BLOCKED` : le nombre maximal de tentatives a été atteint et le contrôle est à l'état verrouillé.

Vous pouvez obtenir l'état en cours à l'aide de la méthode `getState()` héritée.

Dans `StepUpUserLogin`, ajoutez une méthode de simplification pour vérifier si l'utilisateur est connecté. Celle-ci sera utilisée ultérieurement dans le tutoriel.

```java
public boolean isLoggedIn(){
    return this.getState().equals(STATE_SUCCESS);
}
```
{: codeblock}

## La méthode authorize
{: #the-authorize-method}

L'interface `SecurityCheck` définit une méthode appelée `authorize`. Celle-ci est en charge de l'implémentation de la logique principale du contrôle de sécurité, comme l'envoi d'une demande d'authentification ou la validation de la demande.

La classe `CredentialsValidationSecurityCheck`, que `StepUpPinCode` étend, inclut déjà une implémentation pour cette méthode. Toutefois, en l'occurrence, l'objectif consiste à vérifier l'état de StepUpUserLogin avant de lancer le comportement par défaut de la méthode `authorize`.

Pour ce faire, **remplacez** la méthode `authorize` :

```java
@Override
public void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response) {
    if(userLogin.isLoggedIn()){
        super.authorize(scope, credentials, request, response);
    }
}
```
{: codeblock}

Cette implémentation vérifie l'état en cours de la référence `StepUpUserLogin` :

* Si l'état est `STATE_SUCCESS` (l'utilisateur est connecté), le flux normal du contrôle de sécurité continue.
* Si `StepUpUserLogin` est dans un autre état, aucune action n'est effectuée : aucune demande d'authentification n'est envoyée (pas de réussite ni d'échec).

Si la ressource est protégée **à la fois** par `StepUpPinCode` et `StepUpUserLogin`, ce flux s'assure que l'utilisateur est connecté avant qu'il ne soit invité à entrer les données d'identification secondaires (le code PIN). Le client ne reçoit jamais les deux demandes d'authentification simultanément, même si les deux contrôles de sécurité sont activés.

Si la ressource est protégée **uniquement** par `StepUpPinCode` (l'infrastructure n'activera que ce contrôle de sécurité), vous pouvez choisir de changer l'implémentation de la méthode `authorize` afin de déclencher `StepUpUserLogin` manuellement :

```java
@Override
public void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response) {
    if(userLogin.isLoggedIn()){
        //If StepUpUserLogin is successful, continue the normal processing of StepUpPinCode
        super.authorize(scope, credentials, request, response);
    } else {
        //In any other case, process StepUpUserLogin instead.
        userLogin.authorize(scope, credentials, request, response);
    }
}
```
{: codeblock}

## Extraction de l'utilisateur en cours
{: #retrieve-current-user}

Dans le contrôle de sécurité `StepUpPinCode`, vous avez besoin de connaître l'ID de l'utilisateur en cours afin de pouvoir rechercher le code PIN de cet utilisateur dans une base de données.

Dans le contrôle de sécurité `StepUpUserLogin`, ajoutez la méthode suivante afin d'obtenir l'utilisateur en cours depuis le **contexte d'autorisation** :

```java
public AuthenticatedUser getUser(){
    return authorizationContext.getActiveUser();
}
```
{: codeblock}

Ensuite, dans `StepUpPinCode`, vous pouvez utiliser la méthode `userLogin.getUser()` pour obtenir l'utilisateur en cours depuis le contrôle de sécurité `StepUpUserLogin` et consulter le code PIN valide pour cet utilisateur spécifique :

```java
@Override
protected boolean validateCredentials(Map<String, Object> credentials) {
    //Get the correct PIN code from the database
    User user = userManager.getUser(userLogin.getUser().getId());

    if(credentials!=null && credentials.containsKey(PINCODE_FIELD)){
        String pinCode = credentials.get(PINCODE_FIELD).toString();

        if(pinCode.equals(user.getPinCode())){
            errorMsg = null;
            return true;
        }
        else{
            errorMsg = "Wrong credentials. Hint: " + user.getPinCode();
        }
    }
    return false;
}
```
{: codeblock}

## Gestionnaires de demandes d'authentification
{: #challenge-handlers}

Côté client, aucune API spéciale ne permet de gérer plusieurs étapes. A la place, chaque gestionnaire de demandes d'authentification gère sa propre demande d'authentification. Dans cet exemple, vous devez enregistrer deux gestionnaires de demandes d'authentification distincts : l'un pour gérer les demandes d'authentification provenant de `StepUpUserLogin` et l'autre pour gérer les demandes d'authentification provenant de `StepUpPincode`.
