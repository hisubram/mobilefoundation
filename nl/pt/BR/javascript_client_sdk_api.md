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

# API de SDK do Cliente do Javascript
{: #javascript_client_sdk_api}

## APIs para aplicativos Cordova/web (Javascript)

A tabela a seguir lista as funções que podem ser executadas nos aplicativos Javascript e o método de API correspondente.

| Função | Descrição |
|----------|-------------|
| `WL.Client`, `WL.App` | Inicializando e recarregando um aplicativo, Globalizando textos de aplicativos | 
| `WLAuthorizationManager` | Obter o ID de cliente e o cabeçalho de autorização |
| `WL.Logger` | Imprimindo mensagens de log no log para o ambiente |
| `WL.NativePage` | Alternando a tela baseada na web atualmente exibida com uma página gravada nativamente |
| `WLResourceRequest` | Enviar solicitações para recursos protegidos e desprotegidos | 
| `WL.JSONStore` | API do lado do cliente fornecendo um sistema de armazenamento leve e orientado a documentos | 

## Informações adicionais
{: #additional-information }
### O objeto de Opções
{: #the-optional-object }
O objeto `options` contém propriedades que são comuns a todos os métodos. Ele é usado em todas as chamadas assíncronas para o {{ site.data.keys.mf_server }}.

Às vezes, ele é aumentado por propriedades que são aplicáveis somente a métodos específicos. Essas propriedades adicionais são detalhadas como parte da descrição dos métodos específicos.

As propriedades comuns do objeto de opções são as seguintes:

```javascript
options = {
    onSuccess: successHandler(response),
    onFailure: failureHandlder(response),
    invocationContext: invocation-context
};
```
{: code}
O significado de cada propriedade é conforme a seguir:

| Propriedade | Descrição |
|----------|-------------|
| `onSuccess` | Opcional. A função a ser chamada na conclusão bem-sucedida da chamada assíncrona. A sintaxe da função `onSuccess` é: `success-handler-function(response)` em que `response` é um objeto que contém no mínimo a propriedade a seguir: {::nomarkdown}<ul><li><b>invocationContext</b> - O objeto <code>invocationContext</code> que foi passado originalmente para o {{site.data.keys.mf_server }} no objeto <code>options</code> ou <code>undefined</code>, caso nenhum objeto <code>invocationContext</code> tenha sido passado.</li><li><b>status</b> - O status da resposta HTTP</li></ul>{:/} **Nota:** para métodos para os quais o objeto `response` contém propriedades adicionais, essas propriedades são detalhadas como parte da descrição do método específico. |
| `onFailure` | Opcional. A função a ser chamada quando a chamada assíncrona falha. Tais falhas incluem erros do lado do servidor e erros do lado do cliente que ocorreram durante chamadas assíncronas, como falha de conexão do servidor ou chamadas com tempo limite atingido. **Nota:** a função não é chamada para erros do lado do cliente que param a execução lançando uma exceção. A sintaxe da função onFailure é: `failure-handler-function(response)` em que `response` é um objeto que contém as propriedades a seguir: {::nomarkdown}<ul><li><b>invocationContext</b> - O objeto <code>invocationContext</code> que foi passado originalmente para o {{site.data.keys.mf_server }} no objeto <code>options</code> ou <code>undefined</code>, caso nenhum objeto <code>invocationContext</code> tenha sido passado.</li><li><b>errorCode</b> - uma sequência de códigos de erro. Todos os códigos de erro que podem ser retornados são definidos como constantes no objeto <code>WL.ErrorCode</code> no arquivo <b>worklight.js</b>.</li><li><b>errorMsg</b> - uma mensagem de erro que é fornecida pelo {{ site.data.keys.mf_server }}. Essa mensagem é somente para uso do desenvolvedor e não deve ser exibida para o usuário. Ela não será traduzida para o idioma do usuário.</li><li><b>status</b> - O status da resposta HTTP</li></li></ul>{:/} **Nota:** para métodos para os quais o objeto `response` contém propriedades adicionais, essas propriedades são detalhadas como parte da descrição do método específico. |
| `invocationContext` | Opcional. Um objeto que é retornado para os manipuladores de sucesso e de falha. O objeto `invocationContext` é usado para preservar o contexto do serviço assíncrono de chamada após o retorno do serviço. Por exemplo, o método `invokeProcedure` pode ser chamado sucessivamente, usando o mesmo manipulador de sucesso. O manipulador de sucesso precisa ser capaz de identificar qual chamada para invokeProcedure está sendo manipulada. Uma solução é implementar o objeto `invocationContext` como um número inteiro e incrementar seu valor por um para cada chamada de `invokeProcedure`. Quando ele chama o manipulador de sucesso, a estrutura do {{ site.data.keys.product_adj }} passa para ele o objeto `invocationContext` do objeto de opções associado ao método `invokeProcedure`. O valor do objeto `invocationContext` pode ser usado para identificar a chamada para `invokeProcedure` com a qual os resultados que estão sendo manipulados estão associados. | 

## O objeto WL.ClientMessages
{: #the-wlclientmessages-object }
É possível ver uma lista das mensagens do sistema que estão armazenadas no objeto `WL.ClientMessages` e ativar a tradução dessas mensagens do sistema.

Deve-se substituir as mensagens do sistema em um nível JavaScript global porque algumas partes do código são executadas somente após o aplicativo ser inicializado com êxito.
{: note}

O objeto `WL.ClientMessages` é um objeto que armazena as mensagens do sistema que são definidas no arquivo **worklight/messages/messages.json**. Esse arquivo está na pasta de ambiente dos projetos que você gerou com o {{ site.data.keys.product }}. Para ativar a tradução de uma mensagem do sistema, deve-se substituir o valor dessa mensagem no objeto `WL.ClientMessages`, conforme indicado no exemplo de código a seguir:

```javascript
WL.ClientMessages.invalidUsernamePassword="The custom user name and password are not valid";
```
{: code}


* [Referência da API JavaScript](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/#javascript-api-reference)
* [Referência da API Objective-C (para Cordova)](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/#objective-c-api-reference-for-cordova)
{: note}
