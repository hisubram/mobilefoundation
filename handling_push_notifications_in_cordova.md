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


# Handling Push Notifications in Cordova
{: #handling_push_notifications_in_cordova }

Before iOS, Android and Windows Cordova applications are able to receive and display push notifications, the **cordova-plugin-mfp-push** Cordova plug-in needs to be added to the Cordova project. Once an application has been configured, {{ site.data.keyword.mobilefirst_notm }}-provided Notifications API can be used in order to register &amp; unregister devices, subscribe &amp; unsubscribe tags and handle notifications. In this tutorial, you will learn how to handle push notification in Cordova applications.
{: shortdesc}

Authenticated notifications are currently **not supported** in Cordova applications due to a defect. However a workaround is provided: each `MFPPush` API call can be wrapped by `WLAuthorizationManager.obtainAccessToken("push.mobileclient").then( ... );`.
{: note}

For information about Silent or Interactive notifications in iOS, see:

* [Silent Notifications](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [Interactive Notifications](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)

**Prequisites:**

* {{ site.data.keyword.mfserver_short }} to run locally, or a remotely running {{ site.data.keyword.mfserver_short }}
* {{ site.data.keyword.mfp_cli_long_notm }} installed on the developer workstation
* Cordova CLI installed on the developer workstation

## Notifications Configuration
{: #notifications-configuration }

Create a new Cordova project or use an existing one, and add one or more of the supported platforms: iOS, Android, Windows.

> If the {{ site.data.keyword.mobilefirst_notm }} Cordova SDK is not already present in the project, follow the instructions in the [Adding the {{ site.data.keyword.mobilefirst_notm }} SDK to Cordova applications](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/) tutorial.

## Adding the Push plug-in
{: #adding-the-push-plug-in }

1. From a **command-line** window, navigate to the root of the Cordova project.  
2. Add the push plug-in to by running the command:
    ```bash
    cordova plugin add cordova-plugin-mfp-push
    ```
    {: codeblock}

3. Build the Cordova project by running the command:
    ```bash
    cordova build
    ```
    {: codeblock}

## iOS platform
{: #ios-platform }

The iOS platform requires an additional step.  

In Xcode, enable push notifications for your application in the **Capabilities** screen.

>**Important:** the bundleId selected for the application must match the AppId that you have previously created in the Apple Developer site.

![image of where is the capability in Xcode](images/push-capability.png)

## Android platform
{: #android-platform }

The Android platform requires an additional step.  

In Android Studio, add the following `activity` to the `application` tag:

```xml
<activity android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushNotificationHandler" android:theme="@android:style/Theme.NoDisplay"/>
```
{: codeblock}

## Notifications API
{: #notifications-api }

### Client-side
{: #client-side }

| Javascript Function | Description |
| --- | --- |
| [`MFPPush.initialize(success, failure)`](#initialization) | Initialize the MFPPush instance. | 
| [`MFPPush.isPushSupported(success, failure)`](#is-push-supported) | Does the device support push notifications. | 
| [`MFPPush.registerDevice(options, success, failure)`](#register-device) | Registers the device with the Push Notifications Service. | 
| [`MFPPush.getTags(success, failure)`](#get-tags) | Retrieves all the tags available in a push notification service instance. | 
| [`MFPPush.subscribe(tag, success, failure)`](#subscribe) | Subscribes to a particular tag. | 
| [`MFPPush.getSubsciptions(success, failure)`](#get-subscriptions) | Retrieves the tags device is currently subscribed to | 
| [`MFPPush.unsubscribe(tag, success, failure)`](#unsubscribe) | Unsubscribes from a particular tag. | 
| [`MFPPush.unregisterDevice(success, failure)`](#unregister) | Unregisters the device from the Push Notifications Service | 

### API implementation
{: #api-implementation }

#### Initialization
{: #initialization }

Initialize the **MFPPush** instance.

- Required for the client application to connect to MFPPush service with the right application context.  
- The API method should be called first before using any other MFPPush APIs.
- Registers the callback function to handle received push notifications.

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

#### Is push supported
{: #is-push-supported }

Check if the device supports push notifications.

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

#### Register device
{: #register-device }

Register the device to the push notifications service. If no options are required, options can be set to `null`.

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

#### Get tags
{: #get-tags }

Retrieve all the available tags from the push notification service.

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

#### Subscribe
{: #subscribe }

Subscribe to desired tags.

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

#### Get subscriptions
{: #get-subscriptions }

Retrieve tags the device is currently subscribed to.

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

#### Unsubscribe
{: #unsubscribe }

Unsubscribe from tags.

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

#### Unregister
{: #unregister }

Unregister the device from push notification service instance.

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

## Handling a push notification
{: #handling-a-push-notification }

You can handle a received push notification by operating on its response object in the registered callback function.

```javascript
var notificationReceived = function(message) {
    alert(JSON.stringify(message));
};
```
{: codeblock}
