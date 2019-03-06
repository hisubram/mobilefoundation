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


# Handling Push Notifications in iOS
{: #handling_push_notifications_in_ios }

{{ site.data.keyword.mobilefirst_notm }}-provided Notifications API can be used in order to register &amp; unregister devices, and subscribe &amp; unsubscribe to tags. In this tutorial, you will learn how to handle push notification in iOS applications using Swift.
{: shortdesc}

For information about Silent or Interactive notifications, see:

* [Silent Notifications](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [Interactive Notifications](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)

**Prerequisites:**

* {{ site.data.keyword.mfserver_short }} to run locally, or a remotely running {{ site.data.keyword.mfserver_short }}.
* {{ site.data.keyword.mfp_cli_long_notm }} installed on the developer workstation

## Notifications Configuration
{: #notifications-configuration }

Create a new Xcode project or use and existing one.
If the {{ site.data.keyword.mobilefirst_notm }} Native iOS SDK is not already present in the project, follow the instructions in the [Adding the {{ site.data.keyword.mobilefoundation_short }} SDK to iOS applications](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/) tutorial.


## Adding the Push SDK
{: #adding-the-push-sdk }

1. Open the project's existing **podfile** and add the following lines:

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

    - Replace **Xcode-project-target** with the name of your Xcode project's target.

2. Save and close the **podfile**.
3. From a **Command-line** window, navigate into to the project's root folder.
4. Run the command `pod install`
5. Open project using the **.xcworkspace** file.

## Notifications API
{: #notifications-api }

### MFPPush Instance
{: #mfppush-instance }

All API calls must be called on an instance of `MFPPush`. This can be done by using a `var` in a view controller such as `var push = MFPPush.sharedInstance();`, and then calling `push.methodName()` throughout the view controller.

Alternatively you can call `MFPPush.sharedInstance().methodName()` for each instance in which you need to access the push API methods.

## Challenge Handlers
{: #challenge-handlers }

If the `push.mobileclient` scope is mapped to a **security check**, you need to make sure matching **challenge handlers** exist and are registered before using any of the Push APIs.

> Learn more about challenge handlers in the [credential validation](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/ios/) tutorial.

## Client-side
{: #client-side }

| Swift Methods | Description  |
|---------------|--------------|
| [`initialize()`](#initialization) | Initializes MFPPush for supplied context. |
| [`isPushSupported()`](#is-push-supported) | Does the device support push notifications. |
| [`registerDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#register-device--send-device-token) | Registers the device with the Push Notifications Service.|
| [`sendDeviceToken(deviceToken: NSData!)`](#register-device--send-device-token) | Sends the device token to the server |
| [`getTags(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-tags) | Retrieves the tag(s) available in a push notification service instance. |
| [`subscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#subscribe) | Subscribes the device to the specified tag(s). |
| [`getSubscriptions(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-subscriptions)  | Retrieves all tags the device is currently subscribed to. |
| [`unsubscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unsubscribe) | Unsubscribes from a particular tag(s). |
| [`unregisterDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unregister) | Unregisters the device from the Push Notifications Service              |

### Initialization
{: #initialization }

Initialization is required for the client application to connect to MFPPush service.

* The `initialize` method should be called first before using any other MFPPush APIs.
* It registers the callback function to handle received push notifications.

```swift
MFPPush.sharedInstance().initialize();
```
{: codeblock}

### Is push supported
{: #is-push-supported }

Checks if the device supports push notifications.

```swift
let isPushSupported: Bool = MFPPush.sharedInstance().isPushSupported()

if isPushSupported {
    // Push is supported
} else {
    // Push is not supported
}
```
{: codeblock}

### Register device &amp; send device token
{: #register-device--send-device-token }

Register the device to the push notifications service.

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

<!--`options` = `[NSObject : AnyObject]` which is an optional parameter that is a dictionary of options to be passed with your register request, sends the device token to the server to register the device with its unique identifier.-->

```swift
MFPPush.sharedInstance().sendDeviceToken(deviceToken)
```
{: codeblock}

This is typically called in the **AppDelegate** in the `didRegisterForRemoteNotificationsWithDeviceToken` method.
{: note}

### Get tags
{: #get-tags }

Retrieve all the available tags from the push notification service.

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

### Subscribe
{: #subscribe }

Subscribe to desired tags.

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

### Get subscriptions
{: #get-subscriptions }

Retrieve tags the device is currently subscribed to.

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

### Unsubscribe
{: #unsubscribe }

Unsubscribe from tags.

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

### Unregister
{: #unregister }

Unregister the device from push notification service instance.

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

## Handling a push notification
{: #handling-a-push-notification }

Push notifications are handled by the native iOS framework directly. Depending on your application lifecyle, different methods will be called by the iOS framework.

For example if a simple notification is received while the application is running, **AppDelegate**'s `didReceiveRemoteNotification` will be triggered:

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

> Learn more about handling notifications in iOS from the Apple documentation: [http://bit.ly/1ESSGdQ](http://bit.ly/1ESSGdQ)
