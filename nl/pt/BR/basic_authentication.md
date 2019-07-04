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


# Autenticação e segurança
{: #basic_authentication}

A estrutura de segurança do MobileFirst baseia-se no protocolo [OAuth 2.0](http://oauth.net/). De acordo com esse protocolo, um recurso pode ser protegido por um **escopo** que define as permissões necessárias para acessar o recurso. Para acessar um recurso protegido, o cliente deve fornecer um **token de acesso** correspondente, que contenha o escopo da autorização concedida ao cliente.

O protocolo OAuth separa as funções do servidor de autorizações e do servidor de recurso em que o recurso é hospedado.

* O servidor de autorizações gerencia a autorização do cliente e a geração do token.
* O servidor de recurso usa o servidor de autorizações para validar o token de acesso fornecido pelo cliente e assegurar que ele corresponda ao escopo de proteção do recurso solicitado.

A estrutura de segurança é construída em torno de um servidor de autorizações que implementa o protocolo OAuth e expõe os terminais OAuth com os quais o cliente interage para obter tokens de acesso. A estrutura de segurança fornece os blocos de construção para implementar uma lógica de autorização customizada sobre o servidor de autorizações e o protocolo OAuth subjacente. Por padrão, o MobileFirst Server também funciona como o **servidor de autorizações**. No entanto, é possível configurar um dispositivo IBM WebSphere DataPower para agir como o servidor de autorizações e interagir com o MobileFirst Server.

O aplicativo cliente pode então usar esses tokens para acessar recursos em um **servidor de recurso**, que pode ser o próprio MobileFirst Server ou um servidor externo. O servidor de recurso verifica a validade do token para se certificar de que o cliente pode receber acesso ao recurso solicitado. A separação entre o servidor de recurso e o servidor de autorizações cumpre a segurança nos recursos que estão em execução fora do MobileFirst Server.

Os desenvolvedores de aplicativos protegem o acesso a seus recursos definindo o escopo necessário para cada recurso protegido e implementando verificações de segurança e manipuladores de desafios. A estrutura de segurança do lado do servidor e a API do lado do cliente manipulam a troca de mensagem de OAuth e a interação com o servidor de autorizações de forma transparente, permitindo que os desenvolvedores se concentrem apenas na lógica de autorização.

## Entidades de Autorização
{: #acs_authorization_entitiesty}

### Tokens de Acesso
{: #acs_access_tokens}

Um token de acesso do MobileFirst é uma entidade assinada digitalmente que descreve as permissões de autorização de um cliente. Depois que a solicitação de autorização do cliente para um escopo específico é concedida e o cliente é autenticado, o terminal de token do servidor de autorizações envia ao cliente uma resposta HTTP que contém o token de acesso solicitado.

#### Estrutura de token de acesso

O token de acesso do MobileFirst contém as informações a seguir:

* **ID do cliente**: é um identificador exclusivo do cliente.
* **Escopo**: o escopo para o qual o token foi concedido (consulte escopos OAuth). Este escopo não inclui o escopo de aplicativo obrigatório.
* **Horário de expiração de token**: o horário em que o token se torna inválido (expira), em segundos.

#### Expiração do Token
{: #token-expiration}

O token de acesso concedido permanece válido até que seu prazo de expiração decorra. O prazo de expiração do token de acesso é configurado com o prazo mais curto entre os prazos de expiração de todas as verificações de segurança no escopo. Mas se o período até o prazo de expiração mais curto for maior que o período máximo de expiração do token do aplicativo, o prazo de expiração do token será configurado com o horário atual mais o período máximo de expiração. O período máximo padrão de expiração do token (duração da validade) é de 3.600 segundos (1 hora), mas pode ser configurado ao definir o valor da propriedade ``maxTokenExpiration``.

##### Configurando o período máximo de expiração do token de acesso
{: #acs_config-max-access-tokens}

Configure o período máximo de expiração do token de acesso do aplicativo usando um dos métodos alternativos a seguir:

* Usando o MobileFirst Operations Console
    1. Selecione  ** [ seu aplicativo ] **  → guia  ** Segurança ** .
    2. Na seção **Configuração do token**, configure o valor do campo **Período máximo de expiração do token (segundos)** com seu valor preferencial e clique em **Salvar**. É possível repetir esse procedimento, a qualquer momento, para mudar o período máximo de expiração do token ou selecionar **Restaurar valores padrão** para restaurar o valor padrão.

* Editando o arquivo de configuração do aplicativo

    1. A partir de uma CLI, navegue até a pasta raiz do projeto e execute o comando a seguir.
      ```bash
      mfpdev app pull
      ```
    2. Abra o arquivo de configuração, que está localizado na pasta `[project-folder]\mobilefirst`.
    3. Edite o arquivo definindo uma propriedade `maxTokenExpiration` e configure seu valor com o período máximo de expiração do token de acesso, em segundos:
        ```java
        {
            ...
            "maxTokenExpiration": 7200
        }
        ```
        {: codeblock}
    4. Implemente o arquivo JSON de configuração atualizado executando o comando: ``mfpdev app push``.

##### Estrutura de resposta do token de acesso
{: #acs_access-tokens-structure}

Uma resposta de HTTP bem-sucedida a uma solicitação de token de acesso contém um objeto JSON com o token de acesso e os dados extras. A seguir está um exemplo de uma resposta de token válido do servidor de autorizações:

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

O objeto JSON de resposta de token tem estes objetos de propriedade:

* **token_type**: o tipo de token é sempre "*De acesso*", de acordo com a [especificação Uso do Token de Acesso do OAuth 2.0](https://tools.ietf.org/html/rfc6750).
* **expires_in**: o prazo de expiração do token de acesso, em segundos.
* **access_token**: o token de acesso gerado (os tokens de acesso reais são maiores do que os mostrados no exemplo).
* ** scope **: o escopo solicitado.

As informações **expires_in** e **escopo** também estão contidas no próprio token (**access_token**).

**Nota**: A estrutura de uma resposta de token de acesso válida é relevante se você usar a classe `WLAuthorizationManager` de baixo nível e gerenciar a interação OAuth entre o cliente e a autorização e os próprios servidores de recursos, ou se você usar um cliente confidencial. Se você estiver usando a classe `WLResourceRequest` de alto nível, que encapsula o fluxo OAuth para acessar recursos protegidos, a estrutura de segurança manipulará o processamento de respostas do token de acesso para você. Consulte [APIs de segurança do cliente](http://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_oauth_client_apis.html?view=kc#c_oauth_client_apis) e [Clientes confidenciais](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/confidential-clients/).

### Tokens de Atualização
{: #acs_refresh_tokens}

Um token de atualização é um tipo especial de token, que pode ser usado para obter um novo token de acesso quando o token de acesso expira. Para solicitar um novo token de acesso, é possível apresentar um token de atualização válido. Os tokens de atualização são tokens de longa duração e permanecerão válidos por um período de tempo maior em comparação com os tokens de acesso.

O token de atualização deve ser usado com cuidado por um aplicativo porque ele pode permitir que um usuário permaneça autenticado para sempre. Os aplicativos de mídia social, aplicativos de e-commerce, navegação do catálogo do produto e esses aplicativos utilitários, em que o provedor de aplicativos não autentica usuários regularmente, podem usar tokens de atualização. Os aplicativos que exigem autenticação do usuário frequentemente devem evitar o uso de tokens de atualização.
Token de Atualização MobileFirst

Um token de atualização do MobileFirst é uma entidade assinada digitalmente, como o token de acesso que descreve as permissões de autorização de um cliente. O token de atualização pode ser usado para obter um novo token de acesso do mesmo escopo. Depois que a solicitação de autorização do cliente para um escopo específico é concedida e o cliente é autenticado, o terminal de token do servidor de autorizações envia ao cliente uma resposta HTTP que contém o token de acesso e o token de atualização solicitados. Quando o token de acesso expira, o cliente envia o token de atualização para o terminal de token do servidor de autorizações para obter um novo conjunto de tokens de acesso e de atualização.

#### Estrutura de token de atualização
{: #structure_refresh_tokens}

Semelhante ao token de acesso do MobileFirst, o token de atualização do MobileFirst contém as informações a seguir:

* **ID do cliente**: é um identificador exclusivo do cliente.
* **Escopo**: o escopo para o qual o token foi concedido (consulte escopos OAuth). Este escopo não inclui o escopo de aplicativo obrigatório.
* **Horário de expiração de token**: o horário em que o token se torna inválido (expira), em segundos.

##### Expiração do símbolo:
{: #str-token-expiration}

O período de expiração para o token de atualização é maior que o período de expiração do token de acesso típico. O token de atualização que é concedido uma vez permanece válido até que seu prazo de expiração decorra. Dentro desse período de validade, um cliente pode usar o token de atualização para obter um novo conjunto de tokens de acesso e de atualização. O token de atualização tem um período de expiração fixo de 30 dias. Toda vez que o cliente recebe um novo conjunto de tokens de acesso e de atualização com sucesso, a expiração do token de atualização é reconfigurada, fornecendo assim ao cliente uma experiência de um token que nunca expira. As regras de expiração do token de acesso permanecem as mesmas conforme explicado na seção **Token de acesso**.

##### Ativando o recurso de token de atualização
{: #acs_enable-refresh-token}

O recurso de token de atualização pode ser ativado usando as propriedades a seguir no lado do cliente e no lado do servidor, respectivamente.

Propriedade do lado do cliente (Android):
*Nome do arquivo*: mfpclient.properties
*Nome da propriedade*: wlEnableRefreshToken
*Valor da propriedade*: true
Por exemplo,
*wlEnableRefreshToken*=true

Propriedade do lado do cliente (iOS):
*Nome do arquivo*: mfpclient.plist
*Nome da propriedade*: wlEnableRefreshToken
*Valor da propriedade*: true
Por exemplo,
*wlEnableRefreshToken*=true

Propriedade do lado do servidor:
*Nome do arquivo*: server.xml
*Nome da propriedade*: mfp.security.refreshtoken.enabled.apps
*Valor da propriedade*: ID do pacote configurável do aplicativo separado por ‘;’

Por exemplo,

```xml
<jndiEntry jndiName="mfp/mfp.security.refreshtoken.enabled.apps" value='"com.sample.android.myapp1;com.sample.android.myapp2"'/>
```
{: codeblock}
Use IDs de pacotes configuráveis diferentes para plataformas diferentes.

##### Estrutura de resposta do token de atualização
{: #refresh-token-response-structure}

A seguir está um exemplo de uma resposta de token de atualização válido do servidor de autorizações:

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

A resposta de token de atualização tem um objeto de propriedade adicional `refresh_token` além dos outros objetos de propriedade que são explicados como parte da estrutura de resposta de token de acesso.

**Nota**: os tokens de atualização são de longa duração em comparação com os tokens de acesso. Portanto, o recurso de token de atualização deve ser usado com cuidado. Os aplicativos nos quais a autenticação de usuário periódica não é necessária são candidatos ideais para usar o recurso de token de atualização.

O MobileFirst suporta o recurso de token de atualização no iOS, iniciando na Atualização de CD 3.

#### Verificações de Segurança
{: #acs_securitychecks}

Uma verificação de segurança é uma entidade do lado do servidor que implementa a lógica de segurança para proteger os recursos do aplicativo do lado do servidor. Um exemplo simples de verificação de segurança é uma verificação de segurança de login do usuário que recebe as credenciais de um usuário e as verifica com relação a um registro do usuário. Outro exemplo é a verificação de segurança de autenticidade de aplicativo predefinida do MobileFirst, que valida a autenticidade do aplicativo móvel e, portanto, protege contra tentativas ilegais de acessar os recursos do aplicativo. A mesma verificação de segurança também pode ser usada para proteger vários recursos.

Uma verificação de segurança geralmente emite desafios de segurança que requerem que o cliente responda de uma maneira específica para passar na verificação. Esse handshake ocorre como parte do fluxo de aquisição do token de acesso do OAuth. O cliente usa **manipuladores de desafios** para manipular desafios de verificações de segurança.

##### Verificações de segurança integradas
{: #builtin-sec-checks}

As verificações de segurança predefinidas a seguir estão disponíveis:

* Autenticidade do Aplicativo
* Conexão única baseada em LTPA (SSO)
* Direct Update

#### Entidade do Challenge Handler
{: #challengehandler_entity}

Quando você tenta acessar um recurso protegido, o cliente pode ser confrontado com um desafio. Um desafio é uma pergunta, um teste de segurança, um prompt do servidor para se certificar de que o cliente tenha permissão para acessar esse recurso. Mais comumente, esse desafio é uma solicitação para credenciais, como um nome de usuário e uma senha.

Um manipulador de desafios é uma entidade do lado do cliente que implementa a lógica de segurança do lado do cliente e a interação com o usuário relacionada.

**Importante**: depois que um desafio é recebido, ele não pode ser ignorado. Deve-se responder ou cancelá-lo. Ignorar um desafio pode levar a um comportamento inesperado.

### Escopos
{: #scopes}

É possível proteger recursos, como adaptadores, contra acesso não autorizado, designando um escopo ao recurso.

Um escopo é definido como uma sequência de um ou mais elementos de escopo separados por espaço (“scopeElement1 scopeElement2 …”) ou nulo para aplicar o escopo padrão (RegisteredClient). A estrutura de segurança do MobileFirst requer um token de acesso para qualquer recurso do adaptador, mesmo que o recurso não tenha um escopo designado, a menos que você desative a proteção de recurso para o recurso.

#### Elementos do escopo
{: #scopeelements}

Um elemento de escopo pode ser um dos seguintes,

* O nome de uma verificação de segurança.
* Uma palavra-chave arbitrária, como `access-restricted` ou `deletePrivilege`, que define o nível de segurança que é necessário para esse recurso. Essa palavra-chave é mapeada posteriormente para uma verificação de segurança.

#### Mapeamento de Escopo
{: #scopemapping}

Por padrão, os **elementos de escopo** que você grava em seu **escopo** são mapeados para uma **verificação de segurança** com o mesmo nome. Por exemplo, se você gravar uma verificação de segurança que é chamada `PinCodeAttempts`, será possível usar um elemento do escopo com o mesmo nome dentro de seu escopo.

O Mapeamento de escopo permite mapear os elementos do escopo para verificações de segurança. Quando o cliente solicita um elemento do escopo, essa configuração define quais verificações de segurança são necessárias para serem aplicadas. Por exemplo, é possível mapear o elemento do escopo `access-restricted` para a verificação de segurança `PinCodeAttempts`.

O mapeamento de escopo será útil se você desejar proteger um recurso de forma diferente, dependendo de qual aplicativo está tentando acessá-lo. Também é possível mapear um escopo para uma lista de zero ou mais verificações de segurança.

Por exemplo: scope =  ` access-restricted deletePrivilege `

* No app A
    * ` access-restricted `  é mapeado para  ` PinCodeAttempts `.
    * `deletePrivilege` é mapeado para uma sequência de caracteres vazia.
* No app B
    * ` access-restricted `  é mapeado para  ` PinCodeAttempts `.
    * ` deletePrivilege `  é mapeado para  ` UserLogin `.

Para mapear seu elemento de escopo para uma sequência de caracteres vazia, não selecione nenhuma verificação de segurança no menu pop-up **Incluir novo mapeamento de elemento do escopo**.

![Mapeamento de escopo](/images/scope_mapping.png "Tela Incluir novo mapeamento de elemento do escopo")

Também é possível editar manualmente o arquivo JSON de configuração do aplicativo com a configuração necessária e enviar por push as mudanças de volta para um servidor MobileFirst.

1. Em uma **janela de linha de comandos**, navegue até a pasta raiz do projeto e execute `mfpdev app pull`.
2. Abra o arquivo de configuração, que está localizado na pasta `[project-folder]\mobilefirst`.
3. Edite o arquivo definindo uma propriedade `scopeElementMapping`. Nessa propriedade, defina os pares de dados compostos cada um pelo nome de seu elemento de escopo selecionado e uma sequência de zero ou mais verificações de segurança separadas por espaço para as quais o elemento é mapeado. Por exemplo:

```java
     "scopeElementMapping": {
         "UserAuth": "UserAuthentication",
         "SSOUserValidation": "LtpaBasedSSO CredentialsValidation"
     }
```
4. Implemente o arquivo JSON de configuração atualizado, executando o comando a seguir,
  ```bash
  Comando app mfpdev
  ```

Também é possível enviar por push configurações atualizadas para servidores remotos. Consulte o tutorial Usando a CLI do MobileFirst para gerenciar artefatos do MobileFirst.
{: note}

### Protegendo recursos
{: #protecting-resources}

No modelo OAuth, um recurso protegido é aquele que requer um token de acesso. É possível usar a estrutura de segurança do MobileFirst para proteger ambos os recursos hospedados em uma instância do MobileFirst Server e recursos em um servidor externo. Você protege um recurso designando a ele um escopo que define as permissões necessárias para adquirir um token de acesso para o recurso.

É possível proteger seus recursos de várias maneiras:

#### Escopo do aplicativo obrigatório
{: #mandatoryappscope}

No nível do aplicativo, é possível definir um escopo que se aplique a todos os recursos usados pelo aplicativo. A estrutura de segurança executará essas verificações (se existirem) além das verificações de segurança do escopo de recurso solicitado.

** Nota **:
   * O escopo do aplicativo obrigatório não é aplicado quando você acessa um recurso desprotegido.
   * O token de acesso concedido para o escopo de recurso não contém o escopo de aplicativo obrigatório.

No MobileFirst Operations Console, selecione seu aplicativo na seção **Aplicativos** da barra lateral de navegação e, em seguida, selecione a guia **Segurança**. Em **Escopo do aplicativo obrigatório**, selecione **Incluir no escopo**.

![Escopo do aplicativo obrigatório](/images/mandatory-application-scope.png "Tela Configurar o escopo do aplicativo obrigatório")

Também é possível editar manualmente o arquivo JSON de configuração do aplicativo com a configuração necessária e enviar por push as mudanças de volta para um servidor MobileFirst.

1. Em uma **janela de linha de comandos**, navegue até a pasta raiz do projeto e execute `mfpdev app pull`.
2. Abra o arquivo de configuração, que está localizado na pasta **project-folder\mobilefirst**.
3. Edite o arquivo definindo uma propriedade `mandatoryScope` e configurando o valor da propriedade para uma sequência de escopo que contenha uma lista separada por espaço de seus elementos de escopo selecionados. Por exemplo,

    ```java
        "mandatoryScope": "appAuthenticity PincodeValidation"
    ```
4. Implemente o arquivo JSON de configuração atualizado executando o comando: mfpdev app push.

Também é possível enviar por push configurações atualizadas para servidores remotos.

#### Protegendo recursos do adaptador
{: #protectadapterres}

Em seu adaptador, é possível especificar o escopo de proteção para um método Java, um procedimento de recurso JavaScript ou para uma classe de recurso Java inteira. Um escopo é definido como uma sequência de um ou mais elementos de escopo separados por espaço (“scopeElement1 scopeElement2 …”) ou nulo para aplicar o escopo padrão. Para obter mais informações sobre como proteger os recursos do adaptador, consulte [Protegendo adaptadores](/docs/services/mobilefoundation?topic=mobilefoundation-protecting_adapters#protecting_adapters).

### Desativando a proteção de recurso
{: #disablingresprotection}

É possível desativar a proteção de recurso padrão do MobileFirst de um recurso de adaptador Java ou JavaScript específico ou de uma classe Java inteira, conforme descrito nas seções Java e JavaScript a seguir. Quando a proteção de recurso está desativada, a estrutura de segurança do MobileFirst não requer que um token acesse o recurso.

#### Desativando a proteção de OAuth de recurso de Java
{: #disablejavaresoauthprotection}

Para desativar completamente a proteção de OAuth para um método ou classe de recurso Java, inclua a anotação `@OAuthSecurity` na declaração do recurso ou da classe e configure o valor do elemento `enabled` como `false`:

```
@OAuthSecurity (enabled = false)
```

O valor padrão do elemento `enabled` da anotação é `true`. Quando o elemento `enabled` está configurado como `false`, o elemento `scope` é ignorado e o recurso ou a classe de recurso fica desprotegida.

**Nota**: ao designar um escopo a um método de uma classe desprotegida, o método é protegido apesar da anotação de classe, a menos que você configure o elemento `enabled` da anotação de recurso como `false`.

Revise os seguintes exemplos:

O código a seguir desativa a proteção de recurso para um método `helloUser`:

```java
    @GET @Path("/{username}") @OAuthSecurity(enabled = "false") public String helloUser(@PathParam("username") String name){
        ...
    }
```

O código a seguir desativa a proteção de recurso para uma classe `MyUnprotectedResources`:

```java
    @Path("/users") @OAuthSecurity(enabled = "false") public class MyUnprotectedResources {
        ...
    }
```

#### Desativando a proteção de OAuth de recurso do Javascript
{: #disablejavascriptresoauthprotection}

Para desativar completamente a proteção de OAuth para um recurso de adaptador JavaScript (procedimento), no arquivo **adapter.xml**, configure o atributo `secured` do elemento <procedure> como `false`:

```javascript
<procedure name="procedureName" secured="false">
```

Quando o atributo `secured` é configurado como `false`, o atributo `scope` é ignorado e o recurso fica desprotegido.

Verifique
o seguinte exemplo:

O código a seguir desativa a proteção de recurso de um procedimento `userName`:

```javascript
<procedure name="userName" secured="false">
```

### Definindo recursos desprotegidos
{: #defunprotectedresources}

Um recurso desprotegido é um recurso que não requer um token de acesso. A estrutura de segurança do MobileFirst não gerencia o acesso a recursos desprotegidos e não valida nem verifica a identidade de clientes que acessam esses recursos. Portanto, recursos, como Atualização direta, bloqueio de acesso ao dispositivo ou desativação remota de um aplicativo, não são suportados para recursos desprotegidos.

### Protegendo recursos externos
{: #protecextresources}

Para proteger recursos externos, você inclui um filtro de recurso com um módulo de validação de token de acesso no servidor de recurso externo. O módulo de validação de token usa o terminal de introspecção do servidor de autorizações da estrutura de segurança para validar tokens de acesso do MobileFirst antes que o acesso do cliente OAuth seja concedido aos recursos. É possível usar a API de REST do MobileFirst para o tempo de execução do MobileFirst para criar seu próprio módulo de validação de token de acesso para qualquer servidor externo. Como alternativa, use uma das extensões fornecidas do MobileFirst para proteger recursos Java externos.
