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

# Handling Push Notifications in Windows 8.1 Universal and Windows 10 UWP
{: #handling_push_notifications_in_windows }

{{ site.data.keyword.mobilefirst_notm }}-provided Notifications API can be used in order to register &amp; unregister devices, and subscribe &amp; unsubscribe to tags. In this tutorial, you will learn how to handle push notification in native Windows 8.1 Universal and Windows 10 UWP applications using C#.

**Prerequisites:**

* {{ site.data.keyword.mfserver_short_notm }} to run locally, or a remotely running {{ site.data.keyword.mfserver_short_notm }}.
* {{ site.data.keyword.mobilefirst_notm  }} CLI installed on the developer workstation

## Notifications Configuration
{: #notifications-configuration }

Create a new Visual Studio project or use an existing one.  

If the {{ site.data.keyword.mobilefirst_notm }} Native Windows SDK is not already present in the project, follow the instructions in the [Adding the {{ site.data.keyword.mobilefirst_notm }} SDK to Windows applications](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/windows-8-10/) tutorial.

## Adding the Push SDK
{: #adding-the-push-sdk }

1. Select Tools → NuGet Package Manager → Package Manager Console.
2. Choose the project where you want to install the {{ site.data.keyword.mobilefirst_notm }} Push component.
3. Add the {{ site.data.keyword.mobilefirst_notm }} Push SDK by running the **Install-Package IBM.MobileFirstPlatformFoundationPush** command.

## Pre-requisite WNS configuration
{: pre-requisite-wns-configuration }

1. Ensure the application is with Toast notification capability. This can be enabled in Package.appxmanifest.
2. Ensure `Package Identity Name` and `Publisher` should be updated with the values registered with WNS.
3. (Optional) Delete TemporaryKey.pfx file.

## Notifications API
{: #notifications-api }

### MFPPush Instance
{: #mfppush-instance }

All API calls must be called on an instance of `MFPPush`.  This can be done by creating a variable such as `private MFPPush PushClient = MFPPush.GetInstance();`, and then calling `PushClient.methodName()` throughout the class.

Alternatively you can call `MFPPush.GetInstance().methodName()` for each instance in which you need to access the push API methods.

### Challenge Handlers
{: #challenge-handlers }

If the `push.mobileclient` scope is mapped to a **security check**, you need to make sure matching **challenge handlers** exist and are registered before using any of the Push APIs.

> Learn more about challenge handlers in the [credential validation](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/windows-8-10/) tutorial.

## Client-side
{: #client-side }

| C Sharp Methods                                                                                                | Description                                                             |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| [`Initialize()`](#initialization)                                                                            | Initializes MFPPush for supplied context.                               |
| [`IsPushSupported()`](#is-push-supported)                                                                    | Does the device support push notifications.                             |
| [`RegisterDevice(JObject options)`](#register-device--send-device-token)                  | Registers the device with the Push Notifications Service.               |
| [`GetTags()`](#get-tags)                                | Retrieves the tag(s) available in a push notification service instance. |
| [`Subscribe(String[] Tags)`](#subscribe)     | Subscribes the device to the specified tag(s).                          |
| [`GetSubscriptions()`](#get-subscriptions)              | Retrieves all tags the device is currently subscribed to.               |
| [`Unsubscribe(String[] Tags)`](#unsubscribe) | Unsubscribes from a particular tag(s).                                  |
| [`UnregisterDevice()`](#unregister)                     | Unregisters the device from the Push Notifications Service              |

### Initialization
{: #initialization }

Initialization is required for the client application to connect to MFPPush service.

* The `Initialize` method should be called first before using any other MFPPush APIs.
* It registers the callback function to handle received push notifications.

```csharp
MFPPush.GetInstance().Initialize();
```
{: codeblock}

### Is push supported
{: #is-push-supported }

Checks if the device supports push notifications.

```csharp
Boolean isSupported = MFPPush.GetInstance().IsPushSupported();

if (isSupported ) {
    // Push is supported
} else {
    // Push is not supported
}
```
{: codeblock}

### Register device &amp; send device token
{: #register-device--send-device-token }

Register the device to the push notifications service.

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

### Get tags
{: #get-tags }

Retrieve all the available tags from the push notification service.

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

### Subscribe
{: #subscribe }

Subscribe to desired tags.

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

### Get subscriptions
{: #get-subscriptions }

Retrieve tags the device is currently subscribed to.

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

### Unsubscribe
{: #unsubscribe }

Unsubscribe from tags.

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

### Unregister
{: #unregister }

Unregister the device from push notification service instance.

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

## Handling a push notification
{: #handling-a-push-notification }

In order to handle a push notification you will need to set up a `MFPPushNotificationListener`.  This can be achieved by implementing the following method.

1. Create a class by using interface of type MFPPushNotificationListener

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

2. Set the class to be the listener by calling `MFPPush.GetInstance().listen(new NotificationListner())`
3. In the onReceive method you will receive the push notification and can handle the notification for the desired behavior.

## Windows Universal Push Notifications Service
{: #windows-universal-push-notifications-service }

No specific port needs to be open in your server configuration.

WNS uses regular http or https requests.

