---

copyright:
  years: 2018, 2019
lastupdated:  "2019-01-04"

keywords: Mobile Foundation SDK, android sdk, iOS sdk, cordova sdk, react native sdk

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
{:reactnative: .ph data-hd-programlang='reactnative'}
{:csharp: .ph data-hd-programlang='csharp'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}

#	Incluir o SDK do Mobile Foundation em um app
{: #add_sdk_to_app}

### Incluindo o SDK do Android em seu app
{: android}

Abra o Android Studio, escolha a visualização Android e escolha **Scripts do Gradle**. Selecione o arquivo `build.gradle (Module: app)` e, em seguida, execute as etapas a seguir para incluir o SDK do Android em seu aplicativo Android.
{: android}

1. Inclua a linha a seguir na seção `dependencies`.
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundation: 8.0. +'
   ```
   {: codeblock}
   {: android}
2. Inclua a linha a seguir na seção `android`.
   ```
   packagingOptions {
              pickFirst 'META-INF/ASL2.0'
              pickFirst 'META-INF/LICENSE'
              pickFirst 'META-INF/NOTICE'
    }
  ```
  {: codeblock}
  {: android}
3. Na visualização Android, abra o arquivo **app → manifests → AndroidManifest.xml**. Inclua as permissões a seguir antes do elemento `application`.
   ```xml
   <uses-permission android:name="android.permission.INTERNET"/>
   <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
   ```
   {: codeblock}
   {: android}
4. Inclua a atividade de IU do MobileFirst próximo ao elemento `activity` existente.
   ```xml
   <activity android:name="com.worklight.wlclient.ui.UIActivity" />
   ```
   {: codeblock}
   {: android}


### Incluindo o SDK do iOS em seu aplicativo
{: ios}

Esse SDK é o SDK principal para o IBM Mobile Foundation que consiste em APIs para implementar segurança, autorização, criação de log, adaptadores de chamada e outras funções principais do IBM Mobile Foundation. Execute as etapas a seguir para incluir o SDK do iOS em seu aplicativo iOS.
{: ios}

1. Acesse a pasta raiz de seu app iOS, execute o comando a seguir para criar um Arquivo pod.
    ```bash
    pod init
    ```
    {: codeblock}
    {: ios}
2. Usando seu editor de código favorito, abra o Arquivo pod.
   ```bash
   use_frameworks!
   platform :ios, 8.0
   target "ProjectName" do
       pod 'IBMMobileFirstPlatformFoundation'
   end
   ```
   {: codeblock}
   {: ios}
3. Atualize o pod usando o comando a seguir.
   ```bash
   atualização de pod
   ```
   {: codeblock}
   {: ios}
4. Instale o pod usando o comando a seguir.
   ```bash
   instalação do pod
   ```
   {: codeblock}
   {: ios}
5. Abra [ProjectName].xcworkspace para abrir o projeto em Xcode.
6. Inclua o arquivo `mfpclient.plist` na área de trabalho do Xcode.
7. Em um prompt de comandos, navegue para a pasta do projeto e registre seu app com sua instância do Mobile Foundation.
   ```bash
   Registro do aplicativo mfpdev
   ```
   {: codeblock}
   {: ios}

### Incluindo o SDK do Cordova em seu app
{: cordova}

Esse SDK é o SDK principal para o IBM Mobile Foundation que consiste em APIs para implementar segurança, autorização, criação de log, adaptadores de chamada e outras funções principais do Mobile Foundation. Execute as etapas a seguir para incluir o SDK do Cordova em seu aplicativo.
{: cordova}

1. Crie um projeto Cordova.
   ```bash
   cordova create Hello com.example.helloworld HelloWorld
   ```
   {: codeblock}
   {: cordova}
2. Mude o diretório para a raiz do projeto Cordova.
   ```bash
   cd Hello
   ```
   {: codeblock}
   {: cordova}
3. Inclua uma ou mais plataformas suportadas no projeto Cordova usando a CLI do Cordova.
   ```bash
   cordova platform add ios|android|windows
   ```
   {: codeblock}
   {: cordova}
4. Inclua o plug-in Cordova principal do Mobile Foundation.
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
5. Prepare os recursos do aplicativo executando o comando a seguir.
   ```bash
   cordova prepare
   ```
   {: codeblock}
   {: cordova}
6. Registre o aplicativo no servidor Mobile Foundation.
   ```bash
   Registro do aplicativo mfpdev
   ```
   {: codeblock}
   {: cordova}

### Incluindo o plug-in SDK do React Native em seu app
{: reactnative}

Para incluir os recursos do Mobile Foundation em um app React Native existente, é necessário incluir o plug-in `react-native-ibm-mobilefirst` em seu app. O plug-in `react-native-ibm-mobilefirst` contém o SDK do Mobile Foundation. Execute as etapas a seguir para incluir o plug-in React Native em seu aplicativo React Native.
{: reactnative}

1. Inclua esse plug-in da mesma maneira que inclui qualquer outro plug-in `npm` em seu app.
   ```bash
   npm install UNK-native-ibm-mobilefirst -- save
   ```
   {: codeblock}
   {: reactnative}
2. A primeira etapa é criar um projeto React Native, por exemplo, *MobileFirstApp*. Use a CLI do React Native para criar um novo projeto.
   ```bash
   reage-native init MobileFirstApp
   ```
   {: codeblock}
   {: reactnative}
3. Em seguida, inclua o plug-in react native em seu app.
   ```bash
   cd MobileFirstApp
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}
   {: reactnative}
4. Vincule seu projeto para que todas as dependências nativas sejam incluídas no projeto React Native.
   ```bash
   reagir-link nativo
   ```
   {: codeblock}
   {: reactnative}
5. Para o Android, faça as mudanças a seguir em `AndroidManifest.xml` (`<PROJECT_ROOT>/android/app/src/main/ `).
   ```xml
   <manifest 
          xmlns:android="http://schemas.android.com/apk/res/android" 
          xmlns:tools="http://schemas.android.com/tools"
          package="com.mobilefirstapp">
   ```
   {: codeblock}
   {: reactnative}
6. Inclua  ** tools:replace= 'android:allowBackup' **  para a tag  ` application ` .
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
7. Para o iOS, abra o XCode. No navegador do projeto, arraste e solte `mfpclient.plist` da pasta `ios`. Essa etapa é aplicável somente para a plataforma iOS.
{: reactnative}
