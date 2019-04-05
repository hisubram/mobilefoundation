---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-30"

keywords: mobile analytics, instrumenting cordova app, instrumenting iOS app, instrumenting android app

subcollection:  mobilefoundation
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

# Instrumente seu app
{: #instrument_your_app}

O Mobile Analytics é um recurso que está integrado dentro do serviço {{ site.data.keyword.mobilefoundation_short }}. O Mobile Analytics fornece insights importantes sobre o uso e o desempenho de aplicativos para desenvolvedores de aplicativos móveis e proprietários de aplicativos.

É necessário instrumentar seu aplicativo móvel para usar o recurso Mobile Analytics para monitorar o uso do aplicativo, o desempenho e para obter outras estatísticas. A instrumentação de seu aplicativo tem as etapas a seguir:

1.  Importar e instalar o SDK do Mobile Analytics Client.
2.  Instrumentar seu aplicativo com base no tipo de dados de analítica que você deseja coletar.

As seções a seguir fornecem os detalhes para essas etapas, para cada uma das plataformas suportadas.

### Instrumentar um aplicativo Android
{: #instrument_android_app}
{: android}
#### Etapa 1: importar e instalar o SDK do Mobile Analytics Client for Android
{: #install_analytics_sdk_android }
{: android}
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation/badge.svg) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation) [![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics/badge.svg) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics)
{: android}

O SDK do Mobile Analytics Client é distribuído com o Gradle, um gerenciador de dependência para projetos Android. O Gradle faz download automaticamente dos artefatos de repositórios e os disponibiliza para seu aplicativo Android.
{: android}

1. Crie um projeto [Android Studio![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://developer.android.com/sdk/index.html){: new_window} ou abra um projeto existente.
{: android}

2. Abra o arquivo `build.gradle` que está em seu **módulo de aplicativo**.
{: android}

  Seu projeto Android pode ter dois arquivos `build.gradle`: um para o projeto e um para o módulo do app. Certifique-se de usar o arquivo de **módulo de aplicativo**.
  {: tip}
  {: android}

3. Localize a seção `Dependencies` do arquivo `build.gradle`
e inclua uma dependência de compilação para o SDK do cliente
{{site.data.keyword.mobileanalytics_short}}. Sua instrução de repositórios deve ser semelhante ao exemplo de código a seguir:
{: android}

  ```
  dependencies {
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundation: 8.0. +'
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationanalytics:8.0.+@aar'
    // other dependencies
  }
  ```
  {: codeblock}
  {: android}

  A primeira dependência é para o SDK do Mobile Analytics Client capturar e criar log de eventos de tempo de execução do aplicativo
e a segunda é permitir que o feedback do usuário no app interaja com o usuário do aplicativo. A segunda dependência será necessária somente se você ativar o feedback do usuário no app
  {: android}

4. Sincronize seu projeto com Gradle clicando em **Ferramentas &gt; Android &gt; Projeto de sincronização com arquivos Gradle**.
{: android}

5. Abra o arquivo `AndroidManifest.xml` para seu projeto Android. É possível localizar esse arquivo em **app > manifests**. Inclua permissão de acesso à Internet e acesso local sob o elemento `<manifest>` :
{: android}

  ```xml
   <uses-permission android:name="android.permission.INTERNET" />

  ```
  {: codeblock}
  Se você estiver usando a versão do SDK maior que >= 1.2, será necessário colocar as linhas a seguir dentro do elemento `<application>`  do arquivo  ` AndroidManifest.xml ` .
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

#### Etapa 2: instrumentar seu aplicativo Android com base no tipo de dados de analítica que você deseja coletar
{: #instrument_app_based_on_data_android }
{: android}

1. Inicialize seu aplicativo para capturar e enviar dados de analítica para o serviço Mobile Analytics.  Primeiro, inclua as instruções `import` a seguir no início de sua classe de aplicativo ou atividade:
{: android}

   ```Java
    import com.worklight.common.Logger;
    import com.worklight.common.WLAnalytics;
   ```
   {: codeblock}
   {: android}

   Em seguida, use o SDK do Mobile Analytics Client para configurar ou inicializar a captura de dados de analítica.  
   Inclua o código de inicialização no método `onCreate` da atividade principal em seu aplicativo Android ou em um local que funcione melhor para seu projeto do aplicativo.
   {: android}

   ```Java
    WLAnalytics.init (this.getApplication ());
   ```
   {: codeblock}
   {: android}

   Antes de chamar o método de inicialização, deve-se assegurar que o seu aplicativo incorpore o código necessário para autenticar e autorizar o dispositivo com o serviço do MobileFoundation.  Essa etapa é uma etapa comum que é necessária a todos os aplicativos de serviços do Mobile Foundation e não é específica para a captura de dados do Analytics. <!--  Refer <need to link doc that talks about auth> -->
   {: android}

   Com a inicialização concluída, seu aplicativo está agora ativado para capturar informações sobre o dispositivo e os logs do SDK do Mobile Analytics sem nenhum código adicional incluído. Qualquer API e código adicional que for discutido nas seções a seguir é opcional e pode ser incluído com base em qual tipo de dados de analítica você deseja capturar.
   {: android}

2. Para capturar eventos de ciclo de vida do aplicativo, tais como sessão de aplicativo e informações de travamento, configure seu aplicativo incluindo a linha de código a seguir:
  ```Java
    WLAnalytics.addDeviceEventListener (WLAnalytics.DeviceEvent.LIFECYCLE);
  ```
  {: codeblock}
  {: android}

3. Para capturar eventos de rede que capturem as interações de rede de aplicativos, configure o seu aplicativo incluindo a linha de código a seguir:
  ```Java
    WLAnalytics.addDeviceEventListener (WLAnalytics.DeviceEvent.NETWORK);
  ```
  {: codeblock}
  {: android}

4. Agora você inicializou o seu aplicativo para capturar dados de analítica.  Em seguida, deve-se enviar os dados capturados para o serviço do Mobile Analytics.
   Use a API a seguir para enviar dados de analítica ao serviço Mobile Analytics.
   ```java
    WLAnalytics.send ();
   ```
   {: codeblock}
   {: android}

  Você é livre para decidir quando chamar essa API em seu fluxo de aplicativo para enviar os dados de analítica capturados para o serviço Mobile Analytics.  Até que isso seja enviado, todos os dados de analítica capturados serão armazenados localmente no dispositivo.
  {: android}

5. Para capturar e enviar logs do aplicativo para o serviço do Mobile Analytics, use as APIs do criador de logs do SDK do Mobile Analytics Client.     
   A estrutura de criação de log do SDK cliente do Mobile Analytics suporta os níveis de log a seguir, que estão listados do menos para o mais detalhado, com as diretrizes de uso recomendadas:
    * FATAL - Use para travamentos ou interrupções irrecuperáveis. O nível FATAL é reservado para erros irrecuperáveis de criação de log, que aparecem para usuários como um travamento do aplicativo
    * ERROR - Use para exceções inesperadas ou erros de protocolo de rede inesperados
    * WARN - Use para registrar os avisos de uso que não são considerados erros críticos, como uso de APIs descontinuadas ou resposta de rede lenta
    * INFO - Use para relatar eventos de inicialização e outros dados que possam ser importantes, mas não urgentes
    * DEBUG - Use para relatar instruções de depuração para ajudar os desenvolvedores a resolver defeitos do aplicativo.
    Quando o nível do criador de logs é configurado como DEBUG, você também obtém os logs do SDK cliente do Mobile Analytics, que são incluídos quando ao enviar logs.
   {: note}
   {: android}

   Os fragmentos de código a seguir mostram o uso do Criador de logs de amostra para Android:
   ```java
   // Set the logging level (optional)
   // The default setting is Logger.LEVEL.DEBUG
   Logger.setLogLevel(Logger.LEVEL.INFO);

   // Create a logger instance.  You can create multiple loggers to organize your logs as you wish
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

6.  Para obter insights sobre padrões de onboarding do usuário (novos usuários versus usuários retornados), deve-se associar uma identidade do usuário à sua sessão do aplicativo.  Essa etapa pode ser feita chamando a API a seguir
    ```java
      WLAnalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: android}

    Todos os dados de analítica capturados e armazenados localmente são enviados para o serviço Mobile Analytics somente quando a API WLAnalytics.send() é chamada.
    {: android}

7.  Para obter insights sobre os padrões de interação de rede HTTP de seus aplicativos, como APIs HTTP chamadas, número de solicitações e tempos médios de resposta, deve-se usar a classe WLResourceRequest do SDK cliente do Mobile Foundation Service para fazer as chamadas HTTP.  Embora você tenha inicializado o Analytics para a captura de eventos de Rede, o uso de `WLResourceRequest` permite que o Mobile Analytics conecte as chamadas de rede e capture os dados relevantes. Faça as suas chamadas de HTTP como no código a seguir.
    ```java
      WLResourceRequest request = new WLResourceRequest(new URI(url), WLResourceRequest.GET);
            request.send(new WLResponseListener() {

                @Override
                public void onSuccess(WLResponse wlResponse) {
                    //handle success of HTTP call and use wlResponse 
                }

                @Override public void onFailure(WLFailResponse wlFailResponse) {
                    //handle failure of HTTP call and use wlFailResponse
                }
            });
    ```
    {: codeblock}
    {: android}

8.  Para obter a Analítica de travamento e os logs, é possível chamar a API a seguir durante o início de seu aplicativo para verificar se a sessão anterior travou e, portanto, enviar os logs de travamento de captura para o serviço Mobile Analytics.
    ```java
      if (Logger.isUnCaughtExceptionDetected()) {
            //add any other handling you may want and call the following APIs
            WLAnalytics.send();
            Logger.send();
      }
    ```
    {: codeblock}
    {: android}

9.  Para definir a Analítica customizada e definir os seus próprios dados de analítica e acima do que é suportado inerentemente no SDK cliente, seria possível usar a API de criação de log customizada
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

    Os dados customizados registrados podem ser plotados sobre gráficos customizados que podem ser definidos no console Mobile Analytics para derivar insights customizados.
    {: android}

10. Use o Feedback do usuário no app para aprofundar a análise de desempenho do seu aplicativo. É possível ativar os **usuários e testadores** do aplicativo para fornecer feedback contextual rico aos proprietários de app. **Proprietários de app** obtêm feedback em tempo real de seus usuários sobre a experiência de uso do aplicativo na qual os **Proprietários de app** e **Desenvolvedores** podem atuar posteriormente.  Esse recurso traz uma agilidade significativa para a manutenção do aplicativo. Use a API a seguir para alternar o seu aplicativo para o Modo de feedback interativo em qualquer manipulador de ações em seu aplicativo, por exemplo, quando você manipular o clique de um botão ou a seleção de um item de menu.
    ```java
        WLAnalytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: android}

### Instrumentar um aplicativo iOS
{: #instrument_ios_app}
{: ios}

Etapas para instrumentar seu aplicativo iOS:
{: ios}

#### Etapa 1: importar e instalar o SDK do Mobile Analytics Client para iOS
{: #install_analytics_sdk_ios }
{: ios}

[![Compatível com o CocoaPods](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundation.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundation)
[![Compatível com o CocoaPods](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundationAnalytics.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundationAnalytics)
{: ios}

O Swift SDK está disponível para iOS e watchOS.
{: ios}

1. Inclua o seguinte no arquivo pod existente, que está localizado na raiz do projeto Xcode
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

2. Em uma janela da Linha de comandos, navegue para a raiz do projeto Xcode e execute o comando a seguir
   ```bash
   atualização de pod
   ```
   {: codeblock}
   {: ios}

#### Etapa 2: instrumentar seu aplicativo iOS com base no tipo de dados de analítica que você deseja coletar
{: #instrument_app_based_on_data_ios }
{: ios}

1. Inicialize seu aplicativo para ativar o envio de logs para o Mobile Analytics.
   O Swift SDK está disponível para iOS e watchOS. Importe as estruturas `BMSCore` e `BMSAnalytics` ao incluir as instruções de `importação` a seguir no início do seu arquivo de
projeto `AppDelegate.swift`:
    ```Swift
    import IBMMobileFirstPlatformFoundation
    ```
    {: codeblock}
    {: ios}

   Em seguida, use o SDK do Mobile Analytics Client para configurar ou inicializar a captura de dados de analítica.
   Inclua o código de inicialização em um local que funcione melhor para seu projeto do aplicativo.
   {: ios}

   ```Swift
    WLAnalytics.init ()
   ```
   {: codeblock}
   {: ios}

   Antes de chamar o método de inicialização, deve-se assegurar que o seu aplicativo incorpore o código necessário para autenticar e autorizar o dispositivo com o serviço do MobileFoundation.  Essa etapa é uma etapa comum que é necessária a todos os aplicativos de serviços do Mobile Foundation e não é específica para a captura de dados do Analytics. <!--  Refer <need to link doc that talks about auth> -->
   {: ios}

   Com a inicialização, conclua que o seu aplicativo agora está ativado para capturar informações do dispositivo e os logs do SDK do Mobile Analytics sem nenhum código adicional incluído.  Qualquer API e código adicional que for discutido nas seções a seguir é opcional e pode ser incluído com base em qual tipo de dados de analítica você deseja capturar.
   {: ios}

2. Para capturar eventos de ciclo de vida do aplicativo, tais como sessão de aplicativo e informações de travamento, configure seu aplicativo incluindo a linha de código a seguir:
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE)
    ```
    {: codeblock}
    {: ios}

3. Para capturar eventos de rede que capturem as interações de rede de aplicativos, configure o seu aplicativo incluindo a linha de código a seguir:
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK)
    ```
    {: codeblock}
    {: ios}

4. Agora você inicializou o seu aplicativo para capturar dados de analítica.  Em seguida, deve-se enviar os dados capturados para o serviço do Mobile Analytics.
   Use a API a seguir para enviar dados de analítica ao serviço Mobile Analytics.
   ```Swift
    WLAnalytics.sharedInstance ().send ();
   ```
   {: codeblock}
   {: ios}

    Você é livre para decidir quando chamar essa API em seu fluxo de aplicativo para enviar os dados de analítica capturados para o serviço Mobile Analytics.  Até isso, envie todos os dados de analítica capturados que estão armazenados localmente no dispositivo.     {: ios}

5. Para capturar e enviar logs do aplicativo para o serviço do Mobile Analytics, use as APIs do criador de logs do SDK do Mobile Analytics Client.
   A estrutura de criação de log do SDK cliente do Mobile Analytics suporta os níveis de log a seguir, que estão listados do menos para o mais detalhado, com as diretrizes de uso recomendadas:
    * FATAL - Use para travamentos ou interrupções irrecuperáveis. O nível FATAL é reservado para erros irrecuperáveis de criação de log, que aparecem para usuários como um travamento do aplicativo
    * ERROR - Use para exceções inesperadas ou erros de protocolo de rede inesperados
    * WARN - Use para registrar os avisos de uso que não são considerados erros críticos, como uso de APIs descontinuadas ou resposta de rede lenta
    * INFO - Use para relatar eventos de inicialização e outros dados que possam ser importantes, mas não urgentes
    * DEBUG - Use para relatar instruções de depuração para ajudar os desenvolvedores a resolver defeitos do aplicativo.
    Quando o nível do criador de logs é configurado como DEBUG, você também obtém os logs do SDK cliente do Mobile Analytics, que são incluídos quando ao enviar logs.
   {: note}
   {: ios}

    Siga [o tutorial ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/client-side-log-collection/ios/) para obter fragmentos de código para o Criador de logs a fim de incluir recursos de criação de log em aplicativos iOS.

6.  Para obter insights sobre padrões de onboarding do usuário (novos usuários versus usuários retornados), deve-se associar uma identidade do usuário à sua sessão do aplicativo.  Essa associação pode ser feita chamando a API a seguir
    ```Swift
      WLAnalytics.sharedInstance().setUserContext("userName or userIdentity")
    ```
    {: codeblock}
    {: ios}

    Todos os dados de analítica capturados e armazenados localmente são enviados para o serviço Mobile Analytics somente quando a API `WLAnalytics.sharedInstance().send()` é chamada.
    {: ios}

7.  Para obter insights sobre os padrões de interação de rede HTTP de seus aplicativos, como APIs HTTP chamadas, número de solicitações e tempos médios de resposta, deve-se usar a classe WLResourceRequest do SDK cliente do Mobile Foundation Service para fazer as chamadas HTTP.  Embora você tenha inicializado Analítica para a captura de Eventos de rede, o uso de `WLResourceRequest` permite que o Mobile Analytics conecte-se às chamadas de rede e capture os dados relevantes.  Faça as suas chamadas de HTTP como no código a seguir.
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

8.  Para obter a Analítica de travamento e os logs, é possível chamar a API a seguir durante o início de seu aplicativo para verificar se a sessão anterior travou e enviar os logs de travamento de captura para o serviço Mobile Analytics.
    ```Swift
      if (OCLogger.isUnCaughtExceptionDetected()) {
            //add any other handling you may want and call the following APIs
            WLAnalytics.sharedInstance().send();
            OCLogger.send();
      }
    ```
    {: codeblock}
    {: ios}

9.  Para definir a Analítica customizada e definir os seus próprios dados de analítica e acima do que é suportado inerentemente no SDK cliente, seria possível usar a API de criação de log customizada
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

    Os dados customizados registrados podem ser plotados sobre gráficos customizados que podem ser definidos no console Mobile Analytics para derivar insights customizados.
    {: ios}

10. Use o Feedback do usuário no app para aprofundar a análise de desempenho do seu aplicativo.   É possível ativar os **usuários e testadores** do aplicativo para fornecer feedback contextual rico aos proprietários de app. **Proprietários de app** obtêm feedback em tempo real de seus usuários sobre a experiência de uso do aplicativo na qual os **Proprietários de app** e **Desenvolvedores** podem atuar posteriormente.  Esse recurso traz uma agilidade significativa para a manutenção do aplicativo. Use a API a seguir para alternar o seu aplicativo para o Modo de feedback interativo em qualquer manipulador de ações em seu aplicativo, por exemplo, quando você manipular o clique de um botão ou a seleção de um item de menu.
    ```Swift
        WLAnalytics.sharedInstance().triggerFeedbackMode();
    ```
    {: codeblock}
    {: ios}

### Instrumentar um aplicativo Cordova
{: #instrument_cordova_app}
{: cordova}
Etapas para instrumentar seu aplicativo Cordova:
{: cordova}

#### Etapa 1: importar e instalar o plug-in SDK do Foundation and Analytics Client para Cordova
{: #install_analytics_sdk_cordova }
O plug-in do Mobile Analytics Cordova permite instrumentar seu aplicativo móvel.
{: cordova}

#### Instalando o SDK do Cordova Client
{: #install_cordova_sdk}
{: cordova}

1. Crie um projeto [Cordova ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://cordova.apache.org/#getstarted){: new_window} ou abra um projeto existente.
    {: cordova}

2. Inclua as plataformas Android ou iOS de sua preferência no aplicativo Cordova. Execute um ou ambos os comandos a seguir da linha de comandos.<br/>
    **Android**:
    ```
    cordova platform add android
    ```
    {: codeblock}
    {: cordova}

    **iOS**:
    ```
    cordova platform add ios
    ```
    {: codeblock}
    {: cordova}

3. Inclua o plug-in SDK do cliente Foundation `cordova-plugin-mfp`.
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
4. Inclua o plug-in SDK opcional do cliente Analytics `cordova-plugin-mfp-analytics`, que é para recurso de feedback no app em seu app
    {: cordova}
    ```bash
    cordova plugin add cordova-plugin-mfp-analytics
    ```
    {: codeblock}
    {: cordova}
5. Verifique se o plug-in foi instalado com sucesso, executando o comando a seguir:
    {: cordova}
    ```bash
    cordova plugin list
    ```
    {: codeblock}
    {: cordova}

#### Etapa 2: instrumentar seu aplicativo Cordova com base no tipo de dados de analítica que você deseja coletar
{: #instrument_app_based_on_data_cordova }
{: cordova}

1. Em aplicativos Cordova, nenhuma configuração é necessária e a inicialização está integrada.

   Antes de chamar qualquer um dos métodos analíticos abaixo, deve-se assegurar que seu aplicativo integre o código necessário para autenticar e autorizar o dispositivo com o serviço MobileFoundation. Essa etapa é uma etapa comum que é necessária a todos os aplicativos de serviços do Mobile Foundation e não é específica para a captura de dados do Analytics. <!--  Refer <need to link doc that talks about auth> -->
   {: cordova}

   Com a inicialização, conclua que o seu aplicativo agora está ativado para capturar informações do dispositivo e os logs do SDK do Mobile Analytics sem nenhum código adicional incluído.  Qualquer API e código adicional que for discutido nas seções a seguir é opcional e pode ser incluído com base em qual tipo de dados de analítica você deseja capturar.
   {: cordova}

2. Para ativar a captura dos eventos de ciclo de vida, ele deve ser inicializado na plataforma nativa do app Cordova.
    * Para a plataforma Android:
      * Abra o arquivo **[Pasta raiz do aplicativo Cordova] → plataformas → android → src → com → amostra → [nome do app] → MainActivity.java**.
      * Procure o método `onCreate` e ative as atividades `LIFECYCLE` e `NETWORK` seguindo as etapas:
        * Para ativar a criação de log de eventos de ciclo de vida do cliente, inclua a linha a seguir:
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * Para ativar a criação de log de eventos de rede do cliente, inclua a linha a seguir:
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * Construa o projeto Cordova executando o comando: `cordova build`.
    * Para a plataforma do iOS:
      * Abra a pasta **[Pasta raiz do aplicativo Cordova] → plataformas → ios → Classes** e localize o arquivo **AppDelegate.swift** (Swift).
      * Para ativar as atividades `LIFECYCLE` e `NETWORK`, execute as etapas a seguir.
        * Para ativar a criação de log de eventos de ciclo de vida do cliente, inclua a linha a seguir:
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * Para ativar a criação de log de eventos de rede do cliente, inclua a linha a seguir:
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * Construa o projeto Cordova executando o comando: `cordova build`.

3. Agora você inicializou seu aplicativo para coletar dados de analítica. Em seguida, é possível enviar dados de analítica para o Mobile Analytics.
   Use as APIs a seguir para iniciar a gravação e o envio da análise de uso:
   ```Javascript
   // Send recorded usage analytics to the Mobile Analytics

   WL.Analytics.send ();
   ```
   {: codeblock}
   {: cordova}

4. Para capturar e enviar logs do aplicativo para o serviço do Mobile Analytics, use as APIs do criador de logs do SDK do Mobile Analytics Client.
   A estrutura de criação de log do SDK cliente do Mobile Analytics suporta os níveis de log a seguir, que estão listados do menos para o mais detalhado, com as diretrizes de uso recomendadas:
    * FATAL - Use para travamentos ou interrupções irrecuperáveis. O nível FATAL é reservado para erros irrecuperáveis de criação de log, que aparecem para usuários como um travamento do aplicativo
    * ERROR - Use para exceções inesperadas ou erros de protocolo de rede inesperados
    * WARN - Use para registrar os avisos de uso que não são considerados erros críticos, como uso de APIs descontinuadas ou resposta de rede lenta
    * INFO - Use para relatar eventos de inicialização e outros dados que possam ser importantes, mas não urgentes
    * DEBUG - Use para relatar instruções de depuração para ajudar os desenvolvedores a resolver defeitos do aplicativo.
    Quando o nível do criador de logs é configurado como DEBUG, você também obtém os logs do SDK cliente do Mobile Analytics, que são incluídos quando ao enviar logs.
    * TRACE - usado para entrada de método e pontos de saída
   {: note}
   {: cordova}

   Os fragmentos de código a seguir mostram o uso do Criador de logs de amostra para o Cordova:
    ```Javascript
    // Set the logging level (optional)
    // The default setting is DEBUG
    WL.Logger.config({"level": "INFO"});

    // Create a logger instance.  You can create multiple loggers to organize your logs as you wish
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

5.  Para obter insights sobre padrões de onboarding do usuário (novos usuários versus usuários retornados), deve-se associar uma identidade do usuário à sua sessão do aplicativo.  Não há nenhuma API específica disponível no nível de Javascript. Mas isso pode ser feito no código nativo chamando a API a seguir:

    **Android:**
      ```java
        WLAnalytics.setUserContext("userName or userIdentity");
      ```
      {: codeblock}
      {: cordova}

    **iOS:**
      ```Swift
        WLAnalytics.sharedInstance().setUserContext("userName or userIdentity")
      ```
      {: codeblock}
      {: cordova}

    Todos os dados de analítica capturados e armazenados localmente são enviados para o serviço do Mobile Analytics apenas quando a API WL.Analytics.send () no nível de javascript [ ou ] WLAnalytics.send () API em android nativo [ ou ] WLAnalytics.sharedInstance ().send () no iOS nativo é chamada.
    {: cordova}

6. Para obter insights sobre os padrões de interação de rede HTTP de seus aplicativos, como APIs HTTP chamadas, número de solicitações e tempos médios de resposta, deve-se usar a classe WLResourceRequest do SDK cliente do Mobile Foundation Service para fazer as chamadas HTTP.  Embora você tenha inicializado Analítica para a captura de Eventos de rede, o uso de `WLResourceRequest` permite que o Mobile Analytics conecte-se às chamadas de rede e capture os dados relevantes.  Faça suas chamadas HTTP usando o código a seguir.
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

7. Para definir a Analítica customizada e definir os seus próprios dados de analítica e acima do que é suportado inerentemente no SDK cliente, seria possível usar a API de criação de log customizada
    ```Javascript
        //create custom data as key value pair
        WL.Analytics.log({"FromPage" : 'LoginPage'});

        //Send the captured custom data and log to the Mobile Analytics service
        WL.Analytics.send();
    ```
    {: codeblock}
    {: cordova}

    Os dados customizados registrados podem ser plotados sobre gráficos customizados que podem ser definidos no console Mobile Analytics para derivar insights customizados.
    {: cordova}

8. Use o Feedback do usuário no app para aprofundar a análise de desempenho do seu aplicativo.   É possível ativar os **usuários e testadores** do aplicativo para fornecer feedback contextual rico aos proprietários de app. **Proprietários de app** obtêm feedback em tempo real de seus usuários sobre a experiência de uso do aplicativo na qual os **Proprietários de app** e **Desenvolvedores** podem atuar posteriormente.  Essa etapa traz uma agilidade significativa para a manutenção do aplicativo. Use a API a seguir para alternar o seu aplicativo para o Modo de feedback interativo em qualquer manipulador de ações em seu aplicativo, por exemplo, quando você manipular o clique de um botão ou a seleção de um item de menu.
    ```Javascript
        WL.Analytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: cordova}

### Instrumentar um aplicativo da Web
{: #instrument_web_app}
{: web}

Etapas para instrumentar seu aplicativo da Web:
{: web}

#### Etapa 1: importar e instalar o SDK do Mobile Analytics Client para Web
{: #install_analytics_sdk_web }
{: web}

#### Instalando o SDK do Web Client
{: #install_web_sdk}
O SDK do Mobile Analytics permite que você instrumente seu aplicativo da web.
{: web}

1. Navegue para a pasta raiz de seu aplicativo da web e execute o comando a seguir. Inclua o SDK em seu aplicativo da web usando [npm](https://www.npmjs.com/package/ibm-mfp-web-sdk)
    ```bash
    npm install ibm-mfp-web-sdk
    ```
    {: codeblock}
    {: web}


#### Etapa 2: instrumentar seu aplicativo da web com base no tipo de dados de analítica que você deseja coletar
{: #instrument_app_based_on_data_web }
{: web}

1. Referencie os arquivos ibmmfpf.js e ibmmfpfanalytics.js no elemento `head` de sua página HTML principal

    ```html
      <head>
        ....
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/ibmmfpf.js"></script>
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/lib/analytics/ibmmfpfanalytics.js"></script>
      </head>
    ```
    {: codeblock}
    {: web}

2. Inicialize o SDK da web do Mobile Foundation especificando os valores de raiz de contexto e de ID do aplicativo no arquivo JavaScript principal de seu aplicativo da web

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

   Antes de chamar qualquer um dos métodos analíticos abaixo, deve-se assegurar que seu aplicativo integre o código necessário para autenticar e autorizar o dispositivo com o serviço MobileFoundation. Essa etapa é uma etapa comum que é necessária a todos os aplicativos de serviços do Mobile Foundation e não é específica para a captura de dados do Analytics. <!--  Refer <need to link doc that talks about auth> -->
   {: web}

   Com a inicialização completa, agora seu aplicativo está ativado para capturar informações do dispositivo e logs de SDK do Mobile Analytics sem código adicional incluído.  Qualquer API e código adicional que for discutido nas seções a seguir é opcional e pode ser incluído com base em qual tipo de dados de analítica você deseja capturar.
   {: web}

3. Inclua a linha a seguir em sua lógica de aplicativo para capturar eventos de ciclo de vida do aplicativo e eventos de rede.

    ```Javascript
    ibmmfpfanalytics.logger.config({analyticsCapture: true});
    ```
    {: codeblock}
    {: web}

4. Agora você inicializou o seu aplicativo para capturar dados de analítica.  Em seguida, deve-se enviar os dados capturados para o serviço do Mobile Analytics. Use a API a seguir para enviar dados de analítica ao serviço Mobile Analytics.

   ```Javascript
   ibmmfpfanalytics.send();
   ```
   {: codeblock}
   {: web}

    Você é livre para decidir quando chamar essa API em seu fluxo de aplicativo para enviar os dados de analítica capturados para o serviço Mobile Analytics.  Até que isso seja enviado, todos os dados de analítica capturados serão armazenados localmente no dispositivo.
    {: web}

5. Para capturar e enviar logs do aplicativo para o serviço do Mobile Analytics, use as APIs do criador de logs do SDK do Mobile Analytics Client.
   A estrutura de criação de log do SDK cliente do Mobile Analytics suporta os níveis de log a seguir, que estão listados do menos para o mais detalhado, com as diretrizes de uso recomendadas:
    * FATAL - Use para travamentos ou interrupções irrecuperáveis. O nível FATAL é reservado para erros irrecuperáveis de criação de log, que aparecem para usuários como um travamento do aplicativo
    * ERROR - Use para exceções inesperadas ou erros de protocolo de rede inesperados
    * WARN - Use para registrar os avisos de uso que não são considerados erros críticos, como uso de APIs descontinuadas ou resposta de rede lenta
    * INFO - Use para relatar eventos de inicialização e outros dados que possam ser importantes, mas não urgentes
    * DEBUG - Use para relatar instruções de depuração para ajudar os desenvolvedores a resolver defeitos do aplicativo.
    Quando o nível do criador de logs é configurado como DEBUG, você também obtém os logs do SDK cliente do Mobile Analytics, que são incluídos quando ao enviar logs.
    * TRACE - usado para entrada de método e pontos de saída
   {: note}
   {: web}

   Os fragmentos de código a seguir mostram o uso do Criador de logs de amostra para o app da web:
   ```Javascript
    // Create a logger instance.  You can create multiple loggers to organize your logs as you wish

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

   Para o SDK da Web, o nível não pode ser configurado pelo cliente. Toda a criação de log é enviada para o servidor até que a configuração seja mudada recuperando o perfil de configuração do servidor.
   {: note}
   {: web}

6. Para obter insights sobre padrões de onboarding do usuário (novos usuários versus usuários retornados), deve-se associar uma identidade do usuário à sua sessão do aplicativo.  Isso pode ser feito chamando a API a seguir
    ```Javascript
    ibmmfpfanalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: web}

    Todos os dados de analítica capturados e armazenados localmente são enviados para o serviço Mobile Analytics somente quando a API ibmmfpfanalytics.send() é chamada.
    {: web}

7.  Para obter insights sobre os padrões de interação de rede HTTP de seus aplicativos, como APIs HTTP chamadas, número de solicitações e tempos médios de resposta, deve-se usar a classe WLResourceRequest do SDK cliente do Mobile Foundation Service para fazer as chamadas HTTP.  Embora você tenha inicializado Analítica para a captura de Eventos de rede, o uso de `WLResourceRequest` permite que o Mobile Analytics conecte-se às chamadas de rede e capture os dados relevantes.  Faça suas chamadas HTTP usando o código a seguir.
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

8.  Para definir a Analítica customizada e definir seus próprios dados de analítica além do que é suportado inerentemente no SDK do cliente, é possível usar a API de criação de log customizado:

    ```Javascript
        //custom data is sent with the addEvent method
        ibmmfpfanalytics.addEvent({'FromPage':'LoginPage'});

        //Send the captured custom data and log to the Mobile Analytics service
        ibmmfpfanalytics.send();
    ```
    {: codeblock}
    {: web}

    Os dados customizados registrados podem ser plotados sobre gráficos customizados que podem ser definidos no console Mobile Analytics para derivar insights customizados.
    {: web}

## O que fazer em seguida?
{: #next_steps_analytics}

Tente uma amostra simples de [aqui](https://github.com/MobileFirst-Platform-Developer-Center/mfp-analytics-samples).  Em menos de 5 minutos, é possível executar o aplicativo de amostra e obter uma sensação do que a API faz. Também é possível observar como a analítica é mostrada como insights diferentes no console do Analytics.  

O Mobile Foundation Analytics Service fornece APIs de REST para ajudar os desenvolvedores a importarem (POST) e exportarem (GET) dados de analítica.

Experimente a API REST de analítica no Swagger Docs [aqui](https://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/).
