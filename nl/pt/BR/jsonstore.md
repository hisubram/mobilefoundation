---

copyright:
  years: 2018, 2019
lastupdated:  "2019-01-04"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:pre: .pre}


# Armazenamento off-line usando JSONStore
{: #overview }
O {{site.data.keyword.mobilefoundation_short}} **JSONStore** é uma API do lado do cliente opcional que fornece um sistema de armazenamento leve e orientado a documentos. O JSONStore ativa o armazenamento persistente de **documentos JSON**. Os documentos em um aplicativo ficam disponíveis no JSONStore mesmo quando o dispositivo que está executando o aplicativo está off-line. Esse armazenamento persistente e sempre disponível pode ser útil para fornecer aos usuários acesso aos documentos quando, por exemplo, não há nenhuma conexão de rede disponível no dispositivo.

Como é familiar aos desenvolvedores, a terminologia do banco de dados relacional é usada nesta documentação às vezes para ajudar a explicar o JSONStore. No entanto, há muitas diferenças entre um banco de dados relacional e um JSONStore. Por exemplo, o esquema estrito que é usado para armazenar dados em bancos de dados relacionais é diferente da abordagem do JSONStore. Com o JSONStore, é possível armazenar qualquer conteúdo JSON e indexar o conteúdo que você precisa procurar.

## Recursos-chave
{: #key-features }
* Indexação de dados para procura eficiente
* Mecanismo para rastrear mudanças somente locais para os dados armazenados
* Suporte para muitos usuários
* A criptografia AES 256 de dados armazenados fornece segurança e confidencialidade. Será possível segmentar a proteção por usuário com proteção de senha, se houver mais de um usuário em um único dispositivo.

Um único armazenamento pode ter muitas coleções e cada coleção pode ter muitos documentos. Também é possível ter um aplicativo MobileFirst que consiste em múltiplos armazenamentos. Para obter informações, veja Suporte a múltiplos usuários do JSONStore.

## Nível de suporte
{: #support-level }
* O JSONStore é suportado em aplicativos do iOS e do Android Nativos (sem suporte para Windows nativo (Universal e UWP)).
* O JSONStore é suportado em aplicativos do Cordova iOS, do Android e do Windows (Universal e UWP).


## Terminologia geral do JSONStore
{: #general-jsonstore-terminology }
### Documento
{: #document }
Um documento é o bloco de construção básica do JSONStore.

Um documento JSONStore é um objeto JSON com um identificador gerado automaticamente (`_id`) e dados JSON. É semelhante a um registro ou uma linha na terminologia do banco de dados. O valor de `_id` é sempre um número inteiro exclusivo dentro de uma coleção específica. Algumas funções como `add`, `replace` e `remove` na classe `JSONStoreInstance` tomam uma Matriz de Documentos ou Objetos. Esses métodos são úteis para executar operações em vários Documentos ou Objetos de uma vez.

**Documento único**  

```javascript
var doc = { _id: 1, json: {name: 'carlos', age: 99} };
```
{: codeblock}

**Matriz de documentos**

```javascript
var docs = [
  { _id: 1, json: {name: 'carlos', age: 99} }, { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### Coleção
{: #collection }
Uma coleção do JSONStore é semelhante a uma tabela, na terminologia de banco de dados.  
O exemplo de código a seguir não é a maneira como os documentos são armazenados em disco, mas é uma boa maneira de visualizar como uma coleção se parece em um alto nível.

```javascript
[
    { _id: 1, json: {name: 'carlos', age: 99} }, { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### Armazenamento
{: #store }
Um armazenamento é o arquivo JSONStore persistente que consiste em uma ou mais coleções.  
Um armazenamento é semelhante a um banco de dados relacional, na terminologia de banco de dados. Um armazenamento também é referido como um JSONStore.

### Campos de procura
{: #search-fields }
Um campo de procura é um par chave-valor.  
Os campos de procura são chaves indexadas para horários de consulta rápida, semelhantes aos campos ou atributos da coluna, na terminologia de banco de dados.

Os campos de procura extras são chave indexadas, mas que não fazem parte dos dados JSON que estão armazenados. Esses campos definem a chave cujos valores (na coleção JSON) são indexados e podem ser usados para procurar mais rapidamente.

Os tipos de dados válidos são: Sequência, Booleano, Número e Número inteiro. Esses tipos são somente sugestões de tipo, não há validação de tipo. Além disso, esses tipos determinam como os campos indexáveis são armazenados. Por exemplo, `{age: 'number'}` indexará 1 como 1.0 e `{age: 'integer'}` indexará 1 como 1.

**Campos de procura e campos de procura extras**

```javascript
var searchField = {name: 'string', age: 'integer'};
var additionalSearchField = {key: 'string'};
```
{: codeblock}

É possível indexar chaves somente dentro de um objeto, não o próprio objeto. As matrizes são manipuladas de uma maneira de passagem, o que significa que não é possível indexar uma matriz ou um índice específico da matriz (arr[n]), mas é possível indexar objetos dentro de uma matriz.

**Indexando valores dentro de uma matriz**

```javascript

var searchFields = {
    'people.name' : 'string', // matches carlos and tim on myObject
    'people.age' : 'integer' // matches 99 and 100 on myObject
};

var myObject = {
    people : [
        {name: 'carlos', age: 99},
        {name: 'tim', age: 100}
    ]
};
```
{: codeblock}

### Consultas
{: #queries }
Consultas são objetos que usam campos de procura ou campos de procura extras para procurar documentos.  
Esses exemplos presumem que o campo de procura de nome é do tipo sequência o campo de procura de idade é do tipo número inteiro.

**Localize documentos com `name` que corresponda a `carlos`**

```javascript
var query1 = {name: 'carlos'};
```
{: codeblock}

**Localize documentos com `name` que corresponda a `carlos` e `age` corresponda a `99`**

```javascript
var query2 = {name: 'carlos', age: 99};
```
{: codeblock}

### Partes de consulta
{: #query-parts }
As partes de consulta são usadas para construir procuras mais avançadas. Algumas operações do JSONStore, como algumas versões de `find` ou `count`, tomam partes de consulta. Tudo dentro de uma parte de consulta é associado por instruções `AND`, enquanto as próprias partes de consulta são associadas por instruções `OR`. Os critérios de procura retornarão uma correspondência somente se tudo dentro de uma parte de consulta for **true**. É possível usar mais de uma parte de consulta para procurar correspondências que satisfaçam uma ou mais das partes de consulta.

As partes de consulta Localizar com operam em campos de procura de nível superior. Por exemplo: `name`, não `name.first`. Use múltiplas coleções quando todos os campos de procura são de nível superior para obter esse comportamento. As operações de partes de consulta que trabalham com campos de procura não de nível superior são: `equal`, `notEqual`, `like`, `notLike`, `rightLike`, `notRightLike`, `leftLike` e `notLeftLike`. O comportamento será indeterminado se você usar campos de procura de nível não superior.

## Tabela de recursos
{: #features-table }
Compare os recursos do JSONStore com os recursos de outras tecnologias e formatos de armazenamento de dados.

JSONStore é uma API JavaScript para armazenar dados dentro de aplicativos Cordova que usam o plug-in MobileFirst, uma API Objective-C para aplicativos iOS nativos e uma API Java para aplicativos Android nativos. Para referência, aqui está uma comparação de diferentes tecnologias de armazenamento JavaScript para ver como o JSONStore se compara a elas.

JSONStore é semelhante a tecnologias, como LocalStorage, DB Indexado, API de Armazenamento Cordova e API de Arquivo Cordova. A tabela mostra como alguns recursos que são fornecidos pelo JSONStore são comparados com outras tecnologias. O recurso JSONStore está disponível somente em dispositivos iOS e Android e simuladores.

| Recurso                                            | JSONStore      | LocalStorage | IndexedDB | API de armazenamento Cordova | API de arquivo Cordova |
|----------------------------------------------------|----------------|--------------|-----------|---------------------|------------------|
| Suporte ao Android (Cordova e aplicativos nativos)|	     ✔ 	      |      ✔	    |     ✔	     |        ✔	           |         ✔	      |
| Suporte ao iOS (Cordova e aplicativos nativos)	     |	     ✔ 	      |      ✔	    |     ✔	     |        ✔	           |         ✔	      |
| Windows 8.1 Universal e Windows 10 UWP (Aplicativos Cordova)          |	     ✔ 	      |      ✔	    |     ✔	     |        -	           |         ✔	      |
| Criptografia de dados	                                 |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| Armazenamento máximo	                                 |Espaço disponível |    ~5 MB     |   ~5 MB 	 | Espaço disponível	   | Espaço disponível  |
| Armazenamento confiável (veja a nota)	                     |	     ✔ 	      |      -	    |     -	     |        ✔	           |         ✔	      |
| Manter controle de mudanças locais	                     |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| Suporte de multiusuário                                 |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| Indexação	                                         |	     ✔ 	      |      -	    |     ✔	     |        ✔	           |         -	      |
| Tipo de armazenamento	                                 | Documentos JSON | Pares chave-valor | Documentos JSON | Relacional (SQL) | Sequências     |

Armazenamento confiável significa que seus dados não são excluídos, a menos que ocorra um dos eventos a seguir:
* O aplicativo é removido do dispositivo.
* Um dos métodos que remove dados é chamado.
{: note}

## Suporte a múltiplos usuários
{: #multiple-user-support }
Com o JSONStore, é possível criar muitos armazenamentos que consistem em diferentes coleções em um único aplicativo MobileFirst.

A API init (JavaScript) ou open (iOS Nativo e Android Nativo) pode tomar um objeto de opções com um nome de usuário. Os diferentes armazenamentos são arquivos separados no sistema de arquivos. O nome do usuário é usado como o nome do arquivo do armazenamento. Esses armazenamentos separados podem ser criptografados com senhas diferentes por motivos de segurança e privacidade. Chamar a API closeAll remove o acesso a todas as coleções. Também é possível mudar a senha de um armazenamento criptografado chamando a API changePassword.

Um caso de uso de exemplo seria vários funcionários que compartilham um dispositivo físico (por exemplo, um iPad ou tablet Android) e um aplicativo MobileFirst. O suporte a múltiplos usuários é útil quando os funcionários trabalham em diferentes turnos e manipulam dados privados de diferentes clientes, enquanto eles usam o aplicativo MobileFirst.

## Segurança
{: #security }
É possível proteger todas as coleções de um armazenamento, criptografando-as.

Para criptografar todas as coleções em um armazenamento, passe uma senha para a API `init` (JavaScript) ou `open` (iOS Nativo e Android Nativo). Se nenhuma senha for passada, nenhum dos documentos nas coleções de armazenamento será criptografado.

Alguns artefatos de segurança (por exemplo, salt) são armazenados no keychain (iOS), em preferências compartilhadas (Android) e no bloqueador de credenciais (Windows Universal 8.1 e Windows 10 UWP). O armazenamento é criptografado com uma chave Padrão de Criptografia Avançado (AES) de 256 bits. Todas as chaves são potencializadas com a Password-Based Key Derivation Function 2 (PBKDF2). É possível escolher criptografar coletas de dados para um aplicativo, mas não é possível alternar entre formatos criptografados e de texto sem formatação ou combinar formatos em um armazenamento.

A chave que protege os dados no armazenamento é baseada na senha de usuário fornecida. A chave não expira, mas é possível mudá-la ao chamar a API changePassword.

O data protection key (DPK) é a chave usada para decriptografar os conteúdos do armazenamento. O DPK será mantido no keychain do iOS mesmo se o aplicativo for desinstalado. Para remover a chave no keychain e todo o resto que o JSONStore coloca no aplicativo, use a API de destruição. Esse processo não é aplicável ao Android porque o DPK criptografado é armazenado em preferências compartilhadas e apagado quando o aplicativo é desinstalado.

Na primeira vez que o JSONStore abre uma coleção com uma senha, o que significa que o desenvolvedor deseja criptografar dados dentro do armazenamento, o JSONStore precisa de um token aleatório. Esse token aleatório pode ser obtido do cliente ou do servidor.

Quando a chave localKeyGen está presente na implementação de JavaScript da API JSONStore e ela tem um valor true, um token criptograficamente seguro é gerado localmente. Caso contrário, o token é gerado entrando em contato com o servidor, o que requer conectividade com o MobileFirst Server. Esse token será necessário somente na primeira vez em que o armazenamento for aberto com uma senha. As implementações nativas (Objective-C e Java) geram um token criptograficamente seguro localmente, por padrão, ou é possível passar um pela opção secureRandom.

A troca é entre os seguintes:
* abrir um armazenamento off-line e confiar no cliente para gerar esse token aleatório (menos seguro) ou 
* abrir o armazenamento com acesso ao MobileFirst Server (requer conectividade) e confiar no servidor (mais seguro)

### Utilitários de segurança
{: #security-utilities }
A API do lado do cliente do MobileFirst fornece alguns utilitários de segurança para ajudar a proteger os dados de seu usuário. Recursos como o JSONStore são ótimos se você deseja proteger objetos JSON. No entanto, não é sugerido armazenar blobs binários em uma coleção JSONStore.

Em vez disso, armazene dados binários no sistema de arquivos e armazene os caminhos de arquivo e outros metadados dentro de uma coleção JSONStore. Se desejar proteger arquivos como imagens, será possível codificá-los como sequências base64, criptografá-los e gravar a saída no disco. 

Para decriptografar os dados, é possível consultar os metadados em uma coleção do JSONStore, ler os dados criptografados no disco e decriptografá-los usando os metadados que foram armazenados. 

Esses metadados podem incluir a chave, o salt, o Vetor de inicialização (IV), o tipo de arquivo, o caminho para o arquivo e outros.

Saiba mais sobre [Utilitários de segurança do JSONStore](security_utilities.html#security_utilities).
{: tip}

### Criptografia do Windows 8.1 Universal e do Windows 10 UWP
{: #windows-81-universal-and-windows-10-uwp-encryption }
É possível proteger todas as coleções de um armazenamento, criptografando-as.

O JSONStore usa o [SQLCipher ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://sqlcipher.net/){: new_window} como sua tecnologia de banco de dados subjacente. O SQLCipher é uma construção de SQLite que é produzida pelo Zetetic, o LLC inclui uma camada de criptografia no banco de dados.

O JSONStore usa o SQLCipher em todas as plataformas. No Android e no iOS, uma versão de software livre grátis do SQLCipher está disponível, conhecida como Community Edition, que é incorporada nas versões do JSONStore incluído no Mobile Foundation. As versões do Windows de SQLCipher estão disponíveis somente sob uma licença comercial e não podem ser redistribuídas diretamente pelo Mobile Foundation.

Em vez disso, o JSONStore para Windows 8 Universal inclui o SQLite como o banco de dados subjacente. Para criptografar dados para qualquer uma dessas plataformas, é necessário adquirir sua própria versão de SQLCipher e trocar a versão de SQLite que está incluída no Mobile Foundation.

Se você não precisar de criptografia, o JSONStore será totalmente funcional (menos criptografia) usando a versão SQLite no Mobile Foundation.

#### Substituindo o SQLite por SQLCipher para Windows Universal e Windows UWP
{: #replacing-sqlite-with-sqlcipher-for-windows-universal-and-windows-uwp }
1. Execute a extensão SQLCipher for Windows Runtime 8.1/10 que vem com o SQLCipher for Windows Runtime Commercial Edition.
2. Depois que a extensão concluir a instalação, localize a versão SQLCipher do arquivo **sqlite3.dll** que acabou de ser criado. Há um para x86, um para x64 e outro para ARM.

   ```bash
   C:\Program Files (x86)\Microsoft SDKs\Windows\v8.1\ExtensionSDKs\SQLCipher.WinRT81\3.0.1\Redist\Retail\<platform>
   ```
   {: codeblock}

3. Copie e substitua esse arquivo para seu aplicativo MobileFirst.

   ```bash
   <Worklight project name>\apps\<application name>\windows8\native\buildtarget\<platform>
   ```
   {: codeblock}

## Desempenho
{: #performance }
A seguir estão os fatores que podem afetar o desempenho do JSONStore.

### Rede
{: #network }
* Verifique a conectividade de rede antes de executar operações, como o envio de todos os documentos modificados e não salvos para um adaptador.
* A quantia de dados que é enviada por meio da rede para um cliente afeta intensamente o desempenho. Envie somente os dados que são requeridos pelo aplicativo, em vez de copiar tudo dentro do banco de dados de back-end.
* Se você estiver usando um adaptador, considere configurar a sinalização compressResponse como true. Dessa forma, as respostas são compactadas, que geralmente usam menos largura da banda e têm um tempo de transferência mais rápido do que sem compactação.

### Memória
{: #memory }
* Quando você usa a API JavaScript, os documentos do JSONStore são serializados e desserializados como Sequências entre a Camada nativa (Objective-C, Java ou C#) e a Camada do JavaScript. Uma maneira de minimizar possíveis problemas de memória é usando limite e deslocamento ao usar a API de localização. Dessa forma, você limita a quantia de memória que é alocada para os resultados e pode implementar coisas como paginação (mostrar número X de resultados por página).
* Em vez de usar nomes longos de chaves que são eventualmente serializados e desserializados como Sequências, considere mapear esses nomes longos de chaves para menores (por exemplo: `myVeryVeryVerLongKeyName` para `k` ou `key`). Idealmente, você os mapeia para nomes curtos de chaves quando os envia do adaptador para o cliente e os mapeia para os nomes longos de chaves originais quando envia dados de volta para o back-end.
* Considere dividir os dados dentro de um armazenamento em várias coleções. Tenha documentos pequenos sobre várias coleções em vez de documentos monolíticos em uma única coleção. Essa consideração depende de quão relacionados estão os dados e os casos de uso para esses dados.
* Quando você usa a API de inclusão com uma matriz de objetos, é possível se deparar com problemas de memória. Para minimizar esse problema, chame esses métodos com poucos objetos JSON por vez.
* JavaScript e Java™ têm coletores de lixo, enquanto Objective-C tem Contagem Automática de Referência. Permita que funcione, mas não dependa inteiramente disso. Tente anular referências que não são mais usadas e use as ferramentas de criação de perfil para verificar se o uso de memória está caindo quando isso é esperado.

### CPU
{: #cpu }
* A quantia de campos de procura e campos de procura extras que são usados afeta o desempenho quando você chama o método de inclusão, que executa a indexação. Indexe somente os valores que são usados em consultas para o método de localização.
* Por padrão, o JSONStore rastreia mudanças locais em seus documentos. Esse comportamento pode ser desativado, permitindo, portanto, economizar alguns ciclos, configurando a sinalização `markDirty` como **false** quando você usa as APIs de inclusão, remoção e substituição.
* A ativação de segurança inclui alguma sobrecarga para as APIs `init` ou `open` e outras operações que trabalham com documentos dentro da coleção. Considere se a segurança é realmente necessária. Por exemplo, a API de abertura é muito mais lenta com criptografia porque ela deve gerar as chaves de criptografia que são usadas para criptografia e decriptografia.
* As APIs `replace` e `remove` dependem do tamanho da coleção, pois elas devem passar por toda a coleção para substituir ou remover todas as ocorrências. Como deve passar por todos os registros, isso deve decriptografar cada um deles, o que torna muito mais lento quando a criptografia é usada. Essa ocorrência de desempenho é mais perceptível em coleções grandes.
* A API `count` é relativamente cara. No entanto, é possível manter uma variável que mantém a contagem para essa coleção. Atualize-a toda vez que você armazenar ou remover coisas da coleção.
* As APIs `find` (`find`, `findAll` e `findById`) são afetadas pela criptografia, uma vez que devem decriptografar cada documento para ver se ele é uma correspondência ou não. Para a consulta localizar por, se um limite for passado, ela será potencialmente mais rápida, pois parará quando atingir o limite de resultados. O JSONStore não precisa decriptografar o restante dos documentos para descobrir se algum outro resultado da procura permanece.

## Simultaneidade
{: #concurrency }
### JavaScript
{: #javascript }
A maioria das operações que podem ser executadas em uma coleção, como incluir e localizar, são assíncronas. Essas operações retornam uma promessa de jQuery que é resolvida quando a operação é concluída com êxito e rejeitada quando ocorre uma falha. Essas promessas são semelhantes a retornos de chamada de sucesso e de falha.

Um jQuery Deferred é uma promessa que pode ser resolvida ou rejeitada. Os exemplos a seguir não são específicos do JSONStore, mas são destinados a ajudá-lo a entender seu uso em geral.

Em vez de promessas e retornos de chamada, também é possível receber eventos do JSONStore `success` e `failure`. Execute ações que são baseadas nos argumentos que são passados para o listener de eventos.

**Exemplo de definição de promessa**

```javascript
var asyncOperation = function () {
  // Assumes that you have jQuery defined via $ in the environment
  var deferred = $.Deferred();

  setTimeout(function() {
    deferred.resolve('Hello');
  }, 1000);

  return deferred.promise();
};
```

**Exemplo de uso de promessa**

```javascript
// The function that is passed to .then is executed after 1000 ms.
asyncOperation.then(function (response) {
  // response = 'Hello'
});
```

**Exemplo de definição de retorno de chamada**

```javascript
var asyncOperation = function (callback) {
  setTimeout(function() {
    callback('Hello');
  }, 1000);
};
```

**Exemplo de uso de retorno de chamada **

```javascript
// The function that is passed to asyncOperation is executed after 1000 ms.
asyncOperation(function (response) {
  // response = 'Hello'
});
```

**Exemplo de eventos**

```javascript
$(document.body).on('WL/JSONSTORE/SUCCESS', function (evt, data, src, collectionName) {

  // evt - Contains information about the event
  // data - Data that is sent ater the operation (add, find, etc.) finished
  // src - Name of the operation (add, find, push, etc.)
  // collectionName - Name of the collection
});
```

### Objective-C
{: #objective-c }
Quando você usa a API do iOS Nativo para JSONStore, todas as operações são incluídas em uma fila de envio síncrono. Esse comportamento assegura que as operações que tocam o armazenamento sejam executadas em ordem em um encadeamento que não seja o encadeamento principal. Para obter mais informações, veja a documentação da Apple em [Grand Central Dispatch (GCD) ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.apple.com/library/ios/documentation/Performance/Reference/GCD_libdispatch_Ref/Reference/reference.html#//apple_ref/c/func/dispatch_sync){: new_window}.

### Java™
{: #java }
Quando você usa a API do Android Nativo para JSONStore, todas as operações são executadas no encadeamento principal. Deve-se criar encadeamentos ou usar conjuntos de encadeamentos para ter um comportamento assíncrono. Todas as operações de armazenamento são thread-safe.

## Analítico
{: #analytics }
É possível coletar partes da chave de informações de analítica que estão relacionadas ao JSONStore

### Informações do arquivo
{: #file-information }
Se a API JSONStore for chamada com a sinalização de analítica configurada como **true**, as informações do arquivo serão coletadas uma vez por sessão de aplicativo. Uma sessão de aplicativo é definida como carregando o aplicativo na memória e removendo-o da memória. É possível usar essas informações para determinar quanto espaço está sendo usado pelo conteúdo do JSONStore no aplicativo.

### Métricas de desempenho
{: #performance-metrics }
As métricas de desempenho são coletadas toda vez que uma API JSONStore é chamada com informações sobre os horários de início e de encerramento de uma operação. É possível usar essas informações para determinar quanto tempo várias operações levam em milissegundos.

### Exemplos
{: #examples }
#### iOS
{: #ios-example}
```objc
JSONStoreOpenOptions* options = [JSONStoreOpenOptions new];
[options setAnalytics:YES];

[[JSONStore sharedInstance] openCollections:@[...] withOptions:options error:nil];
```

#### Android
{: #android-example }
```java
JSONStoreInitOptions initOptions = new JSONStoreInitOptions();
initOptions.setAnalytics(true);

WLJSONStore.getInstance(...).openCollections(..., initOptions);
```

#### JavaScript
{: #java-script-example }
```javascript
var options = {
  analytics : true
};

WL.JSONStore.init(..., options);
```

## Trabalhando com dados externos
{: #working-with-external-data }
É possível trabalhar com dados externos em vários conceitos diferentes: **Pull** e **Push**.

### Pull
{: #pull }
Muitos sistemas usam o termo pull para se referir à obtenção de dados de uma origem externa.  
Há três partes importantes:

#### Origem de dados externa
{: #external-data-source }
Essa origem pode ser um banco de dados, uma API de REST ou SOAP ou muitas outras. O único requisito é que ela deve ser acessível por meio do MobileFirst Server ou diretamente do aplicativo cliente. Idealmente, você deseja que essa origem retorne dados no formato JSON.

#### Camada de transporte
{: #transport-layer }
Essa origem é como você obtém dados da origem externa para sua origem interna, uma coleção do JSONStore dentro do armazenamento. Uma alternativa é um adaptador.

#### API de origem de dados interna
{: #internal-data-source-api }
Essa origem são as APIs JSONStore que podem ser usadas para incluir dados JSON em uma coleção.

É possível preencher o armazenamento interno com dados lidos por meio de um arquivo, um campo de entrada ou dados codificados permanentemente em uma variável. Isso não precisa vir exclusivamente de uma origem externa que requer comunicação de rede.
{: note}

Todos os exemplos de código a seguir são escritos em pseudocódigo semelhante a JavaScript.

Use adaptadores para a Camada de transporte. Algumas das vantagens de usar adaptadores são XML para JSON, segurança, filtragem e desacoplamento do código do lado do servidor e do código do lado do cliente.
{: note}
**Origem de dados externa: terminal REST de back-end**  
Imagine que você tenha um terminal REST que leia dados de um banco de dados e os retorne como uma matriz de objetos JSON.

```javascript
app.get('/people', function (req, res) {

  var people = database.getAll('people');

  res.json(people);
});
```
{: codeblock}

Os dados que são retornados podem ser semelhantes ao exemplo a seguir:

```xml
[{id: 0, name: 'carlos', ssn: '111-22-3333'},
 {id: 1, name: 'mike', ssn: '111-44-3333'},
 {id: 2, name: 'dgonz' ssn: '111-55-3333')]
```
{: codeblock}

**Camada de transporte: adaptador**  
Imagine que você criou um adaptador chamado people e definiu um procedimento chamado getPeople. O procedimento chama o terminal REST e retorna a matriz de objetos JSON para o cliente. Você talvez deseje executar mais trabalho aqui, por exemplo, retornar somente um subconjunto dos dados para o cliente.

```javascript
function getPeople() {

  var input = {
    method : 'get',
    path : '/people'
  };

  return MFP.Server.invokeHttp(input); }
```
{: codeblock}

No cliente, é possível usar a API WLResourceRequest para obter os dados. Além disso, você talvez deseje passar alguns parâmetros do cliente para o adaptador. Um exemplo é uma data com o último horário em que o cliente obteve novos dados da origem externa por meio do adaptador.

```javascript
var adapter = 'people';
var procedure = 'getPeople';

var resource = new WLResourceRequest('/adapters' + '/' + adapter + '/' + procedure, WLResourceRequest.GET);
resource.send()
.then(function (responseFromAdapter) {
  // ...
});
```
{: codeblock}

É possível que queira aproveitar `compressResponse`, `timeout` e outros parâmetros que possam ser transmitidos para a API `WLResourceRequest`.  
{: note}

Opcionalmente, é possível ignorar o adaptador e usar algo como jQuery.ajax para contatar diretamente o terminal REST com os dados que você deseja armazenar.

```javascript
$.ajax({
  type: 'GET',
  url: 'http://example.org/people',
})
.then(function (responseFromEndpoint) {
  // ...
});
```
{: codeblock}

**API de origem de dados interna: JSONStore**
Depois de obter a resposta do back-end, trabalhe com os dados usando o JSONStore.
O JSONStore fornece uma maneira de rastrear as mudanças de local. Ele permite que algumas APIs marquem os documentos modificados e não salvos. A API registra a última operação que foi executada no documento e quando o documento foi marcado como modificado e não salvo. Em seguida, é possível usar essas informações para implementar recursos como sincronização de dados.

A API de mudança toma os dados e algumas opções:

**replaceCriteria**  
Esses campos de procura fazem parte dos dados de entrada. Eles são usados para localizar documentos que já estão dentro de uma coleção. Por exemplo, se você selecionar:

```javascript
['id', 'ssn']
```
{: codeblock}

Como os critérios de substituição, passe a matriz a seguir como os dados de entrada:

```javascript
[{id: 1, ssn: '111-22-3333', name: 'Carlos'}]
```
{: codeblock}

E a coleção `people` já consiste no documento a seguir:

```javascript
{_id: 1,json: {id: 1, ssn: '111-22-3333', name: 'Carlitos'}}
```
{: codeblock}

A operação `change` localiza um documento que corresponde exatamente à consulta a seguir:

```javascript
{id: 1, ssn: '111-22-3333'}
```
{: codeblock}

Em seguida, a operação `change` executa uma substituição com os dados de entrada e a coleção consiste em:

```javascript
{_id: 1, json: {id:1, ssn: '111-22-3333', name: 'Carlos'}}
```
{: codeblock}

O nome foi mudado de `Carlitos` para `Carlos`. Se mais de um documento corresponder aos critérios de substituição, todos os documentos que corresponderem serão substituídos pelos respectivos dados de entrada.

**addNew**  
Quando nenhum documento corresponde aos critérios de substituição, a API de mudança verifica o valor dessa sinalização. Se a sinalização estiver configurada como **true**, a API de mudança criará um novo documento e o incluirá no armazenamento. Caso contrário, nenhuma ação adicional será tomada.

**markDirty**  
Determina se a API de mudança marca documentos que são substituídos ou incluídos como modificados e não salvos.

Uma matriz de dados é retornada do adaptador:

```javascript
.then(function (responseFromAdapter) {

  var accessor = WL.JSONStore.get('people');

  var data = responseFromAdapter.responseJSON;

  var changeOptions = {
    replaceCriteria : ['id', 'ssn'],
    addNew : true,
    markDirty : false
  };

  return accessor.change(data, changeOptions);
})

.then(function() {
  // ...
})
```
{: codeblock}

É possível usar outras APIs para rastrear mudanças nos documentos locais que estão armazenados. Sempre obtenha um acessador para a coleção na qual você executa operações.

```javascript
var accessor = WL.JSONStore.get('people')
```
{: codeblock}

Em seguida, é possível incluir dados (matriz de objetos JSON) e decidir se você deseja que eles sejam marcados, ou não, como modificados e não salvos. Geralmente, você deseja configurar a sinalização markDirty como false ao obter mudanças da origem externa. Configure a sinalização como true ao incluir dados localmente.

```javascript
accessor.add(data, {markDirty: true})
```
{: codeblock}

Também é possível substituir um documento e optar por marcar, ou não, o documento com as substituições como modificado e não salvo.

```javascript
accessor.replace(doc, {markDirty: true})
```
{: codeblock}

Da mesma forma, é possível remover um documento e optar por marcar a remoção como modificada e não salva. Os documentos que são removidos e marcados como modificados e não salvos não são mostrados quando você usa a API de localização. No entanto, eles ainda estão dentro da coleção até que você use a API `markClean`, que remove fisicamente os documentos da coleção. Se o documento não estiver marcado como modificado e não salvo, ele será removido fisicamente da coleção.

```javascript
accessor.remove(doc, {markDirty: true})
```
{: codeblock}

### Push
{: #push }
Muitos sistemas usam o termo push para se referir ao envio de dados para uma origem externa.

Há três partes importantes:

#### API de origem de dados interna
{: #internal-data-source-api-push }
Essa origem é a API JSONStore que retorna documentos com mudanças somente de local (modificados e não salvos).

#### Camada de transporte
{: #transport-layer-push }
Essa origem é como você deseja entrar em contato com a origem de dados externa para enviar as mudanças.

#### Origem de dados externa
{: #external-data-source-push }
Essa origem é geralmente um terminal de banco de dados, REST ou SOAP, entre outros, que recebe as atualizações que o cliente fez para os dados.

Todos os exemplos de código a seguir são escritos em pseudocódigo semelhante a JavaScript.

Use adaptadores para a Camada de transporte. Algumas das vantagens de usar adaptadores são XML para JSON, segurança, filtragem e desacoplamento do código do lado do servidor e do código do lado do cliente.
{: note}

**API de origem de dados interna: JSONStore**  
Depois de obter um acessador para a coleção, chame a API `getAllDirty` para obter todos os documentos que estão marcados como modificados e não salvos. Esses documentos têm mudanças somente de local que você deseja enviar para a origem de dados externa por meio de uma camada de transporte.

```javascript
var accessor = WL.JSONStore.get('people');

accessor.getAllDirty()

.then(function (dirtyDocs) {
  // ...
});
```
{: codeblock}

O argumento `dirtyDocs` é semelhante ao exemplo a seguir:

```javascript
[{_id: 1,
  json: {id: 1, ssn: '111-22-3333', name: 'Carlos'},
  _operation: 'add',
  _dirty: '1395774961,12902'}]
```
{: codeblock}

Os campos são:
* `_id`: campo interno que o JSONStore usa. Cada documento é designado a um exclusivo.
* `json`: os dados que foram armazenados.
* `_operation`: a última operação que foi executada no documento. Os valores possíveis são add, store, replace e remove.
* `_dirty`: um registro de data e hora que é armazenado como um número para significar quando o documento foi marcado como modificado e não salvo.

**Camada de transporte: adaptador MobileFirst**  
É possível escolher enviar documentos modificados e não salvos para um adaptador. Suponha que você tenha um adaptador `people` que esteja definido com um procedimento `updatePeople`.

```javascript
.then(function (dirtyDocs) {
  var adapter = 'people',
  procedure = 'updatePeople';

  var resource = new WLResourceRequest('/adapters/' + adapter + '/' + procedure, WLResourceRequest.GET)
  resource.setQueryParameter('params', [dirtyDocs]);
  return resource.send();
})

.then(function (responseFromAdapter) {
  // ...
})
```
{: codeblock}

É possível que queira aproveitar `compressResponse`, `timeout` e outros parâmetros que possam ser transmitidos para a API `WLResourceRequest`.
{: note}

No MobileFirst Server, o adaptador tem o procedimento `updatePeople`, que pode ser semelhante ao exemplo a seguir:

```javascript
function updatePeople (dirtyDocs) {

  var input = {
    method : 'post',
    path : '/people',
    body: {
      contentType : 'application/json',
      content : JSON.stringify(dirtyDocs)
    }
  };

  return MFP.Server.invokeHttp(input); }
```
{: codeblock}

Em vez de retransmitir a saída da API `getAllDirty` no cliente, você pode ter que atualizar a carga útil para corresponder a um formato esperado pelo back-end. Você pode ter que dividir as substituições, remoções e inclusões em chamadas API de back-end separadas.

Opcionalmente, é possível iterar sobre a matriz `dirtyDocs` e verificar o campo `_operation`. Em seguida, envie substituições para um procedimento, remoções para outro procedimento e inclusões para outro procedimento. O exemplo anterior envia todos os documentos modificados e não salvos em massa para o adaptador.

```javascript
var len = dirtyDocs.length;
var arrayOfPromises = [];
var adapter = 'people';
var procedure = 'addPerson';
var resource;

while (len--) {

  var currentDirtyDoc = dirtyDocs[len];

  switch (currentDirtyDoc._operation) {

    case 'add':
    case 'store':

    resource = new WLResourceRequest('/adapters/people/addPerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);

      arrayOfPromises.push(resource.send());

    break;

    case 'replace':
    case 'refresh':

    resource = new WLResourceRequest('/adapters/people/replacePerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);


      arrayOfPromises.push(resource.send());

    break;

    case 'remove':
    case 'erase':

    resource = new WLResourceRequest('/adapters/people/removePerson', WLResourceRequest.GET);
    resource.setQueryParameter('params', [currentDirtyDoc]);

      arrayOfPromises.push(resource.send());
  }
}

$.when.apply(this, arrayOfPromises)
.then(function() {
  var len = arguments.length;

  while (len--) {
    // Look at the responses in arguments[len]
  }
});
```
{: codeblock}

Opcionalmente, é possível ignorar o adaptador e entrar em contato com o terminal REST diretamente.

```javascript
.then(function (dirtyDocs) {

  return $.ajax({
    type: 'POST',
    url: 'http://example.org/updatePeople',
    data: dirtyDocs
  });
})

.then(function (responseFromEndpoint) {
  // ...
});
```
{: codeblock}

**Origem de dados externa: terminal REST de back-end**  
O back-end aceita ou rejeita mudanças e, em seguida, retransmite uma resposta de volta para o cliente. Depois que o cliente verifica a resposta, ele pode passar documentos que foram atualizados para a API markClean.

```javascript
.then(function (responseFromAdapter) {

  if (responseFromAdapter is successful) {
    WL.JSONStore.get('people').markClean(dirtyDocs);
  }
})

.then(function() {
  // ...
})
```
{: codeblock}

Após os documentos serem marcados como limpos, eles não aparecem na saída da API `getAllDirty`.

## Resolução de problemas
{: #troubleshooting }

## Fornecer informações quando você pedir ajuda
{: #provide-information-when-you-ask-for-help }
É melhor fornecer mais informações do que arriscar não fornecer informações suficientes. A lista a seguir é um bom ponto de início para as informações que são necessárias para ajudar com problemas do JSONStore.

* Sistema operacional e versão. Por exemplo, Windows XP SP3 Virtual Machine ou Mac OSX 10.8.3.
* Versão do Eclipse. Por exemplo, Eclipse Indigo 3.7 Java EE.
* Versão do JDK. Por exemplo, Java SE Runtime Environment (construção 1.7).
* Versão do {{ site.data.keys.product }}. Por exemplo, IBM Worklight V5.0.6 Developer Edition.
* Versão do iOS. Por exemplo, iOS Simulator 6.1 ou iPhone 4S iOS 6.0 (descontinuado, veja Recursos descontinuados e elementos de API).
* Versão do Android. Por exemplo, Android Emulator 4.1.1 ou Samsung Galaxy Android 4.0, Nível de API 14.
* Versão do Windows. Por exemplo, Windows 8, Windows 8.1 ou Windows Phone 8.1.
* Versão do adb. Por exemplo, Android Debug Bridge versão 1.0.31.
* Logs, como a saída de Xcode em iOS ou a saída de logcat no Android.

## Tentar isolar o problema
{: #try-to-isolate-the-issue }
Siga estas etapas para isolar o problema para relatar um problema de maneira mais precisa.

1. Reconfigure o emulador (Android) ou o simulador (iOS) e chame a API de destruição para iniciar com um sistema limpo.
2. Assegure-se de que você esteja executando em um ambiente de produção suportado.
    * Emulador ou dispositivo Android >= 2.3 ARM v7/ARM v8/x86
    * Simulador ou dispositivo iOS >= 6.0 (descontinuado)
    * Simulador ou dispositivo Windows 8.1/10 ARM/x86/x64
3. Tente desativar a criptografia, não passando uma senha para as APIs init ou open.
4. Examine o arquivo de banco de dados SQLite que é gerado pelo JSONStore. A criptografia deve estar desativada.

   * Emulador de Android:
   
   ```bash
   $ adb shell
   $ cd /data/data/com.<app-name>/databases/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

   * Simulador do iOS:

   ```bash
   $ cd ~/Library/Application Support/iPhone Simulator/7.1/Applications/<id>/Documents/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```  

   * Simulador do Windows 8.1 Universal / Windows 10 UWP

   ```bash
   $ cd C:\Users\<username>\AppData\Local\Packages\<id>\LocalState\wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

   * **Nota:** a implementação somente de JavaScript que é executada em um navegador da web (Firefox, Chrome, Safari, Internet Explorer) não usa um banco de dados SQLite. A arquivo é armazenado no LocalStorage do HTML5.
   * Verifique os `searchFields` com `.schema` e selecione os dados com `SELECT * FROM <collection-name>;`. Para sair do sqlite3, digite `.exit`. Se você passar um nome de usuário para o método de inicialização, o arquivo será chamado **o-username.sqlite**. Se você não passar um nome de usuário, o arquivo será chamado **jsonstore.sqlite** por padrão.
5. (Somente Android) Ative o JSONStore detalhado.

   ```bash
   adb shell setprop log.tag.jsonstore-core VERBOSE
   adb shell getprop log.tag.jsonstore-core
   ```

6. Use o depurador.

## Problemas comuns
{: #common-issues }
Entender as características de JSONStore a seguir pode ajudar a resolver alguns dos problemas comuns que você pode encontrar.  

* A única maneira de armazenar dados binários no JSONStore é codificá-los pela primeira vez em base64. Armazene nomes de arquivos ou caminhos em vez dos arquivos reais no JSONStore.
* Acessar dados do JSONStore do código nativo é possível somente no {{ site.data.keys.v62_product_full }} V6.2.0.
* Não há limite de quanto de dados você pode armazenar dentro do JSONStore, além dos limites impostos pelo sistema operacional de dispositivo móvel.
* O JSONStore fornece armazenamento de dados persistentes. Eles não são armazenados somente na memória.
* A API de inicialização falha quando o nome da coleção é iniciado com um dígito ou símbolo. O IBM Worklight V5.0.6.1 e mais recente retorna um erro apropriado: `4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING`
* Há uma diferença entre um número e um número inteiro nos campos de procura. Os valores numéricos como `1` e `2` são armazenados como `1.0` e `2.0` quando o tipo é `number`. Eles são armazenados como `1` e `2` quando o tipo é `integer`.
* Se um aplicativo for forçado a parar ou se travar, ele sempre falhará com o código de erro -1 quando o aplicativo for iniciado novamente e a API `init` ou `open` for chamada. Se esse problema ocorrer, chame a API `closeAll` primeiro.
* A implementação JavaScript do JSONStore espera que o código seja chamado serialmente. Aguarde até que uma operação seja concluída antes de chamar a próxima.
* Transações não são suportadas no Android 2.3.x para aplicativos Cordova.
* Ao usar o JSONStore em um dispositivo de 64 bits, você pode ver o erro a seguir: `java.lang.UnsatisfiedLinkError: dlopen failed: "..." is 32-bit instead of 64-bit`
* Esse erro significa que você tem bibliotecas nativas de 64 bits em seu projeto Android e o JSONStore não funciona atualmente quando essas bibliotecas são usadas. Para confirmar, acesse **src/main/libs** ou **src/main/jniLibs** em seu projeto Android e verifique se você tem as pastas x86_64 ou arm64-v8a. Se você tiver, exclua essas pastas e o JSONStore poderá funcionar novamente.
* Em alguns casos (ou ambientes), o fluxo entra em `wlCommonInit()` antes de o plug-in JSONStore ser inicializado. Isso faz com que as chamadas API relacionadas ao JSONStore falhem. A autoinicialização `cordova-plugin-mfp` chama `WL.Client.init` automaticamente, que aciona a função `wlCommonInit` quando ela é concluída. Esse processo de inicialização é diferente para o plug-in JSONStore. O plug-in JSONStore não tem uma maneira de _parar_ a chamada `WL.Client.init`. Em ambientes diferentes, pode ocorrer que o fluxo entre em `wlCommonInit()` antes de `mfpjsonjslloaded` ser concluído.
Para assegurar a ordenação de eventos `mfpjsonjsloaded` e `mfpjsloaded`, o desenvolvedor tem a opção de chamar `WL.CLient.init`
manualmente. Isso removerá a necessidade de ter um código específico da plataforma.

  Siga as etapas abaixo para configurar a chamada de `WL.CLient.init` manualmente:                             

  1. No `config.xml`, mude a propriedade `clientCustomInit` para **true**.

  + No arquivo `index.js`:                                   
    * inclua a linha a seguir no início do arquivo:                
      ```javascript
      document.addEventListener('mfpjsonjsloaded', initWL, false);
      ```           
    * deixe a chamada `WL.JSONStore.init` em `wlCommonInit()`                    

    * inclua a função a seguir:  
    ```javascript                                         
    function initWL(){                                                     
        var options = typeof wlInitOptions !== 'undefined' ? wlInitOptions
        : {};                                                                
        WL.Client.init(options);                                           
    } 
    ```                                                                     

Isso esperará o evento `mfpjsonjsloaded` (fora de `wlCommonInit`), que assegurará que o script tenha sido carregado e, subsequentemente, chamará `WL.Client.init`, que acionará `wlCommonInit` que, então, chamará `WL.JSONStore.init`.

## Armazenar internados
{: #store-internals }
Veja um exemplo de como os dados JSONStore são armazenados.

Os elementos-chave neste exemplo simplificado:

* `_id` é o identificador exclusivo (por exemplo, AUTO INCREMENT PRIMARY KEY).
* `json` contém uma representação exata do objeto JSON que está armazenado.
* ` name `  e idade são campos de procura.
* ` key `  é um campo de procura extra.

| _id | key | nome | idade | JSON |
|-----|-----|------|-----|------|
| 1   | c   | carlos | 99 | {9}{9} |
| 2   | t   | tim   | 100 | {1}{0}{0} |

Ao procurar usando uma das consultas a seguir ou uma combinação delas: `{_id : 1}, {name: 'carlos'}`, `{age: 99}, {key: 'c'}`, o documento retornado é `{_id: 1, json: {name: 'carlos', age: 99} }`.

Os outros campos JSONStore internos são:

* `_dirty`: determina se o documento foi marcado como modificado e não salvo ou não. Esse campo é útil para rastrear as mudanças nos documentos.
* `_deleted`: marca um documento como excluído ou não. Esse campo é útil para remover objetos da coleta, para usá-los posteriormente para rastrear mudanças com seu back-end e decidir se as remove ou não.
* `_operation`: uma sequência que reflete a última operação a ser executada no documento (por exemplo, substituição).

## Erros de JSONStore
{: #jsonstore-errors }
### JavaScript
{: #javascript }
O JSONStore usa um objeto de erro para retornar mensagens sobre a causa de falhas.

Quando ocorre um erro durante uma operação JSONStore (por exemplo, os métodos `find` e `add` na classe `JSONStoreInstance`), um objeto de erro é retornado. Isso fornece informações sobre a causa da falha.

```javascript
var errorObject = {
  src: 'find', // Operation that failed.
  err: -50, // Error code.
  msg: 'PERSISTENT\_STORE\_FAILURE', // Error message.
  col: 'people', // Collection name.
  usr: 'jsonstore', // User name.
  doc: {_id: 1, {name: 'carlos', age: 99}}, // Document that is related to the failure.
  res: {...} // Response from the server.
}
```
{: codeblock}
Nem todos os pares chave/valor fazem parte de cada objeto de erro. Por exemplo, o valor doc fica disponível somente quando a operação falha por causa de um documento (por exemplo, o método `remove` na classe `JSONStoreInstance`) que falhou ao remover um documento.

### Objective-C
{: #objective-c }
Todas as APIs que podem falhar usam um parâmetro de erro que usa um endereço para um objeto NSError. Se você não desejar ser notificado sobre erros, será possível passar `nil`. Quando uma operação falha, o endereço é preenchido com um NSError, que tem um erro e alguma `userInfo` em potencial. A `userInfo` pode conter detalhes extras (por exemplo, o documento que causou a falha).

```objc
// This NSError points to an error if one occurs.
NSError* error = nil;

// Perform the destroy.
[JSONStore destroyDataAndReturnError:&error];
```

### Java
{: #java }
Todas as chamadas de API Java lançam uma determinada exceção, dependendo do erro que aconteceu. É possível manipular cada exceção separadamente ou capturar `JSONStoreException` como abrangência para todas as exceções JSONStore.

```java
try {
  WL.JSONStore.closeAll (); } catch (JSONStoreException e) { catch (JSONStoreException e) {
  // Handle error condition.
}
```
{: codeblock}
### Lista de códigos de erro
{: #list-of-error-codes }
Lista de códigos de erro comuns e suas descrições:

|Código de erro  | Descrição |
|----------------|-------------|
| -100 UNKNOWN_FAILURE | Unrecognized error. |
| -75 OS\_SECURITY\_FAILURE | This error code is related to the requireOperatingSystemSecurity flag. It can occur if the destroy API fails to remove security metadata that is protected by operating system security (Touch ID with passcode fallback), or the init or open APIs are unable to locate the security metadata. It can also fail if the device does not support operating system security, but operating system security usage was requested. |
| -50 PERSISTENT\_STORE\_NOT\_OPEN | JSONStore is closed. Try calling the open method in the JSONStore class class first to enable access to the store. |
| -48 TRANSACTION\_FAILURE\_DURING\_ROLLBACK | There was a problem with rolling back the transaction. |
| -47 TRANSACTION\\_FAILURE\_DURING\_REMOVE\_COLLECTION |Cannot call removeCollection while a transaction is in progress. |
| -46 TRANSACTION\_FAILURE\_DURING\_DESTROY | Cannot call destroy while there are transactions in progress. |
| -45 TRANSACTION\_FAILURE\_DURING\_CLOSE\_ALL | Cannot call closeAll while there are transactions in place. |
| -44 TRANSACTION\_FAILURE\_DURING\_INIT | Cannot initialize a store while there are transactions in progress. |
| -43 TRANSACTION_FAILURE | There was a problem with transactions. |
| -42 NO\_TRANSACTION\_IN\_PROGRESS | Cannot commit to rolled back a transaction when there is no transaction is progress |
| -41 TRANSACTION\_IN\_POGRESS | Cannot start a new transaction while another transaction is in progress. |
| -40 FIPS\_ENABLEMENT\_FAILURE |Something is wrong with FIPS. |
| -24 JSON\_STORE\_FILE\_INFO\_ERROR | Problem getting the file information from the file system. |
| -23 JSON\_STORE\_REPLACE\_DOCUMENTS\_FAILURE | Problem replacing documents from a collection. |
| -22 JSON\_STORE\_REMOVE\_WITH\_QUERIES\_FAILURE | Problem removing documents from a collection. |
| -21 JSON\_STORE\_STORE\_DATA\_PROTECTION\_KEY\_FAILURE | Problem storing the Data Protection Key (DPK). |
| -20 JSON\_STORE\_INVALID\_JSON\_STRUCTURE | Problem indexing input data. |
| -12 INVALID\_SEARCH\_FIELD\_TYPES | Check that the types that you are passing to the searchFields are stringinteger,number, orboolean. |
| -11 OPERATION\_FAILED\_ON\_SPECIFIC\_DOCUMENT | An operation on an array of documents, for example the replace method can fail while it works with a specific document. The document that failed is returned and the transaction is rolled back. On Android, this error also occurs when trying to use JSONStore on unsupported architectures. |
| -10 ACCEPT\_CONDITION\_FAILED | The accept function that the user provided returned false. |
| -9 OFFSET\_WITHOUT\_LIMIT | To use offset, you must also specify a limit. |
| -8 INVALID\_LIMIT\_OR\_OFFSET | Validation error, must be a positive integer. |
| -7 INVALID_USERNAME | Validation error (Must be [A-Z] or [a-z] or [0-9] only). |
| -6 USERNAME\_MISMATCH\_DETECTED | To log out, a JSONStore user must call the closeAll method first. There can be only one user at a time. |
| -5 DESTROY\_REMOVE\_PERSISTENT\_STORE\_FAILED |A problem with the destroy method while it tried to delete the file that holds the contents of the store. |
| -4 DESTROY\_REMOVE\_KEYS\_FAILED | Problem with the destroy method while it tried to clear the keychain (iOS) or shared user preferences (Android). |
| -3 INVALID\_KEY\_ON\_PROVISION | Passed the wrong password to an encrypted store. |
| -2 PROVISION\_TABLE\_SEARCH\_FIELDS\_MISMATCH | Search fields are not dynamic. It is not possible to change search fields without calling the destroy method or the removeCollection method before you call the init or openmethod with the new search fields. This error can occur if you change the name or type of the search field. For example: {key: 'string'} to {key: 'number'} or {myKey: 'string'} to {theKey: 'string'}. |
| -1 PERSISTENT\_STORE\_FAILURE | Generic Error. A malfunction in native code, most likely calling the init method. |
| 0 SUCCESS | Em alguns casos, o código nativo JSONStore retorna 0 para indicar sucesso. |
| 1 BAD\_PARAMETER\_EXPECTED\_INT | Erro de validação. |
| 2 BAD\_PARAMETER\_EXPECTED\_STRING | Erro de validação. |
| 3 BAD\_PARAMETER\_EXPECTED\_FUNCTION | Erro de validação. |
| 4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING | Erro de validação. |
| 5 BAD\_PARAMETER\_EXPECTED\_OBJECT | Erro de validação. |
| 6 BAD\_PARAMETER\_EXPECTED\_SIMPLE\_OBJECT | Erro de validação. |
| 7 BAD\_PARAMETER\_EXPECTED\_DOCUMENT | Erro de validação. |
| 8 FAILED\_TO\_GET\_UNPUSHED\_DOCUMENTS\_FROM\_DB |A consulta que seleciona todos os documentos marcados como modificados e não salvos falhou. Um exemplo em SQL da consulta seria: SELECT * FROM [collection] WHERE _dirty > 0. |
| 9 NO\_ADAPTER\_LINKED\_TO\_COLLECTION | Para usar funções como os métodos push e de carregamento na classe JSONStoreCollection, um adaptador deve ser passado para o método de inicialização. |
| 10 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ARRAY\_OF\_DOCUMENTS | Erro de validação |
| 11 INVALID\_PASSWORD\_EXPECTED\_ALPHANUMERIC\_STRING\_WITH\_LENGTH\_GREATER\_THAN\_ZERO | Erro de validação |
| 12 ADAPTER_FAILURE | Problema ao chamar WL.Client.invokeProcedure, especificamente um problema na conexão com o adaptador. Esse erro é diferente de uma falha no adaptador que tenta chamar um back-end. |
| 13 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ID | Erro de validação |
| 14 CAN\_NOT\_REPLACE\_DEFAULT\_FUNCTIONS | Chamar o método de aprimoramento na classe JSONStoreCollection para substituir uma função existente (localizar e incluir) não é permitido. |
| 15 COULD\_NOT\_MARK\_DOCUMENT\_PUSHED | Push envia o documento para um adaptador, mas o JSONStore falha ao marcar o documento como não modificado e não salvo. |
| 16 COULD\_NOT\_GET\_SECURE\_KEY | Para iniciar uma coleção com uma senha, deve haver conectividade com o {{ site.data.keys.mf_server }} porque ele retorna um 'token aleatório seguro'. O IBM Worklight V5.0.6 e mais recente permite que os desenvolvedores gerem o token aleatório seguro localmente passando {localKeyGen: true} para o método de inicialização por meio do objeto de opções. |
| 17 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER | Não foi possível carregar dados porque WL.Client.invokeProcedure chamou o retorno de chamada de falha. |
| 18 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER\_INVALID\_LOAD\_OBJ | O objeto de carregamento que foi passado para o método de inicialização não passou na validação. |
| 19 INVALID\_KEY\_IN\_LOAD\_OBJECT | Há um problema com a chave usada no objeto de carregamento ao chamar o método de inclusão. |
| 20 UNDEFINED\_PUSH\_OPERATION | Não há nenhum procedimento definido para enviar documentos modificados e não salvos por push para o servidor. Por exemplo: o método de inicialização (o novo documento está modificado e não salvo, operação = 'add') e o método de push (localiza o novo documento com a operação = 'add') foram chamados, mas nenhuma chave de inclusão com o procedimento de inclusão foi localizada no adaptador que está vinculado à coleção. A vinculação de um adaptador é feita dentro do método de inicialização. |
| 21 INVALID\_ADD\_INDEX\_KEY | Problema com campos de procura extras. |
| 22 INVALID\_SEARCH\_FIELD | Um de seus campos de procura é inválido. Certifique-se de que nenhum dos campos de procura passados seja _id,json,_deleted ou _operation. |
| 23 ERROR\_CLOSING\_ALL | Erro genérico. Ocorreu um erro quando o código nativo chamou o método closeAll. |
| 24 ERROR\_CHANGING\_PASSWORD | Impossível mudar a senha. A senha antiga passada estava errada, por exemplo. |
| 25 ERROR\_DURING\_DESTROY | Erro genérico. Ocorreu um erro quando o código nativo chamou o método de destruição. |
| 26 ERROR\_CLEARING\_COLLECTION | Erro genérico. Ocorreu um erro quando o código nativo chamou o método removeCollection. |
| 27 INVALID\_PARAMETER\_FOR\_FIND\_BY\_ID | Erro de validação. |
| 28 INVALID\_SORT\_OBJECT | A matriz fornecida para classificação é inválida porque um dos objetos JSON é inválido. A sintaxe correta é uma matriz de objetos JSON, em que cada objeto contém somente uma única propriedade. Essa propriedade procura o campo com o qual classificar e se ele é crescente ou decrescente. Por exemplo: {searchField1 : "ASC"}. |
| 29 INVALID\_FILTER\_ARRAY | A matriz fornecida para a filtragem dos resultados é inválida. A sintaxe correta para essa matriz é uma matriz de sequências, na qual cada sequência é um campo de procura ou um campo interno do JSONStore. Para obter mais informações, consulte Armazenar internados. |
| 30 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_OBJECTS | Erro de validação quando a matriz não é uma matriz de apenas objetos JSON. |
| 31 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_CLEAN\_DOCUMENTS | Erro de validação. |
| 32 BAD\_PARAMETER\_WRONG\_SEARCH\_CRITERIA | Erro de validação. |
