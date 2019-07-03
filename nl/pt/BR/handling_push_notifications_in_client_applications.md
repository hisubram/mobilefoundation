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


# Manipulando Notificações push em Aplicativos Clientes
{: #handling_push_notifications_in_client_applications}

Antes dos aplicativos iOS, Android e Native-based ou baseados em Cordova serem capazes de receber e exibir notificações push de entrada, o aplicativo deve primeiro ser configurado e as APIs devem ser implementadas.
{: shortdesc}

Consulte as seções a seguir para saber como manipular notificações push recebidas em aplicativos clientes:

### Manipulando Notificações push no Android
{: #handling_push_notifications_in_android}
{: android}
Antes que os aplicativos Android sejam capazes de manipular quaisquer notificações push recebidas, o suporte para o Google Play Services precisa ser configurado. Depois que um aplicativo tiver sido configurado, a API de Notificações fornecida pelo, {{ site.data.keyword.mobilefirst_notm }} poderá ser usada para registrar e cancelar o registro de dispositivos e assinar e cancelar a assinatura de tags. Neste tutorial, você aprenderá como manipular a notificação push em aplicativos Android.
{: android}

#### Pré-requisitos
{: #prereqs-andriod}
{: android}
* {{ site.data.keyword.mfserver_short_notm }} para executar localmente ou executar remotamente o {{ site.data.keyword.mfserver_short_notm }}.
* {{ site.data.keyword.mobilefirst_notm  }} CLI instalada na estação de trabalho do desenvolvedor
{: android}

#### Configuração de Notificações
{: #notifications-configuration }
{: android}

Crie um novo projeto Android Studio ou use um existente.  
Se o SDK do Android Nativo do {{ site.data.keyword.mobilefirst_notm }} ainda não estiver presente no projeto, siga as instruções no tutorial [Incluindo o SDK do {{ site.data.keyword.mobilefoundation_short }} em aplicativos Android](https://cloud.ibm.com/docs/services/mobilefoundation/add_sdk_to_app.html#add_sdk_to_app).
{: android}

##### Configuração do projeto
{: #project-setup }
{: android}

1. Em **Android → Scripts Gradle**, selecione o arquivo **build.gradle (Module: app)** e inclua as linhas a seguir em `dependencies`:
{: android}

    ```bash
    com.google.android.gms:play-services-gcm:9.0.2
    ```
    {: codeblock}
    {: android}

    Há um [defeito conhecido do Google](https://code.google.com/p/android/issues/detail?id=212879) que evita o uso da versão mais recente do Play Services (atualmente na versão 9.2.0). Use uma versão que seja menor que a 9.2.0.
    {: note}
    {: android}

    And:
    ```xml
    compile group: 'com.ibm.mobile.foundation',
             name: 'ibmmobilefirstplatformfoundationpush',
             version: '8.0.+',
             ext: 'aar',
             transitive: true
    ```
    {: codeblock}
    {: android}

    Or in a single line:
    ```xml
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationpush:8.0.+'
    ```
    {: codeblock}
    {: android}

1. Em **Android → app → manifestests**, abra o arquivo `AndroidManifest.xml`.
    * Inclua as permissões a seguir na parte superior da tag `manifest`:

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

    * Inclua o seguinte na tag `application`:

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

      Be sure to replace `your.application.package.name` with the actual package name of your application.
      {: note}
      {: android}

    * Add the following `intent-filter` to the application's activity.
        ```xml
        <intent-filter>
            <action android:name="your.application.package.name.IBMPushNotification" />
            <category android:name="android.intent.category.DEFAULT" />
        </intent-filter>
        ```
        {: codeblock}
        {: android}

#### API de Notificações
{: #notifications-api }
{: android}

##### Instância de MFPPush
{: #mfppush-instance }
{: android}

Todas as chamadas de API devem ser chamadas em uma instância de `MFPPush`.  Isso pode ser feito criando um campo de nível de classe, como `private MFPPush push = MFPPush.getInstance();` e, em seguida, chamando `push.<api-call>` em toda a classe.
{: android}

Como alternativa, é possível chamar `MFPPush.getInstance().<api_call>` para cada instância na qual é necessário acessar os métodos de API push.
{: android}

##### Manipuladores de Desafio
{: #challenge-handlers }
{: android}

Se o escopo `push.mobileclient` estiver mapeado para uma **verificação de segurança**, será necessário certificar-se de que **manipuladores de desafios** correspondentes existam e sejam registrados antes de usar qualquer uma das APIs de push.
{: android}

Saiba mais sobre manipuladores de desafios no tutorial [Validação de credencial ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/android/).
{: note}
{: android}

##### Lado do Cliente
{: #client-side }
{: android}

| Métodos Java | Descrição |
|-----------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| [`initialize(Context context);`](#initialization) | Inicializa o MFPPush para o contexto fornecido. |
| [`isPushSupported();`](#is-push-supported) | O dispositivo suporta notificações push. |
| [`registerDevice(JSONObject, MFPPushResponseListener);`](#register-device) | Registra o dispositivo com o Serviço de notificações push. |
| [`getTags(MFPPushResponseListener)`](#get-tags) | Recupera a(s) tag(s) disponível(eis) em uma instância de serviço de notificação de push. |
| [`subscribe(String[] tagNames, MFPPushResponseListener)`](#subscribe) | Assina o dispositivo para a(s) tag(s) especificada(s). |
| [`getSubscriptions(MFPPushResponseListener)`](#get-subscriptions) | Recupera todas as tags nas quais o dispositivo está atualmente inscrito. |
| [`unsubscribe(String[] tagNames, MFPPushResponseListener)`](#unsubscribe) | Cancela a assinatura de uma tag (s) específica. |
| [`unregisterDevice(MFPPushResponseListener)`](#unregister) | Cancela o registro do dispositivo por meio do Serviço de notificações push |
{: caption="Tabela 1. Métodos Java" caption-side="top"}
{: android}

###### Inicialização
{: #initialization}
{: android}

Necessário para o aplicativo cliente se conectar ao serviço do MFPPush com o contexto de aplicativo correto.
{: android}

* O método de API deve ser chamado primeiro antes de usar quaisquer outras APIs de MFPPush.
* Registra a função de retorno de chamada para manipular notificações push recebidas.
{: android}

```java
MFPPush.getInstance().initialize(this);
```
{: codeblock}
{: android}

###### É suportado por push
{: #is-push-supported }
{: android}

Verifica se o dispositivo suporta notificações push.
{: android}

```java
Boolean isSupported = MFPPush.getInstance().isPushSupported();

if (isSupported ) {
    // Push is supported
} else {
    // Push is not supported
}
```
{: codeblock}
{: android}

###### Registrar dispositivo
{: #register-device }
{: android}

Registre o dispositivo para o serviço de notificações push.
{: android}

```java
MFPPush.getInstance().registerDevice(null, new MFPPushResponseListener<String>() {
    @Override
    public void onSuccess(String s) {
        // Successfully registered
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Registration failed with error
    }
});
```
{: codeblock}
{: android}

###### Obter tags
{: #get-tags }
{: android}

Recupere todas as tags disponíveis por meio do serviço de notificação de push.
{: android}

```java
MFPPush.getInstance().getTags(new MFPPushResponseListener<List<String>>() {
    @Override
    public void onSuccess(List<String> strings) {
        // Successfully retrieved tags as list of strings
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to receive tags with error
    }
});
```
{: codeblock}
{: android}

###### Assinar
{: #subscribe }
{: android}

Assinar tags desejadas.
{: android}

```java
String[] tags = {"Tag 1", "Tag 2"};

MFPPush.getInstance().subscribe(tags, new MFPPushResponseListener<String[]>() {
    @Override
    public void onSuccess(String[] strings) {
        // Subscribed successfully
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to subscribe
    }
});
```
{: codeblock}
{: android}

###### Obter assinaturas
{: #get-subscriptions }
{: android}

Recupere as tags nas quais o dispositivo está atualmente inscrito.
{: android}

```java
MFPPush.getInstance().getSubscriptions(new MFPPushResponseListener<List<String>>() {
    @Override
    public void onSuccess(List<String> strings) {
        // Successfully received subscriptions as list of strings
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to retrieve subscriptions with error
    }
});
```
{: codeblock}
{: android}

###### Cancelar a assinatura
{: #unsubscribe }
{: android}

Cancelar assinatura de tags.
{: android}

```java
String[] tags = {"Tag 1", "Tag 2"};

MFPPush.getInstance().unsubscribe(tags, new MFPPushResponseListener<String[]>() {
    @Override
    public void onSuccess(String[] strings) {
        // Unsubscribed successfully
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to unsubscribe
    }
});
```
{: codeblock}
{: android}

###### Cancelar Registro
{: #unregister }
{: android}

Cancele o registro do dispositivo por meio da instância de serviço de notificação de push.
{: android}

```java
MFPPush.getInstance().unregisterDevice(new MFPPushResponseListener<String>() {
    @Override
    public void onSuccess(String s) {
        disableButtons();
        // Unregistered successfully
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to unregister
    }
});
```
{: codeblock}
{: android}

#### Manipulando uma Notificação push
{: #handling-a-push-notification }
{: android}

Para manipular uma notificação de push, será necessário configurar um `MFPPushNotificationListener`.  Isso pode ser alcançado pela implementação de um dos métodos a seguir.
{: android}

##### Opção Um
{: #option-one }
{: android}

Na atividade na qual você deseja que manipular notificações push.
{: android}

1. Inclua `implements MFPPushNofiticationListener` para a declaração de classe.
2. Configure a classe para ser o listener, chamando `MFPPush.getInstance().listen(this)` no método `onCreate`.
2. Em seguida, será necessário incluir o método *necessário* a seguir:
    ```java
    @Override
    public void onReceive(MFPSimplePushNotification mfpSimplePushNotification) {
         // Handle push notification here
    }
    ```
    {: codeblock}
    {: android}

3. Nesse método, você receberá o `MFPSimplePushNotification` e poderá manipular a notificação para o comportamento desejado.
{: android}

##### Opção Dois
{: #option-two }
{: android}

Crie um listener chamando `listen(new MFPPushNofiticationListener())` em uma instância de `MFPPush` conforme descrito no exemplo a seguir:
```java
MFPPush.getInstance().listen(new MFPPushNotificationListener() {
    @Override
    public void onReceive(MFPSimplePushNotification mfpSimplePushNotification) {
        // Handle push notification here
    }
});
```
{: codeblock}
{: android}

#### Migrar seus apps clientes no Android para o FCM
{: #migrate-to-fcm }
{: android}

Google Cloud Messaging (GCM) foi [descontinuado](https://developers.google.com/cloud-messaging/faq) e está integrado com o Firebase Cloud Messaging (FCM). O Google desativará a maioria dos serviços GCM até abril de 2019.
{: android}

Se você estiver usando um projeto GCM, [migre os apps do cliente GCM no Android para o FCM](https://developers.google.com/cloud-messaging/android/android-migrate-fcm).
{: android}

Por enquanto, os aplicativos existentes que usam os serviços GCM continuarão a funcionar no estado em que se encontram. Como o serviço Push Notifications foi atualizado para usar os terminais do FCM, no futuro, todos os novos aplicativos deverão usar o FCM.
{: android}

Depois de migrar para o FCM, atualize seu projeto para usar as credenciais do FCM em vez de as credenciais antigas do GCM.
{: note}
{: android}

##### Configuração do Projeto FCM
{: #fcm_project_setup}
{: android}

A configuração de um aplicativo no FCM é um pouco diferente comparado com o modelo GCM antigo.
{: android}

1. Obtenha suas credenciais do provedor de notificação, crie um projeto FCM e inclua o mesmo em seu aplicativo Android. Inclua o nome do pacote de seu aplicativo como `com.ibm.mobilefirstplatform.clientsdk.android.push`. Consulte a [documentação aqui](https://cloud.ibm.com/docs/services/mobilepush/push_step_1.html#push_step_1_android), até a etapa na qual você concluiu a geração do arquivo `google-services.json`
   {: android}
2. Configure seu arquivo Gradle. Inclua o seguinte no arquivo `build.gradle` do app
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

    - Inclua a dependência a seguir na seção `buildscript` do build.gradle raiz
      `classpath 'com.google.gms:google-services:3.0.0'`
      {: codeblock}
      {: android}

    - Remova após o plug-in do GCM do arquivo build.gradle `compile  com.google.android.gms:play-services-gcm:+`
     {: android}
 3. Configure o arquivo AndroidManifest. As seguintes mudanças são necessárias no `AndroidManifest.xml`
    {: android}

    Remova as entradas a seguir:
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

    As entradas a seguir precisam de modificação:
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

    Modifique as entradas para:
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

    Inclua a seguinte entrada:
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

 4. Abra o app no Android Studio. Copie o arquivo `google-services.json` que você criou na **etapa 1** dentro do diretório de aplicativos. Observe que o arquivo `google-service.json` inclui o nome do pacote que você incluiu.		
 5. Compile o SDK. Construa o aplicativo.
{: android}

### Manipulando Notificações Push no iOS
{: #handling_push_notifications_in_ios }
{: ios}

A API de Notificações fornecida pelo {{ site.data.keyword.mobilefirst_notm }} pode ser usada para registrar e cancelar o registro de dispositivos além de assinar e cancelar a assinatura de tags. Neste tutorial, você aprenderá como manipular a notificação push em aplicativos iOS usando o Swift.
{: shortdesc}
{: ios}

Para obter informações sobre notificações Silenciosa ou Interativa, consulte:

* [Notificações Silenciosas](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [Notificações interativas](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
{: ios}

#### Pré-requisitos
{: #prereqs-ios}
{: ios}

* {{ site.data.keyword.mfserver_short }} para executar localmente ou executar remotamente o {{ site.data.keyword.mfserver_short }}.
* {{ site.data.keyword.mfp_cli_long_notm }} instalado na estação de trabalho do desenvolvedor
{: ios}

#### Configuração de Notificações
{: #notifications-configuration_ios}
{: ios}

Crie um novo projeto Xcode ou use um existente.
Se o SDK do iOS Nativo do {{ site.data.keyword.mobilefirst_notm }} ainda não estiver presente no projeto, siga as instruções no tutorial [Incluindo o SDK do {{ site.data.keyword.mobilefoundation_short }} em aplicativos iOS](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/).
{: ios}

#### Incluindo o Push SDK
{: #adding-the-push-sdk }
{: ios}

1. Abra o **podfile** existente do projeto e inclua as linhas a seguir:
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

    - Substitua **Xcode-project-target** pelo nome do destino de seu projeto Xcode.
2. Salve e feche o  ** podfile **.
3. A partir de uma janela **Linha de Comandos**, navegue até a pasta raiz do projeto.
4. Execute o comando  ` pod install `
5. Abra o projeto usando o arquivo  ** .xcworkspace ** .
{: ios}

#### API de Notificações
{: #notifications-api-ios}
{: ios}

##### Instância de MFPPush
{: #mfppush-instance-ios}
{: ios}

Todas as chamadas de API devem ser chamadas em uma instância de `MFPPush`. Isso pode ser feito usando um `var` em um controlador de visualização, tal como `var push = MFPPush.sharedInstance ();`e, em seguida, chamando `push.methodName ()` em todo o controlador de visualização.
{: ios}

Como alternativa, é possível chamar `MFPPush.sharedInstance().methodName()` para cada instância na qual é necessário acessar os métodos de API push.
{: ios}

#### Manipuladores de Desafio
{: #challenge-handlers-ios}
{: ios}

Se o escopo `push.mobileclient` estiver mapeado para uma **verificação de segurança**, será necessário certificar-se de que **manipuladores de desafios** correspondentes existam e sejam registrados antes de usar qualquer uma das APIs de push.
{: ios}

Saiba mais sobre manipuladores de desafios no tutorial [Validação de credencial ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/ios/).
{: note}
{: ios}

#### Lado do Cliente
{: #client-side-ios}
{: ios}

| Métodos Swift | Descrição  |
|---------------|--------------|
| [`initialize()`](#initialization) | Inicializa o MFPPush para o contexto fornecido. |
| [`isPushSupported()`](#is-push-supported) | O dispositivo suporta notificações push. |
| [`registerDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#register-device--send-device-token) | Registra o dispositivo com o Serviço de notificações push.|
| [`sendDeviceToken(deviceToken: NSData!)`](#register-device--send-device-token) | Envia o token do dispositivo para o servidor |
| [`getTags(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-tags) | Recupera a(s) tag(s) disponível(eis) em uma instância de serviço de notificação de push. |
| [`subscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#subscribe) | Assina o dispositivo para a(s) tag(s) especificada(s). |
| [`getSubscriptions(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-subscriptions)  | Recupera todas as tags nas quais o dispositivo está atualmente inscrito. |
| [`unsubscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unsubscribe) | Cancela a assinatura de uma tag (s) específica. |
| [`unregisterDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unregister) | Cancela o registro do dispositivo por meio do Serviço de notificações push              |
{: caption="Tabela 2. Métodos Swift" caption-side="top"}
{: ios}

##### Inicialização
{: #initialization-ios}
{: ios}

A inicialização é necessária para o aplicativo cliente se conectar ao serviço do MFPPush.
{: ios}

* O método `initialize` deve ser chamado primeiro antes de usar qualquer outra API MFPPush.
* Ele registra a função de retorno de chamada para manipular notificações push recebidas.
{: ios}

```swift
MFPPush.sharedInstance ().initialize ();
```
{: codeblock}
{: ios}

##### É suportado por push
{: #is-push-supported-ios}
{: ios}

Verifica se o dispositivo suporta notificações push.
{: ios}

```swift
let isPushSupported: Bool = MFPPush.sharedInstance().isPushSupported()

if isPushSupported {
    // Push is supported
} else {
    // Push is not supported
}
```
{: codeblock}
{: ios}

##### Registrar dispositivo e enviar token do dispositivo
{: #register-device--send-device-token-ios}
{: ios}

Registre o dispositivo para o serviço de notificações push.
{: ios}

```swift
MFPPush.sharedInstance().registerDevice(nil) { (response, error) -> Void in
    if error == nil {
        self.enableButtons()
        self.showAlert("Registered successfully")
        print(response?.description ?? "")
    } else { else {
        self.showAlert (" Registros com falha.  Error \(error?.localizedDescription)")
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

Isso é geralmente chamado no **AppDelegate** no método `didRegisterForRemoteNotificationsWithDeviceToken`.
{: note}
{: ios}

##### Obter tags
{: #get-tags-ios}
{: ios}

Recupere todas as tags disponíveis por meio do serviço de notificação de push.
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
    } else { else {
        self.showAlert("Error \(error?.localizedDescription)")
        print("Error \(error?.localizedDescription)")
    }
}
```
{: codeblock}
{: ios}

##### Assinar
{: #subscribe-ios}
{: ios}

Assinar tags desejadas.
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

##### Obter assinaturas
{: #get-subscriptions-ios}
{: ios}

Recupere as tags nas quais o dispositivo está atualmente inscrito.
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

##### Cancelar a assinatura
{: #unsubscribe-ios}
{: ios}

Cancelar assinatura de tags.
{: ios}

```swift
var tags: [String] = {"Tag 1", "Tag 2"};

// Unsubscribe from tags
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

##### Cancelar Registro
{: #unregister-ios}
{: ios}

Cancele o registro do dispositivo por meio da instância de serviço de notificação de push.
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

#### Manipulando uma Notificação push
{: #handling-a-push-notification-ios}
{: ios}

As notificações push são manipuladas diretamente pela estrutura iOS nativa. Dependendo de seu ciclo de vida do aplicativo, diferentes métodos serão chamados pela estrutura do iOS.
{: ios}

Por exemplo, se uma notificação simples for recebida enquanto o aplicativo estiver em execução, o `didReceiveRemoteNotification` do **AppDelegate** será acionado:
{: ios}

```swift
func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable: Any]) {
    print("Received Notification in didReceiveRemoteNotification \(userInfo)")
    // display the alert body
      if let notification = userInfo["aps"] as? NSDictionary,
        let alert = notification["alert"] as? NSDictionary,
        let body = alert["body"] as? String {
          showAlert(body)
        }
}
```
{: codeblock}
{: ios}

Saiba mais sobre como manipular notificações no iOS a partir da [Documentação da Apple ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1).
{: note}
{: ios}

### Manipulando Notificações Push em Cordova
{: #handling_push_notifications_in_cordova }
{: cordova}

Antes que os aplicativos iOS, Android e Windows Cordova sejam capazes de receber e exibir notificações push, o plug-in Cordova **cordova-plugin-mfp-push** precisa ser incluído no projeto Cordova. Depois que um aplicativo tiver sido configurado, a API de Notificações fornecida pelo {{ site.data.keyword.mobilefirst_notm }} poderá ser usada para registrar e cancelar o registro de dispositivos, assinar e cancelar a assinatura de tags e manipular notificações. Neste tutorial, você aprenderá como manipular a notificação push em aplicativos Cordova.
{: shortdesc}
{: cordova}

As notificações autenticadas **não são suportadas** atualmente em aplicativos Cordova devido a um defeito. No entanto, uma solução alternativa é fornecida: cada chamada da API `MFPPush` pode ser agrupada por `WLAuthorizationManager.obtainAccessToken("push.mobileclient").then( ... );`.
{: note}
{: cordova}

Para obter informações sobre notificações Silenciosas ou Interativas no iOS, consulte:
{: cordova}

* [Notificações Silenciosas](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [Notificações interativas](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
{: cordova}

#### Pré-requisitos
{: #prereqs-cordova}
{: cordova}

* {{ site.data.keyword.mfserver_short }} para executar localmente ou um {{ site.data.keyword.mfserver_short }} em execução remotamente
* {{ site.data.keyword.mfp_cli_long_notm }} instalado na estação de trabalho do desenvolvedor
* CLI do Cordova instalada na estação de trabalho do desenvolvedor
{: cordova}

#### Configuração de Notificações
{: #notifications-configuration-cordova}
{: cordova}

Crie um novo projeto Cordova ou use um existente e inclua uma ou mais plataformas suportadas: iOS, Android, Windows.
{: cordova}

Se o SDK do Cordova do {{ site.data.keyword.mobilefirst_notm }} ainda não estiver presente no projeto, siga as instruções no tutorial [Incluindo o SDK do {{ site.data.keyword.mobilefirst_notm }} em aplicativos Cordova](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/).
{: cordova}
{: note}

#### Incluindo o plug-in Push
{: #adding-the-push-plug-in-cordova}
{: cordova}

1. Em uma janela de **linha de comandos**, navegue até a raiz do projeto Cordova.  
2. Inclua o plug-in de push executando o comando:
    ```bash
    cordova plugin add cordova-plugin-mfp-push
    ```
    {: codeblock}

3. Construa o projeto Cordova executando o comando:
    ```bash
    cordova build
    ```
    {: codeblock}
{: cordova}

#### Plataforma iOS
{: #ios-platform }
{: cordova}

A plataforma iOS requer uma etapa adicional.  
{: cordova}

Em Xcode, ative as notificações push para seu aplicativo na tela **Recursos**.
{: cordova}

O bundleId selecionado para o aplicativo deve corresponder ao AppId que você criou anteriormente no site do Apple Developer.
{: important}
{: cordova}

![image of where is the capability in Xcode](images/push-capability.png)
{: cordova}

#### Plataforma Android
{: #android-platform }
{: cordova}

A plataforma Android requer uma etapa adicional.  
{: cordova}

No Android Studio, inclua a `atividade` a seguir na tag `application`:
```xml
<activity android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushNotificationHandler" android:theme="@android:style/Theme.NoDisplay"/>
```
{: codeblock}
{: cordova}

#### API de Notificações
{: #notifications-api-cordova}
{: cordova}

##### Lado do Cliente
{: #client-side-cordova}
{: cordova}

| Função Javascript | Descrição |
| --- | --- |
| [`MFPPush.initialize(success, failure)`](#initialization-cordova) | Inicialize a instância MFPPush. |
| [`MFPPush.isPushSupported(success, failure)`](#is-push-supported-cordova) | O dispositivo suporta notificações push. |
| [`MFPPush.registerDevice(options, success, failure)`](#register-device-cordova) | Registra o dispositivo com o Serviço de notificações push. |
| [`MFPPush.getTags(success, failure)`](#get-tags-cordova) | Recupera todas as tags disponíveis em uma instância de serviço de notificação push. |
| [`MFPPush.subscribe(tag, success, failure)`](#subscribe-cordova) | Assinar uma tag específica. |
| [`MFPPush.getSubsciptions (sucesso, falha)`](#get-subscriptions-cordova) | Recupera as tags nas quais dispositivo está atualmente inscrito |
| [`MFPPush.unsubscribe(tag, success, failure)`](#unsubscribe-cordova) | Cancela a assinatura de uma tag específica. |
| [`MFPPush.unregisterDevice(success, failure)`](#unregister-cordova) | Cancela o registro do dispositivo por meio do Serviço de notificações push |
{: caption="Tabela 3. Funções do JavaScript" caption-side="top"}
{: cordova}

##### Implementação de API
{: #api-implementation }
{: cordova}

###### Inicialização
{: #initialization-cordova}
{: cordova}

Inicialize a instância  ** MFPPush ** .
{: cordova}

- Necessário para o aplicativo cliente se conectar ao serviço do MFPPush com o contexto de aplicativo correto.  
- O método de API deve ser chamado primeiro antes de usar quaisquer outras APIs de MFPPush.
- Registra a função de retorno de chamada para manipular notificações push recebidas.
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

###### É suportado por push
{: #is-push-supported-cordova}
{: cordova}

Verifique se o dispositivo suporta notificações push.
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

###### Registrar dispositivo
{: #register-device-cordova}
{: cordova}

Registre o dispositivo para o serviço de notificações push. Se nenhuma opção for necessária, as opções poderão ser configuradas como `null`.
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

###### Obter tags
{: #get-tags-cordova}
{: cordova}

Recupere todas as tags disponíveis por meio do serviço de notificação de push.
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

###### Assinar
{: #subscribe-cordova}
{: cordova}

Assinar tags desejadas.
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

###### Obter assinaturas
{: #get-subscriptions-cordova}
{: cordova}

Recupere as tags nas quais o dispositivo está atualmente inscrito.
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

###### Cancelar a assinatura
{: #unsubscribe-cordova}
{: cordova}

Cancelar assinatura de tags.
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

###### Cancelar Registro
{: #unregister-cordova}
{: cordova}

Cancele o registro do dispositivo por meio da instância de serviço de notificação de push.
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

#### Manipulando uma Notificação push
{: #handling-a-push-notification-cordova}
{: cordova}

É possível manipular uma notificação push recebida ao operar em seu objeto de resposta na função de retorno de chamada registrada.
{: cordova}

```javascript
var notificationReceived = function(message) {
    alert(JSON.stringify(message));
};
```
{: codeblock}
{: cordova}

### Manipulando notificações push no Windows 8.1 Universal e no Windows 10 UWP
{: #handling_push_notifications_in_windows }
{: windows}

A API de Notificações fornecida pelo {{ site.data.keyword.mobilefirst_notm }} pode ser usada para registrar e cancelar o registro de dispositivos além de assinar e cancelar a assinatura de tags. Neste tutorial, você aprenderá como manipular a notificação push nos aplicativos nativos do Windows 8.1 Universal e Windows 10 UWP usando C#.
{: windows}

#### Pré-requisitos
{: #prereqs-windows}
{: windows}

* {{ site.data.keyword.mfserver_short_notm }} para executar localmente ou executar remotamente o {{ site.data.keyword.mfserver_short_notm }}.
* {{ site.data.keyword.mobilefirst_notm  }} CLI instalada na estação de trabalho do desenvolvedor
{: windows}

#### Configuração de Notificações
{: #notifications-configuration-windows}
{: windows}

Crie um novo projeto Visual Studio ou use um existente.  
{: windows}

Se o SDK do Windows Nativo do {{ site.data.keyword.mobilefirst_notm }} ainda não estiver presente no projeto, siga as instruções no tutorial [Incluindo o SDK do {{ site.data.keyword.mobilefirst_notm }} em aplicativos Windows](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/windows-8-10/).
{: windows}

#### Incluindo o Push SDK
{: #adding-the-push-sdk-windows}
{: windows}

1. Selecione Ferramentas → NuGet Package Manager → Package Manager Console.
2. Escolha o projeto no qual você deseja instalar o componente Push do {{ site.data.keyword.mobilefirst_notm }}.
3. Inclua o SDK do Push do {{ site.data.keyword.mobilefirst_notm }} executando o comando **Install-Package IBM.MobileFirstPlatformFoundationPush**.
{: windows}

#### Configuração do WNS de pré-requisito
{: pre-requisite-wns-configuration }
{: windows}

1. Assegure-se de que o aplicativo esteja com o recurso de notificação de Aviso. Isso pode ser ativado no Package.appxmanifest.
2. Assegure-se de que `Package Identity Name` e `Publisher` sejam atualizados com os valores registrados com o WNS.
3. (Opcional) Exclua o arquivo TemporaryKey.pfx.
{: windows}

#### API de Notificações
{: #notifications-api-windows}
{: windows}

##### Instância de MFPPush
{: #mfppush-instance-windows}
{: windows}

Todas as chamadas de API devem ser chamadas em uma instância de `MFPPush`.  Isso pode ser feito criando uma variável, tal como `private MFPPush PushClient = MFPPush.GetInstance();` e, em seguida, chamando `PushClient.methodName()` em toda a classe.
{: windows}

Como alternativa, é possível chamar `MFPPush.GetInstance().methodName()` para cada instância na qual é necessário acessar os métodos de API push.
{: windows}

##### Manipuladores de Desafio
{: #challenge-handlers-windows}
{: windows}

Se o escopo `push.mobileclient` estiver mapeado para uma **verificação de segurança**, será necessário certificar-se de que **manipuladores de desafios** correspondentes existam e sejam registrados antes de usar qualquer uma das APIs de push.
{: windows}

Saiba mais sobre manipuladores de desafios no tutorial [Validação de credencial ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/windows-8-10/).
{: note}
{: windows}

#### Lado do Cliente
{: #client-side-windows}
{: windows}

| C Sharp Methods                                                                                                | Descrição                                                             |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| [`Initialize()`](#initialization-windows)                                                                            | Inicializa o MFPPush para o contexto fornecido.                               |
| [`IsPushSupported()`](#is-push-supported-windows)                                                                    | O dispositivo suporta notificações push.                             |
| [`RegisterDevice(JObject options)`](#register-device--send-device-token-windows)                  | Registra o dispositivo com o Serviço de notificações push.               |
| [`GetTags ()`](#get-tags-windows)                                | Recupera a(s) tag(s) disponível(eis) em uma instância de serviço de notificação de push. |
| [`Subscribe(String[] Tags)`](#subscribe-windows)     | Assina o dispositivo para a(s) tag(s) especificada(s).                          |
| [`GetSubscriptions()`](#get-subscriptions-windows)              | Recupera todas as tags nas quais o dispositivo está atualmente inscrito.               |
| [`Unsubscribe(String[] Tags)`](#unsubscribe-windows) | Cancela a assinatura de uma tag (s) específica.                                  |
| [`UnregisterDevice()`](#unregister-windows)                     | Cancela o registro do dispositivo por meio do Serviço de notificações push              |
{: caption="Tabela 4. Métodos C Sharp" caption-side="top"}
{: windows}

##### Inicialização
{: #initialization-windows}
{: windows}

A inicialização é necessária para o aplicativo cliente se conectar ao serviço do MFPPush.
{: windows}

* O método `Initialize` deve ser chamado primeiro antes de usar quaisquer outras APIs MFPPush.
* Ele registra a função de retorno de chamada para manipular notificações push recebidas.
{: windows}

```csharp
MFPPush.GetInstance().Initialize();
```
{: codeblock}
{: windows}

##### É suportado por push
{: #is-push-supported-windows}
{: windows}

Verifica se o dispositivo suporta notificações push.
{: windows}

```csharp
Boolean isSupported = MFPPush.GetInstance().IsPushSupported();

if (isSupported ) {
    // Push is supported
} else {
    // Push is not supported
}
```
{: codeblock}
{: windows}

##### Registrar dispositivo e enviar token do dispositivo
{: #register-device--send-device-token-windows}
{: windows}

Registre o dispositivo para o serviço de notificações push.
{: windows}

```csharp
JObject Options = new JObject();
MFPPushMessageResponse Response = await MFPPush.GetInstance().RegisterDevice(Options);         
if (Response.Success == true)
{
    // Successfully registered
} else {
    // Registration failed with error
}
```
{: codeblock}
{: windows}

##### Obter tags
{: #get-tags-windows}
{: windows}

Recupere todas as tags disponíveis por meio do serviço de notificação de push.
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

##### Assinar
{: #subscribe-windows}
{: windows}

Assinar tags desejadas.
{: windows}

```csharp
string[] Tags = ["Tag1" , "Tag2"];

// Get subscription tag
MFPPushMessageResponse Response = await MFPPush.GetInstance().Subscribe(Tags);
if (Response.Success == true)
{
    //successfully subscribed to push tag
}
else
{
    //failed to subscribe to push tags
}
```
{: codeblock}
{: windows}

##### Obter assinaturas
{: #get-subscriptions-windows}
{: windows}

Recupere as tags nas quais o dispositivo está atualmente inscrito.
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

##### Cancelar a assinatura
{: #unsubscribe-windows}
{: windows}

Cancelar assinatura de tags.
{: windows}

```csharp
string[] Tags = ["Tag1" , "Tag2"];

// unsubscribe tag
MFPPushMessageResponse Response = await MFPPush.GetInstance().Unsubscribe(Tags);
if (Response.Success == true)
{
    //succes
}
else
{
    //failed to subscribe to tags
}
```
{: codeblock}
{: windows}

##### Cancelar Registro
{: #unregister-windows}
{: windows}

Cancele o registro do dispositivo por meio da instância de serviço de notificação de push.
{: windows}

```csharp
MFPPushMessageResponse Response = await MFPPush.GetInstance().UnregisterDevice();         
if (Response.Success == true)
{
    // Successfully registered
} else {
    // Registration failed with error
}
```
{: codeblock}
{: windows}

#### Manipulando uma Notificação push
{: #handling-a-push-notification-windows}
{: windows}

Para manipular uma notificação de push, será necessário configurar um `MFPPushNotificationListener`.  Isso pode ser alcançado pela implementação do método a seguir.
{: windows}

1. Crie uma classe usando a interface do tipo MFPPushNotificationListener

    ```csharp
    internal class NotificationListner : MFPPushNotificationListener
    {
         public async void onReceive(String properties, String payload)
    {
         // Handle push notification here      
    }
    }
    ```
    {: codeblock}
{: windows}
2. Configure a classe para ser o listener, chamando `MFPPush.GetInstance().listen(new NotificationListner())`
3. No método onReceive, você receberá a notificação push e poderá manipular a notificação para o comportamento desejado.
{: windows}

#### Serviço de Notificações push do Windows Universal
{: #windows-universal-push-notifications-service }
{: windows}

Nenhuma porta específica precisa ser aberta em sua configuração do servidor.
{: windows}

O WNS usa solicitações http ou https regulares.
{: windows}
