---

copyright:
  years: 2018, 2019
lastupdated: "2018-10-16"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Empêcher les attaques potentielles (MitM) 
{: #prevent_man_in_the_middle_attack}


Lorsque vous communiquez sur des réseaux publics, il est essentiel d'envoyer et de recevoir les informations de façon sécurisée. Le protocole largement
utilisé pour sécuriser ces communications est SSL/TLS. SSL/TLS
utilise des certificats numériques pour assurer l'authentification et le chiffrement. Ces protocoles, qui s'appuient sur la vérification d'une chaîne de certificats, sont vulnérables et peuvent faire l'objet de nombreuses attaques dangereuses, notamment des attaques potentielles (MitM), qui ont lieu lorsqu'une partie prenante non autorisée peut afficher et modifier l'intégralité du trafic entre l'appareil mobile et les systèmes de back end.
{: shortdesc}

Limitez les attaques de sécurité par le biais de l'[épinglage de certificat](#cert_pinning) et empêchez les attaques potentielles (MitM). 


>**Remarque** : facultatif et disponible uniquement pour les plans Professionnel. 

Le processus d'épinglage de certificat comprend les étapes suivantes : 

1. Effectuez la [configuration du certificat](#certsetup).
2. Configurez vote code client pour appeler la méthode [API d'épinglage de certificat](#certpinapi) appropriée avant d'émettre une demande sécurisée. 

Au cours de l'établissement de liaison SSL (première demande envoyée au serveur), le logiciel SDK client de Mobile Foundation vérifie que la clé publique du certificat serveur correspond à la clé publique du certificat qui est stocké dans l'application. 

>**Remarque** : vous devez utiliser un certificat acheté auprès d'une autorité de certification. Les certificats autosignés **ne sont pas pris en charge**.

## Epinglage de certificat 
{: #cert_pinning}

Les protocoles qui s'appuient sur la vérification d'une chaîne de certificats sont vulnérables et peuvent faire l'objet de nombreuses attaques dangereuses, notamment des attaques potentielles (MitM), qui ont lieu lorsqu'une partie prenante non autorisée peut afficher et modifier l'intégralité du trafic entre l'appareil mobile et les systèmes de back end.
Pour garantir qu'un certificat est authentique et valide, il est signé numériquement par un certificat racine appartenant à une autorité de certification digne de confiance. Les systèmes d'exploitation et les navigateurs gèrent des listes de certificats racine d'autorité de certification digne de confiance pour pouvoir facilement déterminer quels sont les certificats émis et signés par les autorités de certification.

IBM Mobile Foundation met à disposition une API permettant l'**épinglage de certificat**. Cette API est prise en charge dans les applications MobileFirst iOS natives, Android natives et Cordova multiplateformes. 

## Processus d'épinglage de certificat
{: #certpinprocess}

L'épinglage de certificat est le processus d'association d'un hôte à sa clé publique prévue. Vous pouvez configurer votre code client pour accepter uniquement un certificat spécifique pour votre nom de domaine, au lieu de n'importe quel certificat correspondant à un certificat racine d'autorité de certification digne de confiance reconnu par le système d'exploitation ou le navigateur. Une copie du certificat est placée dans votre application client. Au cours de l'établissement de liaison SSL (première demande envoyée au serveur), le logiciel SDK client de MobileFirst vérifie que la clé publique du certificat serveur correspond à la clé publique du certificat qui est stocké dans l'application. 

>**Remarque** : vous pouvez aussi épingler plusieurs certificats avec votre application client. Une copie de tous les certificats doit être placée dans votre application client. Au cours de l'établissement de liaison SSL (première demande envoyée au serveur), le logiciel SDK client de MobileFirst vérifie que la clé publique du certificat serveur correspond à la clé publique de l'un des certificats qui sont stockés dans l'application. 

**Important**

* Certains systèmes d'exploitation mobiles peuvent mettre en cache le résultat du contrôle de validation de certificat. Par conséquent, votre code doit appeler la méthode API d'épinglage de certificat **avant** d'émettre une demande sécurisée. Sinon, toutes les demandes suivantes risquent d'ignorer le contrôle de validation de certificat et d'épinglage.
* Assurez-vous de n'utiliser que des API Mobile Foundation pour toutes les communications avec l'hôte associé, même après l'épinglage de certificat. L'utilisation d'API de tiers pour interagir avec le même hôte peut entraîner un comportement inattendu, comme la mise en cache d'un certificat non vérifié par le système d'exploitation mobile. 
* Si vous appelez la méthode API d'épinglage de certificat une deuxième fois, l'opération d'épinglage précédente est écrasée. 

Si le processus d'épinglage aboutit, la clé publique qui figure dans le certificat fourni est utilisée pour vérifier l'intégrité du certificat du serveur MobileFirst au cours de l'établissement de liaison SSL/TLS pour la demande sécurisée. S'il échoue, toutes les demandes SSL/TLS envoyées au serveur sont rejetées par l'application client. 

## Configuration du certificat
{: #certsetup}

Vous devez utiliser un certificat acheté auprès d'une autorité de certification. Les certificats autosignés **ne sont pas pris en charge**. En vue de la compatibilité avec les environnements pris en charge, veillez à utiliser un certificat codé au format **DER** (fichier de règles de codage distinctif, comme défini dans la norme International Telecommunications Union X.690).

Le certificat doit être placé sur le serveur MobileFirst et dans votre application. Placez le certificat comme suit :

* Sur le serveur MobileFirst (WebSphere Application Server, WebSphere Application Server Liberty ou Apache Tomcat) : consultez la documentation de votre serveur d'applications pour des informations sur la configuration du protocole SSL/TLS et des certificats. 
* Dans votre application :
    * iOS native : ajoutez le certificat dans le **bundle** d'applications 
    * Android native : placez le certificat dans le dossier **assets**
    * Cordova : placez le certificat dans le dossier **app-name\www\certificates** (si le dossier n'existe pas, créez-le) 

## API d'épinglage de certificat
{: #certpinapi}

L'épinglage de certificat comprend la méthode API surchargée suivante, où une première méthode possède un paramètre ``certificateFilename``, où ``certificateFilename`` est le nom du fichier certificat et une deuxième méthode comporte un paramètre ``certificateFilenames``, où ``certificateFilenames`` est un tableau de noms de fichiers certificat. 

### Android
{: #certpinapiandroid}

* Certificat unique :
 
    Syntaxe : pinTrustedCertificatePublicKeyFromFile(String certificateFilename); 

    Exemple :

    ```java
    WLClient.getInstance().pinTrustedCertificatePublicKey("myCertificate.cer");
    ```

* Plusieurs certificats : 

    Syntaxe : pinTrustedCertificatePublicKeyFromFile(String[] certificateFilename);

    Exemple :

    ```java
    String[] certificates={"myCertificate.cer","myCertificate1.cer"};
    WLClient.getInstance().pinTrustedCertificatePublicKey(certificates);
    ```

La méthode d'épinglage de certificat émet une exception dans deux cas :

* Le fichier n'existe pas
* Le format du fichier n'est pas correct

### iOS
{: #certpinapiios}

* Epinglage d'un certificat unique 

    Syntaxe : pinTrustedCertificatePublicKeyFromFile:(NSString*) certificateFilename;

    La méthode d'épinglage de certificat émet une exception dans deux cas :

    * Le fichier n'existe pas
    * Le format du fichier n'est pas correct

* Epinglage de plusieurs certificats  
 
    Syntaxe : pinTrustedCertificatePublicKeyFromFiles:(NSArray*) certificateFilenames;

    La méthode d'épinglage de certificat émet une exception dans deux cas :

    * Aucun des fichiers certificat n'existe 
    * Le format d'aucun des fichiers certificat n'est correct 

**Dans Objective-C**:

Exemple pour un certificat unique : 

```
[[WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFile:@"myCertificate.cer"];
```

Exemple pour plusieurs certificats : 

```
NSArray *arrayOfCerts = [NSArray arrayWithObjects:@“Cert1”,@“Cert2”,@“Cert3",nil];
[[WLClient sharedInstance]pinTrustedCertificatePublicKeyFromFiles:arrayOfCerts];
```

**Dans Swift**:

Exemple pour un certificat unique : 

```
WLClient.sharedInstance().pinTrustedCertificatePublicKeyFromFile("myCertificate.cer")
```

Exemple pour plusieurs certificats : 

```
let arrayOfCerts : [Any] = ["Cert1", "Cert2”, "Cert3”];
WLClient.sharedInstance().pinTrustedCertificatePublicKey( fromFiles: arrayOfCerts)
```

La méthode d'épinglage de certificat émet une exception dans deux cas :

* Le fichier n'existe pas
* Le format du fichier n'est pas correct

### Cordova
{: #certpinapicordova}

* Epinglage d'un certificat unique : 

    ```javascript
    WL.Client.pinTrustedCertificatePublicKey('myCertificate.cer').then(onSuccess, onFailure);
    ```

* Epinglage de plusieurs certificats : 

    ```javascript
    WL.Client.pinTrustedCertificatePublicKey(['Cert1.cer','Cert2.cer','Cert3.cer']).then(onSuccess, onFailure);
    ```

La méthode d'épinglage de certificat renvoie une promesse :

* La méthode d'épinglage de certificat appelle la méthode onSuccess si l'épinglage aboutit 
* La méthode d'épinglage de certificat déclenche le rappel onFailure dans deux cas :
* Le fichier n'existe pas
* Le format du fichier n'est pas correct

Ultérieurement, si une demande sécurisée est envoyée à un serveur dont le certificat n'est pas épinglé, le rappel ``onFailure`` de la demande spécifique (par exemple ``obtainAccessToken`` ou ``WLResourceRequest``) est appelé. 

>Pour plus d'informations sur la méthode API d'épinglage de certificat, voir la [référence d'API](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/).
