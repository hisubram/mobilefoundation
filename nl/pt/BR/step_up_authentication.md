---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-19"

keywords: mobile foundation security, authentication, using challenge handlers

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
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:xml: .ph data-hd-programlang='xml'}

# Autenticação de Etapas
{: #step_up_authentication}

Os recursos podem ser protegidos por várias verificações de segurança. Nesse cenário, o MobileFirst Server envia todos os desafios relevantes simultaneamente para o aplicativo.

Uma verificação de segurança também pode ser dependente de outra verificação de segurança. Portanto, é importante ser capaz de controlar quando os desafios são enviados.

Por exemplo, este tutorial descreve um aplicativo que tem dois recursos protegidos por um nome de usuário e uma senha, em que o segundo recurso também requer um código PIN adicional.

## Referenciando uma Verificação de Segurança
{: #referencing-a-security-check}

Crie duas verificações de segurança: `StepUpPinCode` e `StepUpUserLogin`.

Neste exemplo, `StepUpPinCode` depende de `StepUpUserLogin`. É necessário solicitar ao usuário que insira um código PIN somente após um login bem-sucedido em `StepUpUserLogin`. Para esse propósito, `StepUpPinCode` deve ser capaz de referenciar a classe `StepUpUserLogin`.

A estrutura do MobileFirst fornece uma anotação para injetar uma referência.

Em sua classe `StepUpPinCode`, no nível de classe, inclua:

```java
@SecurityCheckReference private transient StepUpUserLogin userLogin;
```
{: codeblock}

>**Importante**: ambas as implementações de verificação de segurança precisam ser empacotadas dentro do mesmo adaptador.
{.important}

Para resolver essa referência, a estrutura consulta a verificação de segurança com a classe apropriada e injeta sua referência na verificação de segurança dependente.

Se houver mais de uma verificação de segurança da mesma classe, a anotação terá um parâmetro de nome opcional, que poderá ser usado para especificar o nome exclusivo da verificação referida.

## Máquina de estado
{: #state-machine}

Todas as classes que ampliam `CredentialsValidationSecurityCheck` (que inclui tanto `StepUpPinCode` quanto `StepUpUserLogin`) herdam uma máquina de estado simples. Em qualquer momento determinado, a verificação de segurança pode estar em um destes estados:

* `STATE_ATTEMPTING`: um desafio foi enviado e a verificação de segurança está aguardando a resposta do cliente. A contagem de tentativas é mantida durante esse estado.
* `STATE_SUCCESS`: as credenciais foram validadas com sucesso.
* `STATE_BLOCKED`: o número máximo de tentativas foi atingido e a verificação está no estado bloqueado.

O estado atual pode ser obtido usando o método `getState()` herdado.

em `StepUpUserLogin`, inclua um método de conveniência para verificar se o usuário está conectado atualmente. Esse método é usado posteriormente no tutorial.

```java
public boolean isLoggedIn(){
    return this.getState().equals(STATE_SUCCESS);
}
```
{: codeblock}

## O Método de Autorização
{: #the-authorize-method}

A interface `SecurityCheck` define um método chamado `authorize`. Esse método é responsável por implementar a lógica principal da verificação de segurança, como enviar um desafio ou validar a solicitação.

A classe `CredentialsValidationSecurityCheck`, ampliada por `StepUpPinCode`, já inclui uma implementação para esse método. No entanto, nesse caso, o objetivo é verificar o estado de StepUpUserLogin antes de iniciar o comportamento padrão do método `authorize`.

Para fazer isso, **substitua** o método `authorize`:

```java
@Override
public void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response) {
    if(userLogin.isLoggedIn()){
        super.authorize(scope, credentials, request, response);
    }
}
```
{: codeblock}

Essa implementação verifica o estado atual da referência `StepUpUserLogin`:

* Se o estado for `STATE_SUCCESS` (o usuário está conectado), o fluxo normal da verificação de segurança continuará.
* Se `StepUpUserLogin` estiver em qualquer outro estado, nada será feito: nenhum desafio será enviado, nem de sucesso nem de falha.

Supondo que o recurso esteja protegido por **ambos**, `StepUpPinCode` e `StepUpUserLogin`, esse fluxo garante que o usuário esteja conectado antes de ser solicitado pela credencial secundária (código PIN). O cliente nunca recebe os dois desafios ao mesmo tempo, mesmo que ambas as verificações de segurança estejam ativadas.

Como alternativa, se o recurso estiver protegido **somente** por `StepUpPinCode` (a estrutura ativará somente essa verificação de segurança), será possível mudar a implementação de `authorize` para acionar `StepUpUserLogin` manualmente:

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

## Recuperar usuário atual
{: #retrieve-current-user}

Na verificação de segurança `StepUpPinCode`, você está interessado em saber o ID do usuário atual para que possa consultar o código PIN desse usuário em algum banco de dados.

Na verificação de segurança `StepUpUserLogin`, inclua o método a seguir para obter o usuário atual por meio do **contexto de autorização**:

```java
public AuthenticatedUser getUser(){
    return authorizationContext.getActiveUser();
}
```
{: codeblock}

Em `StepUpPinCode`, é possível então usar o método `userLogin.getUser()` para obter o usuário atual por meio da verificação de segurança `StepUpUserLogin` e verificar o código PIN válido desse usuário específico:

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

## Manipuladores de Desafio
{: #challenge-handlers}

No lado do cliente, não há APIs especiais para manipular diversas etapas. Em vez disso, cada manipulador de desafios manipula seu próprio desafio. Neste exemplo, deve-se registrar dois manipuladores de desafios separados: um para manipular desafios por meio do `StepUpUserLogin` e um para manipular desafios por meio do `StepUpPincode`.
