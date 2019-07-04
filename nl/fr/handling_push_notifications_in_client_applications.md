---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: push notifications, notifications, set up android app for notification, set up iOS app for notification, set up cordova app for notification, set up windows app for notification

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
{:windows: .ph data-hd-programlang='Windows'}


# Traitement des notifications push dans les applications client
{: #handling_push_notifications_in_client_applications}

Pour que les applications iOS, Android et Windows natives ou basées sur Cordova puissent recevoir et afficher des notifications push entrantes, vous devez d'abord configurer l'application et implémenter des API.
{: shortdesc}

Consultez les sections suivantes pour savoir comment traiter les notifications push entrantes dans les applications client.

### Traitement des notifications push dans Android
{: #handling_push_notifications_in_android}
{: android}
Pour que les applications Android puissent traiter les notifications push reçues, vous devez configurer la prise en charge des services Google Play. Une fois qu'une application a été configurée, les API de notifications fournies par {{ site.data.keyword.mobilefirst_notm }} peuvent être utilisées pour enregistrer et désenregistrer des appareils, ainsi que pour s'abonner à des étiquettes et s'en désabonner. Dans ce tutoriel, vous apprendrez comment traiter les notifications push dans les applications Android.
{: android}

#### Prérequis
{: #prereqs-andriod}
{: android}
* {{ site.data.keyword.mfserver_short_notm }} doit s'exécuter localement ou à distance.
* L'interface de ligne de commande {{ site.data.keyword.mobilefirst_notm  }} doit être installée sur le poste de travail du développeur.
{: android}

#### Configuration des notifications
{: #notifications-configuration }
{: android}

Créez un nouveau projet Android Studio ou utilisez un projet existant.  
Si le logiciel SDK Android natif {{ site.data.keyword.mobilefirst_notm }} ne figure pas encore dans le projet, suivez les instructions du tutoriel [Ajout du logiciel SDK de {{ site.data.keyword.mobilefoundation_short }} à des applications Android](https://cloud.ibm.com/docs/services/mobilefoundation/add_sdk_to_app.html#add_sdk_to_app).
{: android}

##### Configuration du projet
{: #project-setup }
{: android}

1. Dans **Android → Gradle scripts**, sélectionnez le fichier **build.gradle (Module: app)** et ajoutez les lignes suivantes à `dependencies` :
{: android}

    ```bash
    com.google.android.gms:play-services-gcm:9.0.2
    ```
    {: codeblock}
    {: android}

    Il existe un [incident Google connu](https://code.google.com/p/android/issues/detail?id=212879) qui empêche d'utiliser la dernière version des services Play (actuellement 9.2.0). Utilisez une version antérieure à 9.2.0.
    {: note}
    {: android}

    Et :
    ```xml
    compile group: 'com.ibm.mobile.foundation',
             name: 'ibmmobilefirstplatformfoundationpush',
             version: '8.0.+',
             ext: 'aar',
             transitive: true
    ```
    {: codeblock}
    {: android}

    Ou sur une seule ligne :
    ```xml
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationpush:8.0.+'
    ```
    {: codeblock}
    {: android}

1. Dans **Android → app → manifests**, ouvrez le fichier `AndroidManifest.xml`.
    * Ajoutez les droits suivants en haut de l'étiquette `manifest` :

        ```xml
        <!-- Permissions -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />

        <!-- GCM Permissions -->
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <permission
    	     android:name="your.application.package.name.permission.C2D_MESSAGE"
    	     android:protectionLevel="signature" />
        ```
        {: codeblock}
        {: android}

    * Ajoutez le code suivant à l'étiquette `application` :

        ```xml
        <!-- GCM Receiver -->
        <receiver
             android:name="com.google.android.gms.gcm.GcmReceiver"
             android:exported="true"
             android:permission="com.google.android.c2dm.permission.SEND">
             <intent-filter>
                 <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                 <category android:name="your.application.package.name" />
             </intent-filter>
        </receiver>

        <!-- MFPPush Intent Service -->
        <service
              android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushIntentService"
              android:exported="false">
              <intent-filter>
                  <action android:name="com.google.android.c2dm.intent.RECEIVE" />
              </intent-filter>
        </service>

        <!-- MFPPush Instance ID Listener Service -->
        <service
              android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushInstanceIDListenerService"
              android:exported="false">
              <intent-filter>
                  <action android:name="com.google.android.gms.iid.InstanceID" />
              </intent-filter>
        </service>

        <activity android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushNotificationHandler"
             android:theme="@android:style/Theme.NoDisplay"/>
 	    ```
      {: codeblock}
      {: android}

      Veillez à remplacer `your.application.package.name` par le nom de package réel de votre application.
      {: note}
      {: android}

    * Ajoutez l'élément `intent-filter` suivant à l'activité de l'application.
        ```xml
        <intent-filter>
            <action android:name="your.application.package.name.IBMPushNotification" />
            <category android:name="android.intent.category.DEFAULT" />
        </intent-filter>
        ```
        {: codeblock}
        {: android}

#### API de notifications
{: #notifications-api }
{: android}

##### Instance MFPPush
{: #mfppush-instance }
{: android}

Tous les appels API doivent être effectués sur une instance de `MFPPush`.  Cette opération peut être exécutée en créant une zone au niveau de la classe, telle que `private MFPPush push = MFPPush.getInstance();`, puis en appelant `push.<api-call>` dans la classe.
{: android}

Vous pouvez également appeler `MFPPush.getInstance().<api_call>` pour chaque instance dans laquelle vous devez accéder aux méthodes d'API push.
{: android}

##### Gestionnaires de demandes d'authentification
{: #challenge-handlers }
{: android}

Si la portée `push.mobileclient` est mappée à un **contrôle de sécurité**, vous devez vous assurer qu'il existe des **gestionnaires de demandes d'authentification** correspondants et qu'ils sont enregistrés avant d'utilier des API Push.
{: android}

Vous trouverez plus d'informations sur les gestionnaires de demandes d'authentification dans le tutoriel [Validation des données d'identification ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/android/).
{: note}
{: android}

##### Côté client
{: #client-side }
{: android}

| Méthodes Java | Description |
|-----------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| [`initialize(Context context);`](#initialization) | Initialise MFPPush pour le contexte fourni. |
| [`isPushSupported();`](#is-push-supported) | Permet à l'appareil de prendre en charge les notifications push. |
| [`registerDevice(JSONObject, MFPPushResponseListener);`](#register-device) | Enregistre l'appareil sur le service de notification push. |
| [`getTags(MFPPushResponseListener)`](#get-tags) | Extrait les étiquettes disponibles dans une instance de service de notification push. |
| [`subscribe(String[] tagNames, MFPPushResponseListener)`](#subscribe) | Abonne l'appareil aux étiquettes spécifiées. |
| [`getSubscriptions(MFPPushResponseListener)`](#get-subscriptions) | Extrait toutes les étiquettes auxquelles l'appareil est actuellement abonné. |
| [`unsubscribe(String[] tagNames, MFPPushResponseListener)`](#unsubscribe) | Se désabonne d'une ou plusieurs étiquettes spécifiques. |
| [`unregisterDevice(MFPPushResponseListener)`](#unregister) | Désabonne l'appareil du service de notification push. |
{: caption="Tableau 1. Méthodes Java" caption-side="top"}
{: android}

###### Initialisation
{: #initialization}
{: android}

Elle est requise pour que l'application client se connecte au service MFPPush avec le contexte d'application approprié.
{: android}

* La méthode d'API doit d'abord être appelée avant d'utiliser d'autres API MFPPush.
* Enregistre la fonction de rappel pour traiter les notifications push reçues.
{: android}

```java
MFPPush.getInstance().initialize(this);
```
{: codeblock}
{: android}

###### Prise en charge de push
{: #is-push-supported }
{: android}

Vérifie si l'appareil prend en charge les notifications push.
{: android}

```java
Boolean isSupported = MFPPush.getInstance().isPushSupported();

if (isSupported ) {
    // Push est pris en charge
} else {
    // Push n'est pas pris en charge
}
```
{: codeblock}
{: android}

###### Enregistrement d'un appareil
{: #register-device }
{: android}

Enregistre l'appareil sur le service de notification push.
{: android}

```java
MFPPush.getInstance().registerDevice(null, new MFPPushResponseListener<String>() {
    @Override
    public void onSuccess(String s) {
        // Enregistrement réussi
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Echec de l'enregistrement avec une erreur
    }
});
```
{: codeblock}
{: android}

###### Obtention d'étiquettes
{: #get-tags }
{: android}

Extrait toutes les étiquettes disponibles à partir du service de notification push.
{: android}

```java
MFPPush.getInstance().getTags(new MFPPushResponseListener<List<String>>() {
    @Override
    public void onSuccess(List<String> strings) {
        // Etiquettes extraites sous forme de liste de chaînes
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Echec de la réception d'étiquettes avec une erreur
    }
});
```
{: codeblock}
{: android}

###### Abonnement
{: #subscribe }
{: android}

S'abonne aux étiquettes souhaitées.
{: android}

```java
String[] tags = {"Tag 1", "Tag 2"};

MFPPush.getInstance().subscribe(tags, new MFPPushResponseListener<String[]>() {
    @Override
    public void onSuccess(String[] strings) {
        // Abonnement réussi
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Echec de l'abonnement
    }
});
```
{: codeblock}
{: android}

###### Obtention d'abonnements
{: #get-subscriptions }
{: android}

Extrait les étiquettes auxquelles l'appareil est actuellement abonné.
{: android}

```java
MFPPush.getInstance().getSubscriptions(new MFPPushResponseListener<List<String>>() {
    @Override
    public void onSuccess(List<String> strings) {
        // Abonnements reçus sous forme de liste de chaînes
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Echec de l'extraction d'abonnements avec une erreur
    }
});
```
{: codeblock}
{: android}

###### Désabonnement
{: #unsubscribe }
{: android}

Se désabonne d'étiquettes.
{: android}

```java
String[] tags = {"Tag 1", "Tag 2"};

MFPPush.getInstance().unsubscribe(tags, new MFPPushResponseListener<String[]>() {
    @Override
    public void onSuccess(String[] strings) {
        // Désabonnement réussi
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Echec du désabonnement
    }
});
```
{: codeblock}
{: android}

###### Désabonnement
{: #unregister }
{: android}

Désabonne l'appareil de l'instance de service de notification push.
{: android}

```java
MFPPush.getInstance().unregisterDevice(new MFPPushResponseListener<String>() {
    @Override
    public void onSuccess(String s) {
        disableButtons();
        // Désabonnement réussi
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Echec du désabonnement
    }
});
```
{: codeblock}
{: android}

#### Traitement d'une notification push
{: #handling-a-push-notification }
{: android}

Pour traiter une notification push, vous devez configurer un programme d'écoute `MFPPushNotificationListener`.  Pour ce faire, vous pouvez implémenter l'une des méthodes suivantes.
{: android}

##### Option 1
{: #option-one }
{: android}

Dans l'activité dans laquelle vous souhaitez traiter les notifications push.
{: android}

1. Ajoutez `implements MFPPushNofiticationListener` à la déclaration de classe.
2. Définissez la classe pour qu'elle devienne le programme d'écoute en appelant `MFPPush.getInstance().listen(this)` dans la méthode `onCreate`.
2. Vous devrez ensuite ajouter la méthode *obligatoire* suivante :
    ```java
    @Override
    public void onReceive(MFPSimplePushNotification mfpSimplePushNotification) {
         // Traitement de la notification push ici
    }
    ```
    {: codeblock}
    {: android}

3. Dans cette méthode, vous allez recevoir la notification `MFPSimplePushNotification` et pourrez la traiter pour obtenir le comportement souhaité.
{: android}

##### Option 2
{: #option-two }
{: android}

Créez un programme d'écoute en appelant `listen(new MFPPushNofiticationListener())` sur une instance de `MFPPush` comme illustré dans l'exemple suivant. 
```java
MFPPush.getInstance().listen(new MFPPushNotificationListener() {
    @Override
    public void onReceive(MFPSimplePushNotification mfpSimplePushNotification) {
        // Traitement de la notification push ici
    }
});
```
{: codeblock}
{: android}

#### Migration de vos applications client sur Android vers FCM
{: #migrate-to-fcm }
{: android}

Google Cloud Messaging (GCM) est désormais [obsolète](https://developers.google.com/cloud-messaging/faq) et est intégré à Firebase Cloud Messaging (FCM). Google désactivera la plupart des services GCM en avril 2019.
{: android}

Si vous utilisez un projet GCM, [migrez les applications client GCM sur Android vers FCM](https://developers.google.com/cloud-messaging/android/android-migrate-fcm).
{: android}

Pour l'instant, les applications existantes qui utilisent des services GCM continueront à fonctionner en l'état. Etant donné que le service de notification push a été mis à jour pour utiliser les noeuds finaux FCM, toutes les nouvelles applications à venir doivent utiliser FCM.
{: android}

Après la migration vers FCM, mettez à jour votre projet pour qu'il utilise des données d'identification FCM au lieu des anciennes données d'identification GCM.
{: note}
{: android}

##### Configuration d'un projet FCM
{: #fcm_project_setup}
{: android}

La configuration d'une application dans FCM est un peu différente par rapport à l'ancien modèle GCM.
{: android}

1. Obtenez les données d'identification de votre fournisseur de notification, créez un projet FCM et ajoutez-le à votre application Android. Incluez le nom de package de votre application sous la forme `com.ibm.mobilefirstplatform.clientsdk.android.push`. Consultez la [documentation ici](https://cloud.ibm.com/docs/services/mobilepush/push_step_1.html#push_step_1_android), jusqu'à l'étape où vous avez fini de générer le fichier `google-services.json`
   {: android}
2. Configurez votre fichier Gradle. Ajoutez le code suivant dans le fichier `build.gradle` de l'application.
    ```xml
    dependencies {
       ......
       compile 'com.google.firebase:firebase-messaging:10.2.6'
       .....
    }

    apply plugin: 'com.google.gms.google-services'
    ```
    {: codeblock}
    {: android}

    - Ajoutez la dépendance suivante dans la section `buildscript` de la racine build.gradle
      `classpath 'com.google.gms:google-services:3.0.0'`
      {: codeblock}
      {: android}

    - Supprimez ensuite le plug-in GCM du fichier build.gradle `compile  com.google.android.gms:play-services-gcm:+`
     {: android}
 3. Configurez le fichier AndroidManifest. Les modifications suivantes sont requises dans `AndroidManifest.xml`
    {: android}

    Supprimez les entrées suivantes :
    {: android}
    ```xml
        <receiver android:exported="true" android:name="com.google.android.gms.gcm.GcmReceiver" android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="your.application.package.name" />
            </intent-filter>
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <category android:name="your.application.package.name" />
            </intent-filter>
        </receiver>  

        <service android:exported="false" android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushInstanceIDListenerService">
            <intent-filter>
                <action android:name="com.google.android.gms.iid.InstanceID" />
            </intent-filter>
        </service>

        <uses-permission android:name="your.application.package.name.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    ```
    {: codeblock}
    {: android}

    Les entrées suivantes requièrent des modifications :
    {: android}

    ```xml
        <service android:exported="true" android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushIntentService">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            </intent-filter>
        </service>
    ```
    {: codeblock}
    {: android}

    Modifiez les entrées en :
    {: android}

    ```xml
        <service android:exported="true" android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushIntentService">
            <intent-filter>
                <action android:name="com.google.firebase.MESSAGING_EVENT" />
            </intent-filter>
        </service>
    ```
    {: codeblock}
    {: android}

    Ajoutez l'entrée suivante :
    {: android}

    ```xml
        <service android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPush"
                android:exported="true">
                <intent-filter>
                    <action android:name="com.google.firebase.INSTANCE_ID_EVENT" />
                </intent-filter>
        </service>
    ```
    {: codeblock}
    {: android}

 4. Ouvrez l'application dans Android Studio. Copiez le fichier `google-services.json` que vous avez créé à l'**Etape 1** dans le répertoire de l'application. Notez que le fichier `google-service.json` inclut le nom de package que vous avez ajouté.		
 5. Compilez le logiciel SDK. Générez l'application.
{: android}

### Traitement des notifications push dans iOS
{: #handling_push_notifications_in_ios }
{: ios}

Les API de notifications fournies par {{ site.data.keyword.mobilefirst_notm }} peuvent être utilisées pour enregistrer et désenregistrer des appareils, ainsi que
pour s'abonner à des étiquettes et s'en désabonner. Dans ce tutoriel, vous apprendrez comment traiter les notifications push dans les applications iOS à l'aide de Swift.
{: shortdesc}
{: ios}

Pour plus d'informations sur les notifications silencieuses ou interactives, voir :

* [Notifications silencieuses](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [Notifications interactives](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
{: ios}

#### Prérequis
{: #prereqs-ios}
{: ios}

* {{ site.data.keyword.mfserver_short }} doit s'exécuter localement ou à distance.
* {{ site.data.keyword.mfp_cli_long_notm }} doit être installé sur le poste de travail du développeur.
{: ios}

#### Configuration des notifications
{: #notifications-configuration_ios}
{: ios}

Créez un nouveau projet Xcode ou utilisez un projet existant.
Si le logiciel SDK iOS natif {{ site.data.keyword.mobilefirst_notm }} ne figure pas encore dans le projet, suivez les instructions du tutoriel [Ajout du logiciel SDK de {{ site.data.keyword.mobilefoundation_short }}
à des applications iOS](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/).
{: ios}

#### Ajout du logiciel SDK Push
{: #adding-the-push-sdk }
{: ios}

1. Ouvrez le **fichier Pod** existant du projet et ajoutez les lignes suivantes :
    ```xml
    use_frameworks!

    platform :ios, 8.0
    target "Xcode-project-target" do
         pod 'IBMMobileFirstPlatformFoundation'
         pod 'IBMMobileFirstPlatformFoundationPush'
    end

    post_install do |installer|
         workDir = Dir.pwd

         installer.pods_project.targets.each do |target|
             debugXcconfigFilename = "#{workDir}/Pods/Target Support Files/#{target}/#{target}.debug.xcconfig"
             xcconfig = File.read(debugXcconfigFilename)
             newXcconfig = xcconfig.gsub(/HEADER_SEARCH_PATHS = .*/, "HEADER_SEARCH_PATHS = ")
             File.open(debugXcconfigFilename, "w") { |file| file << newXcconfig }

             releaseXcconfigFilename = "#{workDir}/Pods/Target Support Files/#{target}/#{target}.release.xcconfig"
             xcconfig = File.read(releaseXcconfigFilename)
             newXcconfig = xcconfig.gsub(/HEADER_SEARCH_PATHS = .*/, "HEADER_SEARCH_PATHS = ")
             File.open(releaseXcconfigFilename, "w") { |file| file << newXcconfig }
         end
    end
    ```
    {: codeblock}
    {: ios}

    - Remplacez **Xcode-project-target** par le nom de la cible de votre projet Xcode.
2. Sauvegardez et fermez le **fichier Pod**.
3. A partir d'une fenêtre de **ligne de commande**, accédez au dossier racine du projet.
4. Exécutez la commande `pod install`
5. Ouvrez le projet à l'aide du fichier **.xcworkspace**.
{: ios}

#### API de notifications
{: #notifications-api-ios}
{: ios}

##### Instance MFPPush
{: #mfppush-instance-ios}
{: ios}

Tous les appels API doivent être effectués sur une instance de `MFPPush`. Cette opération peut être exécutée à l'aide d'un code `var` dans un contrôleur d'affichage comme `var push = MFPPush.sharedInstance();`, puis en appelant `push.methodName()` via le contrôleur d'affichage.
{: ios}

Vous pouvez également appeler `MFPPush.sharedInstance().methodName()` pour chaque instance dans laquelle vous devez accéder aux méthodes d'API Push.
{: ios}

#### Gestionnaires de demandes d'authentification
{: #challenge-handlers-ios}
{: ios}

Si la portée `push.mobileclient` est mappée à un **contrôle de sécurité**, vous devez vous assurer qu'il existe des **gestionnaires de demandes d'authentification** correspondants et qu'ils sont enregistrés avant d'utilier des API Push.
{: ios}

Vous trouverez plus d'informations sur les gestionnaires de demandes d'authentification dans le tutoriel
[Validation des données
d'identification ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/ios/).
{: note}
{: ios}

#### Côté client
{: #client-side-ios}
{: ios}

| Méthodes Swift | Description  |
|---------------|--------------|
| [`initialize()`](#initialization) | Initialise MFPPush pour le contexte fourni. |
| [`isPushSupported()`](#is-push-supported) | Permet à l'appareil de prendre en charge les notifications push. |
| [`registerDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#register-device--send-device-token) | Enregistre l'appareil sur le service de notification push.|
| [`sendDeviceToken(deviceToken: NSData!)`](#register-device--send-device-token) | Envoie le jeton d'appareil au serveur. |
| [`getTags(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-tags) | Extrait les étiquettes disponibles dans une instance de service de notification push. |
| [`subscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#subscribe) | Abonne l'appareil aux étiquettes spécifiées. |
| [`getSubscriptions(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-subscriptions)  | Extrait toutes les étiquettes auxquelles l'appareil est actuellement abonné. |
| [`unsubscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unsubscribe) | Se désabonne d'une ou plusieurs étiquettes spécifiques. |
| [`unregisterDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unregister) | Désabonne l'appareil du service de notification push.              |
{: caption="Tableau 2. Méthodes Swift" caption-side="top"}
{: ios}

##### Initialisation
{: #initialization-ios}
{: ios}

L'initialisation est requise pour que l'application client se connecte au service MFPPush.
{: ios}

* La méthode `initialize` doit d'abord être appelée avant d'utiliser d'autres API MFPPush.
* Elle enregistre la fonction de rappel pour traiter les notifications push reçues.
{: ios}

```swift
MFPPush.sharedInstance().initialize();
```
{: codeblock}
{: ios}

##### Prise en charge de push
{: #is-push-supported-ios}
{: ios}

Vérifie si l'appareil prend en charge les notifications push.
{: ios}

```swift
let isPushSupported: Bool = MFPPush.sharedInstance().isPushSupported()

if isPushSupported {
    // Push est pris en charge
} else {
    // Push n'est pas pris en charge
}
```
{: codeblock}
{: ios}

##### Enregistrement d'appareil et envoi de jeton d'appareil
{: #register-device--send-device-token-ios}
{: ios}

Enregistre l'appareil sur le service de notification push.
{: ios}

```swift
MFPPush.sharedInstance().registerDevice(nil) { (response, error) -> Void in
    if error == nil {
        self.enableButtons()
        self.showAlert("Registered successfully")
        print(response?.description ?? "")
    } else {
        self.showAlert("Registrations failed.  Error \(error?.localizedDescription)")
        print(error?.localizedDescription ?? "")
    }
}
```
{: codeblock}
{: ios}

<!--`options` = `[NSObject : AnyObject]` which is an optional parameter that is a dictionary of options to be passed with your register request, sends the device token to the server to register the device with its unique identifier.-->

```swift
MFPPush.sharedInstance().sendDeviceToken(deviceToken)
```
{: codeblock}
{: ios}

Ce code est généralement appelé dans **AppDelegate** dans la méthode `didRegisterForRemoteNotificationsWithDeviceToken`.
{: note}
{: ios}

##### Obtention d'étiquettes
{: #get-tags-ios}
{: ios}

Extrait toutes les étiquettes disponibles à partir du service de notification push.
{: ios}

```swift
MFPPush.sharedInstance().getTags { (response, error) -> Void in
    if error == nil {
        print("The response is: \(response)")
        print("The response text is \(response?.responseText)")
        if response?.availableTags().isEmpty == true {
            self.tagsArray = []
            self.showAlert("There are no available tags")
        } else {
            self.tagsArray = response!.availableTags() as! [String]
            self.showAlert(String(describing: self.tagsArray))
            print("Tags response: \(response)")
        }
    } else {
        self.showAlert("Error \(error?.localizedDescription)")
        print("Error \(error?.localizedDescription)")
    }
}
```
{: codeblock}
{: ios}

##### Abonnement
{: #subscribe-ios}
{: ios}

S'abonne aux étiquettes souhaitées.
{: ios}

```swift
var tagsArray: [String] = ["Tag 1", "Tag 2"]

MFPPush.sharedInstance().subscribe(self.tagsArray) { (response, error)  -> Void in
    if error == nil {
        self.showAlert("Subscribed successfully")
        print("Subscribed successfully response: \(response)")
    } else {
        self.showAlert("Failed to subscribe")
        print("Error \(error?.localizedDescription)")
    }
}
```
{: codeblock}
{: ios}

##### Obtention d'abonnements
{: #get-subscriptions-ios}
{: ios}

Extrait les étiquettes auxquelles l'appareil est actuellement abonné.
{: ios}

```swift
MFPPush.sharedInstance().getSubscriptions { (response, error) -> Void in
   if error == nil {
       var tags = [String]()
       let json = (response?.responseJSON)! as [AnyHashable: Any]
       let subscriptions = json["subscriptions"] as? [[String: AnyObject]]
       for tag in subscriptions! {
           if let tagName = tag["tagName"] as? String {
               print("tagName: \(tagName)")
               tags.append(tagName)
           }
       }
       self.showAlert(String(describing: tags))
   } else {
       self.showAlert("Error \(error?.localizedDescription)")
       print("Error \(error?.localizedDescription)")
   }
}
```
{: codeblock}
{: ios}

##### Désabonnement
{: #unsubscribe-ios}
{: ios}

Se désabonne d'étiquettes.
{: ios}

```swift
var tags: [String] = {"Tag 1", "Tag 2"};

// Désabonnement d'étiquettes
MFPPush.sharedInstance().unsubscribe(self.tagsArray) { (response, error)  -> Void in
    if error == nil {
        self.showAlert("Unsubscribed successfully")
        print(String(describing: response?.description))
    } else {
        self.showAlert("Error \(error?.localizedDescription)")
        print("Error \(error?.localizedDescription)")
    }
}
```
{: codeblock}
{: ios}

##### Désabonnement
{: #unregister-ios}
{: ios}

Désabonne l'appareil de l'instance de service de notification push.
{: ios}

```swift
MFPPush.sharedInstance().unregisterDevice { (response, error)  -> Void in
   if error == nil {
       // Disable buttons
       self.disableButtons()
       self.showAlert("Unregistered successfully")
       print("Subscribed successfully response: \(response)")
   } else {
       self.showAlert("Error \(error?.localizedDescription)")
       print("Error \(error?.localizedDescription)")
   }
}
```
{: codeblock}
{: ios}

#### Traitement d'une notification push
{: #handling-a-push-notification-ios}
{: ios}

Les notifications push sont traitées directement par l'infrastructure iOS native. En fonction du cycle de vie de votre application, différentes méthodes seront appelées par l'infrastructure iOS.
{: ios}

Par exemple, si une notification simple est reçue pendant l'exécution de l'application, le code `didReceiveRemoteNotification`
de **AppDelegate** sera déclenché :
{: ios}

```swift
func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable: Any]) {
    print("Received Notification in didReceiveRemoteNotification \(userInfo)")
    // affichage du corps de l'alerte
      if let notification = userInfo["aps"] as? NSDictionary,
        let alert = notification["alert"] as? NSDictionary,
        let body = alert["body"] as? String {
          showAlert(body)
        }
}
```
{: codeblock}
{: ios}

Vous trouverez plus d'informations sur le traitement des notifications dans iOS dans la [documentation Apple ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1).
{: note}
{: ios}

### Traitement des notifications push dans Cordova
{: #handling_push_notifications_in_cordova }
{: cordova}

Pour que les applications iOS, Android et Windows puissent recevoir et afficher des notifications push, le plug-in Cordova **cordova-plugin-mfp-push** doit être ajouté au projet Cordova. Une fois qu'une application a été configurée, les API de notifications fournies par {{ site.data.keyword.mobilefirst_notm }} peuvent être utilisées pour enregistrer et désenregistrer des appareils, pour s'abonner à des étiquettes et s'en désabonner, ainsi que pour traiter des notifications. Dans ce tutoriel, vous apprendrez comment traiter les notifications push dans les applications Cordova.
{: shortdesc}
{: cordova}

Les notifications authentifiées **ne sont pas prises en charge** actuellement dans les applications Cordova en raison d'un incident. Toutefois, une solution palliative est fournie : chaque appel API `MFPPush` peut être encapsulé par `WLAuthorizationManager.obtainAccessToken("push.mobileclient").then( ... );`.
{: note}
{: cordova}

Pour plus d'informations sur les notifications silencieuses ou interactives dans iOS, voir :
{: cordova}

* [Notifications silencieuses](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [Notifications interactives](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
{: cordova}

#### Prérequis
{: #prereqs-cordova}
{: cordova}

* {{ site.data.keyword.mfserver_short }} doit s'exécuter localement ou à distance.
* {{ site.data.keyword.mfp_cli_long_notm }} doit être installé sur le poste de travail du développeur.
* L'interface de ligne de commande Cordova doit être installée sur le poste de travail du développeur.
{: cordova}

#### Configuration des notifications
{: #notifications-configuration-cordova}
{: cordova}

Créez un nouveau projet Cordova ou utilisez un projet existant, puis ajoutez une ou plusieurs des plateformes prises en charge : iOS, Android, Windows.
{: cordova}

Si le logiciel SDK Cordova {{ site.data.keyword.mobilefirst_notm }} ne figure pas encore dans le projet, suivez les instructions du tutoriel [Ajout du logiciel SDK de {{ site.data.keyword.mobilefirst_notm }}
à des applications Cordova](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/).
{: cordova}
{: note}

#### Ajout du plug-in Push
{: #adding-the-push-plug-in-cordova}
{: cordova}

1. A partir d'une fenêtre de **ligne de commande**, accédez à la racine du projet Cordova.  
2. Ajoutez le plug-in Push en exécutant la commande suivante :
    ```bash
    cordova plugin add cordova-plugin-mfp-push
    ```
    {: codeblock}

3. Générez le projet Cordova en exécutant la commande suivante :
    ```bash
    cordova build
    ```
    {: codeblock}
{: cordova}

#### Plateforme iOS
{: #ios-platform }
{: cordova}

La plateforme iOS nécessite une étape supplémentaire.  
{: cordova}

Dans Xcode, activez les notifications push pour votre application dans l'écran **Capabilities**.
{: cordova}

L'ID de bundle sélectionné pour l'application doit correspondre à l'ID d'application que vous avez créé précédemment sur le site Apple Developer.
{: important}
{: cordova}

![image de l'emplacement de la fonction dans Xcode](images/push-capability.png)
{: cordova}

#### Plateforme Android
{: #android-platform }
{: cordova}

La plateforme Android nécessite une étape supplémentaire.  
{: cordova}

Dans Android Studio, ajoutez l'`activité` suivante à l'étiquette `application` :
```xml
<activity android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushNotificationHandler" android:theme="@android:style/Theme.NoDisplay"/>
```
{: codeblock}
{: cordova}

#### API de notifications
{: #notifications-api-cordova}
{: cordova}

##### Côté client
{: #client-side-cordova}
{: cordova}

| Fonction Javascript | Description |
| --- | --- |
| [`MFPPush.initialize(success, failure)`](#initialization-cordova) | Initialise l'instance MFPPush. |
| [`MFPPush.isPushSupported(success, failure)`](#is-push-supported-cordova) | Permet à l'appareil de prendre en charge les notifications push. |
| [`MFPPush.registerDevice(options, success, failure)`](#register-device-cordova) | Enregistre l'appareil sur le service de notification push. |
| [`MFPPush.getTags(success, failure)`](#get-tags-cordova) | Extrait toutes les étiquettes disponibles dans une instance de service de notification push. |
| [`MFPPush.subscribe(tag, success, failure)`](#subscribe-cordova) | S'abonne à une étiquette spécifique. |
| [`MFPPush.getSubsciptions(success, failure)`](#get-subscriptions-cordova) | Extrait les étiquettes auxquelles l'appareil est actuellement abonné. |
| [`MFPPush.unsubscribe(tag, success, failure)`](#unsubscribe-cordova) | Se désabonne d'une étiquette spécifique. |
| [`MFPPush.unregisterDevice(success, failure)`](#unregister-cordova) | Désabonne l'appareil du service de notification push. |
{: caption="Tableau 3. Fonctions Javascript" caption-side="top"}
{: cordova}

##### Implémentation d'API
{: #api-implementation }
{: cordova}

###### Initialisation
{: #initialization-cordova}
{: cordova}

Initialise l'instance **MFPPush**.
{: cordova}

- Elle est requise pour que l'application client se connecte au service MFPPush avec le contexte d'application approprié.  
- La méthode d'API doit d'abord être appelée avant d'utiliser d'autres API MFPPush.
- Enregistre la fonction de rappel pour traiter les notifications push reçues.
{: cordova}

```javascript
MFPPush.initialize (
    function(successResponse) {
        alert("Successfully intialized");
        MFPPush.registerNotificationsCallback(notificationReceived);
    },
    function(failureResponse) {
        alert("Failed to initialize");
    }
);
```
{: codeblock}
{: cordova}

###### Prise en charge de push
{: #is-push-supported-cordova}
{: cordova}

Vérifie si l'appareil prend en charge les notifications push.
{: cordova}

```javascript
MFPPush.isPushSupported (
    function(successResponse) {
        alert("Push Supported: " + successResponse);
    },
    function(failureResponse) {
        alert("Failed to get push support status");
    }
);
```
{: codeblock}
{: cordova}

###### Enregistrement d'un appareil
{: #register-device-cordova}
{: cordova}

Enregistre l'appareil sur le service de notification push. Si aucune option n'est requise, vous pouvez affecter la valeur `null` aux options.
{: cordova}

```javascript
var options = { };
MFPPush.registerDevice(
    options,
    function(successResponse) {
        alert("Successfully registered");
    },
    function(failureResponse) {
        alert("Failed to register");
    }
);
```
{: codeblock}
{: cordova}

###### Obtention d'étiquettes
{: #get-tags-cordova}
{: cordova}

Extrait toutes les étiquettes disponibles à partir du service de notification push.
{: cordova}

```javascript
MFPPush.getTags (
    function(tags) {
        alert(JSON.stringify(tags));
},
    function() {
        alert("Failed to get tags");
    }
);
```
{: codeblock}
{: cordova}

###### Abonnement
{: #subscribe-cordova}
{: cordova}

S'abonne aux étiquettes souhaitées.
{: cordova}

```javascript
var tags = ['sample-tag1','sample-tag2'];

MFPPush.subscribe(
    tags,
    function(tags) {
        alert("Subscribed successfully");
    },
    function() {
        alert("Failed to subscribe");
    }
);
```
{: codeblock}
{: cordova}

###### Obtention d'abonnements
{: #get-subscriptions-cordova}
{: cordova}

Extrait les étiquettes auxquelles l'appareil est actuellement abonné.
{: cordova}

```javascript
MFPPush.getSubscriptions (
    function(subscriptions) {
        alert(JSON.stringify(subscriptions));
    },
    function() {
        alert("Failed to get subscriptions");
    }
);
```
{: codeblock}
{: cordova}

###### Désabonnement
{: #unsubscribe-cordova}
{: cordova}

Se désabonne d'étiquettes.
{: cordova}

```javascript
var tags = ['sample-tag1','sample-tag2'];

MFPPush.unsubscribe(
    tags,
    function(tags) {
        alert("Unsubscribed successfully");
    },
    function() {
        alert("Failed to unsubscribe");
    }
);
```
{: codeblock}
{: cordova}

###### Désabonnement
{: #unregister-cordova}
{: cordova}

Désabonne l'appareil de l'instance de service de notification push.
{: cordova}

```javascript
MFPPush.unregisterDevice(
    function(successResponse) {
        alert("Unregistered successfully");
    },
    function() {
        alert("Failed to unregister");
    }
);
```
{: codeblock}
{: cordova}

#### Traitement d'une notification push
{: #handling-a-push-notification-cordova}
{: cordova}

Vous pouvez traiter une notification push reçue en agissant sur son objet réponse dans la fonction de rappel enregistrée.
{: cordova}

```javascript
var notificationReceived = function(message) {
    alert(JSON.stringify(message));
};
```
{: codeblock}
{: cordova}

### Traitement des notifications push dans Windows 8.1 Universal et Windows 10 UWP
{: #handling_push_notifications_in_windows }
{: windows}

Les API de notifications fournies par {{ site.data.keyword.mobilefirst_notm }} peuvent être utilisées pour enregistrer et désenregistrer des appareils, ainsi que
pour s'abonner à des étiquettes et s'en désabonner. Dans ce tutoriel, vous apprendrez comment traiter les notifications push dans les applications natives Windows 8.1 Universal et Windows 10 UWP à l'aide de C#.
{: windows}

#### Prérequis
{: #prereqs-windows}
{: windows}

* {{ site.data.keyword.mfserver_short_notm }} doit s'exécuter localement ou à distance.
* L'interface de ligne de commande {{ site.data.keyword.mobilefirst_notm  }} doit être installée sur le poste de travail du développeur.
{: windows}

#### Configuration des notifications
{: #notifications-configuration-windows}
{: windows}

Créez un nouveau projet Visual Studio ou utilisez un projet existant.  
{: windows}

Si le logiciel SDK Windows natif {{ site.data.keyword.mobilefirst_notm }} ne figure pas encore dans le projet, suivez les instructions du tutoriel [Ajout du logiciel SDK de {{ site.data.keyword.mobilefirst_notm }}
à des applications Windows](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/windows-8-10/).
{: windows}

#### Ajout du logiciel SDK Push
{: #adding-the-push-sdk-windows}
{: windows}

1. Sélectionnez Tools → NuGet Package Manager → Package Manager Console.
2. Choisissez le projet dans lequel vous souhaitez installer le composant {{ site.data.keyword.mobilefirst_notm }} Push.
3. Ajoutez le logiciel SDK {{ site.data.keyword.mobilefirst_notm }} Push en exécutant la commande **Install-Package IBM.MobileFirstPlatformFoundationPush**.
{: windows}

#### Configuration WNS prérequise
{: pre-requisite-wns-configuration }
{: windows}

1. Vérifiez que l'application est dotée de la fonction de notification Toast. Elle peut être activée dans Package.appxmanifest.
2. Vérifiez que `Package Identity Name` et `Publisher` sont mis à jour avec les valeurs enregistrées dans WNS.
3. (Facultatif) Supprimez le fichier TemporaryKey.pfx.
{: windows}

#### API de notifications
{: #notifications-api-windows}
{: windows}

##### Instance MFPPush
{: #mfppush-instance-windows}
{: windows}

Tous les appels API doivent être effectués sur une instance de `MFPPush`.  Cette opération peut être exécutée en créant une variable telle que `private MFPPush PushClient = MFPPush.GetInstance();`, puis en appelant `PushClient.methodName()` dans la classe.
{: windows}

Vous pouvez également appeler `MFPPush.GetInstance().methodName()` pour chaque instance dans laquelle vous devez accéder aux méthodes d'API Push.
{: windows}

##### Gestionnaires de demandes d'authentification
{: #challenge-handlers-windows}
{: windows}

Si la portée `push.mobileclient` est mappée à un **contrôle de sécurité**, vous devez vous assurer qu'il existe des **gestionnaires de demandes d'authentification** correspondants et qu'ils sont enregistrés avant d'utilier des API Push.
{: windows}

Vous trouverez plus d'informations sur les gestionnaires de demandes d'authentification dans le tutoriel
[Validation des données
d'identification ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/windows-8-10/).
{: note}
{: windows}

#### Côté client
{: #client-side-windows}
{: windows}

| Méthodes C Sharp                                                                                                | Description                                                             |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| [`Initialize()`](#initialization-windows)                                                                            | Initialise MFPPush pour le contexte fourni.                               |
| [`IsPushSupported()`](#is-push-supported-windows)                                                                    | Permet à l'appareil de prendre en charge les notifications push.                             |
| [`RegisterDevice(JObject options)`](#register-device--send-device-token-windows)                  | Enregistre l'appareil sur le service de notification push.               |
| [`GetTags()`](#get-tags-windows)                                | Extrait les étiquettes disponibles dans une instance de service de notification push. |
| [`Subscribe(String[] Tags)`](#subscribe-windows)     | Abonne l'appareil aux étiquettes spécifiées.                          |
| [`GetSubscriptions()`](#get-subscriptions-windows)              | Extrait toutes les étiquettes auxquelles l'appareil est actuellement abonné.               |
| [`Unsubscribe(String[] Tags)`](#unsubscribe-windows) | Se désabonne d'une ou plusieurs étiquettes spécifiques.                                  |
| [`UnregisterDevice()`](#unregister-windows)                     | Désabonne l'appareil du service de notification push.              |
{: caption="Tableau 4. Méthodes C Sharp" caption-side="top"}
{: windows}

##### Initialisation
{: #initialization-windows}
{: windows}

L'initialisation est requise pour que l'application client se connecte au service MFPPush.
{: windows}

* La méthode `Initialize` doit d'abord être appelée avant d'utiliser d'autres API MFPPush.
* Elle enregistre la fonction de rappel pour traiter les notifications push reçues.
{: windows}

```csharp
MFPPush.GetInstance().Initialize();
```
{: codeblock}
{: windows}

##### Prise en charge de push
{: #is-push-supported-windows}
{: windows}

Vérifie si l'appareil prend en charge les notifications push.
{: windows}

```csharp
Boolean isSupported = MFPPush.GetInstance().IsPushSupported();

if (isSupported ) {
    // Push est pris en charge
} else {
    // Push n'est pas pris en charge
}
```
{: codeblock}
{: windows}

##### Enregistrement d'appareil et envoi de jeton d'appareil
{: #register-device--send-device-token-windows}
{: windows}

Enregistre l'appareil sur le service de notification push.
{: windows}

```csharp
JObject Options = new JObject();
MFPPushMessageResponse Response = await MFPPush.GetInstance().RegisterDevice(Options);         
if (Response.Success == true)
{
    // Enregistrement réussi
} else {
    // Echec de l'enregistrement avec une erreur
}
```
{: codeblock}
{: windows}

##### Obtention d'étiquettes
{: #get-tags-windows}
{: windows}

Extrait toutes les étiquettes disponibles à partir du service de notification push.
{: windows}

```csharp
MFPPushMessageResponse Response = await MFPPush.GetInstance().GetTags();
if (Response.Success == true)
{
    Message = new MessageDialog("Avalibale Tags: " + Response.ResponseJSON["tagNames"]);
} else{
    Message = new MessageDialog("Failed to get Tags list");
}
```
{: codeblock}
{: windows}

##### Abonnement
{: #subscribe-windows}
{: windows}

S'abonne aux étiquettes souhaitées.
{: windows}

```csharp
string[] Tags = ["Tag1" , "Tag2"];

// Get subscription tag
MFPPushMessageResponse Response = await MFPPush.GetInstance().Subscribe(Tags);
if (Response.Success == true)
{
    //abonnement réussi à l'étiquette push
}
else
{
    //échec de l'abonnement aux étiquettes push
}
```
{: codeblock}
{: windows}

##### Obtention d'abonnements
{: #get-subscriptions-windows}
{: windows}

Extrait les étiquettes auxquelles l'appareil est actuellement abonné.
{: windows}

```csharp
MFPPushMessageResponse Response = await MFPPush.GetInstance().GetSubscriptions();
if (Response.Success == true)
{
    Message = new MessageDialog("Avalibale Tags: " + Response.ResponseJSON["tagNames"]);
}
else
{
    Message = new MessageDialog("Failed to get subcription list...");
}
```
{: codeblock}
{: windows}

##### Désabonnement
{: #unsubscribe-windows}
{: windows}

Se désabonne d'étiquettes.
{: windows}

```csharp
string[] Tags = ["Tag1" , "Tag2"];

// unsubscribe tag
MFPPushMessageResponse Response = await MFPPush.GetInstance().Unsubscribe(Tags);
if (Response.Success == true)
{
    //succès
}
else
{
    //échec de l'abonnement aux étiquettes
}
```
{: codeblock}
{: windows}

##### Désabonnement
{: #unregister-windows}
{: windows}

Désabonne l'appareil de l'instance de service de notification push.
{: windows}

```csharp
MFPPushMessageResponse Response = await MFPPush.GetInstance().UnregisterDevice();         
if (Response.Success == true)
{
    // Enregistrement réussi
} else {
    // Echec de l'enregistrement avec une erreur
}
```
{: codeblock}
{: windows}

#### Traitement d'une notification push
{: #handling-a-push-notification-windows}
{: windows}

Pour traiter une notification push, vous devez configurer un programme d'écoute `MFPPushNotificationListener`.  Pour ce faire, vous pouvez implémenter la méthode suivante.
{: windows}

1. Créez une classe à l'aide d'une interface de type MFPPushNotificationListener

    ```csharp
    internal class NotificationListner : MFPPushNotificationListener
    {
         public async void onReceive(String properties, String payload)
    {
         // Traitement de la notification push ici      
    }
    }
    ```
    {: codeblock}
{: windows}
2. Définissez la classe pour qu'elle devienne le programme d'écoute en appelant `MFPPush.GetInstance().listen(new NotificationListner())`
3. Dans la méthode onReceive, vous allez recevoir la notification push et pourrez la traiter pour obtenir le comportement souhaité.
{: windows}

#### Service de notification push Windows Universal
{: #windows-universal-push-notifications-service }
{: windows}

Aucun port spécifique ne doit être ouvert dans la configuration du serveur.
{: windows}

WNS utilise des demandes HTTP ou HTTPS normales.
{: windows}
