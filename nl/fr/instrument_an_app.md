---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-30"

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
{:web: .ph data-hd-programlang='Web'}

# Instrumentation de votre application
{: #instrument_your_app}

Mobile Analytics est une fonction imbriquée dans le service {{ site.data.keyword.mobilefoundation_short }}. Elle fournit des informations essentielles sur l'utilisation et les performances des applications aux développeurs d'applications mobiles et aux propriétaires d'application. 

Vous devez instrumenter votre application mobile pour commencer à utiliser la fonction Mobile Analytics afin de surveiller l'utilisation et les performances de votre application, et d'obtenir d'autres statistiques. Pour instrumenter votre application, effectuez les étapes suivantes :  

1.  Importez et installez le logiciel SDK client de Mobile Analytics. 
2.  Instrumentez votre application en fonction du type de données d'analyse que vous voulez collecter. 

Les sections ci-après expliquent ces étapes en détail, pour chaque plateforme prise en charge. 

### Instrumentation d'une application Android 
{: #instrument_android_app}
{: android}
#### Etape 1 : importez et installez le logiciel SDK client de Mobile Analytics 
{: #install_analytics_sdk_android }
{: android}
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics)
{: android}

Le logiciel SDK client de Mobile Analytics est distribué avec Gradle, qui est un gestionnaire de dépendances pour les projets Android. Gradle télécharge automatiquement des artefacts depuis des référentiels et les met à la disposition de votre application Android.
{: android}

1. Créez un projet [Android Studio ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://developer.android.com/sdk/index.html){: new_window} ou ouvrez un projet existant.
{: android}

2. Ouvrez le fichier `build.gradle` qui se trouve dans votre **module d'application**.
{: android}

  Votre projet Android peut contenir deux fichiers `build.gradle` : un pour le projet et l'autre pour le module d'application. Assurez-vous d'utiliser le fichier du **module d'application**.
  {: tip}
  {: android}

3. Localisez la section `Dependencies` dans le fichier `build.gradle` et ajoutez une dépendance de compilation pour le logiciel SDK client de {{site.data.keyword.mobileanalytics_short}}. Votre instruction relative aux référentiels doit être similaire à l'exemple de code suivant :
{: android}

  ```
  dependencies {
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundation:8.0.+'
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationanalytics:8.0.+@aar'
    // other dependencies
  }
  ```
  {: codeblock}
  {: android}

  La première dépendance concerne le logiciel SDK client de Mobile Analytics, pour la capture et la journalisation d'événements du contexte d'exécution de l'application ; la deuxième dépendance
permet d'activer les commentaires en retour des utilisateurs de l'application lors de l'interaction avec l'utilisateur de l'application. Elle n'est requise que si vous activez les commentaires en retour des utilisateurs de l'application.
  {: android}

4. Synchronisez votre projet avec Gradle en cliquant sur **Tools &gt; Android &gt; Sync Project with Gradle Files**.
{: android}

5. Ouvrez le fichier `AndroidManifest.xml` pour votre projet Android. Il se trouve dans **app > manifests**. Ajoutez les droits d'accès Internet et les droits d'accès à l'emplacement sous l'élément `<manifest>` :
{: android}

  ```xml
   <uses-permission android:name="android.permission.INTERNET" />
 
  ```
  {: codeblock}
  Si vous utilisez la version 1.2 du logiciel SDK ou une version ultérieure, vous devez insérer les lignes suivantes dans l'élément `<application>` du fichier `AndroidManifest.xml` :
  {: android}

  ```
    <activity
  android:name="com.ibm.mobilefirstplatform.clientsdk.android.ui.UIActivity"
  android:label="@string/app_name"
  android:launchMode="singleTask">
  <intent-filter>
  <action android:name="android.intent.action.MAIN" />
  <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
  </activity>
  ```
  {: codeblock}
  {: android}

#### Etape 2 : instrumentez votre application Android en fonction du type de données d'analyse à collecter
{: #instrument_app_based_on_data_android }
{: android}

1. Initialisez votre application pour capturer et envoyer des données d'analyse au service Mobile Analytics. Tout d'abord, ajoutez les instructions `import` suivantes au début de votre classe d'application ou d'activité :
{: android}

   ```Java
    import com.worklight.common.Logger;
    import com.worklight.common.WLAnalytics;
   ```
   {: codeblock}
   {: android}

   Ensuite, utilisez le logiciel SDK client de Mobile Analytics pour configurer ou initialiser la capture des données d'analyse.   
   Ajoutez le code d'initialisation dans la méthode `onCreate` de l'activité principale dans votre application Android ou à un emplacement adapté à votre projet d'application.
   {: android}

   ```Java
    WLAnalytics.init(this.getApplication());
   ```
   {: codeblock}
   {: android}

   Avant d'appeler la méthode init, vous devez vous assurer que votre application imbrique le code requis pour l'authentification et l'autorisation du appareil dans le service MobileFoundation. Il s'agit d'une étape commune requise par toutes les applications des services Mobile Foundation ; elle n'est pas propre à la capture de données d'analyse. <!--  Refer <need to link doc that talks about auth> -->
   {: android}

   Une fois l'initialisation terminée, votre application peut capturer des informations sur les appareils et les journaux du logiciel SDK de Mobile Analytics sans qu'il ne soit nécessaire d'ajouter du code. Les API et le code présentés dans les sections ci-après sont facultatifs ; vous pouvez les ajouter selon le type de données d'analyse à capturer.
   {: android}

2. Pour capturer des événements de cycle de vie de l'application, comme des informations sur les sessions et les pannes de l'application, configurez votre application en ajoutant la ligne suivante : 
  ```Java
    WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.LIFECYCLE);
  ```
  {: codeblock}
  {: android}

3. Pour capturer des événements de réseau qui capturent les interactions des applications sur le réseau, configurez votre application en ajoutant la ligne suivante :  
  ```Java
    WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.NETWORK);
  ```
  {: codeblock}
  {: android}

4. Vous venez d'initialiser votre application pour capturer des données d'analyse. Maintenant, vous devez envoyer les données capturées au service Mobile Analytics.
   Utilisez l'API suivante pour envoyer les données d'analyse au service Mobile Analytics :

   ```java
    WLAnalytics.send();
   ```
   {: codeblock}
   {: android}

  Vous pouvez décider à quel moment appeler cette API dans votre flux d'application afin d'envoyer les données d'analyse capturées au service Mobile Analytics. En attendant, toutes les données d'analyse capturées sont stockées localement sur l'appareil.
  {: android}

5. Pour capturer et envoyer des journaux d'application au service Mobile Analytics, utilisez les API du consignateur du logiciel SDK client de Mobile Analytics.      
   L'infrastructure de journalisation du logiciel SDK client de Mobile Analytics prend en charge les niveaux de journalisation suivants, répertoriés du moins prolixe au plus prolixe, avec des recommandations d'utilisation : 
    * FATAL : pour les pannes et les blocages irrémédiables. Le niveau FATAL est réservé aux erreurs irrémédiables de journalisation, qui se matérialisent pour l'utilisateur sous la forme d'une panne de l'application.
    * ERROR : pour les exceptions inattendues ou les erreurs de protocole de réseau inattendues.
    * WARN : pour les avertissements relatifs à l'utilisation qui ne sont pas considérés comme des erreurs critiques, par exemple l'utilisation d'API dépréciées ou un temps de réponse long du réseau. 
    * INFO : pour afficher des événements d'initialisation et d'autres données qui peuvent être importants, mais non urgents.
    * DEBUG : pour afficher des informations de débogage pouvant aider les développeurs à résoudre les bogues de l'application. Lorsque le niveau de journalisation est DEBUG, vous obtenez également les journaux du logiciel SDK client de Mobile Analytics qui sont inclus lorsque vous envoyez des journaux.
   {: note}
   {: android}

   Les fragments de code suivants représentent un exemple d'utilisation du consignateur pour Android : 
   ```java
   // Set the logging level (optional)
   // The default setting is Logger.LEVEL.DEBUG
   Logger.setLogLevel(Logger.LEVEL.INFO);
    
   //Create a logger instance.  You can create multiple loggers to organize your logs as you wish
   Logger logger = Logger.getInstance("loggerName");
   
   // Log messages with different levels
   // Debug message for feature 1
   // Info message for feature 2
   logger.debug("debug message");
   //the logger.debug message is not logged because the logLevelFilter is set to Info
   logger.info("info message");
      
   // Send logs to the Mobile Analytics
   Logger.send();
   ```
   {: codeblock}
   {: android}

6.  Pour obtenir des informations sur les modèles d'intégration utilisateur (nouveaux utilisateurs/utilisateurs existants), vous devez associer une identité d'utilisateur à votre session d'application. Pour ce faire, vous pouvez appeler l'API suivante :
    ```java
      WLAnalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: android}

    Notez que toutes les données d'analyse capturées et stockées localement sont envoyées au service Mobile Analytics uniquement lorsque l'API WLAnalytics.send() est appelée.
    {: android}

7.  Pour obtenir des informations sur vos modèles d'interaction sur le réseau HTTP des applications, comme les API HTTP appelées, le nombre de demandes et les temps de réponse moyens, vous devez utiliser la classe WLResourceRequest du logiciel SDK client de Mobile Foundation Service pour effectuer les appels HTTP. Bien que vous ayez peut-être initialisé Analytics pour la capture d'événements de réseau, si vous utilisez WLResourceRequest, Mobile Analytics peut s'introduire dans les appels de réseau et capturer les données pertinentes. Voici comment vous devez effectuer vos appels HTTP :
```java
      WLResourceRequest request = new WLResourceRequest(new URI(url), WLResourceRequest.GET);
            request.send(new WLResponseListener() {

                @Override
                    public void onSuccess(WLResponse wlResponse) {
                    //handle success of HTTP call and use wlResponse 
                }

                @Override
            public void onFailure(WLFailResponse wlFailResponse) {
                    //handle failure of HTTP call and use wlFailResponse
                }
            });
    ```
    {: codeblock}
    {: android}

8.  Pour obtenir une analyse des pannes et les journaux, vous pouvez appeler l'API suivante au démarrage de votre application afin de vérifier si la session précédente est tombée en panne et le cas échéant, envoyer les journaux de panne capturés au service Mobile Analytics :
```java
      if (Logger.isUnCaughtExceptionDetected()) {
            //add any other handling you may want and call the following APIs
            WLAnalytics.send();
            Logger.send();
      }
    ```
    {: codeblock}
    {: android}

9.  Pour définir des analyses personnalisées et vos propres données d'analyse au-delà des éléments pris en charge intrinsèquement dans le logiciel SDK client, vous pouvez utiliser l'API de journalisation personnalisée :
    ```java
        //create a JSON to capture the custom data 
        JSONObject jsonObject = new JSONObject();
        jsonObject.put("FromPage", "LoginPage");
   
        //log the custom data with a message
        WLAnalytics.log("log message", jsonObject);
        
        //Send the captured custom data and log to the Mobile Analytics service
        WLAnalytics.send();
    ```
    {: codeblock}
    {: android}

    Les données personnalisées journalisées peuvent être représentées dans des graphiques personnalisés que vous pouvez définir dans la console Mobile Analytics afin de déduire des informations personnalisées.
    {: android}

10. Utilisez les commentaires en retour des utilisateurs de l'application pour approfondir votre analyse des performances de l'application. Vous pouvez permettre aux **utilisateurs et testeurs** de votre application d'envoyer des commentaires en retour contextuels et riches aux propriétaires d'application. Les **propriétaires d'application** obtiennent des commentaires en retour en temps réel de la part de leurs utilisateurs concernant l'expérience d'utilisation de l'application, que les **propriétaires d'application** et les **développeurs** peuvent ensuite améliorer. Ces commentaires apportent une grande souplesse pour la maintenance de l'application. Utilisez l'API suivante afin de définir le mode de commentaires en retour interactifs pour votre application dans tout gestionnaire d'action de votre application, par exemple lors du traitement du clic d'un bouton ou de la sélection d'un élément de menu.
    ```java
        WLAnalytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: android}

### Instrumentation d'une application iOS 
{: #instrument_ios_app}
{: ios}

Etapes pour l'instrumentation de votre application iOS :
{: ios}

#### Etape 1 : importez et installez le logiciel SDK client de Mobile Analytics pour iOS 
{: #install_analytics_sdk_ios }
{: ios}

[![Compatible avec CocoaPods](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundation.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundation)
[![Compatible avec CocoaPods](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundationAnalytics.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundationAnalytics)
{: ios}

Le logiciel SDK Swift est disponible pour iOS et watchOS.
{: ios}

1. Ajoutez le code suivant au fichier Pod existant, qui se trouve à la racine du projet Xcode : 
   ```bash
   use_frameworks!
   platform :ios, 8.0
   target "ProjectName" do
       pod 'IBMMobileFirstPlatformFoundation'
       pod 'IBMMobileFirstPlatformFoundationAnalytics'
   end
   ```
   {: codeblock}
   {: ios}

2. Depuis une fenêtre de ligne de commande, accédez à la racine du projet Xcode et exécutez la commande suivante :
   ```bash
   pod update
   ```
   {: codeblock}
   {: ios}

#### Etape 2 : instrumentez votre application iOS en fonction du type de données d'analyse à collecter 
{: #instrument_app_based_on_data_ios }
{: ios}

1. Initialisez votre application pour permettre l'envoi de journaux à Mobile Analytics.
   Le logiciel SDK Swift est disponible pour iOS et watchOS. Importez les infrastructures `BMSCore` et `BMSAnalytics` en ajoutant les instructions `import` suivantes au début de votre fichier de projet `AppDelegate.swift` :
	```Swift
    import IBMMobileFirstPlatformFoundation
    ```
    {: codeblock}
    {: ios}

   Ensuite, utilisez le logiciel SDK client de Mobile Analytics pour configurer ou initialiser la capture des données d'analyse. Ajoutez le code d'initialisation dans un emplacement adapté à vote projet d'application.
   {: ios}

   ```Swift
    WLAnalytics.init()
   ```
   {: codeblock}
   {: ios}

   Avant d'appeler la méthode init, vous devez vous assurer que votre application imbrique le code requis pour l'authentification et l'autorisation de l'appareil dans le service MobileFoundation. Il s'agit d'une étape commune requise par toutes les applications des services Mobile Foundation ; elle n'est pas propre à la capture de données d'analyse. <!--  Refer <need to link doc that talks about auth> -->
   {: ios}

   Une fois l'initialisation terminée, votre application peut capturer des informations sur les appareils et les journaux du logiciel SDK de Mobile Analytics sans qu'il ne soit nécessaire d'ajouter du code. Les API et le code présentés dans les sections ci-après sont facultatifs ; vous pouvez les ajouter selon le type de données d'analyse à capturer.
   {: ios}

2. Pour capturer des événements de cycle de vie de l'application, comme des informations sur les sessions et les pannes de l'application, configurez votre application en ajoutant la ligne suivante :
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE)
    ```
    {: codeblock}
    {: ios}

3. Pour capturer des événements de réseau qui capturent les interactions des applications sur le réseau, configurez votre application en ajoutant la ligne suivante :
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK)
    ```
    {: codeblock}
    {: ios}

4. Vous venez d'initialiser votre application pour capturer des données d'analyse. Maintenant, vous devez envoyer les données capturées au service Mobile Analytics.
   Utilisez l'API suivante pour envoyer les données d'analyse au service Mobile Analytics : 
   ```Swift
    WLAnalytics.sharedInstance().send();
   ```
   {: codeblock}
   {: ios}

    Vous pouvez décider à quel moment appeler cette API dans votre flux d'application afin d'envoyer les données d'analyse capturées au service Mobile Analytics. En attendant, toutes les données d'analyse capturées sont stockées localement sur l'appareil.
    {: ios}

5. Pour capturer et envoyer des journaux d'application au service Mobile Analytics, utilisez les API du consignateur du logiciel SDK client de Mobile Analytics.
   L'infrastructure de journalisation du logiciel SDK client de Mobile Analytics prend en charge les niveaux de journalisation suivants, répertoriés du moins prolixe au plus prolixe, avec des recommandations d'utilisation : 
    * FATAL : pour les pannes et les blocages irrémédiables. Le niveau FATAL est réservé aux erreurs irrémédiables de journalisation, qui se matérialisent pour l'utilisateur sous la forme d'une panne de l'application.
    * ERROR : pour les exceptions inattendues ou les erreurs de protocole de réseau inattendues.
    * WARN : pour les avertissements relatifs à l'utilisation qui ne sont pas considérés comme des erreurs critiques, par exemple l'utilisation d'API dépréciées ou un temps de réponse long du réseau. 
    * INFO : pour afficher des événements d'initialisation et d'autres données qui peuvent être importants, mais non urgents.
    * DEBUG : pour afficher des informations de débogage pouvant aider les développeurs à résoudre les bogues de l'application. Lorsque le niveau de journalisation est DEBUG, vous obtenez également les journaux du logiciel SDK client de Mobile Analytics qui sont inclus lorsque vous envoyez des journaux.
   {: note}
    {: ios}

    Suivez [le tutoriel ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mobilefirstplatform.ibmcloud.com/tutorials/it/foundation/8.0/application-development/client-side-log-collection/ios/) pour découvrir des fragments de code de consignateur permettant d'ajouter des fonctions de journalisation dans les applications iOS. 

6.  Pour obtenir des informations sur les modèles d'intégration utilisateur (nouveaux utilisateurs/utilisateurs existants), vous devez associer une identité d'utilisateur à votre session d'application. Pour ce faire, vous pouvez appeler l'API suivante :
    ```Swift
      WLAnalytics.sharedInstance().setUserContext("userName or userIdentity")
    ```
    {: codeblock}
    {: ios}

    Notez que toutes les données d'analyse capturées et stockées localement sont envoyées au service Mobile Analytics uniquement lorsque l'API WLAnalytics.sharedInstance().send() est appelée.
    {: ios}

7.  Pour obtenir des informations sur vos modèles d'interaction sur le réseau HTTP des applications, comme les API HTTP appelées, le nombre de demandes et les temps de réponse moyens, vous devez utiliser la classe WLResourceRequest du logiciel SDK client de Mobile Foundation Service pour effectuer les appels HTTP. Bien que vous ayez peut-être initialisé Analytics pour la capture d'événements de réseau, si vous utilisez WLResourceRequest, Mobile Analytics peut s'introduire dans les appels de réseau et capturer les données pertinentes. Voici comment vous devez effectuer vos appels HTTP :
    ```Swift
      let request = WLResourceRequest( url: URL(string: "/adapters/JavaAdapter/users"), method: WLHttpMethodGet )

      request!.send { (response, error) -> Void in
          if(error == nil){
              //handle failure of HTTP call and use wlFailResponse
          }
          else{
              //handle success of HTTP call and use wlResponse
          }
      }
    ```
    {: codeblock}
    {: ios}

8.  Pour obtenir une analyse des pannes et les journaux, vous pouvez appeler l'API suivante au démarrage de votre application afin de vérifier si la session précédente est tombée en panne et le cas échéant, envoyer les journaux de panne capturés au service Mobile Analytics :
    ```Swift
      if (OCLogger.isUnCaughtExceptionDetected()) {
            //add any other handling you may want and call the following APIs
            WLAnalytics.sharedInstance().send();
            OCLogger.send();
      }
    ```
    {: codeblock}
    {: ios}

9.  Pour définir des analyses personnalisées et vos propres données d'analyse au-delà des éléments pris en charge intrinsèquement dans le logiciel SDK client, vous pouvez utiliser l'API de journalisation personnalisée :
```Swift
        //create an array object with key value pair as custom data
        let eventObject = ["FromPage": "LoginPage"]

        //log the custom data with a message
        WLAnalytics.sharedInstance().log("log message", withMetadata: eventObject);

        //Send the captured custom data and log to the Mobile Analytics service
        WLAnalytics.sharedInstance().send();
    ```
    {: codeblock}
    {: ios}

    The logged custom data can be plotted over custom charts you can define in the Mobile Analytics console to derive custom insights.
    {: ios}

10. Utilisez les commentaires en retour des utilisateurs de l'application pour approfondir votre analyse des performances de l'application. Vous pouvez permettre aux **utilisateurs et testeurs** de votre application d'envoyer des commentaires en retour contextuels et riches aux propriétaires d'application. Les **propriétaires d'application** obtiennent des commentaires en retour en temps réel de la part de leurs utilisateurs concernant l'expérience d'utilisation de l'application, que les **propriétaires d'application** et les **développeurs** peuvent ensuite améliorer. Ces commentaires apportent une grande souplesse pour la maintenance de l'application. Utilisez l'API ci-après afin de définir le mode de commentaires en retour interactifs pour votre application dans tout gestionnaire d'action de votre application, par exemple lors du traitement du clic d'un bouton ou de la sélection d'un élément de menu.
    ```Swift
        WLAnalytics.sharedInstance().triggerFeedbackMode();
    ```
    {: codeblock}
    {: ios}

### Instrumentation d'une application Cordova 
{: #instrument_cordova_app}
{: cordova}
Etapes pour l'instrumentation de votre application Cordova :
{: cordova}

#### Etape 1 : importez et installez le plug-in du logiciel SDK client de Foundation et d'Analytics pour Cordova 
{: #install_analytics_sdk_cordova }
Le plug-in Cordova de Mobile Analytics vous permet d'instrumenter votre application mobile.
{: cordova}

#### Installation du logiciel SDK client Cordova 
{: #install_cordova_sdk}
{: cordova}

1. Créez un projet [Cordova ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://cordova.apache.org/#getstarted){: new_window} ou ouvrez un projet existant.
    {: cordova}

2. Ajoutez les plateformes Android ou iOS de votre choix à votre application Cordova. Exécutez l'une des commandes ci-dessous ou les deux à partir de la ligne de commande. <br/>
    **Android** :
    ```
    cordova platform add android
    ```
    {: codeblock}
    {: cordova}

    **iOS** :
    ```
    cordova platform add ios
    ```
    {: codeblock}
    {: cordova}

3. Ajoutez le plug-in du logiciel SDK client de Foundation `cordova-plugin-mfp`.
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
4. Ajoutez le plug-in SDK client d'Analytics facultatif `cordova-plugin-mfp-analytics`, qui permet d'ajouter une fonction de commentaires en retour pour les utilisateurs de l'application à votre application.
    {: cordova}
    ```bash
    cordova plugin add cordova-plugin-mfp-analytics
    ```
    {: codeblock}
    {: cordova}
5. Vérifiez que l'installation du plug-in a abouti en exécutant la commande suivante :
    {: cordova}
    ```bash
    cordova plugin list
    ```
    {: codeblock}
    {: cordova}

#### Etape 2 : instrumentez votre application Cordova en fonction du type de données d'analyse à collecter 
{: #instrument_app_based_on_data_cordova }
{: cordova}

1. Dans les applications Cordova, aucune configuration n'est nécessaire et l'initialisation est intégrée.  

   Avant d'appeler l'une des méthodes d'analyse ci-dessous, vous devez vous assurer que votre application imbrique le code requis pour l'authentification et l'autorisation de l'appareil dans le service MobileFoundation. Il s'agit d'une étape commune requise par toutes les applications des services Mobile Foundation ; elle n'est pas propre à la capture de données d'analyse. <!--  Refer <need to link doc that talks about auth> -->
   {: cordova}

   Une fois l'initialisation terminée, votre application peut capturer des informations sur les appareils et les journaux du logiciel SDK de Mobile Analytics sans qu'il ne soit nécessaire d'ajouter du code. Les API et le code présentés dans les sections ci-après sont facultatifs ; vous pouvez les ajouter selon le type de données d'analyse à capturer.
   {: cordova}

2. Pour que la capture des événements de cycle de vie soit activée, elle doit être initialisée sur la plateforme native de l'application Cordova. 
    * Pour la plateforme Android : 
      * Ouvrez le fichier **[dossier_racine_application_Cordova] → platforms → android → src → com → sample → [nom_application] → MainActivity.java**. 
      * Recherchez la méthode `onCreate` et suivez les étapes ci-après pour activer les activités `LIFECYCLE` et `NETWORK` : 
        * Pour activer la journalisation des événements de cycle de vie :
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * Pour activer la journalisation des événements de réseau client :
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * Générez le projet Cordova en exécutant la commande suivante : `cordova build`.
    * Pour la plateforme iOS : 
      * Ouvrez le fichier **[dossier_racine_application_Cordova] → platforms → ios → Classes** et recherchez le fichier **AppDelegate.swift** (Swift). 
      * Suivez les étapes ci-après pour activer les activités `LIFECYCLE` et `NETWORK`. 
        * Pour activer la journalisation des événements de cycle de vie :
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * Pour activer la journalisation des événements de réseau client :
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * Générez le projet Cordova en exécutant la commande suivante : `cordova build`.

3. Vous avez initialisé votre application pour collecter des données d'analyse. A présent, vous pouvez envoyer ces données d'analyse à Mobile Analytics.
   Utilisez les API suivantes pour démarrer l'enregistrement et l'envoi des données d'analyse d'utilisation :
   ```Javascript
   // Send recorded usage analytics to the Mobile Analytics

   WL.Analytics.send();
   ```
   {: codeblock}
   {: cordova}

4. Pour capturer et envoyer des journaux d'application au service Mobile Analytics, utilisez les API du consignateur du logiciel SDK client de Mobile Analytics.
   L'infrastructure de journalisation du logiciel SDK client de Mobile Analytics prend en charge les niveaux de journalisation suivants, répertoriés du moins prolixe au plus prolixe, avec des recommandations d'utilisation : 
    * FATAL : pour les pannes et les blocages irrémédiables. Le niveau FATAL est réservé aux erreurs irrémédiables de journalisation, qui se matérialisent pour l'utilisateur sous la forme d'une panne de l'application.
    * ERROR : pour les exceptions inattendues ou les erreurs de protocole de réseau inattendues.
    * WARN : pour les avertissements relatifs à l'utilisation qui ne sont pas considérés comme des erreurs critiques, par exemple l'utilisation d'API dépréciées ou un temps de réponse long du réseau. 
    * INFO : pour afficher des événements d'initialisation et d'autres données qui peuvent être importants, mais non urgents.
    * DEBUG : pour afficher des informations de débogage pouvant aider les développeurs à résoudre les bogues de l'application. Lorsque le niveau de journalisation est DEBUG, vous obtenez également les journaux du logiciel SDK du client Mobile Analytics qui sont inclus lorsque vous envoyez des journaux.
    * TRACE : pour les points d'entrée et de sortie de méthode.
   {: note}
   {: cordova}

   Les fragments de code suivants représentent un exemple d'utilisation du consignateur pour Cordova : 
    ```Javascript
    // Set the logging level (optional)
    // The default setting is DEBUG
    WL.Logger.config({"level": "INFO"});

    //Create a logger instance.  You can create multiple loggers to organize your logs as you wish
    var logger = WL.Logger.create({ pkg: 'loggerName' });

    // Log messages with different levels
    // Debug message for feature 1
    // Info message for feature 2
    logger.debug("debug","debug message");
    //the logger.debug message is not logged because the logLevelFilter is set to Info
    logger.info("info","info message");

    // Send logs to the Mobile Analytics
    logger.send();
    ```
    {: codeblock}
    {: cordova}

5.  Pour obtenir des informations sur les modèles d'intégration utilisateur (nouveaux utilisateurs/utilisateurs existants), vous devez associer une identité d'utilisateur à votre session d'application. Aucune API spécifique n'est disponible depuis le niveau Javascript. Toutefois, vous pouvez effectuer cette action au niveau du code natif en appelant l'API suivante :

    **Android :**
      ```java
        WLAnalytics.setUserContext("userName or userIdentity");
      ```
      {: codeblock}
      {: cordova}

    **iOS :**
      ```Swift
        WLAnalytics.sharedInstance().setUserContext("userName or userIdentity")
      ```
      {: codeblock}
      {: cordova}

    Notez que toutes les données d'analyse capturées et stockées localement sont envoyées au service Mobile Analytics uniquement lorsque l'API WL.Analytics.send() au niveau javascript [ou] WLAnalytics.send() API sur un système d'exploitation android natif [ou] WLAnalytics.sharedInstance().send() sur un système d'exploitation iOS natif est appelée.
    {: cordova}

6. Pour obtenir des informations sur vos modèles d'interaction sur le réseau HTTP des applications, comme les API HTTP appelées, le nombre de demandes et les temps de réponse moyens, vous devez utiliser la classe WLResourceRequest du logiciel SDK client de Mobile Foundation Service pour effectuer les appels HTTP. Bien que vous ayez peut-être initialisé Analytics pour la capture d'événements de réseau, si vous utilisez WLResourceRequest, Mobile Analytics peut s'introduire dans les appels de réseau et capturer les données pertinentes. Voici comment vous devez effectuer vos appels HTTP :
    ```Javascript
      var resourceRequest = new WLResourceRequest("url-path", WLResourceRequest.GET);
      resourceRequest.send().then(
        onSuccess: function(response) {
            resultText = "Successfully called the resource: " + response.responseText;
        },

        onFailure: function(response) {
            resultText = "Failed to call the resource:" + response.errorMsg;
        }
      );
    ```
    {: codeblock}
    {: cordova}

7. Pour définir des analyses personnalisées et vos propres données d'analyse au-delà des éléments pris en charge intrinsèquement dans le logiciel SDK client, vous pouvez utiliser l'API de journalisation personnalisée :
    ```Javascript
        //create custom data as key value pair
        WL.Analytics.log({"FromPage" : 'LoginPage'});

        //Send the captured custom data and log to the Mobile Analytics service
        WL.Analytics.send();
    ```
    {: codeblock}
    {: cordova}

    Les données personnalisées journalisées peuvent être représentées dans des graphiques personnalisés que vous pouvez définir dans la console Mobile Analytics afin de déduire des informations personnalisées.
    {: cordova}

8. Utilisez les commentaires en retour des utilisateurs de l'application pour approfondir votre analyse des performances de l'application. Vous pouvez permettre aux **utilisateurs et testeurs** de votre application d'envoyer des commentaires en retour contextuels et riches aux propriétaires d'application. Les **propriétaires d'application** obtiennent des commentaires en retour en temps réel de la part de leurs utilisateurs concernant l'expérience d'utilisation de l'application, que les **propriétaires d'application** et les **développeurs** peuvent ensuite améliorer. Ces commentaires apportent une grande souplesse pour la maintenance de l'application. Utilisez l'API ci-après afin de définir le mode de commentaires en retour interactifs pour votre application dans tout gestionnaire d'action de votre application, par exemple lors du traitement du clic d'un bouton ou de la sélection d'un élément de menu.
    ```Javascript
        WL.Analytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: cordova}

### Instrumentation d'une application Web 
{: #instrument_web_app}
{: web}

Etapes pour l'instrumentation de votre application Web :
{: web}

#### Etape 1 : importez et installez le logiciel SDK client de Mobile Analytics pour Web 
{: #install_analytics_sdk_web }
{: web}

#### Installation du logiciel SDK client Web 
{: #install_web_sdk}
Le logiciel SDK de Mobile Analytics vous permet d'instrumenter votre application Web.
{: web}

1. Accédez au dossier racine de votre application Web et exécutez la commande ci-après. Ajoutez le logiciel SDK à votre application Web avec [npm](https://www.npmjs.com/package/ibm-mfp-web-sdk).
    ```bash
    npm install ibm-mfp-web-sdk
    ```
    {: codeblock}
    {: web}


#### Etape 2 : instrumentez votre application Web en fonction du type de données d'analyse à collecter 
{: #instrument_app_based_on_data_web }
{: web}

1. Référencez les fichiers ibmmfpf.js et ibmmfpfanalytics.js dans l'élément `head` de votre page html principale 

    ```html
      <head>
        ....
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/ibmmfpf.js"></script>
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/lib/analytics/ibmmfpfanalytics.js"></script>
      </head>
    ```
    {: codeblock}
    {: web}

2. Initialisez le logiciel SDK Web de Mobile Foundation en spécifiant les valeurs de racine contextuelle et d'ID d'application dans le fichier JavaScript principal de votre application Web 

    ```Javascript
      var wlInitOptions = {
        mfpContextRoot : '/mfp', // "mfp" is the default context root in the Mobile Foundation
        applicationId : 'com.sample.mywebapp' // Replace with your own value.
      };

      WL.Client.init(wlInitOptions).then (
        function() {
          // Application logic.
        }
      );
    ```
    {: codeblock}
    {: web}

   Avant d'appeler l'une des méthodes d'analyse ci-dessous, vous devez vous assurer que votre application imbrique le code requis pour l'authentification et l'autorisation de l'appareil dans le service MobileFoundation. Il s'agit d'une étape commune requise par toutes les applications des services Mobile Foundation ; elle n'est pas propre à la capture de données d'analyse. <!--  Refer <need to link doc that talks about auth> -->
   {: web}

   Une fois l'initialisation terminée, votre application peut capturer des informations sur les appareils et les journaux du logiciel SDK de Mobile Analytics sans qu'il ne soit nécessaire d'ajouter du code. Les API et le code présentés dans les sections ci-après sont facultatifs ; vous pouvez les ajouter selon le type de données d'analyse à capturer.
   {: web}

3. Ajoutez la ligne suivante à votre logique d'application pour capturer des événements de cycle de vie de l'application et des événements de réseau. 

    ```Javascript
    ibmmfpfanalytics.logger.config({analyticsCapture: true});
    ```
    {: codeblock}
    {: web}

4. Vous venez d'initialiser votre application pour capturer des données d'analyse. Maintenant, vous devez envoyer les données capturées au service Mobile Analytics. Utilisez l'API suivante pour envoyer les données d'analyse au service Mobile Analytics :
   

   ```Javascript
   ibmmfpfanalytics.send();
   ```
   {: codeblock}
   {: web}

    Vous pouvez décider à quel moment appeler cette API dans votre flux d'application afin d'envoyer les données d'analyse capturées au service Mobile Analytics. En attendant, toutes les données d'analyse capturées sont stockées localement sur l'appareil.
    {: web}

5. Pour capturer et envoyer des journaux d'application au service Mobile Analytics, utilisez les API du consignateur du logiciel SDK client de Mobile Analytics.
   L'infrastructure de journalisation du logiciel SDK client de Mobile Analytics prend en charge les niveaux de journalisation suivants, répertoriés du moins prolixe au plus prolixe, avec des recommandations d'utilisation : 
    * FATAL : pour les pannes et les blocages irrémédiables. Le niveau FATAL est réservé aux erreurs irrémédiables de journalisation, qui se matérialisent pour l'utilisateur sous la forme d'une panne de l'application.
    * ERROR : pour les exceptions inattendues ou les erreurs de protocole de réseau inattendues.
    * WARN : pour les avertissements relatifs à l'utilisation qui ne sont pas considérés comme des erreurs critiques, par exemple l'utilisation d'API dépréciées ou un temps de réponse long du réseau. 
    * INFO : pour afficher des événements d'initialisation et d'autres données qui peuvent être importants, mais non urgents.
    * DEBUG : pour afficher des informations de débogage pouvant aider les développeurs à résoudre les bogues de l'application. Lorsque le niveau de journalisation est DEBUG, vous obtenez également les journaux du logiciel SDK client de Mobile Analytics qui sont inclus lorsque vous envoyez des journaux.
    * TRACE : pour les points d'entrée et de sortie de méthode.
   {: note}
    {: web}

   Les fragments de code suivants présentent un exemple d'utilisation du consignateur pour une application Web : 
   ```Javascript
    //Create a logger instance.  You can create multiple loggers to organize your logs as you wish
    
    var logger = ibmmfpfanalytics.logger.pkg("loggerName");
    // Log messages with different levels
    // Debug message for feature 1
    // Info message for feature 2
    logger.debug("debug message");
    logger.info("info message");

    // Send logs to the Mobile Analytics
    ibmmfpfanalytics.send();
   ```
   {: codeblock}
   {: web}
   
   Pour le logiciel SDK Web, le niveau ne peut pas être défini par le client. Toutes les données journalisées sont envoyées au serveur jusqu'à ce que la configuration soit changée via l'extraction du profil de configuration de serveur.
   {: note}
   {: web}

6. Pour obtenir des informations sur les modèles d'intégration utilisateur (nouveaux utilisateurs/utilisateurs existants), vous devez associer une identité d'utilisateur à votre session d'application. Pour ce faire, vous pouvez appeler l'API suivante :
    ```Javascript
    ibmmfpfanalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: web}

    Notez que toutes les données d'analyse capturées et stockées localement sont envoyées au service Mobile Analytics uniquement lorsque l'API ibmmfpfanalytics.send() est appelée.
    {: web}

7.  Pour obtenir des informations sur vos modèles d'interaction sur le réseau HTTP des applications, comme les API HTTP appelées, le nombre de demandes et les temps de réponse moyens, vous devez utiliser la classe WLResourceRequest du logiciel SDK client de Mobile Foundation Service pour effectuer les appels HTTP. Bien que vous ayez peut-être initialisé Analytics pour la capture d'événements de réseau, si vous utilisez WLResourceRequest, Mobile Analytics peut s'introduire dans les appels de réseau et capturer les données pertinentes. Voici comment vous devez effectuer vos appels HTTP :
```Javascript
      var resourceRequest = new WL.ResourceRequest("url_path", WLResourceRequest.GET);
      resourceRequest.send().then(function(response) {
            console.log('handle success' + JSON.stringify(data));
        },function(error){
            console.log('handle failure: ' + error);
        });
    ```
    {: codeblock}
    {: web}

8.  Pour définir des analyses personnalisées et vos propres données d'analyse au-delà des éléments pris en charge intrinsèquement dans le logiciel SDK client, vous pouvez utiliser l'API de journalisation personnalisée :

    ```Javascript
        //custom data is sent with the addEvent method
        ibmmfpfanalytics.addEvent({'FromPage':'LoginPage'});

        //Send the captured custom data and log to the Mobile Analytics service
        ibmmfpfanalytics.send();
    ```
    {: codeblock}
    {: web}

    Les données personnalisées journalisées peuvent être représentées dans des graphiques personnalisés que vous pouvez définir dans la console Mobile Analytics afin de déduire des informations personnalisées.
    {: web}

## Etapes suivantes
{: #next_steps}

Mobile Foundation met à disposition des API REST qui permettent aux développeurs d'importer (POST) et d'exporter (GET) des données d'analyse. 

Essayez l'API REST d'analyse décrite dans la documentation Swagger depuis [cette page](https://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/). 

{: note}

A présent, vous pouvez accéder à la console Mobile Analytics pour consulter des données d'utilisation, par exemple les nouveaux appareils et le nombre total d'appareils utilisant votre application. Vous pouvez aussi surveiller votre application en créant des graphiques personnalisés, en définissant des alertes et en surveillant les pannes de l'application. 
