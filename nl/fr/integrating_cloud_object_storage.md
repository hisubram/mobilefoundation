---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-13"

keywords: mobile foundation, integration, cloud object storage, COS, ibm cloud

subcollection:  mobilefoundation
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
{:reactnative: .ph data-hd-programlang='React Native'}
{:csharp: .ph data-hd-programlang='csharp'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}
{:xml: .ph data-hd-programlang='xml'}

# Utilisation d'{{ site.data.keyword.cos_full_notm }} avec {{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}}
{: #using_ibm_cloud_object_storage_with_ibm_mobile_foundation}

{{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}} (MF) offre des fonctions de niveau entreprise qui sont uniquement conçues pour prendre en charge la création et le déploiement de la prochaine génération d'applications mobiles cognitives, contextuelles et personnalisées. {{ site.data.keyword.cos_full_notm }} (COS) constitue une solution de stockage en cloud souple, économique et évolutive pour les données non structurées. Ce guide pratique explique comment une application mobile utilisant {{ site.data.keyword.mobilefoundation_short}} peut se connecter et extraire ou télécharger des données vers {{ site.data.keyword.cos_full_notm }} via une application Ionic. L'application Ionic, l'adaptateur et les fichiers connexes pour ce tutoriel pratique sont disponibles
[ici](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App).
{: shortdesc}


## Prérequis
{: #cos-prerequisites}

1. Installez [mfpdev-cli](https://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_wl_cli_description.html) en exécutant `npm install -g mfpdev-cli`. Cette interface de ligne de commande permet d'enregistrer l'application Ionic et de déployer l'adaptateur sur le serveur MF. Ces activités peuvent également être effectuées à partir du tableau de bord du serveur MF. 

2. Installez l'interface de ligne de commande [{{ site.data.keyword.cloud_notm}} ](https://console.bluemix.net/docs/cli/index.html#overview) sur votre machine. 

3. Installez l'interface de ligne de commande Ionic en exécutant la commande `npm install -g ionic`

4. Installez cordova en exécutant la commande `npm install -g cordova`


## Configuration du serveur {{ site.data.keyword.mobilefoundation_short}}
{: #mobile-foundation-server-setup}

Le serveur {{ site.data.keyword.mobilefoundation_short}} est défini sur {{ site.data.keyword.cloud_notm}}. Configurez une instance {{ site.data.keyword.cloud_notm}} du serveur {{ site.data.keyword.mobilefoundation_short}} comme suit. 

* Dans le catalogue {{ site.data.keyword.cloud_notm}}, recherchez "{{ site.data.keyword.mobilefoundation_short}}". Cliquez sur la vignette **{{ site.data.keyword.mobilefoundation_short}}**. 

    ![Catalogue MFP](images/mfp_catalog.png)

* Indiquez un nom adapté à l'instance de serveur {{ site.data.keyword.mobilefoundation_short}} et cliquez sur **Create**. 

    ![Nouveau BMMFP](images/bmmfpnew.png)

* Une nouvelle instance de serveur MF est créée, qui nécessite la saisie de vos données d'identification. 

    ![Connexion MFP](images/mfp_login.png)

* Les données d'identification qui permettent de se connecter au serveur MF se trouvent sur l'onglet **Service Credentials** dans le menu de gauche. 

    ![Données d'identification MFP](images/mfp_credentials.png)

* Indiquez ces données d'identification et connectez-vous pour accéder au tableau de bord MF. 

    ![Tableau de bord MFP](images/mfp_dashboard.png)


## Configuration de Cloud Object Storage
{: #cloud-object-storage-setup}

* Dans le catalogue {{ site.data.keyword.cloud_notm}}, recherchez "Cloud Object Storage". Cliquez sur la vignette **Object Storage**. 

    ![Catalogue](images/catalog.png)

* Indiquez un nom pour votre instance COS et cliquez sur **Create**. 

    ![Création de COS](images/cos_create.png)

* Ensuite, cliquez sur **Buckets** dans les options du menu de gauche. Indiquez un nom approprié pour votre compartiment (dans cet exemple, nous avons choisi de nommer le compartiment `sharedgallery`) et cliquez sur **Create**. 

    ![Création de compartiment](images/bucketcreate.png)

* Une fois le compartiment créé, la page d'arrivée du compartiment ressemble à l'image suivante. Elle contient des options permettant de charger directement des données. 

    ![Page d'arrivée du compartiment](images/bucketlanding.png)

* Ajoutez ici les trois fichiers fournis dans le dossier **Short stories** de
l'[exemple](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App). 


## Application Ionic MFP-COS et adaptateur Java
{: #mfp-cos-ionic-app-and-java-adapter}

Téléchargez ce [référentiel git](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App) ou clonez-le. Ce référentiel est composé de deux éléments principaux : 

1. Un adaptateur MF Java
2. Une application mobile Ionic

### Configuration de mfpdev-cli
{: #configuring-mfpdev-cli}

Ajoutez les informations de serveur à l'interface de ligne de commande. A l'invite de commande, exécutez `mfpdev server add`. 

```
? Saisissez le nom du nouveau profil de serveur :
```

Indiquez un nom pour le serveur et appuyez sur Entrée. Dans cet exemple, le nom indiqué est `mfpserver`. 

```
? Saisissez l'URL qualifiée complète de ce serveur :
```

Saisissez l'URL du serveur. Pour un serveur MF sur {{ site.data.keyword.cloud_notm}}, les données d'identification du service contiennent l'URL. Le port pour le serveur MF HTTPS sur {{ site.data.keyword.cloud_notm}} est 443 et pour l'instance de serveur MF HTTP est 80. 

```
? Saisissez l'ID de connexion administrateur au serveur MobileFirst : (admin)
```

Saisissez `admin` et appuyez sur **Entrée**. 

```
? Saisissez le mot de passe administrateur du serveur MobileFirst :
```

Saisissez le mot de passe disponible dans les données d'identification du service. 

```
? Save the administrator password for this server?: (Y/n)
```

En fonction de votre préférence, saisissez *Y/N* ainsi que les détails demandés par les invites. 

```
? Enter the MobileFirst Server connection timeout in seconds: 30
```

Définissez 30 secondes comme délai par défaut.

```
? Make this server the default?: (Y/n)
```

Saisissez *Y* et appuyez sur **Entrée**. 

***Sortie attendue*** :

```
Verifying server configuration...
The following runtimes are currently installed on this server: mfp
Server profile 'mfpserver' added successfully.
```

### Configuration de l'adaptateur MF Java
{: #configuring-the-mf-java-adapter}

Pour vous connecter à votre instance COS, vous devez fournir certains détails relatifs à votre instance COS dans le fichier `adapter.xml`. Indiquez des valeurs pour les zones suivantes : 

1. **endpointURL** : cette zone est l'URL du noeud final public pour votre objet COS. Cette URL se trouve sur le tableau de bord de votre COS,
sous **Buckets (dans les options du menu de gauche) -> <nom-votre-compartiment> (`sharedgallery` dans cet exemple) -> Configuration -> Endpoints -> Public**
2. **AuthToken** : dans ce tutoriel, nous utilisons l'authentification IAM. 

Pour que l'adaptateur Java se connecte à votre instance de COS, une authentification utilisant IAM ou HMAC est nécessaire. Vous trouverez ci-dessous la procédure d'obtention du jeton IAM. Pour plus de détails sur les processus d'authentification IAM et HMAC, cliquez [ici](https://cloud.ibm.com/docs/services/cloud-object-storage/api-reference/api-reference-buckets.html#bucket-operations#AuthenticationOptions). 

#### Obtention d'un jeton IAM Oauth à l'aide de l'interface de ligne de commande {{ site.data.keyword.cloud_notm}}
{: #obtaining-iam-oath-token-using-ibm-cloud-cli}

1. Vérifiez d'abord que vous disposez d'une clé d'API. Obtenez la clé d'API à partir d'[{{ site.data.keyword.cloud_notm}} Identity and Access Management](https://cloud.ibm.com/iam/#/users). 
2. Connectez-vous à la plateforme {{ site.data.keyword.cloud_notm}} à l'aide de l'interface de ligne de commande. 

  ```bash
	ibmcloud login --apikey <valeur>
  ```
  {: codeblock}
  La sortie est similaire au fragment suivant : 

	```text
	Authenticating...
	OK

	Targeted account <nom-compte> (<ID-compte>)

	Targeted resource group default

	API endpoint:     https://api.ng.bluemix.net (API version: 	2.75.0)
	Region:           us-south
	User:             <adresse-électronique>
	Account:          <nom-compte> (<ID-compte>)
	Resource group:   default
	```

3. Pour obtenir toutes les instances de service figurant sur votre compte {{ site.data.keyword.cloud_notm}}, exécutez la commande suivante sur l'interface de ligne de commande. 

	```bash
  ibmcloud resource service-instances
  ```
  {: codeblock}

	Sortie attendue :

	```text
 	Retrieving service instances in resource group Default and all 	locations under account <nom-compte> as <adresse-électronique>...
	OK
	Name                                               Location     	State    Type               Tags
	<nom-instance-ressource>                     global       	active   service_instance
	```

4. Exécutez la commande suivante pour obtenir les détails de votre instance COS. <nom-instance> est le nom de votre service COS (`newObject` pour ce tutoriel).

  ```bash
	ibmcloud resource service-instance <nom-instance>
  ```
  {: codeblock}

	Sortie attendue :

	```text
	Retrieving service instance <nom-instance> in resource group 	Default under account <nom-compte> as<adresse-électronique>...
	OK

	Name:                  <nom-instance-ressource>
	ID:                    <ID-instance-ressource>
	Location:                 global
	Service Name:          cloud-object-storage
	Resource Group Name:   Default
	State:                 active
	Type:                  service_instance
	Tags:
	```

5. Pour obtenir le jeton IAM, exécutez la commande suivante :
  ```bash
	ibmcloud iam oauth-tokens
  ```

	Sortie attendue :

	```text
	IAM token:  Bearer <jeton>
	UAA token:  Bearer <jeton-actualisation>
	```

Après avoir ajouté les valeurs *endpointURL* et *authToken*, générez l'adaptateur. Accédez au dossier racine de l'adaptateur à l'invite de commande et exécutez
```bash
mfpdev adapter build
```
{: codeblock}

Le fichier `*.adapter` est créé dans le dossier `target`. Exécutez la commande suivante :

```bash
mfpdev adapter deploy
```
{: codeblock}

L'adaptateur est déployé sur l'instance MF.

L'adaptateur peut également être déployé sur le tableau de bord du serveur MF. Ouvrez le tableau de bord du serveur MF. Dans le menu de gauche, cliquez sur **Adapters -> New** pour ouvrir la page illustrée dans l'image suivante.

![MFPNewAdapterRegister](images/mfp_new_adapter_register.png)

Cliquez ensuite sur **Deploy Adapter** et téléchargez le fichier `.adapter` à partir du dossier **target**.


### Configuration de l'application Ionic
{: #configuring-the-ionic-app}

Dans l'application, procédez comme suit :

1. Accédez au dossier qui contient l'application Ionic.

2. Ajoutez le plug-in MF cordova.

	```bash
  ionic cordova plugin add cordova-plugin-mfp
  ```

3. Ajoutez la plateforme Android ou iOS.

  ```bash
	ionic cordova platform add android
  ```
	Ou

	```bash
	ionic cordova platform add ios
	```

4. Enregistrez votre application sur le serveur MF en exécutant la commande

  ```bash
	mfpdev app register
	```

	L'application peut également être enregistrée sur le tableau de bord du serveur MF. Ouvrez le tableau de bord du serveur MF et dans le menu de gauche, cliquez sur **Applications -> New**.
	La page présentée dans l'image suivante est chargée.

	![MFPNewAppRegister](images/mfp_new_app_register.png)

	Saisissez les informations demandées. Affectez un nom à votre application dans la zone de texte 'Application Name'. Choisissez la plateforme requise.

	Pour Android, la zone de texte **Package** accepte la valeur *Application identifier*. Ce paramètre se trouve dans le fichier `AndroidManifest.xml` en tant que package de votre application Android. La zone de texte **Version** doit être renseignée à l'aide de la valeur *versionName* provenant du fichier `AndroidManifest.xml`.
	Pour iOS, la zone de texte **Bundle ID** accepte la valeur *Application identifier* (sensible à la casse). Ce paramètre se trouve dans le fichier `mfpclient.plist` de votre application iOS. La zone de texte **Version** doit être renseignée à l'aide de la valeur *version* provenant du fichier `mfpclient.plist` de votre application iOS.
5. Exécutez `ionic cordova prepare` pour que les modifications soient transmises aux environnements ajoutés.
6. Exécutez la commande
  ```bash
	ionic cordova build android
  ```
	Ou
  ```bash
	ionic cordova build ios
  ```
	pour vous assurer que les modifications TypeScript sont ajoutées aux environnements.

7. Connectez l'appareil ou lancez un émulateur ou un simulateur et exécutez la commande suivante.

	```bash
  ionic cordova run android
	```
	Ou
	```bash
	ionic cordova run ios
  ```

### Navigation dans l'application Ionic
{: #navigating-the-ionic-application}

L'application Ionic affiche une liste de brèves histoires qui ont été téléchargées précédemment (au cours de la dernière étape de la section [Configuration de Cloud Object Storage](#cloud-object-storage-setup)), à partir de l'instance COS que vous avez créée. Lors de
la sélection d'une option particulière de l'histoire, l'histoire sélectionnée est chargée et affichée. Une option permet également d'ajouter une histoire
qui est ensuite téléchargée vers l'instance COS. 

La liste initiale des objets COS se présente comme dans l'image suivante. 

![COS avant](images/cos_before.png)

La page d'accueil de l'application fournit une option permettant d'accéder à toutes les histoires ou d'ajouter une histoire. 

![Ecran d'accueil de l'application](images/app-home-screen.png)

Cliquez sur **Get all stories** pour afficher les histoires disponibles sur COS. 

![Application de sélection d'histoire](images/app-select-story.png)

Sélectionnez une histoire à charger à partir du menu déroulant et cliquez sur **Load**. 

![Application de chargement d'histoire](images/app-story-loaded.png)

Cliquez ensuite sur le bouton **Add story** pour ajouter votre propre histoire. Entrez un titre pour l'histoire et saisissez son contenu dans la zone de texte, puis cliquez sur **Add**. 

![Application d'ajout d'entrée](images/app-add-input.png)

Une fois l'histoire ajoutée, un message s'affiche pour indiquer que l'ajout de l'histoire a abouti. 

![Application d'ajout d'histoire](images/app-story-added.png)

L'histoire est désormais ajoutée au tableau de bord COS à partir de l'application. 

![Ajout COS](images/cos_added.png)


Le jeton IAM Oauth obtenu à l'aide de `ibmcloud iam oauth-tokens` possède une date d'expiration et l'adaptateur échoue avec l'exception `403 - Forbidden`. Il convient alors de s'assurer que le jeton est valide sur l'adaptateur déployé pour que l'application fonctionne comme prévu.
{: note}
