---

copyright:
  years: 2018, 2019
lastupdated:  "2019-01-04"

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
{:reactnative: .ph data-hd-programlang='reactnative'}
{:csharp: .ph data-hd-programlang='csharp'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}

#	Ajout du logiciel SDK de Mobile Foundation à une application 
{: #add_sdk_to_app}

### Ajout du logiciel SDK Android à votre application 
{: android}

Ouvrez Android Studio, choisissez la vue Android, puis choisissez **Gradle Scripts**, sélectionnez le fichier `build.gradle (Module: app)`, et suivez les étapes ci-dessous pour ajouter le logiciel SDK Android à votre application android.
{: android}

1. Ajoutez la ligne suivante à la section `dependencies` : 
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundation:8.0.+'
   ```
   {: codeblock}
   {: android}
2. Ajoutez la ligne suivante à la section `android` : 
   ```
   packagingOptions {
              pickFirst 'META-INF/ASL2.0'
              pickFirst 'META-INF/LICENSE'
              pickFirst 'META-INF/NOTICE'
    }
  ```
  {: codeblock}
  {: android}
3. Dans la vue Android, ouvrez le fichier **app → manifests → AndroidManifest.xml**. Ajoutez les droits suivants au-dessus de l'élément `application` :
   ```xml
   <uses-permission android:name="android.permission.INTERNET"/>
   <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
   ```
   {: codeblock}
   {: android}
4. Ajoutez l'activité UI de MobileFirst à côté de l'élément `activity` existant.
   ```xml
   <activity android:name="com.worklight.wlclient.ui.UIActivity" />
   ```
   {: codeblock}
   {: android}


### Ajout du logiciel SDK iOS à votre application
{: ios}

Il s'agit du logiciel SDK de base pour IBM Mobile Foundation, qui se compose d'API pour l'implémentation de la sécurité, de l'autorisation, de la journalisation, de l'appel d'adaptateurs et d'autres fonctions de base de Mobile Foundation. Suivez les étapes ci-dessous pour ajouter le logiciel SDK iOS à votre application iOS.
{: ios}

1. Accédez au dossier racine de votre application iOS et exécutez la commande suivante pour créer un fichier Pod :
```bash
    pod init
    ```
    {: codeblock}
    {: ios}
2. Ouvrez le fichier Pod dans l'éditeur de code de votre choix. 
   ```bash
   use_frameworks!
   platform :ios, 8.0
   target "ProjectName" do
       pod 'IBMMobileFirstPlatformFoundation'
   end
   ```
   {: codeblock}
   {: ios}
3. Mettez à jour le pod à l'aide de la commande suivante : 
   ```bash
   pod update
   ```
   {: codeblock}
   {: ios}
4. Installez le pod à l'aide de la commande suivante : 
   ```bash
   pod install
   ```
   {: codeblock}
   {: ios}
5. Ouvrez [NomProjet].xcworkspace pour ouvrir le projet dans Xcode. 
6. Ajoutez le fichier `mfpclient.plist` dans l'espace de travail Xcode. 
7. Depuis une invite de commande, accédez au dossier du projet et enregistrez votre application dans votre instance Mobile Foundation. 
   ```bash
   mfpdev app register
   ```
   {: codeblock}
   {: ios}

### Ajout du logiciel SDK Cordova à votre application 
{: cordova}

Il s'agit du logiciel SDK de base pour IBM Mobile Foundation, qui se compose d'API pour l'implémentation de la sécurité, de l'autorisation, de la journalisation, de l'appel d'adaptateurs et d'autres fonctions de base de Mobile Foundation. Suivez les étapes ci-dessous pour ajouter le logiciel SDK Cordova à votre application.
{: cordova}

1. Créez un projet Cordova. 
   ```bash
   cordova create Hello com.example.helloworld HelloWorld
   ```
   {: codeblock}
   {: cordova}
2. Placez-vous dans le répertoire racine du projet Cordova. 
   ```bash
   cd Hello
   ```
   {: codeblock}
   {: cordova}
3. Ajoutez une ou plusieurs plateformes prises en charge au projet Cordova via l'interface de ligne de commande Cordova. 
   ```bash
   cordova platform add ios|android|windows
   ```
   {: codeblock}
   {: cordova}
4. Ajoutez le plug-in Cordova de base pour Mobile Foundation. 
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
5. Préparez les ressources d'application en exécutant la commande suivante : 
   ```bash
   cordova prepare
   ```
   {: codeblock}
   {: cordova}
6. Enregistrez l'application sur le serveur Mobile Foundation. 
   ```bash
   mfpdev app register
   ```
   {: codeblock}
   {: cordova}

### Ajout du plug-in de logiciel SDK de React Native à votre application 
{: reactnative}

Pour pouvoir ajouter des fonctions Mobile Foundation à une application React Native existante, vous devez ajouter le plug-in `react-native-ibm-mobilefirst` à votre application. Le plug-in `react-native-ibm-mobilefirst` contient le logiciel SDK de Mobile Foundation. Suivez les étapes ci-dessous pour ajouter le plug-in React Native à votre application React Native.
{: reactnative}

1. Ajoutez ce plug-in de la même façon que n'importe quel autre plug-in `npm` à votre application. 
   ```bash
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}
   {: reactnative}
2. La première étape consiste à créer un projet React Native, par exemple *MobileFirstApp*. Utilisez l'interface de ligne de commande de React Native pour créer un projet. 
   ```bash
   react-native init MobileFirstApp
   ```
   {: codeblock}
   {: reactnative}
3. Ensuite, ajoutez le plug-in React Native à votre application. 
   ```bash
   cd MobileFirstApp
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}
   {: reactnative}
4. Liez votre projet pour que toutes les dépendances natives soient ajoutées à votre projet React Native. 
   ```bash
   react-native link
   ```
   {: codeblock}
   {: reactnative}
5. Pour Android, apportez les modifications suivantes à `AndroidManifest.xml` (`<PROJECT_ROOT>/android/app/src/main/`).
   ```xml
   <manifest 
          xmlns:android="http://schemas.android.com/apk/res/android" 
          xmlns:tools="http://schemas.android.com/tools"
          package="com.mobilefirstapp">
   ```
   {: codeblock}
   {: reactnative}
6. Ajoutez **tools:replace='android:allowBackup'** à la balise `application`. 
   ```xml
   <application
            android:name=".MainApplication"
            android:label="@string/app_name"
            android:icon="@mipmap/ic_launcher"
            android:allowBackup="false"
            android:theme="@style/AppTheme"
            tools:replace="android:allowBackup">
   ```
   {: codeblock}
   {: reactnative}
7. Pour iOS, ouvrez XCode dans le navigateur de projets, puis faites glisser et déposez `mfpclient.plist` depuis le dossier `ios`. Cette étape est valable pour la plateforme iOS uniquement.
{: reactnative}

