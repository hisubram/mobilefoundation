---

copyright:
  years: 2018
lastupdated:  "2018-11-19"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Stockage hors ligne à l'aide de JSONStore
{: #overview }
**JSONStore** {{site.data.keyword.mobilefoundation_short}} est une API facultative côté client qui fournit un système de stockage léger, orienté document. JSONStore active le stockage persistant de **documents JSON**. Les documents dans une application sont disponibles dans JSONStore même si l'appareil qui exécute l'application est déconnecté. Ce stockage permanent et toujours disponible peut être utile pour donner aux utilisateurs un accès aux documents lorsque, par exemple, aucune connexion réseau n'est disponible sur l'appareil. 

Dans la mesure où elle est familière aux développeurs, la terminologie des bases de données relationnelles est parfois utilisée dans cette documentation pour expliquer JSONStore. Cependant, il existe de nombreuses différences entre une base de données relationnelle et JSONStore. Par exemple, le schéma strict qui est utilisé pour
stocker des données dans les bases de données relationnelles est différent de l'approche suivie dans JSONStore. Avec JSONStore, vous pouvez stocker n'importe quel contenu JSON, et indexer le contenu dans lequel vous devez effectuer une recherche.

## Fonctions principales
{: #key-features }
* Indexation des données pour une recherche efficace
* Mécanisme de suivi des modifications locales uniquement apportées aux données stockées
* Prise en charge de nombreux utilisateurs
* Le chiffrement AES 256 des données stockées assure la sécurité
et la confidentialité. Vous pouvez segmenter la protection par utilisateur à l'aide de la protection par mot de passe si plusieurs utilisateurs se trouvent sur un même appareil.

Un magasin unique peut comporter de nombreuses collections, et chaque collection
peut compter de nombreux documents. Il est également possible d'avoir une application MobileFirst composée de plusieurs magasins. Pour plus d'informations, voir la rubrique relative à la prise en charge de plusieurs utilisateurs JSONStore.

## Niveau de prise en charge
{: #support-level }
* JSONStore est pris en charge dans les applications natives iOS et Android (pas de prise en charge de Windows natif (Universal et UWP)).
* JSONStore est pris en charge dans les applications Cordova iOS, Android et Windows (Universal et UWP).


## Terminologie générale JSONStore
{: #general-jsonstore-terminology }
### Document
{: #document }
Un document constitue le bloc de construction de base de JSONStore.

Un document JSONStore est un objet JSON comportant un identifiant généré automatiquement (`_id`) et des données JSON. Dans la terminologie des bases de données, il est similaire à un enregistrement ou une ligne. La valeur `_id` est toujours un entier unique dans une collection spécifique. Certaines fonctions telles que `add`, `replace` et `remove` de la classe `JSONStoreInstance` prennent un tableau de documents ou d'objets. Ces méthodes sont utiles pour effectuer des opérations sur différents documents ou objets à la fois.

**Document unique**  

```javascript
var doc = { _id: 1, json: {name: 'carlos', age: 99} };
```
{: codeblock}

**Tableau de documents**

```javascript
var docs = [
  { _id: 1, json: {name: 'carlos', age: 99} },
  { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### Collection
{: #collection }
Une collection JSONStore est similaire à une table dans la terminologie des bases de données.  
L'exemple de code suivant ne décrit pas la manière dont les documents sont stockés sur le disque, mais il représente un bon moyen d'avoir un aperçu général d'une collection.

```javascript
[
    { _id: 1, json: {name: 'carlos', age: 99} },
    { _id: 2, json: {name: 'tim', age: 100} }
]
```
{: codeblock}

### Magasin
{: #store }
Un magasin est le fichier JSONStore persistant constitué d'une ou de plusieurs collections.  
Un magasin est similaire à une base de données relationnelle dans la terminologie des bases de données. Il est également appelé JSONStore.

### Zones de recherche
{: #search-fields }
Une zone de recherche est une paire clé-valeur.  
Les zones de recherche sont des clés qui
sont indexées pour assurer des temps de recherche rapides, et sont similaires aux champs de colonne ou aux attributs dans la terminologie des bases de
données.

Les zones de recherche supplémentaires sont des clés indexées mais ne faisant pas partie des données JSON stockées. Elles définissent la clé dont les valeurs (dans la collection JSON) sont indexées et peuvent être utilisées pour une recherche plus rapide.

Les types de données valides sont : String (chaîne), Boolean (booléen), Number (nombre) et Integer (entier). Ces types ne sont que des indications de type, il n'y a pas de validation de type. De plus, ces types déterminent le mode de stockage des zones pouvant être indexées. Par exemple, `{age: 'number'}` indexe 1 en tant que 1.0
et `{age: 'integer'}` indexe 1 en tant que 1.

**Zones de recherche et zones de recherche supplémentaires**

```javascript
var searchField = {name: 'string', age: 'integer'};
var additionalSearchField = {key: 'string'};
```
{: codeblock}

Il est uniquement possible d'indexer des clés dans un objet, pas l'objet lui-même. Les tableaux sont gérés de manière transparente, ce qui signifie que vous ne pouvez pas indexer un tableau ni un index spécifique du tableau (arr[n]), mais vous pouvez indexer des objets dans un tableau.

**Indexation des valeurs dans un tableau**

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

### Requêtes
{: #queries }
Les requêtes sont des objets qui utilisent des zones de recherche ou des zones de recherche
supplémentaires pour rechercher des documents.  
Ces exemples supposent que la zone de recherche name est de type chaîne et que la zone de recherche age est de type entier.

**Recherche de documents avec `name` correspondant à `carlos`**

```javascript
var query1 = {name: 'carlos'};
```
{: codeblock}

**Recherche de documents avec `name` correspondant à `carlos` et `age` correspondant à `99`**

```javascript
var query2 = {name: 'carlos', age: 99};
```
{: codeblock}

### Composantes de requête
{: #query-parts }
Les composantes de requête sont utilisées pour générer des recherches plus avancées. Certaines
opérations JSONStore, comme certaines versions de `find` ou `count`, admettent des composantes de requête. Tous les
éléments d'une composante de requête sont joints par des instructions `AND`, alors que les composantes de requête elles-mêmes sont jointes
par des instructions `OR`. Les critères de recherche renvoient une correspondance uniquement si tous les éléments d'une composante de
requête ont pour valeur **true**. Vous pouvez utiliser plusieurs composantes de requête pour rechercher les correspondances satisfaisant une
ou plusieurs des composantes de requête.

La recherche à l'aide de composantes de requête fonctionne uniquement pour les zones de recherche de
niveau supérieur. Exemple : `name` et non `name.first`. Utilisez plusieurs collections où toutes les zones de recherche sont de niveau supérieur pour contourner ce problème. Les opérateurs des composantes de requête qui fonctionnent avec les zones de
recherche de niveau supérieur sont les suivants : `equal`, `notEqual`, `like`, `notLike`, `rightLike`, `notRightLike`, `leftLike` et `notLeftLike`. Le
comportement est indéterminé si vous utilisez des zones de recherche autres que de niveau supérieur.

## Tableau des fonctions
{: #features-table }
Comparez les fonctions JSONStore aux fonctions d'autres technologies et formats de stockage de données.

JSONStore est une API JavaScript permettant de stocker des données dans des applications Cordova utilisant le plug-in MobileFirst, une API Objective-C pour les applications iOS natives et une API Java pour des applications Android natives. A titre de référence, voici une comparaison de différentes technologies de stockage JavaScript pour voir comment JSONStore se compare à celles-ci.

JSONStore est similaire aux technologies telles que LocalStorage, Indexed DB, l'API Cordova Storage et l'API Cordova File. Le tableau compare certaines fonctions fournies par JSONStore à d'autres technologies. La fonction JSONStore n'est disponible que sur les terminaux et simulateurs iOS et Android.

| Fonction                                            | JSONStore      | LocalStorage | IndexedDB | API de stockage Cordova | API de fichier Cordova |
|----------------------------------------------------|----------------|--------------|-----------|---------------------|------------------|
| Prise en charge d'Android (application Cordova et natives)|	     ✔ 	      |      ✔	    |     ✔	     |        ✔	           |         ✔	      |
| Prise en charge d'iOS (applications Cordova et natives)	     |	     ✔ 	      |      ✔	    |     ✔	     |        ✔	           |         ✔	      |
| Windows 8.1 Universal et Windows 10 UWP (applications Cordova)          |	     ✔ 	      |      ✔	    |     ✔	     |        -	           |         ✔	      |
| Chiffrement de données	                                 |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| Stockage maximal	                                 |Espace disponible |    ~5 Mo     |   ~5 Mo 	 | Espace disponible	   | Espace disponible  |
| Stockage fiable (voir note)	                     |	     ✔ 	      |      -	    |     -	     |        ✔	           |         ✔	      |
| Suivi des modifications locales	                     |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| Prise en charge multiutilisateur                                 |	     ✔ 	      |      -	    |     -	     |        -	           |         -	      |
| Indexation	                                         |	     ✔ 	      |      -	    |     ✔	     |        ✔	           |         -	      |
| Type de stockage	                                 | Documents JSON | Paires clé-valeur | Documents JSON | Relationnel (SQL) | Chaînes     |

**Remarque :** Stockage fiable signifie que vos données ne sont supprimées que si l'un des événements suivants se produit :

* L'application est supprimée de l'appareil.
* L'une des méthodes de suppression de données est appelée.

## Prise en charge de plusieurs utilisateurs
{: #multiple-user-support }
Avec JSONStore, vous pouvez créer de nombreux magasins composés de différentes collections dans une seule application MobileFirst.

L'API init (JavaScript) ou open (iOS natif et Android natif) admet un objet options avec un nom d'utilisateur. Des magasins
différents correspondent à des fichiers distincts dans le système de fichiers. Le nom d'utilisateur est utilisé comme nom de fichier du
magasin. Ces
magasins distincts peuvent être chiffrés avec différents mots de passe pour des raisons de sécurité et de confidentialité. L'appel de
l'API closeAll supprime l'accès à toutes les collections. Il est également possible de changer le mot de passe d'un magasin chiffré en appelant l'API changePassword.

Un exemple d'utilisation serait le cas de plusieurs employés partageant un appareil physique (par exemple un iPad ou une tablette Android) et une application MobileFirst. La prise en charge de plusieurs utilisateurs est utile lorsque les employés effectuent différents quarts de travail et gèrent les données privées de différents clients lorsqu'ils utilisent l'application MobileFirst.

## Sécurité
{: #security }
Vous pouvez sécuriser toutes les collections dans un magasin en les chiffrant.

Pour chiffrer toutes les collections dans un magasin, transmettez un mot de passe à l'API `init` (JavaScript) ou `open` (iOS
natif ou Android natif). Si aucun mot de passe n'est transmis, aucun des documents des collections de magasins n'est chiffré.

Certains artefacts de sécurité (par exemple, salt) sont stockés dans la chaîne de certificats (iOS), les préférences partagées (Android) et le casier des données d'identification (Windows Universal 8.1 et Windows 10 UWP). Le magasin est chiffré avec une clé AES (Advanced
Encryption Standard) 256 bits. Toutes les clés sont renforcées avec PBKDF2 (Password-Based Key Derivation Function 2). Vous pouvez choisir de chiffrer les collections de données pour une application, mais vous ne pouvez pas basculer entre les formats chiffré et texte brut, ni mélanger les formats dans un magasin.

La clé qui protège les données dans le magasin repose sur le mot de passe utilisateur que vous indiquez. La clé n'expire pas, mais vous pouvez la changer en appelant l'API changePassword.

La clé de protection des données est la clé qui est utilisée pour déchiffrer le contenu du magasin. Elle est conservée dans la chaîne de certificats
iOS même si l'application est désinstallée. Pour supprimer la clé dans la chaîne de certificats et tous les autres éléments que JSONStore place dans
l'application, utilisez l'API destroy. Ce processus ne s'applique pas à Android car la clé DPK chiffrée est stockée dans des préférences partagées et effacée lors de la désinstallation de l'application.

Lorsque JSONStore ouvre pour la première fois une collection avec un mot de passe (ce qui signifie que le développeur souhaite chiffrer les données
dans
le magasin), il a besoin d'un jeton aléatoire. Ce jeton aléatoire peut être obtenu auprès du client ou du serveur.

Lorsque la clé localKeyGen se trouve dans l'implémentation JavaScript de l'API JSONStore et que sa valeur est true, un jeton sécurisé par chiffrement est généré localement. Sinon, le jeton est généré en contactant le serveur, ce qui nécessite une connectivité à MobileFirst Server. Ce jeton n'est requis que lorsqu'un magasin est ouvert pour la première fois avec un mot de passe. Les implémentations natives (Objective-C et Java) génèrent un jeton sécurisé par chiffrement localement par défaut ou vous pouvez transmettre un jeton via l'option secureRandom.

Les deux solutions suivantes sont possibles :
* ouvrir un magasin hors ligne et faire confiance au client pour générer ce jeton aléatoire (moins sécurisé) ou 
* ouvrir le magasin avec accès à MobileFirst Server (nécessite une connectivité) et faire confiance au serveur (plus sécurisé)

### Utilitaires de sécurité
{: #security-utilities }
L'API côté client de MobileFirst fournit des utilitaires de sécurité permettant de protéger les données de vos utilisateurs. Des fonctions telles que JSONStore sont idéales pour
protéger les objets JSON. Cependant, il est déconseillé de stocker des objets blobs binaires dans une collection JSONStore.

A la place, stockez les données binaires dans le système de fichiers, et stockez les chemins d'accès aux fichiers et d'autres métadonnées dans une
collection JSONStore. Si vous voulez protéger des fichiers tels que des images, vous pouvez les coder sous forme de chaînes base64, les chiffrer et écrire
la sortie sur le disque. 

Pour déchiffrer les données, vous pouvez rechercher les métadonnées dans une collection JSONStore, lire les données chiffrées à partir du disque et les déchiffrer à l'aide des métadonnées stockées. 

Ces métadonnées peuvent inclure la clé, le sel de cryptage, le vecteur d'initialisation, le type de fichier, le chemin d'accès au fichier, etc.

En savoir plus sur les [Utilitaires de sécurité JSONStore](security_utilities.html#security_utilities).
{: tip}

### Chiffrement Windows 8.1 Universal et Windows 10 UWP
{: #windows-81-universal-and-windows-10-uwp-encryption }
Vous pouvez sécuriser toutes les collections dans un magasin en les chiffrant.

JSONStore utilise [SQLCipher ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://sqlcipher.net/){: new_window} comme technologie de base de données sous-jacente. Il s'agit d'une génération de SQLite mise à disposition par Zetetic LLC, qui ajoute une couche de chiffrement à la base de données.

JSONStore utilise SQLCipher sur toutes les plateformes. Sous Android et iOS, une version à code source ouvert de SQLCipher est disponible, appelée Community Edition, intégrée aux versions de JSONStore incluses dans Mobile Foundation. Les versions Windows de SQLCipher ne sont disponibles que sous une licence commerciale et ne peuvent pas être directement redistribuées par Mobile Foundation.

A la place, JSONStore pour Windows 8 Universal inclut SQLite comme base de données sous-jacente. Pour chiffrer les données de l'une ou l'autre de ces plateformes, vous devez acquérir votre propre version de SQLCipher et échanger la version de SQLite incluse dans Mobile Foundation.

Si vous n'avez pas besoin de chiffrement, JSONStore est entièrement fonctionnel (excepté le chiffrement) à l'aide de la version SQLite de Mobile Foundation.

#### Remplacement de SQLite par SQLCipher pour Windows Universal et Windows UWP
{: #replacing-sqlite-with-sqlcipher-for-windows-universal-and-windows-uwp }
1. Exécutez l'extension SQLCipher pour Windows Runtime 8.1/10 fournie avec SQLCipher pour Windows Runtime Commercial Edition.
2. Une fois l'installation de l'extension terminée, localisez la version de SQLCipher
dans le fichier **sqlite3.dll** qui vient d'être créé. Il y en existe une pour x86, une pour x64 et une pour ARM.

   ```bash
   C:\Program Files (x86)\Microsoft SDKs\Windows\v8.1\ExtensionSDKs\SQLCipher.WinRT81\3.0.1\Redist\Retail\<platform>
   ```
   {: codeblock}

3. Copiez et remplacez ce fichier dans votre application MobileFirst.

   ```bash
   <Worklight project name>\apps\<application name>\windows8\native\buildtarget\<platform>
   ```
   {: codeblock}

## Performances
{: #performance }
Les facteurs suivants peuvent affecter les performances de JSONStore.

### Réseau
{: #network }
* Vérifiez la connectivité du réseau avant d'effectuer des opérations, telles que l'envoi de tous les documents altérés vers un adaptateur.
* La quantité de données qui est envoyée sur le réseau à un client affecte considérablement les performances. N'envoyez que les données requises par
l'application, au lieu de copier tous les éléments qui se trouvent dans votre base de données de back end.
* Si vous utilisez un adaptateur, envisagez de définir l'indicateur compressResponse sur true. De cette façon, les réponses sont compressées, ce qui
permet généralement d'utiliser moins de bande passante et de bénéficier d'un temps de transfert plus rapide.

### Mémoire
{: #memory }
* Lorsque vous utilisez l'API JavaScript, les documents JSONStore sont
sérialisés et désérialisés en tant que chaînes entre la couche native (Objective-C, Java, ou C#) et la couche JavaScript. L'un des moyens d'atténuer de possibles problèmes de mémoire consiste à définir une limite et
un décalage lorsque vous utilisez l'API find. Ainsi, vous limitez la quantité de mémoire qui est allouée pour les
résultats et pouvez implémenter des options telles que la pagination (afficher X résultats par page).
* Au lieu d'utiliser des noms de clé longs qui sont finalement sérialisés et désérialisés en tant que chaînes, envisagez de mapper ces noms de clé longs sur des noms plus petits (par exemple : `myVeryVeryVerLongKeyName` sur `k` ou `key`). Dans l'idéal, vous les mappez à des noms de clé courts lorsque vous les envoyez depuis l'adaptateur au client, et mappez les noms de clé courts aux noms de clé longs
d'origine lorsque vous renvoyez les données au système de back end.
* Envisagez de répartir les données d'un magasin dans diverses collections, et d'avoir de petits documents dans plusieurs collections au lieu de
documents volumineux dans une seule collection. Cette décision dépend de la mesure dans laquelle ces données sont liées et des cas
d'utilisation de ces
données.
* Lorsque vous utilisez l'API add avec un tableau d'objets, vous risquez de rencontrer des problèmes de mémoire. Pour
atténuer ce problème, appelez ces méthodes avec moins d'objets JSON à la fois.
* JavaScript et Java ™ possèdent des récupérateur de place, tandis qu'Objective-C a un comptage automatique des références. Laissez-le fonctionner, mais ne comptez pas entièrement dessus. Essayez d'annuler les références qui ne sont plus utilisées et d'utiliser des outils de profilage pour vérifier que l'utilisation de la mémoire diminue lorsque vous prévoyez qu'elle va diminuer.

### Unité centrale
{: #cpu }
* Le nombre de zones de recherche et de zones de recherche supplémentaires qui sont utilisées a un impact sur les performances lorsque vous appelez la
méthode add, qui procède à l'indexation. Indexez uniquement les valeurs qui sont utilisées dans des requêtes de la méthode find.
* Par défaut, JSONStore effectue le suivi des modifications locales apportées à ses documents. Ce comportement peut être désactivé, ce qui permet d'économiser quelques cycles en définissant l'indicateur `markDirty` sur **false** lorsque vous utilisez les API add, remove et replace.
* L'activation de la sécurité ajoute une surcharge aux API `init` ou `open` et aux autres opérations qui fonctionnent avec les documents de la collection. Déterminez si la sécurité est vraiment nécessaire. Par exemple, l'API open est beaucoup plus lente si le chiffrement est activé car
elle doit générer les clés de chiffrement qui sont utilisées pour le chiffrement et le déchiffrement.
* Les API `replace` et `remove` dépendent de la taille de la collection car elles
doivent parcourir l'intégralité de la collection pour remplacer ou supprimer toutes les occurrences. Etant donné qu'elle doivent parcourir chaque
enregistrement, si le chiffrement est utilisé, elle doivent déchiffrer chacun d'entre eux, ce qui les ralentit. Cet impact sur les performances se remarque
plus dans les grandes collections.
* L'API `count` est relativement coûteuse. Toutefois, vous pouvez définir une variable qui compte les éléments pour
cette collection. Mettez-la à jour à chaque fois que vous stockez ou supprimez des éléments dans la collection.
* Les API `find` (`find`, `findAll` et `findById`) sont affectées par le chiffrement, car elles doivent déchiffrer chaque document pour déterminer s'il s'agit d'une correspondance ou non. Pour l'API find by query, si une limite est indiquée, elle est potentiellement plus rapide car elle s'arrête dès qu'elle atteint la limite des résultats. JSONStore n'a pas besoin de déchiffrer le reste des documents pour déterminer s'il reste d'autres résultats de recherche.

## Simultanéité
{: #concurrency }
### JavaScript
{: #javascript }
La plupart des opérations pouvant être effectuées sur une collection, telles que l'ajout et la recherche, sont asynchrones. Ces opérations renvoient une promesse jQuery qui est résolue lorsque l'opération se termine avec succès et qui est rejetée en cas d'échec. Ces promesses sont similaires aux rappels de succès et d'échec.

Une promesse jQuery Deferred peut être résolue ou rejetée. Les exemples suivants ne sont pas spécifiques à JSONStore, mais sont destinés à vous aider à comprendre leur utilisation en général.

Au lieu des promesses et des rappels, vous pouvez également écouter les événements JSONStore `success` et `failure`. Effectuez des actions en fonction des arguments transmis à l'écouteur d'événements.

**Exemple de définition de promesse**

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
{: codeblock}

**Exemple d'utilisation de promesse**

```javascript
// The function that is passed to .then is executed after 1000 ms.
asyncOperation.then(function (response) {
  // response = 'Hello'
});
```
{: codeblock}

**Exemple de définition de rappel**

```javascript
var asyncOperation = function (callback) {
  setTimeout(function() {
    callback('Hello');
  }, 1000);
};
```
{: codeblock}

**Exemple d'utilisation de rappel**

```javascript
// The function that is passed to asyncOperation is executed after 1000 ms.
asyncOperation(function (response) {
  // response = 'Hello'
});
```
{: codeblock}

**Exemple d'événements**

```javascript
$(document.body).on('WL/JSONSTORE/SUCCESS', function (evt, data, src, collectionName) {

  // evt - Contains information about the event
  // data - Data that is sent ater the operation (add, find, etc.) finished
  // src - Name of the operation (add, find, push, etc.)
  // collectionName - Name of the collection
});
```
{: codeblock}

### Objective-C
{: #objective-c }
Lorsque vous utilisez l'API iOS natif pour JSONStore, toutes les opérations sont ajoutées à une file d'attente de
répartition synchrone. Ce comportement garantit que les opérations qui touchent le magasin sont exécutées dans l'ordre sur une unité d'exécution qui n'est pas l'unité d'exécution principale. Pour plus d'informations, voir la documentation Apple à l'adresse [Grand Central Dispatch (GCD) ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.apple.com/library/ios/documentation/Performance/Reference/GCD_libdispatch_Ref/Reference/reference.html#//apple_ref/c/func/dispatch_sync){: new_window}.

### Java™
{: #java }
Lorsque vous utilisez l'API Android natif pour JSONStore, toutes les opérations sont exécutées sur l'unité d'exécution principale. Vous devez créer des unités d'exécution ou utiliser des pools d'unités
d'exécution pour obtenir un comportement asynchrone. Toutes les opérations de magasin autorisent les unités d'exécution multiples.

## Analyse
{: #analytics }
Vous pouvez collecter des informations d'analyse clés liées à JSONStore.

### Informations sur les fichiers
{: #file-information }
Si l'API JSONStore est appelée avec l'indicateur d'analyse défini sur **true**, les informations sur le fichier sont collectées une fois par session d'application. Une session d'application englobe le chargement de l'application dans la mémoire et son retrait de la mémoire. Vous pouvez utiliser ces informations pour déterminer la quantité d'espace utilisée par le contenu JSONStore dans
l'application.

### Mesures de performance
{: #performance-metrics }
Les mesures des performances sont collectées à chaque fois qu'une API JSONStore est appelée avec des informations sur les heures de début et de fin
d'une opération. Vous pouvez utiliser ces informations pour déterminer la durée de diverses opérations en millisecondes.

### Exemples
{: #examples }
#### iOS
{: #ios-example}
```objc
JSONStoreOpenOptions* options = [JSONStoreOpenOptions new];
[options setAnalytics:YES];

[[JSONStore sharedInstance] openCollections:@[...] withOptions:options error:nil];
```
{: codeblock}

#### Android
{: #android-example }
```java
JSONStoreInitOptions initOptions = new JSONStoreInitOptions();
initOptions.setAnalytics(true);

WLJSONStore.getInstance(...).openCollections(..., initOptions);
```
{: codeblock}

#### JavaScript
{: #java-script-example }
```javascript
var options = {
  analytics : true
};

WL.JSONStore.init(..., options);
```
{: codeblock}

## Utilisation de données externes
{: #working-with-external-data }
Vous pouvez utiliser des données externes dans plusieurs concepts différents : **Pull** (extraire) et **Push** (envoyer).

### Pull
{: #pull }
De nombreux systèmes utilisent le terme pull pour désigner l'obtention de données depuis une
source externe.  
Trois éléments sont importants :

#### Source de données externe
{: #external-data-source }
Il peut s'agir d'une base de données, d'une API REST ou SOAP, etc. La seule condition requise est qu'elle soit accessible depuis MobileFirst Server ou directement depuis l'application client. Dans
l'idéal, elle doit renvoyer les données au format JSON.

#### Couche transport
{: #transport-layer }
C'est par cette source que vous obtenez des données depuis la source externe dans votre source interne, une collection
JSONStore dans le
magasin. Vous pouvez aussi utiliser un adaptateur.

#### API de source de données interne
{: #internal-data-source-api }
Cette source correspond aux API JSONStore que vous pouvez utiliser pour ajouter des données JSON à une collection.

**Remarque :** Vous pouvez remplir le magasin interne avec les données lues à partir d'un fichier, d'un champ de saisie ou de données codées en dur dans une variable. Elle ne doivent pas nécessairement provenir exclusivement d'une source externe nécessitant une communication réseau.

Tous les exemples de code ci-dessous sont écrits dans un pseudocode
similaire à
JavaScript.

**Remarque :** Utilisez des adaptateurs pour la couche de transport. L'utilisation d'adaptateurs présente certains avantages, notamment le passage de XML à JSON, la sécurité, le filtrage et le découplage du code côté serveur et du code côté client.

**Source de données externe : noeud final REST de back end**  
Supposez que vous disposez d'un noeud final REST qui lit des données depuis une base de données et les renvoie sous forme de tableau d'objets
JSON.

```javascript
app.get('/people', function (req, res) {

  var people = database.getAll('people');

  res.json(people);
});
```
{: codeblock}

Les données qui sont renvoyées peuvent être similaires aux suivantes :

```xml
[{id: 0, name: 'carlos', ssn: '111-22-3333'},
 {id: 1, name: 'mike', ssn: '111-44-3333'},
 {id: 2, name: 'dgonz' ssn: '111-55-3333')]
```
{: codeblock}

**Couche transport : adaptateur**  
Supposez que vous avez créé un adaptateur appelé people et défini une procédure appelée getPeople. La procédure appelle le noeud final REST et renvoie le tableau d'objets JSON au client. Ici, vous pouvez décider de ne renvoyer qu'un sous-ensemble de données au
client.

```javascript
function getPeople () {

  var input = {
    method : 'get',
    path : '/people'
  };

  return MFP.Server.invokeHttp(input);
}
```
{: codeblock}

Sur le client, vous pouvez utiliser l'API WLResourceRequest pour obtenir les données. En outre, vous souhaiterez peut-être transmettre certains paramètres du client à l'adaptateur. Par exemple, la date à laquelle le client a reçu pour la dernière fois de nouvelles données de la source externe via l'adaptateur.

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

**Remarque :** Vous pouvez tirer parti des paramètres `compressResponse`, `timeout`, et d'autres paramètres pouvant être transmis à l'API `WLResourceRequest`.  
Vous pouvez éventuellement ignorer l'adaptateur et utiliser un élément tel que jQuery.ajax pour contacter directement le noeud final REST avec les données que vous souhaitez stocker.

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

**API de source de données interne : JSONStore**
Après avoir reçu la réponse du serveur de back end, utilisez ces données à l'aide de JSONStore.
JSONStore permet d'effectuer le suivi des modifications locales. Il permet à certaines API de signaler des documents comme modifiés. L'API
enregistre la dernière opération qui a été effectuée sur le document, et à quel moment le document a été signalé comme modifié. Ensuite, vous pouvez vous servir de ces informations pour implémenter des fonctions telles que la synchronisation de données.

L'API change
admet les données et quelques options :

**replaceCriteria**  
Ces zones de recherche font partie des données d'entrée. Elles sont utilisées pour localiser des documents qui se trouvent déjà dans une collection. Par
exemple, si vous sélectionnez :

```javascript
['id', 'ssn']
```
{: codeblock}

En tant que critère de remplacement, transmettez le tableau suivant en tant que données d'entrée :

```javascript
[{id: 1, ssn: '111-22-3333', name: 'Carlos'}]
```
{: codeblock}

Et la collection `people` comprend déjà le document suivant :

```javascript
{_id: 1,json: {id: 1, ssn: '111-22-3333', name: 'Carlitos'}}
```
{: codeblock}

L'opération
`change` localise un document qui correspond exactement à la requête suivante :

```javascript
{id: 1, ssn: '111-22-3333'}
```
{: codeblock}

Ensuite, l'opération `change` effectue un remplacement avec les données d'entrée et la collection se présente comme suit :

```javascript
{_id: 1, json: {id:1, ssn: '111-22-3333', name: 'Carlos'}}
```
{: codeblock}

Le nom `Carlitos` a
été remplacé par `Carlos`. Si plusieurs documents correspondent aux critères de remplacement, tous les documents correspondants sont remplacés par les données d'entrée respectives.

**addNew**  
Lorsqu'aucun document ne correspond aux critères de remplacement, l'API change tient compte de la valeur de cet
indicateur. Si l'indicateur a pour valeur **true**, l'API change crée un document et l'ajoute au
magasin. Sinon, aucune autre action n'est effectuée.

**markDirty**  
Détermine si l'API change signale les documents qui sont remplacés ou ajoutés comme modifiés.

Un tableau de données est renvoyé par l'adaptateur :

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

Vous pouvez utiliser d'autres API pour effectuer le suivi des modifications apportées aux documents locaux qui sont stockés. Obtenez toujours un accesseur à la collection dans laquelle vous voulez effectuer des opérations.

```javascript
var accessor = WL.JSONStore.get('people')
```
{: codeblock}

Ensuite,
vous pouvez ajouter des données (tableau d'objets JSON) et décider de les signaler comme modifiées ou non. En général, il est judicieux d'associer
l'indicateur markDirty à la valeur false lorsque vous obtenez des modifications depuis la source externe. Définissez l'indicateur sur true lorsque vous ajoutez des données localement.

```javascript
accessor.add(data, {markDirty: true})
```
{: codeblock}

Vous
pouvez aussi remplacer un document et choisir de signaler le document comportant les remplacements comme modifié ou non.

```javascript
accessor.replace(doc, {markDirty: true})
```
{: codeblock}

De même, vous pouvez supprimer un document et choisir
de marquer la suppression comme une modification ou non. Les documents supprimés et marqués comme modifiés ne s'affichent pas lorsque vous utilisez l'API find. Cependant, ils restent dans la collection jusqu'à ce que vous utilisiez l'API `markClean`, qui supprime physiquement les documents de la collection. Si le document n'est pas marqué comme modifié, il est physiquement supprimé de la collection.

```javascript
accessor.remove(doc, {markDirty: true})
```
{: codeblock}

### Push
{: #push }
De nombreux systèmes utilisent le terme push pour désigner l'envoi de données à une source
externe.

Trois éléments sont importants :

#### API de source de données interne
{: #internal-data-source-api-push }
Cette source est l'API JSONStore qui renvoie les documents comportant des modifications locales seulement (modifiés).

#### Couche transport
{: #transport-layer-push }
Cette source permet de contacter la source de données externe pour envoyer les modifications.

#### Source de données externe
{: #external-data-source-push }
En général, cette source peut être, entre autres, une base de données ou un noeud final REST ou SOAP, qui reçoit les mises à jour que le
client a apportées aux
données.

Tous les exemples de code ci-dessous sont écrits dans un pseudocode
similaire à
JavaScript.

**Remarque :** Utilisez des adaptateurs pour la couche de transport. L'utilisation d'adaptateurs présente certains avantages, notamment le passage de XML à JSON, la sécurité, le filtrage et le découplage du code côté serveur et du code côté client.

**API de source de données interne : JSONStore**  
Une fois que vous avez obtenu un accesseur à la collection, appelez l'API `getAllDirty` pour obtenir tous les documents marqués comme modifiés. Ces documents comportent des modifications locales seulement que vous voulez envoyer à la source de données
externe par le biais d'une couche transport.

```javascript
var accessor = WL.JSONStore.get('people');

accessor.getAllDirty()

.then(function (dirtyDocs) {
  // ...
});
```
{: codeblock}

L'argument `dirtyDocs` est similaire à l'exemple suivant :

```javascript
[{_id: 1,
  json: {id: 1, ssn: '111-22-3333', name: 'Carlos'},
  _operation: 'add',
  _dirty: '1395774961,12902'}]
```
{: codeblock}

Les zones sont les suivantes :
* `_id` : Zone interne utilisée par JSONStore. Chaque document est affecté à une zone unique.
* `json` : Données qui ont été stockées.
* `_operation` : Dernière opération effectuée sur le document. Les valeurs possibles sont add, store, replace et remove.
* `_dirty` : Horodatage stocké au format numérique pour indiquer quand le document a été marqué comme modifié.

**Couche transport : adaptateur MobileFirst**  
Vous pouvez choisir d'envoyer des documents modifiés à un adaptateur. Supposons que vous disposez d'un adaptateur `people` défini avec une procédure`updatePeople`.

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

**Remarque :** Vous pouvez tirer parti des paramètres `compressResponse`, `timeout`, et d'autres paramètres pouvant être transmis à l'API `WLResourceRequest`.

Sur le serveur MobileFirst, l'adaptateur comporte la procédure `updatePeople` qui peut se présenter comme suit :

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

  return MFP.Server.invokeHttp(input);
}
```
{: codeblock}

Au lieu de relayer la sortie de l'API `getAllDirty` sur le client, vous devrez peut-être mettre à jour le contenu pour qu'il corresponde à un format attendu par le serveur de back end. Vous devrez peut-être fractionner les remplacements, les suppressions et les inclusions en différents appels d'API de back end.

Vous pouvez éventuellement effectuer une itération sur le tableau `dirtyDocs` et vérifier la zone `_operation`. Ensuite, envoyez des remplacements à une procédure, des retraits à une autre procédure et des inclusions à une autre procédure. L'exemple précédent envoie tous les documents modifiés en vrac à l'adaptateur.

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
.then(function () {
  var len = arguments.length;

  while (len--) {
    // Look at the responses in arguments[len]
  }
});
```
{: codeblock}

Vous pouvez éventuellement ignorer l'adaptateur et contacter directement le noeud final REST.

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

**Source de données externe : noeud final REST de back end**  
Le système de back end accepte ou rejette les modifications, puis relaie une réponse au client. Une fois que le client a consulté la réponse, il peut transmettre des documents qui ont été mis à jour à l'API markClean.

```javascript
.then(function (responseFromAdapter) {

  if (responseFromAdapter is successful) {
    WL.JSONStore.get('people').markClean(dirtyDocs);
  }
})

.then(function () {
  // ...
})
```
{: codeblock}

Une fois que les documents sont marqués comme étant propres, ils ne s'affichent pas dans la sortie de l'API `getAllDirty`.

## Identification et résolution des problèmes
{: #troubleshooting }

## Fournissez des informations lorsque vous demandez de l'aide
{: #provide-information-when-you-ask-for-help }
Il vaut mieux fournir trop d'informations que risquer de n'en fournir pas assez. La liste ci-après est un bon point de départ pour savoir quelles informations sont requises lorsque vous demandez de l'aide concernant
des problèmes liés à JSONStore.

* Système d'exploitation et version. Par exemple, Windows XP SP3 Virtual Machine ou Mac OSX 10.8.3.
* Version d'Eclipse. Par exemple, Eclipse Indigo 3.7 Java EE.
* Version du kit Java Development Kit. Par exemple, Java SE Runtime Environment (génération 1.7).
* Version d'{{ site.data.keys.product }}. Par exemple, IBM Worklight V5.0.6 Developer Edition.
* Version d'iOS. Par exemple, iOS Simulator 6.1 ou iPhone 4S iOS 6.0 (déprécié, voir Fonctions et éléments d'API dépréciés).
* Version d'Android. Exemple : Android Emulator 4.1.1 ou Samsung Galaxy Android 4.0 API niveau 14.
* Version de Windows. Par exemple, Windows 8, Windows 8.1 ou Windows Phone 8.1.
* Version d'adb. Exemple : Android Debug Bridge version 1.0.31.
* Journaux, tels que la sortie Xcode sous iOS ou la sortie logcat sous Android.

## Essayez d'isoler le problème
{: #try-to-isolate-the-issue }
Procédez comme suit afin d'isoler le problème et de le signaler plus précisément.

1. Réinitialisez l'émulateur (Android) ou le simulateur (iOS) et appelez l'API destroy pour commencer avec un système propre.
2. Vous devez vous trouver dans un environnement de production pris en charge.
    * Android >= émulateur ou appareil 2.3 ARM v7/ARM v8/x86
    * iOS >= simulateur ou appareil 6.0 (déprécié)
    * Windows : simulateur ou appareil 8.1/10 ARM/x86/x64
3. Essayez de désactiver le chiffrement en ne transmettant pas de mot de passe à l'API init ou open.
4. Reportez-vous au fichier de base de données SQLite généré par JSONStore. Le chiffrement doit être désactivé.

   * Emulateur Android :

   ```bash
   $ adb shell
   $ cd /data/data/com.<app-name>/databases/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

   * Simulateur iOS :

   ```bash
   $ cd ~/Library/Application Support/iPhone Simulator/7.1/Applications/<id>/Documents/wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```  

   * Simulateur Windows 8.1 Universal / Windows 10 UWP

   ```bash
   $ cd C:\Users\<username>\AppData\Local\Packages\<id>\LocalState\wljsonstore
   $ sqlite3 jsonstore.sqlite
   ```

   * **Remarque :** L'unique implémentation de JavaScript qui s'exécute sur un navigateur Web (Firefox, Chrome, Safari, Internet Explorer) n'utilise pas de base de données SQLite. Le fichier est stocké dans HTML5 LocalStorage.
   * Consultez `searchFields` avec `.schema` et sélectionnez des données à l'aide de `SELECT * FROM <collection-name>;`. Pour quitter sqlite3, entrez `.exit`. Si vous transmettez un nom d'utilisateur à la méthode init, le fichier est nommé **the-username.sqlite**. Si vous ne transmettez pas de nom d'utilisateur, le fichier est appelé **jsonstore.sqlite** par défaut.
5. (Android seulement) Activez le journal prolixe de JSONStore.

   ```bash
   adb shell setprop log.tag.jsonstore-core VERBOSE
   adb shell getprop log.tag.jsonstore-core
   ```

6. Utilisez le débogueur.

## Problèmes courants
{: #common-issues }
La compréhension des caractéristiques JSONStore suivantes peut vous aider à résoudre certains problèmes courants que vous pouvez rencontrer.  

* Pour stocker des données binaires dans JSONStore, vous devez d'abord les coder en base64. Stockez les chemins d'accès et les noms de fichier au lieu des fichiers réels dans JSONStore.
* L'accès aux données JSONStore depuis le code natif n'est possible que dans {{ site.data.keys.v62_product_full }} version 6.2.0.
* La quantité de données que vous pouvez stocker dans JSONStore n'est pas limitée, en dehors des limites imposées par le système d'exploitation mobile.
* JSONStore fournit un stockage de persistance des données. Les données ne sont pas seulement stockées en mémoire.
* L'API init échoue si le nom de la collection commence par un chiffre ou un symbole. IBM Worklight versions 5.0.6.1 et ultérieures renvoient une erreur appropriée : `4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING`
* Les zones de recherche différencient les nombres et les entiers. Les valeurs numériques telles que `1` et `2` sont
stockées sous la forme `1.0` et `2.0` lorsque le type est `number`. Elles sont stockées sous la forme `1` et `2` lorsque le type est `integer`.
* Si une application est forcée de s'arrêter ou tombe en panne, elle échoue toujours avec le code d'erreur -1 lorsqu'elle est redémarrée et que l'API `init` ou `open` est appelée. Si ce problème survient, appelez d'abord l'API `closeAll`.
* L'implémentation JavaScript de JSONStore s'attend à ce que le code soit appelé en série. Attendez que l'opération en cours se termine avant d'appeler la suivante.
* Les transactions ne sont pas prises en charge dans Android 2.3.x pour les applications Cordova.
* Lorsque vous utilisez JSONStore sur un appareil 64 bits, l'erreur suivante peut s'afficher : `java.lang.UnsatisfiedLinkError: dlopen failed: "..." is 32-bit instead of 64-bit`
* Cette erreur signifie que vous disposez de bibliothèques natives 64 bits dans votre projet Android et que JSONStore ne fonctionne pas lorsque vous utilisez ces bibliothèques. Pour confirmation, accédez au répertoire **src/main/libs** ou **src/main/jniLibs** sous votre projet Android et vérifiez si le dossier x86_64 ou arm64-v8a existe. Si tel est le cas, supprimez-le ; JSONStore fonctionnera à nouveau.
* Dans certains cas (ou environnements), le flux entre `wlCommonInit()` avant l'initialisation du plug-in JSONStore. Cela entraîne l'échec des appels d'API liés à JSONStore. L'amorçage de `cordova-plugin-mfp` appelle automatiquement `WL.Client.init`, qui, une fois terminé, déclenche la fonction `wlCommonInit`. Ce processus d'initialisation est différent pour le plug-in JSONStore. En effet, ce dernier ne dispose d'aucun moyen d'arrêter (_halt_) l'appel de `WL.Client.init`. Dans certains environnements, il peut arriver que le flux indique `wlCommonInit()` avant la fin de `mfpjsonjslloaded`.
Pour garantir l'ordre des événements `mfpjsonjsloaded` et `mfpjsloaded`, le développeur a la possibilité d'appeler manuellement `WL.CLient.init`. Il n'est alors plus nécessaire de disposer d'un code spécifique à la plateforme.

  Pour configurer l'appel de `WL.CLient.init` manuellement, suivez les étapes ci-après :                             

  1. Dans `config.xml`, modifiez la propriété `clientCustomInit` et définissez-la sur **true**.

  + Dans le fichier `index.js` :                                   
    * ajoutez la ligne suivante au début du fichier :                
      ```javascript
      document.addEventListener('mfpjsonjsloaded', initWL, false);
      ```           
    * laissez l'appel de `WL.JSONStore.init` dans `wlCommonInit()`                    

    * ajoutez la fonction suivante :  
    ```javascript                                         
function initWL(){                                                     
        var options = typeof wlInitOptions !== 'undefined' ? wlInitOptions
        : {};                                                                
        WL.Client.init(options);                                           
    }                                                                      
    ```                                                                       

  Cela permet d'attendre l'événement `mfpjsonjsloaded` (en dehors de `wlCommonInit`), cela garantit le chargement du script et appelle ensuite `WL.Client.init` qui déclenchera `wlCommonInit`, lequel appellera ensuite `WL.JSONStore.init`.

## Stockage des éléments internes
{: #store-internals }
Voir un exemple de la manière dont les données JSONStore sont stockées.

Les éléments clés de cet exemple simplifié sont les suivants :

* `_id` est l'identificateur unique (par exemple, AUTO INCREMENT PRIMARY KEY).
* `json` contient une représentation exacte de l'objet JSON stocké.
* `name` et age sont des zones de recherche.
* `key` est une zone de recherche supplémentaire.

| _id | key | name | age | JSON |
|-----|-----|------|-----|------|
| 1   | c   | carlos | 99 | {name: 'carlos', age: 99} |
| 2   | t   | tim   | 100 | {name: 'tim', age: 100} |

Lorsque vous effectuez une recherche à l'aide de l'une ou d'une combinaison des requêtes suivantes : ``{_id : 1}, {name: 'carlos'}`, `{age: 99}, {key: 'c'}`, le document renvoyé est `{_id: 1, json: {name: 'carlos', age: 99} }`.

Les autres zones JSONStore internes sont :

* `_dirty`: détermine si le document a été signalé comme modifié ou non. Cette zone est utile pour assurer le suivi des modifications apportées aux documents.
* `_deleted`: signale un document comme supprimé ou non. Cette zone est utile pour retirer des objets d'une collection afin de les utiliser ultérieurement pour
assurer le suivi des modifications avec votre système de back end et décider de les retirer ou non.
* `_operation`: chaîne qui reflète la dernière opération effectuée sur le document (par exemple, un remplacement).

## Erreurs JSONStore
{: #jsonstore-errors }
### JavaScript
{: #javascript }
JSONStore utilise un objet erreur pour renvoyer des messages sur la cause des échecs.

Lorsqu'une erreur survient au cours d'une opération JSONStore (par exemple, les méthodes `find` et `add` dans la classe `JSONStoreInstance`), un objet erreur est renvoyé. Il fournit des informations sur la cause de l'échec.

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

Les paires clé-valeur n'apparaissent pas toutes dans chaque objet erreur. Par exemple, la valeur doc est disponible uniquement lorsque l'opération échoue après l'échec d'un document (par exemple, la méthode `remove` dans la classe `JSONStoreInstance`) qui n'est pas parvenu à retirer un document.

### Objective-C
{: #objective-c }
Toutes les API pouvant échouer admettent un paramètre d'erreur dont la valeur est une adresse vers un objet NSError. Si vous ne voulez pas être averti des erreurs, vous pouvez transmettre la valeur `nil`. Lorsqu'une opération échoue, l'adresse est remplie avec un objet NSError, qui comporte une erreur et potentiellement un objet `userInfo`. L'objet `userInfo` peut contenir des détails supplémentaires (par exemple, le document à l'origine de l'erreur).

```objc
// This NSError points to an error if one occurs.
NSError* error = nil;

// Perform the destroy.
[JSONStore destroyDataAndReturnError:&error];
```

### Java
{: #java }
Tous les appels d'API Java lancent une certaine exception, selon l'erreur qui s'est produite. Vous pouvez traiter chaque exception séparément, ou intercepter `JSONStoreException` pour toutes les autres exceptions JSONStore.

```java
try {
  WL.JSONStore.closeAll();
}

catch(JSONStoreException e) {
  // Handle error condition.
}
```

### Liste des codes d'erreur
{: #list-of-error-codes }
Liste des codes d'erreur courants et leur description :

|Code d'erreur      | Description |
|----------------|-------------|
| -100 UNKNOWN_FAILURE | Erreur non reconnue. |
| -75 OS\_SECURITY\_FAILURE | Ce code d'erreur est lié à l'indicateur requireOperatingSystemSecurity. Il peut être émis si l'API ne parvient pas à supprimer des métadonnées de sécurité qui sont protégées par la fonction de sécurité du système d'exploitation (Touch ID avec rétromigration vers le code d'accès) ou si l'API init ou open ne parvient pas à localiser les métadonnées de sécurité. Il peut également être émis si l'appareil ne prend pas en charge la fonction de sécurité du système d'exploitation, alors que l'utilisation de cette fonction a été demandée. |
| -50 PERSISTENT\_STORE\_NOT\_OPEN | JSONStore est fermé. Essayez d'appeler d'abord la méthode open de la classe JSONStore pour permettre l'accès au magasin. |
| -48 TRANSACTION\_FAILURE\_DURING\_ROLLBACK | Un problème est survenu lors de l'annulation de la transaction. |
| -47 TRANSACTION\\_FAILURE\_DURING\_REMOVE\_COLLECTION |Impossible d'appeler removeCollection lorsqu'une transaction est en cours. |
| -46 TRANSACTION\_FAILURE\_DURING\_DESTROY | Impossible d'appeler destroy tant que des transactions sont en cours. |
| -45 TRANSACTION\_FAILURE\_DURING\_CLOSE\_ALL | Impossible d'appeler closeAll tant que des transactions sont en place. |
| -44 TRANSACTION\_FAILURE\_DURING\_INIT | Impossible d'initialiser un magasin tant que des transactions sont en cours. |
| -43 TRANSACTION_FAILURE | Un problème est survenu avec des transactions. |
| -42 NO\_TRANSACTION\_IN\_PROGRESS | Impossible de valider l'annulation d'une transaction lorsqu'aucune transaction n'est en cours |
| -41 TRANSACTION\_IN\_POGRESS | Impossible de démarrer une nouvelle transaction lorsqu'une autre transaction est en cours. |
| -40 FIPS\_ENABLEMENT\_FAILURE |Un problème lié à FIPS est survenu. |
| -24 JSON\_STORE\_FILE\_INFO\_ERROR | Problème d'obtention des informations sur le fichier à partir du système de fichiers. |
| -23 JSON\_STORE\_REPLACE\_DOCUMENTS\_FAILURE | Problème de remplacement des documents d'une collection. |
| -22 JSON\_STORE\_REMOVE\_WITH\_QUERIES\_FAILURE | Problème lors de la suppression de documents d'une collection. |
| -21 JSON\_STORE\_STORE\_DATA\_PROTECTION\_KEY\_FAILURE | Problème de stockage de la clé de protection des données (DPK). |
| -20 JSON\_STORE\_INVALID\_JSON\_STRUCTURE | Problème d'indexation des données d'entrée. |
| -12 INVALID\_SEARCH\_FIELD\_TYPES | Vérifiez que les types que vous transmettez à searchFields sont stringinteger ,number ou boolean. |
| -11 OPERATION\_FAILED\_ON\_SPECIFIC\_DOCUMENT | Une opération sur un tableau de documents, par exemple la méthode replace peut échouer si elle fonctionne avec un document spécifique. Le document à l'origine de l'échec est renvoyé et la transaction est annulée. Sur Android, cette erreur se produit également lorsque vous essayez d'utiliser JSONStore sur des architectures non prises en charge. |
| -10 ACCEPT\_CONDITION\_FAILED | La fonction accept fournie par l'utilisateur a renvoyé la valeur false. |
| -9 OFFSET\_WITHOUT\_LIMIT | Pour utiliser le décalage, vous devez également spécifier une limite. |
| -8 INVALID\_LIMIT\_OR\_OFFSET | Erreur de validation, doit être un entier positif. |
| -7 INVALID_USERNAME | Erreur de validation (Doit être [AZ] ou [az] ou [0-9] uniquement). |
| -6 USERNAME\_MISMATCH\_DETECTED | Pour se déconnecter, un utilisateur JSONStore doit d'abord appeler la méthode closeAll. Un seul utilisateur à la fois est autorisé. |
| -5 DESTROY\_REMOVE\_PERSISTENT\_STORE\_FAILED |Problème lié à la méthode destroy alors qu'elle tentait de supprimer le fichier qui contient le contenu du magasin. |
| -4 DESTROY\_REMOVE\_KEYS\_FAILED | Problème lié à la méthode destroy alors qu'elle tentait d'effacer la chaîne de certificats (iOS) ou les préférences utilisateur partagées (Android). |
| -3 INVALID\_KEY\_ON\_PROVISION | Mot de passe incorrect transmis à un magasin chiffré. |
| -2 PROVISION\_TABLE\_SEARCH\_FIELDS\_MISMATCH | Les zones de recherche ne sont pas dynamiques. Vous ne pouvez pas les modifier sans appeler la méthode destroy ou la méthode removeCollection avant d'appeler la méthode init ou openmethod avec les nouvelles zones de recherche. Cette erreur peut survenir si vous changez le nom ou le type de la zone de recherche. Par exemple : {key: 'string'} en {key: 'number'} ou {myKey: 'string'} en {theKey: 'string'}. |
| -1 PERSISTENT\_STORE\_FAILURE | Erreur générique. Dysfonctionnement dans le code natif, probablement lors de l'appel de la méthode init. |
| 0 SUCCESS | Dans certains cas, le code natif JSONStore renvoie 0 pour indiquer le succès. |
| 1 BAD\_PARAMETER\_EXPECTED\_INT | Erreur de validation. |
| 2 BAD\_PARAMETER\_EXPECTED\_STRING | Erreur de validation. |
| 3 BAD\_PARAMETER\_EXPECTED\_FUNCTION | Erreur de validation. |
| 4 BAD\_PARAMETER\_EXPECTED\_ALPHANUMERIC\_STRING | Erreur de validation. |
| 5 BAD\_PARAMETER\_EXPECTED\_OBJECT | Erreur de validation. |
| 6 BAD\_PARAMETER\_EXPECTED\_SIMPLE\_OBJECT | Erreur de validation. |
| 7 BAD\_PARAMETER\_EXPECTED\_DOCUMENT | Erreur de validation. |
| 8 FAILED\_TO\_GET\_UNPUSHED\_DOCUMENTS\_FROM\_DB |La requête qui sélectionne tous les documents marqués comme ayant échoué a échoué. Un exemple de la requête au format SQL : SELECT * FROM [collection] WHERE _dirty > 0. |
| 9 NO\_ADAPTER\_LINKED\_TO\_COLLECTION | Pour utiliser des fonctions telles que les méthodes push et load de la classe JSONStoreCollection, un adaptateur doit être transmis à la méthode init. |
| 10 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ARRAY\_OF\_DOCUMENTS | Erreur de validation |
| 11 INVALID\_PASSWORD\_EXPECTED\_ALPHANUMERIC\_STRING\_WITH\_LENGTH\_GREATER\_THAN\_ZERO | Erreur de validation |
| 12 ADAPTER_FAILURE | Problème lors de l'appel de WL.Client.invokeProcedure, en particulier problème de connexion à l'adaptateur. Cette erreur est différente d'une défaillance de l'adaptateur qui tente d'appeler un serveur. |
| 13 BAD\_PARAMETER\_EXPECTED\_DOCUMENT\_OR\_ID | Erreur de validation |
| 14 CAN\_NOT\_REPLACE\_DEFAULT\_FUNCTIONS | cL'appel de la méthode enhance de la classe JSONStoreCollection pour remplacer une fonction existante (find et add) n'est pas autorisé. |
| 15 COULD\_NOT\_MARK\_DOCUMENT\_PUSHED | Push envoie le document à un adaptateur, mais JSONStore ne parvient pas à marquer le document comme non modifié. |
| 16 COULD\_NOT\_GET\_SECURE\_KEY | Pour initier une collection avec un mot de passe, une connexion au {{ site.data.keys.mf_server }} doit être établie, car celui-ci renvoie un 'jeton aléatoire sécurisé'. IBM Worklight version 5.0.6 et versions ultérieures permet aux développeurs de générer le jeton aléatoire sécurisé en transmettant {localKeyGen: true} à la méthode init via l'objet options. |
| 17 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER | Impossible de charger les données car WL.Client.invokeProcedure a appelé le rappel d'échec. |
| 18 FAILED\_TO\_LOAD\_INITIAL\_DATA\_FROM\_ADAPTER\_INVALID\_LOAD\_OBJ | L'objet de chargement transmis à la méthode init n'a pas réussi la validation. |
| 19 INVALID\_KEY\_IN\_LOAD\_OBJECT | Un problème lié à la clé utilisée dans l'objet load s'est produit lorsque vous avez appelé la méthode add. |
| 20 UNDEFINED\_PUSH\_OPERATION | Aucune procédure n'est définie pour envoyer des documents modifiés au serveur. Par exemple, la méthode init (nouveau document modifié, opération 'add') et la méthode push (qui recherche le nouveau document avec l'opération 'add') ont été appelées, mais aucune clé d'ajout avec la procédure d'ajout n'a été trouvée dans l'adaptateur qui est lié à la collection. La liaison d'un adaptateur est effectuée dans la méthode init. |
| 21 INVALID\_ADD\_INDEX\_KEY | Problème avec des zones de recherche supplémentaires. |
| 22 INVALID\_SEARCH\_FIELD | L'une de vos zones de recherche n'est pas valide. Vérifiez qu'aucune zone de recherche transmise ne correspond à _id,json,_deleted ou _operation. |
| 23 ERROR\_CLOSING\_ALL | Erreur générique. Une erreur s'est produite lorsque le code natif a appelé la méthode closeAll. |
| 24 ERROR\_CHANGING\_PASSWORD | Impossible de changer le mot de passe. L'ancien mot de passe transmis était erroné, par exemple. |
| 25 ERROR\_DURING\_DESTROY | Erreur générique. Une erreur s'est produite lorsque le code natif a appelé la méthode destroy. |
| 26 ERROR\_CLEARING\_COLLECTION | Erreur générique. Une erreur s'est produite lorsque le code natif a appelé la méthode removeCollection. |
| 27 INVALID\_PARAMETER\_FOR\_FIND\_BY\_ID | Erreur de validation. |
| 28 INVALID\_SORT\_OBJECT | Le tableau fourni pour le tri n'est pas valide car l'un des objets JSON n'est pas valide. La syntaxe correcte est un tableau d'objets JSON, où chaque objet contient une seule propriété. Cette propriété recherche la zone en fonction de laquelle procéder au tri, et détermine si le tri est croissant ou décroissant. Par exemple : {searchField1 : "ASC"}. |
| 29 INVALID\_FILTER\_ARRAY | Le tableau fourni pour filtrer les résultats n'est pas valide. La syntaxe correcte pour ce tableau est un tableau de chaînes, dans lequel chaque chaîne est une zone de recherche ou une zone JSONStore interne. Pour plus d'informations, voir Stockage des éléments internes. |
| 30 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_OBJECTS | Erreur de validation lorsque le tableau n'est pas un tableau composé uniquement d'objets JSON. |
| 31 BAD\_PARAMETER\_EXPECTED\_ARRAY\_OF\_CLEAN\_DOCUMENTS | Erreur de validation. |
| 32 BAD\_PARAMETER\_WRONG\_SEARCH\_CRITERIA | Erreur de validation. |
