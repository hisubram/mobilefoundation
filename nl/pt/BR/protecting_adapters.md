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

# Protegendo adaptadores
{: #protecting_adapters}

## Protegendo recursos do adaptador
{: #protecting_adapters_resources}

Em seu adaptador, é possível especificar o escopo de proteção para um método Java ou procedimento de recurso JavaScript ou para uma classe de recursos Java inteira, conforme descrito nas seções [Java](#protect-java-adapter-resources) e [JavaScript](#protect-javascript-adapter-resources) a seguir. Um escopo é definido como uma sequência de um ou mais elementos de escopo separados por espaço (“scopeElement1 scopeElement2 …”) ou nulo para aplicar o escopo padrão.

O escopo padrão do MobileFirst é `RegisteredClient`, que requer um token de acesso para acessar o recurso e verifica se a solicitação de recurso é de um aplicativo que está registrado com o MobileFirst Server. Essa proteção é sempre aplicada, a menos que você [desative a proteção de recurso](#disabling-resource-protection). Portanto, mesmo se você não configurar um escopo para seu recurso, ele ainda estará protegido.

>**Nota**: `RegisteredClient` é uma palavra-chave reservada do MobileFirst. Não defina elementos de escopo customizados ou verificações de segurança com esse nome.
{.note}

### Protegendo recursos do adaptador Java
{: #protect-java-adapter-resources}

Para designar um escopo de proteção a um método ou uma classe JAX-RS, inclua a anotação `@OAuthSecurity` no método ou na declaração de classe e configure o elemento `scope` da anotação com seu escopo preferencial. Substitua `YOUR_SCOPE` por uma sequência de um ou mais elementos de escopo (“scopeElement1 scopeElement2 …”):

```java
@OAuthSecurity(scope = "YOUR_SCOPE")
```
{: codeblock}

Um escopo de classe aplica-se a todos os métodos na classe, exceto a métodos que tenham sua própria anotação `@OAuthSecurity`.

>**Nota**: quando o elemento ativado da anotação `@OAuthSecurity` estiver configurado como `false`, o elemento do escopo será ignorado. Consulte  [ Desativando a proteção de recurso Java ](#disabling-java-resource-protection).

**Exemplos**

O código a seguir protege um método `helloUser` com um escopo que contém os elementos de escopo `UserAuthentication` e `Pincode`:

```java
@GET
@Path("/{username}")
@OAuthSecurity(scope = "UserAuthentication Pincode")
public String helloUser(@PathParam("username") String name){
    ...
}
```
{: codeblock}

O código a seguir protege uma classe `WebSphereResources` com a verificação de segurança `LtpaBasedSSO` predefinida:

```java
@Path("/users")
@OAuthSecurity(scope = "LtpaBasedSSO")
public class WebSphereResources {
    ...
}
```
{: codeblock}

### Protegendo recursos do adaptador JavaScript
{: #protect-javascript-adapter-resources}

Para designar um escopo de proteção a um procedimento JavaScript, no arquivo **adapter.xml**, configure o atributo de escopo do elemento <procedure> com seu escopo preferencial. Substitua `PROCEDURE_NANE` pelo nome de seu procedimento e `YOUR SCOPE` por uma sequência de um ou mais elementos de escopo (“scopeElement1 scopeElement2 …”):

```javascript
<procedure name="PROCEDURE_NANE" scope="YOUR_SCOPE">
```
{: codeblock}

>**Nota**: quando o atributo `secured` do elemento <procedure> estiver configurado como false, o atributo `scope` será ignorado. Consulte  [ Desativando a proteção de recurso JavaScript ](#disabling-javascript-resource-protection).
{.note}

** Exemplo **

O código a seguir protege um procedimento `userName` com um escopo que contém os elementos de escopo `UserAuthentication` e `Pincode`:

```javascript
<procedure name="userName" scope="UserAuthentication Pincode">
```
{: codeblock}

## Desativando a proteção de recurso
{: #disabling-resource-protection}

É possível desativar a [proteção de recurso padrão do MobileFirst](#protecting_adapters_resources) de um recurso de adaptador Java ou JavaScript específico ou de uma classe Java inteira, conforme descrito nas seções Java e JavaScript a seguir. Quando a proteção de recurso está desativada, a estrutura de segurança do MobileFirst não requer que um token acesse o recurso.

### Desativando a proteção de recurso Java
{: #disabling-java-resource-protection}

Para desativar completamente a proteção de OAuth para um método ou classe de recurso Java, inclua a anotação `@OAuthSecurity` na declaração do recurso ou da classe e configure o valor do elemento `enabled` como `false`:

```java
@OAuthSecurity (enabled = false)
```
{: codeblock}

O valor padrão do elemento `enabled` da anotação é `true`. Quando o elemento `enabled` está configurado como `false`, o elemento `scope` é ignorado e o recurso ou a classe de recurso fica desprotegida.

>**Nota**: ao designar um escopo a um método de uma classe desprotegida, o método é protegido apesar da anotação de classe, a menos que você configure o elemento `enabled` da anotação de recurso como `false`.
{.note}

**Exemplos**

O código a seguir desativa a proteção de recurso para um método `helloUser`:

```java
    @GET
    @Path("/{username}")
    @OAuthSecurity(enabled = "false")
    public String helloUser(@PathParam("username") String name){
        ...
    }
```
{: codeblock}

O código a seguir desativa a proteção de recurso para uma classe `MyUnprotectedResources`:

```java
    @Path("/users")
    @OAuthSecurity(enabled = "false")
    public class MyUnprotectedResources {
        ...
    }
```
{: codeblock}

### Desativando a proteção de recurso JavaScript
{: #disabling-javascript-resource-protection}

Para desativar completamente a proteção de OAuth para um recurso de adaptador JavaScript (procedimento), no arquivo **adapter.xml**, configure o atributo `secured` do elemento <procedure> como `false`:

```javascript
<procedure name="procedureName" secured="false">
```
{: codeblock}

Quando o atributo `secured` é configurado como `false`, o atributo `scope` é ignorado e o recurso fica desprotegido.

** Exemplo **

O código a seguir desativa a proteção de recurso de um procedimento `userName`:

```javascript
<procedure name="userName" secured="false">
```
{: codeblock}

## Recursos desprotegidos
{: #unprotected-resources}

Um recurso desprotegido é um recurso que não requer um token de acesso. A estrutura de segurança do MobileFirst não gerencia o acesso a recursos desprotegidos e não valida nem verifica a identidade de clientes que acessam esses recursos. Portanto, recursos, como Atualização direta, bloqueio de acesso ao dispositivo ou desativação remota de um aplicativo, não são suportados para recursos desprotegidos.

