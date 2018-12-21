---

copyright:
  years: 2018
lastupdated: "2018-11-26"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Configure Offline Storage
{: #configure_offline_storage}

The Mobile Foundation JSONStore is an optional client-side API that provides a lightweight, document-oriented storage system. JSONStore enables persistent storage of JSON documents. Documents in an application are available in JSONStore even when the device that is running the application is offline. This persistent, always-available storage can be useful to give users access to documents when, for example, there's no network connection available in the device. For an overview of JSONStore concepts and terminology, see [here](jsonstore.html).

## Configure offline storage for Cordova or Ionic apps
{: #configure_offline_storage_cordova}

Make sure the Mobile Foundation Cordova SDK was added to the project. 

Follow the [Adding the Mobile Foundation SDK to Cordova applications ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/) tutorial.
{: tip}

### Adding JSONStore to your Cordova project
{: #adding_jsonstore_cordova}

1. Open a command line window and navigate to your Cordova project folder.
2. Run the command: 
   ```bash
   cordova plugin add cordova-plugin-mfp-jsonstore
   ```

### Initialize JSONStore collection
{: #initialize_jsonstore_cordova}   

Use `init` to start one or more JSONStore collections.
Starting or provisioning a collection means creating the persistent storage that contains the collection and documents, if it does not exist. If a password is passed to options, the persistent storage is encrypted with the password.

```javascript
var collections = {
    people : {
        searchFields: {name: 'string', age: 'integer'}
    }
};

WL.JSONStore.init(collections).then(function (collections) {
    // handle success - collection.people (people's collection)
}).fail(function (error) {
    // handle failure
});
```

### Get an accessor to your JSONStore collection
{: #get_jsonstore_cordova} 

Use `get` to create an accessor to the collection. You must call `init` before you call get otherwise the result of `get` will be *undefined*.

```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
The variable *people* can now be used to perform operations on the *people* collection such as `add`, `find`, and `replace`.

### Add documents to a collection
{: #add_jsonstore_cordova} 

Use `add` to store data as documents inside a collection.

```javascript
var collectionName = 'people';
var options = {};
var data = {name: 'yoel', age: 23};

WL.JSONStore.get(collectionName).add(data, options).then(function () {
    // handle success
}).fail(function (error) {
    // handle failure
});
```

### Find documents inside a collection
{: #find_jsonstore_cordova} 

* Use `find` to locate a document inside a collection by using a query.
* Use `findAll` to retrieve all the documents inside a collection.
* Use `findById` to search by the document unique identifier.

The default behavior for find is to do a "fuzzy" search.

```javascript
var query = {name: 'yoel'};
var collectionName = 'people';
var options = {
  exact: false, //default
  limit: 10 // returns a maximum of 10 documents, default: return every document
};

WL.JSONStore.get(collectionName).find(query, options).then(function (results) {
    // handle success - results (array of documents found)
}).fail(function (error) {
    // handle failure
});
```

```javascript
var age = document.getElementById("findByAge").value || '';

if(age == "" || isNaN(age)){
  alert("Please enter a valid age to find");
}
else {
  query = {age: parseInt(age, 10)};
  var options = {
    exact: true,
    limit: 10 //returns a maximum of 10 documents
  };
  WL.JSONStore.get(collectionName).find(query, options).then(function (res) {
    // handle success - results (array of documents found)
  }).fail(function (errorObject) {
    // handle failure
  });
}
```

### Replace documents inside a collection
{: #replace_jsonstore_cordova} 

Use `replace` to modify documents inside a collection. The field that you use to perform the replacement is `_id`, the document unique identifier.

```javascript
var document = {
  _id: 1, json: {name: 'chevy', age: 23}
};
var collectionName = 'people';
var options = {};

WL.JSONStore.get(collectionName).replace(document, options).then(function (numberOfDocsReplaced) {
    // handle success
}).fail(function (error) {
    // handle failure
});
```
This examples assumes that the document `{_id: 1, json: {name: 'yoel', age: 23} }` is in the collection.


### Remove documents from a collection
{: #remove_jsonstore_cordova} 

Use `remove` to delete a document from a collection.
Documents are not erased from the collection until you call push.

```javascript
var query = {_id: 1};
var collectionName = 'people';
var options = {exact: true};
WL.JSONStore.get(collectionName).remove(query, options).then(function (numberOfDocsRemoved) {
    // handle success
}).fail(function (error) {
    // handle failure
});
```

### Remove an entire collection
{: #remove_collection_jsonstore_cordova} 

Use `removeCollection` to delete all the documents that are stored inside a collection. This operation is similar to dropping a table in database terms.

### Destroy JSONStore
{: #destroy_jsonstore_cordova} 

Use `destroy` to remove the following data:
* All documents
* All collections
* All Stores
* All JSONStore metadata and security artifacts

## Configure offline storage for iOS apps
{: #configure_offline_storage_ios}

Make sure the Mobile Foundation Native SDK was added to the Xcode project. 

Follow the [Adding the Mobile Foundation SDK to iOS applications ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/) tutorial.
{: tip}

### Adding JSONStore to your iOS project
{: #adding_jsonstore_ios}

1. Add the following to the existing `podfile`, at the root of the Xcode project.
   ```bash
   pod 'IBMMobileFirstPlatformFoundationJSONStore'
   ```
2. From command line, go to the root of the Xcode project and run the command: 
   ```bash
   pod install
   ``` 
3. Whenever you want to use JSONStore, make sure that you import the JSONStore header:
   **Objective-C**:
   ```objectivec
   #import <IBMMobileFirstPlatformFoundationJSONStore/IBMMobileFirstPlatformFoundationJSONStore.h>
   ``` 
   **Swift:**
   ```swift
   import IBMMobileFirstPlatformFoundationJSONStore
   ```   

### Open JSONStore collection: iOS
{: #open_ios} 

Use `openCollections` to open one or more JSONStore collections.

```swift
let collection:JSONStoreCollection = JSONStoreCollection(name: "people")

collection.setSearchField("name", withType: JSONStore_String)
collection.setSearchField("age", withType: JSONStore_Integer)

do {
  try JSONStore.sharedInstance().openCollections([collection], withOptions: nil)
} catch let error as NSError {
  // handle error
}
```

### Get an accessor to your JSONStore collection
{: #get_jsonstore_ios} 

Use `getCollectionWithName` to create an accessor to the collection. You must call `openCollections` before you call `getCollectionWithName`.

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)
```
The variable collection can now be used to perform operations on the `people` collection such as `add`, `find`, and `replace`.

### Add documents to a collection
{: #add_jsonstore_ios} 

Use `addData` to store data as documents inside a collection.

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

let data = ["name" : "yoel", "age" : 23]

do  {
  try collection.addData([data], andMarkDirty: true, withOptions: nil)
} catch let error as NSError {
  // handle error
}
```

### Find documents inside a collection
{: #find_jsonstore_ios} 

Use `findWithQueryParts` to locate a document inside a collection by using a query. Use `findAllWithOptions` to retrieve all the documents inside a collection. Use `findWithIds` to search by the document unique identifier.

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

let options:JSONStoreQueryOptions = JSONStoreQueryOptions()
// returns a maximum of 10 documents, default: returns every document
options.limit = 10

let query:JSONStoreQueryPart = JSONStoreQueryPart()
query.searchField("name", like: "yoel")

do  {
  let results:NSArray = try collection.findWithQueryParts([query], andOptions: options)
} catch let error as NSError {
  // handle error
}
```

### Replace documents inside a collection
{: #replace_jsonstore_ios} 

Use `replaceDocuments` to modify documents inside a collection. The field that you use to perform the replacement is `_id`, the document unique identifier.

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

var document:Dictionary<String,AnyObject> = Dictionary()
document["name"] = "chevy"
document["age"] = 23

var replacement:Dictionary<String, AnyObject> = Dictionary()
replacement["_id"] = 1
replacement["json"] = document

do {
  try collection.replaceDocuments([replacement], andMarkDirty: true)
} catch let error as NSError {
  // handle error
}
```
This example assumes that the document `{_id: 1, json: {name: 'yoel', age: 23} }` is in the collection.

### Remove documents from a collection
{: #remove_jsonstore_ios} 

Use `removeWithIds` to delete a document from a collection. Documents aren’t erased from the collection until you call `markDocumentClean`. 

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeWithIds([1], andMarkDirty: true)
} catch let error as NSError {
  // handle error
}
```

### Remove an entire collection
{: #remove_collection_jsonstore_ios} 

Use `removeCollection` to delete all the documents that are stored inside a collection. This operation is similar to dropping a table in database terms.

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeCollection()
} catch let error as NSError {
  // handle error
}
```

### Destroy JSONStore
{: #destroy_jsonstore_ios} 

Use `destroyData` to remove the following data:
* All documents
* All collections
* All Stores
* All JSONStore metadata and security artifacts

```swift
do {
  try JSONStore.sharedInstance().destroyData()
} catch let error as NSError {
  // handle error
}
```

## Configure offline storage for Android apps
{: #configure_offline_storage_android}

Make sure the Mobile Foundation Native SDK was added to the Android Studio project. 

Follow the [Adding the Mobile Foundation SDK to Android applications ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/) tutorial.
{: tip}

### Adding JSONStore to your Android project
{: #adding_jsonstore_android}

1. From **Android → Gradle Scripts**, select the `build.gradle (Module: app)` file.
2. Add the following to the existing `dependencies` section: 
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
   ``` 
3. Add the following to the `DefaultConfig` section of your `build.gradle` file:
   ```gradle
   ndk {
     abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
   }
   ``` 
   We add the `abiFilters` to ensure that the apps having JSONStore will run in any of the architectures specified above. This is required as JSONStore is dependent on a third party library which supports only these architectures.
   {: note}


### Open JSONStore collection: Android
{: #open_android} 

Use `openCollections` to open one or more JSONStore collections.

```java
Context context = getContext();
try {
  JSONStoreCollection people = new JSONStoreCollection("people");
  people.setSearchField("name", SearchFieldType.STRING);
  people.setSearchField("age", SearchFieldType.INTEGER);
  List<JSONStoreCollection> collections = new LinkedList<JSONStoreCollection>();
  collections.add(people);
  WLJSONStore.getInstance(context).openCollections(collections);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```

### Get an accessor to your JSONStore collection
{: #get_jsonstore_android} 

Use `getCollectionByName` to create an accessor to the collection. You must call `openCollections` before you call `getCollectionByName`.

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
The variable collection can now be used to perform operations on the `people` collection such as `add`, `find`, and `replace`.

### Add documents to a collection
{: #add_jsonstore_android} 

Use `addData` to store data as documents inside a collection.

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  //Add options.
  JSONStoreAddOptions options = new JSONStoreAddOptions();
  options.setMarkDirty(true);
  JSONObject data = new JSONObject("{age: 23, name: 'yoel'}")
  collection.addData(data, options);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```

### Find documents inside a collection
{: #find_jsonstore_android} 

Use `findDocuments` to locate a document inside a collection by using a query. Use `findAllDocuments` to retrieve all the documents inside a collection. Use `findDocumentById` to search by the document unique identifier..

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreQueryPart queryPart = new JSONStoreQueryPart();
  // fuzzy search LIKE
  queryPart.addLike("name", name);
  JSONStoreQueryParts query = new JSONStoreQueryParts();
  query.addQueryPart(queryPart);
  JSONStoreFindOptions options = new JSONStoreFindOptions();
  // returns a maximum of 10 documents, default: returns every document
  options.setLimit(10);
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  List<JSONObject> results = collection.findDocuments(query, options);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```

### Replace documents inside a collection
{: #replace_jsonstore_android} 

Use `replaceDocuments` to modify documents inside a collection. The field that you use to perform the replacement is `_id`, the document unique identifier.

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  JSONStoreReplaceOptions options = new JSONStoreReplaceOptions();
  // mark data as dirty
  options.setMarkDirty(true);
  JSONStore replacement = new JSONObject("{_id: 1, json: {age: 23, name: 'chevy'}}");
  collection.replaceDocument(replacement, options);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
This example assumes that the document `{_id: 1, json: {name: 'yoel', age: 23} }` is in the collection.

### Remove documents from a collection
{: #remove_jsonstore_android} 

Use `removeDocumentById` to delete a document from a collection. Documents aren’t erased from the collection until you call `markDocumentClean`. 

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  JSONStoreRemoveOptions options = new JSONStoreRemoveOptions();
  // Mark data as dirty
  options.setMarkDirty(true);
  collection.removeDocumentById(1, options);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```

### Remove an entire collection
{: #remove_collection_jsonstore_android} 

Use `removeCollection` to delete all the documents that are stored inside a collection. This operation is similar to dropping a table in database terms.

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  collection.removeCollection();
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```

### Destroy JSONStore
{: #destroy_jsonstore_android} 

Use `destroy` to remove the following data:
* All documents
* All collections
* All Stores
* All JSONStore metadata and security artifacts

```java
Context context = getContext();
try {
  WLJSONStore.getInstance(context).destroy();
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```

## Advanced offline storage configuration
{: #advanced_offline_storage}

For advanced offline storage configuration, see [here](advanced_jsonstore.html).