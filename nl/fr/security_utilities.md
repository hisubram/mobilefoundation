---

copyright:
  years: 2018, 2019
lastupdated:  "2018-11-19"

keywords: security

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

#	Utilitaires de sécurité
{: #security_utilities}

L'API côté client de Mobile Foundation fournit des utilitaires de sécurité permettant de protéger les données de vos utilisateurs. Des fonctions telles que JSONStore sont idéales pour
protéger les objets JSON. Il est conseillé de ne pas stocker des objets blobs binaires dans une collection JSONStore.

A la place, stockez les données binaires dans le système de fichiers, et stockez les chemins d'accès aux fichiers et d'autres métadonnées dans une
collection JSONStore. Si vous voulez protéger des fichiers tels que des images, vous pouvez les coder sous forme de chaînes base64, les chiffrer et écrire
la sortie sur le disque. Pour déchiffrer les données, vous pouvez rechercher les métadonnées dans une collection JSONStore. Lisez les données chiffrées à partir du disque et déchiffrez-les à l'aide des métadonnées stockées. Ces métadonnées peuvent inclure la clé, le sel de cryptage, le vecteur d'initialisation, le type de fichier, le chemin d'accès au fichier, etc.

Globalement, l'API SecurityUtils fournit les API suivantes :

* Génération de clé - plutôt que de transmettre un mot de passe directement à la fonction de chiffrement, cette fonction de génération de clé utilise la fonction PBKDF2 (Password-Based Key Derivation Function 2) pour générer une clé forte de 256 bits pour l'API de chiffrement. Elle admet un paramètre spécifiant le nombre
d'itérations. Plus le nombre est élevé, plus un agresseur informatique aura besoin de temps pour attaquer votre clé par force brute. Utilisez une valeur
d'au moins 10 000. Le sel de cryptage doit être unique, afin de compliquer le travail des agresseurs informatiques qui se servent d'informations de hachage
existantes pour pirater votre mot de passe. Utilisez une longueur de 32 octets.
* Chiffrement - l'entrée est chiffrée avec la norme AES (Advanced Encryption Standard). L'API admet une clé qui est générée avec l'API de génération de
clé. En interne, elle génère un vecteur d'initialisation sécurisé qui est utilisé pour ajouter un ordre aléatoire au premier chiffrement par
bloc. Le texte est chiffré. Si vous voulez chiffrer une image ou un autre format binaire, convertissez votre fichier binaire en texte base64 à l'aide de
ces API. Cette fonction de chiffrement renvoie un objet composé des parties suivantes :
    * ct (texte chiffré)
    * IV
    * v (version, qui permet à l'API d'évoluer tout en restant compatible avec une version précédente)
* Déchiffrement - prend la sortie de l'API de chiffrement comme entrée, et déchiffre le texte chiffré en texte en clair.
* Chaîne aléatoire distante - obtient une chaîne hexadécimale aléatoire en contactant un générateur aléatoire sur le serveur MobileFirst. La valeur par défaut est 20 octets, mais vous pouvez indiquer un nombre jusqu'à 64 octets.
* Chaîne aléatoire locale - obtient une chaîne hexadécimale aléatoire en en générant une localement, à la différence de l'API de chaîne aléatoire
distante, qui requiert un accès au réseau. La valeur par défaut est 32 octets et il n'y a pas de valeur maximale. La durée de l'opération est proportionnelle au nombre d'octets.
* Codage base64 - applique le codage base64 à une chaîne. En cas d'application du codage base64, étant donné la nature de l'algorithme, la taille des
données est augmentée d'environ 1,37 fois la taille d'origine.
* Décodage base64 - applique le décodage base64 à une chaîne codée en base64.

## Configuration
Veillez à importer les fichiers ci-après pour l'utilisation des API des utilitaires de sécurité JSONStore.

### Configuration d'iOS

```objc
#import "WLSecurityUtils.h"
```

### Configuration d'Android

```java
import com.worklight.wlclient.api.SecurityUtils
```

### Configuration de JavaScript
Aucune configuration n'est requise.

## Exemples pour iOS
### Chiffrement et déchiffrement dans iOS

```objc
// User provided password, hardcoded only for simplicity.
NSString* password = @"HelloPassword";

// Random salt with recommended length.
NSString* salt = [WLSecurityUtils generateRandomStringWithBytes:32];

// Recomended number of iterations.
int iterations = 10000;

// Populated with an error if one occurs.
NSError* error = nil;

// Call that generates the key.
NSString* key = [WLSecurityUtils generateKeyWithPassword:password
                                 andSalt:salt
                                 andIterations:iterations
                                 error:&error];

// Text that is encrypted.
NSString* originalString = @"My secret text";
NSDictionary* dict = [WLSecurityUtils encryptText:originalString
                                      withKey:key
                                      error:&error];

// Should return: 'My secret text'.
NSString* decryptedString = [WLSecurityUtils decryptWithKey:key
                                             andDictionary:dict
                                             error:&error];
```
{: codeblock}

### Codage et décodage base64 dans iOS

```objc
// Input string.
NSString* originalString = @"Hello world!";

// Encode to base64.
NSData* originalStringData = [originalString dataUsingEncoding:NSUTF8StringEncoding];
NSString* encodedString = [WLSecurityUtils base64StringFromData:originalStringData length:originalString.length];

// Should return: 'Hello world!'.
NSString* decodedString = [[NSString alloc] initWithData:[WLSecurityUtils base64DataFromString:encodedString] encoding:NSUTF8StringEncoding];
```
{: codeblock}

### Obtention d'une chaîne aléatoire distante dans iOS

```objc
[WLSecurityUtils getRandomStringFromServerWithBytes:32
                 timeout:1000
                 completionHandler:^(NSURLResponse *response, NSData *data, NSError *connectionError) {

  // You might want to see the response and the connection error before moving forward.

  // Get the secure random string.
  NSString* secureRandom = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
}];
```
{: codeblock}

## Exemples pour Android
### Chiffrement et déchiffrement dans Android

```java
String password = "HelloPassword";
String salt = SecurityUtils.getRandomString(32);
int iterations = 10000;

String key = SecurityUtils.generateKey(password, salt, iterations);

String originalText = "Hello World!";

JSONObject encryptedObject = SecurityUtils.encrypt(key, originalText);

// Deciphered text will be the same as the original text.
String decipheredText = SecurityUtils.decrypt(key, encryptedObject);
```
{: codeblock}

### Codage et décodage base64 dans Android

```java
import android.util.Base64;

String originalText = "Hello World";
byte[] base64Encoded = Base64.encode(text.getBytes("UTF-8"), Base64.DEFAULT);

String encodedText = new String(base64Encoded, "UTF-8");

byte[] base64Decoded = Base64.decode(text.getBytes("UTF-8"), Base64.DEFAULT);

// Decoded text will be the same as the original text.
String decodedText = new String(base64Decoded, "UTF-8");
```
{: codeblock}

### Obtention d'une chaîne aléatoire distante dans Android

```java
Context context; // This is the current Activity's context.
int byteLength = 32;

// Listener calls the callback functions after it gets the response from the server.
WLRequestListener listener = new WLRequestListener(){
  @Override
  public void onSuccess(WLResponse wlResponse) {
    // Implement the success handler.
  }

  @Override
            public void onFailure(WLFailResponse wlFailResponse) {
    // Implement the failure handler.
    }
};

SecurityUtils.getRandomStringFromServer(byteLength, context, listener);
```
{: codeblock}

### Obtention d'une chaîne aléatoire locale dans Android

```java
int byteLength = 32;
String randomString = SecurityUtils.getRandomString(byteLength);
```
{: codeblock}

## Exemples pour JavaScript
### Chiffrement et déchiffrement dans JavaScript

```javascript
// Keep the key in a variable so that it can be passed to the encrypt and decrypt API.
var key;

// Generate a key.
WL.SecurityUtils.keygen({
  password: 'HelloPassword',
  salt: Math.random().toString(),
  iterations: 10000
})

.then(function (res) {

  // Update the key variable.
  key = res;

  // Encrypt text.
  return WL.SecurityUtils.encrypt({
    key: key,
    text: 'My secret text'
  });
})

.then(function (res) {

  // Append the key to the result object from encrypt.
  res.key = key;

  // Decrypt.
  return WL.SecurityUtils.decrypt(res);
})

.then(function (res) {

  // Remove the key from memory.
  key = null;

  //res => 'My secret text'
})

.fail(function (err) {
  // Handle failure in any of the previously called APIs.
});
```
{: codeblock}

### Codage et décodage base64 dans JavaScript

```javascript
WL.SecurityUtils.base64Encode('Hello World!')
.then(function (res) {
  return WL.SecurityUtils.base64Decode(res);
})
.then(function (res) {
  //res => 'Hello World!'
})
.fail(function (err) {
  // Handle failure.
});
```
{: codeblock}

### Obtention d'une chaîne aléatoire distante dans JavaScript

```javascript
WL.SecurityUtils.remoteRandomString(32)
.then(function (res) {
  // res => deba58e9601d24380dce7dda85534c43f0b52c342ceb860390e15a638baecc7b
})
.fail(function (err) {
  // Handle failure.
});
```
{: codeblock}

### Obtention d'une chaîne aléatoire locale dans JavaScript

```javascript
WL.SecurityUtils.localRandomString(32)
.then(function (res) {
  // res => 40617812588cf3ddc1d1ad0320a907a7b62ec0abee0cc8c0dc2de0e24392843c
})
.fail(function (err) {
  // Handle failure.
});
```
{: codeblock}
