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

# API per applicazioni Cordova/web (Javascript)

La seguente tabella elenca le funzioni che puoi eseguire nelle applicazioni Javascript e il metodo API corrispondente.

| Funzione | Descrizione |
|----------|-------------|
| `WL.Client`, `WL.App` | Inizializzazione e ricaricamento di un'applicazione, Globalizzazione dei testi dell'applicazione | 
| `WLAuthorizationManager` | Acquisizione di ID client e intestazione dell'autorizzazione |
| `WL.Logger` | Stampa dei messaggi di log nel log per l'ambiente |
| `WL.NativePage` | Passaggio dalla schermata basata sul web attualmente visualizzata alla pagina scritta in modo nativo |
| `WLResourceRequest` | Invio di richieste a risorse protette e non protette | 
| `WL.JSONStore` | API lato client che fornisce un semplice sistema di archiviazione orientato ai documenti | 

## Ulteriori informazioni
{: #additional-information }
### Oggetto Options
{: #the-optional-object }
L'oggetto `options` contiene proprietà che sono comuni a tutti i metodi. Viene utilizzato in tutte le chiamate asincrone a {{ site.data.keys.mf_server }}.

A volte è ampliato da proprietà che sono applicabili solo a metodi specifici. Queste proprietà aggiuntive vengono dettagliate come parte della descrizione degli specifici metodi.

Le proprietà comuni dell'oggetto options sono le seguenti:

```javascript
options = {
    onSuccess: successHandler(response),
    onFailure: failureHandlder(response),
    invocationContext: invocation-context
};
```
{: code}
Il significato di ogni proprietà è il seguente:

| Proprietà | Descrizione |
|----------|-------------|
| `onSuccess` | Facoltativa. La funzione da richiamare al completamento con esito positivo della chiamata asincrona. La sintassi della funzione `onSuccess` è: `success-handler-function(response)` dove `response` è un oggetto che contiene almeno la seguente proprietà: {::nomarkdown}<ul><li><b>invocationContext</b> - L'oggetto <code>invocationContext</code> che è stato originariamente passato al {{ site.data.keys.mf_server }} nell'oggetto <code>options</code> o <code>undefined</code> se non è stato passato alcun oggetto <code>invocationContext</code>.</li><li><b>status</b> - Lo stato della risposta HTTP</li></ul>{:/} **Nota:** per i metodi per i quali l'oggetto `response` contiene proprietà aggiuntive, queste proprietà vengono dettagliate come parte della descrizione del metodo specifico. |
| `onFailure` | Facoltativa. La funzione da richiamare quando la chiamata asincrona non riesce. Tali errori includono sia errori lato server che errori lato client che si sono verificati durante le chiamate asincrone, come errori di connessione al server o timeout delle chiamate. **Nota:** la funzione non viene chiamata per errori lato client che interrompono l'esecuzione generando un'eccezione. La sintassi della funzione onFailure è: `failure-handler-function(response)` dove `response` è un oggetto che contiene le seguenti proprietà: {::nomarkdown}<ul><li><b>invocationContext</b> - L'oggetto <code>invocationContext</code> che è stato originariamente passato al {{ site.data.keys.mf_server }} nell'oggetto <code>options</code> o <code>undefined</code> se non è stato passato alcun oggetto <code>invocationContext</code>.</li><li><b>errorCode</b> - Una stringa di codice di errore. Tutti i codici di errore che possono essere restituiti sono definiti come costanti nell'oggetto <code>WL.ErrorCode</code> all'interno del file <b>worklight.js</b>.</li><li><b>errorMsg</b> - Un messaggio di errore fornito dal {{ site.data.keys.mf_server }}. Questo messaggio è solo per uso dello sviluppatore e non deve essere mostrato all'utente. Non verrà tradotto nella lingua dell'utente.</li><li><b>status</b> - Lo stato della risposta HTTP</li></li></ul>{:/} **Nota:** per i metodi per i quali l'oggetto `response` contiene proprietà aggiuntive, queste proprietà vengono dettagliate come parte della descrizione del metodo specifico. |
| `invocationContext` | Facoltativa. Un oggetto che viene restituito ai gestori di esito positivo ed esito negativo. L'oggetto `invocationContext` viene utilizzato per preservare il contesto del servizio asincrono chiamante alla restituzione dal servizio. Ad esempio, il metodo `invokeProcedure` potrebbe essere chiamato successivamente, utilizzando lo stesso gestore di esito positivo. Il gestore di esito positivo deve essere in grado di identificare quale chiamata a invokeProcedure verrà gestita. Una soluzione consiste nell'implementare l'oggetto `invocationContext` come un intero e incrementarne il valore di uno per ogni chiamata di `invokeProcedure`. Quando richiama il gestore di esito positivo, il framework {{ site.data.keys.product_adj }} gli passa l'oggetto `invocationContext` dell'oggetto options associato al metodo `invokeProcedure`. Il valore dell'oggetto `invocationContext` può essere utilizzato per identificare la chiamata a `invokeProcedure` a cui sono associati i risultati che vengono gestiti. | 

## Oggetto WL.ClientMessages
{: #the-wlclientmessages-object }
Puoi visualizzare un elenco dei messaggi di sistema memorizzati nell'oggetto `WL.ClientMessages` e abilitare la traduzione di questi messaggi di sistema.

Devi sovrascrivere i messaggi di sistema su un livello JavaScript globale poiché alcune parti del codice vengono eseguite solo dopo l'inizializzazione dell'applicazione.
{: note}

L'oggetto `WL.ClientMessages` è un oggetto che memorizza i messaggi di sistema definiti nel file **worklight/messages/messages.json**. Questo file si trova nella cartella di ambiente dei progetti che hai generato con {{ site.data.keys.product }}. Per abilitare la traduzione di un messaggio di sistema, devi sovrascrivere il valore di questo messaggio nell'oggetto `WL.ClientMessages`, come indicato nel seguente esempio di codice:

```javascript
WL.ClientMessages.invalidUsernamePassword="The custom user name and password are not valid";
```
{: code}


* [Riferimento API JavaScript](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/#javascript-api-reference)
* [Riferimento API Objective-C (per Cordova)](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/#objective-c-api-reference-for-cordova)
{: note}
