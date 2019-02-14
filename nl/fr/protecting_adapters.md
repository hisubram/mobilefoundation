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

# Protection des adaptateurs
{: #protecting_adapters}

## Protection des ressources d'adaptateur 
{: #protecting_adapters_resources}

Dans votre adaptateur, vous pouvez spécifier la portée de protection pour une méthode Java ou une procédure de ressource JavaScript, ou pour une classe de ressources Java entière, comme expliqué dans les sections [Java](#protect-java-adapter-resources) et [JavaScript](#protect-javascript-adapter-resources) ci-après. Une portée est définie sous forme de chaîne composée d'un ou de plusieurs éléments de portée séparés par un espace (“élémentPortée1 élémentPortée2 …”), ou comme valeur null pour appliquer la portée par défaut. 

La portée MobileFirst par défaut est `RegisteredClient` ; elle requiert un jeton d'accès pour l'accès à la ressource et vérifie que la demande de ressource provient d'une application qui est enregistrée sur le serveur MobileFirst. Cette protection est toujours appliquée, sauf si vous [désactivez la protection des ressources](#disabling-resource-protection). Par conséquent, même si vous ne définissez pas de portée pour votre ressource, celle-ci sera protégée. 

>**Remarque** : `RegisteredClient` est un mot clé MobileFirst réservé. Ne définissez pas d'élément de portée ni de contrôle de sécurité personnalisé de ce nom. 
{.note}

### Protection des ressources d'adaptateur Java
{: #protect-java-adapter-resources}

Pour affecter une portée de protection à une méthode JAX-RS ou à une classe, ajoutez l'annotation `@OAuthSecurity` à la déclaration de méthode ou de classe et associez l'élément `scope` de l'annotation à la portée de votre choix. Remplacez `YOUR_SCOPE` par une chaîne d'un ou de plusieurs éléments (“élémentPortée1 élémentPortée2 …”) : 

```java
@OAuthSecurity(scope = "YOUR_SCOPE")
```
{: codeblock}

Une portée de classe s'applique à toutes les méthodes de la classe, sauf aux méthodes possédant leur propre annotation `@OAuthSecurity`. 

>**Remarque** : lorsque l'élément enabled de l'annotation `@OAuthSecurity` a pour valeur `false`, l'élément de portée est ignoré. Voir [Désactivation de la protection des ressources Java](#disabling-java-resource-protection). 

**Exemples**

Le code suivant protège une méthode `helloUser` avec une portée contenant les éléments de portée `UserAuthentication` et `Pincode` : 

```java
@GET
@Path("/{username}")
@OAuthSecurity(scope = "UserAuthentication Pincode")
public String helloUser(@PathParam("username") String name){
    ...
}
```
{: codeblock}

Le code suivant protège une classe `WebSphereResources` avec le contrôle de sécurité prédéfini `LtpaBasedSSO` :

```java
@Path("/users")
@OAuthSecurity(scope = "LtpaBasedSSO")
public class WebSphereResources {
    ...
}
```
{: codeblock}

### Protection des ressources d'adaptateur JavaScript
{: #protect-javascript-adapter-resources}

Pour affecter une portée de protection à une procédure JavaScript, dans le fichier **adapter.xml**, associez l'attribut de portée de l'élément <procedure> à la portée de votre choix. Remplacez `PROCEDURE_NAME` par le nom de votre procédure et `YOUR SCOPE` par une chaîne d'un ou de plusieurs éléments (“élémentPortée1 élémentPortée2 …”) : 

```javascript
<procedure name="PROCEDURE_NAME" scope="YOUR_SCOPE">
```
{: codeblock}

>**Remarque** : lorsque l'attribut `secured` de l'élément <procedure> a pour valeur false, l'attribut `scope` est ignoré. Voir [Désactivation de la protection des ressources JavaScript](#disabling-javascript-resource-protection).
{.note}

**Exemple**

Le code suivant protège une procédure `userName` avec une portée qui contient les éléments de portée
`UserAuthentication` et `Pincode` :

```javascript
<procedure name="userName" scope="UserAuthentication Pincode">
```
{: codeblock}

## Désactivation de la protection des ressources 
{: #disabling-resource-protection}

Vous pouvez désactiver la [protection des ressources MobileFirst par défaut](#protecting_adapters_resources) pour une ressource d'adaptateur Java ou JavaScript spécifique ou pour une classe Java entière, comme expliqué dans les sections Java et JavaScript ci-après. Lorsque la protection des ressources est désactivée, l'infrastructure de sécurité MobileFirst ne requiert pas de jeton pour l'accès à la ressource. 

### Désactivation de la protection des ressources Java 
{: #disabling-java-resource-protection}

Afin de désactiver entièrement la protection OAuth pour une classe ou une méthode de ressources Java, ajoutez l'annotation `@OAuthSecurity` à la déclaration de classe ou de ressource et définissez la valeur `false` pour l'élément `enabled` : 

```java
@OAuthSecurity(enabled = false)
```
{: codeblock}

La valeur par défaut de l'élément `enabled` de l'annotation est `true`. Lorsque l'élément `enabled` a pour valeur `false`, l'élément `scope` est ignoré et la ressource ou la classe de ressources n'est pas protégée. 

>**Remarque** : lorsque vous affectez une portée à une méthode d'une classe non protégée, la méthode est protégée malgré l'annotation de classe, sauf si vous avez défini la valeur `false` pour l'élément `enabled` de l'annotation de ressource. 
{.note}

**Exemples**

Le code suivant désactive la protection des ressources pour une méthode `helloUser` :

```java
    @GET
    @Path("/{username}")
    @OAuthSecurity(enabled = "false")
    public String helloUser(@PathParam("username") String name){
        ...
    }
```
{: codeblock}

Le code suivant désactive la protection des ressources pour une classe `MyUnprotectedResources` : 

```java
    @Path("/users")
    @OAuthSecurity(enabled = "false")
    public class MyUnprotectedResources {
        ...
    }
```
{: codeblock}

### Désactivation de la protection des ressources JavaScript 
{: #disabling-javascript-resource-protection}

Afin de désactiver entièrement la protection OAuth pour une ressource d'adaptateur JavaScript (procédure), dans le fichier **adapter.xml**, définissez la valeur `false` pour l'attribut `secured` de l'élément <procedure> : 

```javascript
<procedure name="procedureName" secured="false">
```
{: codeblock}

Lorsque l'attribut `secured` a pour valeur `false`, l'attribut `scope` est ignoré et la ressource n'est pas protégée. 

**Exemple**

Le code suivant désactive la protection des ressources pour une procédure `userName` :

```javascript
<procedure name="userName" secured="false">
```
{: codeblock}

## Ressources non protégées
{: #unprotected-resources}

Une ressource non protégée est une ressource qui ne requiert pas de jeton d'accès. L'infrastructure de sécurité MobileFirst ne gère pas les accès aux ressources non protégées et ne valide pas ni ne vérifie l'identité des clients qui accèdent à ces ressources. Par conséquent, les fonctions telles que la mise à jour directe, le blocage de l'accès d'un appareil ou la désactivation à distance d'une application ne sont pas prises en charge pour les ressources non protégées. 

