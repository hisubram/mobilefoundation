---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-28"

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


# Handling Push Notifications in Android
{: #handling_push_notifications_in_android }

Before Android applications are able to handle any received push notifications, support for Google Play Services needs to be configured. Once an application has been configured, {{ site.data.keyword.mobilefirst_notm }}-provided Notifications API can be used in order to register &amp; unregister devices, and subscribe &amp; unsubscribe to tags. In this tutorial, you will learn how to handle push notification in Android applications.

**Prerequisites:**

* {{ site.data.keyword.mfserver_short_notm }} to run locally, or a remotely running {{ site.data.keyword.mfserver_short_notm }}.
* {{ site.data.keyword.mobilefirst_notm  }} CLI installed on the developer workstation


## Notifications Configuration
{: #notifications-configuration }

Create a new Android Studio project or use an existing one.  
If the {{ site.data.keyword.mobilefirst_notm }} Native Android SDK is not already present in the project, follow the instructions in the [Adding the {{ site.data.keyword.mobilefoundation_short }} SDK to Android applications](https://cloud.ibm.com/docs/services/mobilefoundation/add_sdk_to_app.html#add_sdk_to_app) tutorial.

### Project setup
{: #project-setup }

1. In **Android → Gradle scripts**, select the **build.gradle (Module: app)** file and add the following lines to `dependencies`:

    ```bash
    com.google.android.gms:play-services-gcm:9.0.2
    ```
    {: codeblock}

    There is a [known Google defect](https://code.google.com/p/android/issues/detail?id=212879) preventing use of the latest Play Services version (currently at 9.2.0). Use a lower version.
    {: note}

    And:

    ```xml
    compile group: 'com.ibm.mobile.foundation',
             name: 'ibmmobilefirstplatformfoundationpush',
             version: '8.0.+',
             ext: 'aar',
             transitive: true
    ```
    {: codeblock}

    Or in a single line:

    ```xml
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationpush:8.0.+'
    ```
    {: codeblock}

2. In **Android → app → manifests**, open the `AndroidManifest.xml` file.

    * Add the following permissions to the top the `manifest` tag:

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
 
    * Add the following to the `application` tag:

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

        Be sure to replace `your.application.package.name` with the actual package name of your application.
        {: note}

    * Add the following `intent-filter` to the application's activity.

        ```xml
        <intent-filter>
            <action android:name="your.application.package.name.IBMPushNotification" />
            <category android:name="android.intent.category.DEFAULT" />
        </intent-filter>
        ```
        {: codeblock}

## Notifications API
{: #notifications-api }

### MFPPush Instance
{: #mfppush-instance }

All API calls must be called on an instance of `MFPPush`.  This can be done by creating a class level field such as `private MFPPush push = MFPPush.getInstance();`, and then calling `push.<api-call>` throughout the class.

Alternatively you can call `MFPPush.getInstance().<api_call>` for each instance in which you need to access the push API methods.

### Challenge Handlers
{: #challenge-handlers }

If the `push.mobileclient` scope is mapped to a **security check**, you need to make sure matching **challenge handlers** exist and are registered before using any of the Push APIs.

> Learn more about challenge handlers in the [credential validation](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/android/) tutorial.

### Client-side
{: #client-side }

| Java Methods | Description |
|-----------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| [`initialize(Context context);`](#initialization) | Initializes MFPPush for supplied context. |
| [`isPushSupported();`](#is-push-supported) | Does the device support push notifications. |
| [`registerDevice(JSONObject, MFPPushResponseListener);`](#register-device) | Registers the device with the Push Notifications Service. |
| [`getTags(MFPPushResponseListener)`](#get-tags) | Retrieves the tag(s) available in a push notification service instance. |
| [`subscribe(String[] tagNames, MFPPushResponseListener)`](#subscribe) | Subscribes the device to the specified tag(s). |
| [`getSubscriptions(MFPPushResponseListener)`](#get-subscriptions) | Retrieves all tags the device is currently subscribed to. |
| [`unsubscribe(String[] tagNames, MFPPushResponseListener)`](#unsubscribe) | Unsubscribes from a particular tag(s). |
| [`unregisterDevice(MFPPushResponseListener)`](#unregister) | Unregisters the device from the Push Notifications Service |

#### Initialization
{: #initialization }

Required for the client application to connect to MFPPush service with the right application context.

* The API method should be called first before using any other MFPPush APIs.
* Registers the callback function to handle received push notifications.

```java
MFPPush.getInstance().initialize(this);
```
{: codeblock}

#### Is push supported
{: #is-push-supported }

Checks if the device supports push notifications.

```java
Boolean isSupported = MFPPush.getInstance().isPushSupported();

if (isSupported ) {
    // Push is supported
} else {
    // Push is not supported
}
```
{: codeblock}

#### Register device
{: #register-device }

Register the device to the push notifications service.

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

#### Get tags
{: #get-tags }

Retrieve all the available tags from the push notification service.

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

#### Subscribe
{: #subscribe }

Subscribe to desired tags.

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

#### Get subscriptions
{: #get-subscriptions }

Retrieve tags the device is currently subscribed to.

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

#### Unsubscribe
{: #unsubscribe }

Unsubscribe from tags.

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

#### Unregister
{: #unregister }

Unregister the device from push notification service instance.

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

## Handling a push notification
{: #handling-a-push-notification }

In order to handle a push notification you will need to set up a `MFPPushNotificationListener`.  This can be achieved by implementing one of the following methods.

### Option One
{: #option-one }

In the activity in which you wish the handle push notifications.

1. Add `implements MFPPushNofiticationListener` to the class declaration.
2. Set the class to be the listener by calling `MFPPush.getInstance().listen(this)` in the `onCreate` method.
2. Then you will need to add the following *required* method:

    ```java
    @Override
    public void onReceive(MFPSimplePushNotification mfpSimplePushNotification) {
         // Handle push notification here
    }
    ```
    {: codeblock}

3. In this method you will receive the `MFPSimplePushNotification` and can handle the notification for the desired behavior.

### Option Two
{: #option-two }

Create a listener by calling `listen(new MFPPushNofiticationListener())` on an instance of `MFPPush` as outlined below:

```java
MFPPush.getInstance().listen(new MFPPushNotificationListener() {
    @Override
    public void onReceive(MFPSimplePushNotification mfpSimplePushNotification) {
        // Handle push notification here
    }
});
```
{: codeblock}

## Migrate your client Apps on Android to FCM
{: #migrate-to-fcm }

Google Cloud Messaging (GCM) has been [deprecated](https://developers.google.com/cloud-messaging/faq) and is integrated with Firebase Cloud Messaging (FCM). Google will turn off most GCM services by April 2019.

If you are using a GCM project, [then migrate the GCM client apps on Android to FCM](https://developers.google.com/cloud-messaging/android/android-migrate-fcm) .

For now, the existing applications using GCM services will continue to work as-is. Since the Push Notifications service has been updated to use the FCM endpoints, going forward all the new applications must be using FCM.

After migrating to FCM, update your project to use FCM credentials instead of the old GCM credentials.
{: note}

### FCM Project Setup
{: #fcm_project_setup}

Setting up an application in FCM is a bit different compared to the old GCM model.

1. Obtain your notification provider credentials, create a FCM project and add the same to your Android application. Include the package name of your application as `com.ibm.mobilefirstplatform.clientsdk.android.push`. Refer the [documentation here](https://cloud.ibm.com/docs/services/mobilepush/push_step_1.html#push_step_1_android) , until the step where you have finished generating the `google-services.json` file

2. Configure your Gradle file. Add the following in the app's `build.gradle` file

    ```xml
    dependencies {
       ......
       compile 'com.google.firebase:firebase-messaging:10.2.6'
       .....
    }
    
    apply plugin: 'com.google.gms.google-services'
    ```
    {: codeblock}
    
    - Add the following dependency in the root build.gradle's `buildscript` section

      `classpath 'com.google.gms:google-services:3.0.0'`
      {: codeblock}

    - Remove below GCM plugin from build.gradle file `compile  com.google.android.gms:play-services-gcm:+`

 3. Configure the AndroidManifest file. Following changes are required in the `AndroidManifest.xml`

    **Remove the following entries :**

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

    **The below entries needs modification:**

    ```xml
        <service android:exported="true" android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushIntentService">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            </intent-filter>
        </service>
    ```

    **Modify the entries to :**

    ```xml
        <service android:exported="true" android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushIntentService">
            <intent-filter>
                <action android:name="com.google.firebase.MESSAGING_EVENT" />
            </intent-filter>
        </service>
    ```
    {: codeblock}

    **Add the following entry :**

    ```xml
        <service android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPush"
                android:exported="true">
                <intent-filter>
                    <action android:name="com.google.firebase.INSTANCE_ID_EVENT" />
                </intent-filter>
        </service>
    ```
    {: codeblock}

 4. Open the app in Android Studio. Copy the `google-services.json` file that you have created in the **step-1** inside the app directory. Note that the `google-service.json` file includes the package name you have added.		
 5. Compile the SDK. Build the application.
