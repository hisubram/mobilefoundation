---

copyright:
  years: 2018
lastupdated:  "2018-05-11"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

#	Direct Update in Cordova applications
{: #direct_update_cordova_apps}

Web resources (JavaScript, HTML, CSS or image files) in Cordova applications can be updated *over-the-air (OTA)* with Direct Update. Using the Direct Update feature businesses can ensure that the end users use the latest version of their apps.
To update an application, the updated web resources of the application need to be packaged and uploaded to the MobileFirst Server, using the MobileFirst CLI or by deploying a generated archive file. Direct Update is then activated automatically. It is then be enforced on every user request to a protected resource.

Direct Update is supported in the Cordova iOS and Cordova Android platforms.

For development and testing purposes, developers typically use Direct Update by simply uploading an archive to the development server. While this process is easy to implement, it is not secure. For this phase, an internal RSA key pair that is extracted from an embedded MobileFirst self-signed certificate is used.

However, for the live production or pre-production testing phase, it is recommended to implement secure Direct Update before you publish your application to the app store. Secure Direct Update requires an RSA key pair that is extracted from a real CA signed server certificate.

* Take care that you do not modify the keystore configuration after the application is published. Updates downloaded cannot be authenticated before reconfiguring the application with a new public key and republishing the application. If you do not perform the two steps above, Direct Update would fail on the client.
* Direct Update updates only the application’s web resources. To update native resources a new application version must be submitted to the respective app store.
* When you use the Direct Update feature and the web resources checksum feature is enabled, a new checksum base is established with each Direct Update.
* If the MobileFirst Server was upgraded by using a fix pack, it continues to serve direct updates properly. However, if a recently built Direct Update archive (.zip file) is uploaded, it can halt updates to older clients. The reason is that the archive contains the version of the `cordova-plugin-mfp` plug-in. Before it serves that archive to a mobile client, the server compares the client version with the plug-in version. If both versions are close enough (meaning that the three most significant digits are identical), Direct Update occurs normally. Otherwise, MobileFirst Server silently skips the update. One solution for the version mismatch is to download the `cordova-plugin-mfp` with the same version as the one in your original Cordova project and regenerate the Direct Update archive.
* At optimal conditions, a single MobileFirst Server can push data to clients at the rate of 250 MB per second. If higher rates are required, consider a cluster or a CDN service.
{: tip}

## How does Direct Update work?
{: #working_direct_update}

The application web resources are initially packaged with the application to ensure first offline availability. Afterwards, the application checks for updates on every request to the MobileFirst Server.

>**Note:** After a Direct Update is performed, it is checked for again after 60 minutes.

![Diagram of how direct update works](images/internal_function.jpg)

After a Direct Update, the application no longer uses the pre-packaged web resources. Instead, it uses the downloaded web resources from the application’s sandbox. If the application’s cache on the device is cleared, the original packaged web resources are used again.

>A Direct Update applies only to a specific version. In other words, updates generated for an application versioned 2.0 cannot be applied to a different version of the same application.

## Creating and deploying updated web resources
{: #creating_deploying_updates}

The updated web resources needs to be packaged and uploaded to the MobileFirst Server.

1. Open a command-line window and navigate to the root of the Cordova project.
2. Run the command:
  ```
  mfpdev app webupdate
  ```
  {: pre}
The `mfpdev app webupdate` command packages the updated web resources to a `.zip` file and uploads it to the default MobileFirst Server running in the developer workstation. The packaged web resources can be found at the `[cordova-project-root-folder]/mobilefirst/` folder.

**Alternative steps:**

* Build the `.zip` file and upload it to a different MobileFirst Server:  `mfpdev app webupdate [server-name] [runtime-name]`.
  For example:
  ```
  mfpdev app webupdate myQAServer MyBankApps
  ```
  {: pre}

* Upload a previously generated `.zip` file: `mfpdev app webupdate [server-name] [runtime-name] --file [path-to-packaged-web-resources]`.
  For example:
  ```
  mfpdev app webupdate myQAServer MyBankApps --file mobilefirst/ios/com.mfp.myBankApp-1.0.1.zip
  ```
  {: pre}

* Manually upload packaged web resources to the MobileFirst Server:
  1. Build the .zip file without uploading it:
      ```
      mfpdev app webupdate --build
      ```
      {: pre}
  2. Load the MobileFirst Operations Console and click the application entry.
  3. Click **Upload Web Resources File** to upload the packaged web resources.    
      ![Upload Direct Update .zip file from the console](images/upload-direct-update-package.png)

Run the command `mfpdev help app webupdate` to learn more.
{: tip}

## User experience
{: #user_experience}

After a Direct Update is received a dialog is displayed, by default, and the user is asked permission to start the update process. After the user approves a progress bar dialog is displayed and the web resources are downloaded. The application is automatically reloaded after the update is complete.

![Direct update example](images/direct-update-flow.png)

## Customizing the Direct Update UI
{: #customize_du_ui}

Direct Update UI presented to the end-user can be customized.
Add the following inside the `wlCommonInit()` function in **index.js**:
```JavaScript
wl_DirectUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext) {
    // Implement custom Direct Update logic
};
```
{: codeblock}

*directUpdateData* is a JSON object containing the downloadSize property that represents the file size (in bytes) of the update package to be downloaded from MobileFirst Server.
*directUpdateContext* is a JavaScript object exposing the .start() and .stop() functions, which start and stop the Direct Update flow.

If the web resources are newer on the MobileFirst Server than in the application, Direct Update challenge data is added to the server response. Whenever the MobileFirst client-side framework detects this direct update challenge, it invokes the `wl_directUpdateChallengeHandler.handleDirectUpdate` function.

The function provides a default Direct Update design: A default message dialog that is displayed when a Direct Update is available and a default progress screen that is displayed when the direct update process is initiated. You can implement custom Direct Update user interface behaviour or customise the Direct Update dialog box by overriding this function and implementing your own logic.

In the example code below, a `handleDirectUpdate` function implements a custom message in the Direct Update dialog. Add this code into the `www/js/index.js` file of the Cordova project.
Additional examples for a customized Direct Update UI:
* A dialog that is created by using a third-party JavaScript framework (such as Dojo or jQuery Mobile, Ionic, …)
* Fully native UI by executing a Cordova plug-in
* An alternate HTML that is presented to the user with options
and so on.

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext) {        
    navigator.notification.confirm(  // Creates a dialog.
        'Custom dialog body text',
        // Handle dialog buttons.
          directUpdateContext.start();
        },
        'Custom dialog title text',
        ['Update']
    );
};
```
{: codeblock}

You can start the Direct Update process by running the `directUpdateContext.start()` method whenever the user clicks the dialog button. The default progress screen, which resembles the one in previous versions of MobileFirst Server is shown.

This method supports the following types of invocation:
* When no parameters are specified, the MobileFirst Server uses the default progress screen.
* When a listener function such as `directUpdateContext.start(directUpdateCustomListener)` is supplied, the Direct Update process runs in the background while the process sends lifecycle events to the listener. The custom listener must implement the following methods:

```JavaScript
var  directUpdateCustomListener  = {
    onStart : function ( totalSize ){ },
    onProgress : function ( status , totalSize , completedSize ){ },
    onFinish : function ( status ){ }
};
```
{: codeblock}

The listener methods are started during the direct update process according to following rules:
* `onStart` is called with the `totalSize` parameter that holds the size of the update file.
* `onProgress` is called multiple times with status `DOWNLOAD_IN_PROGRESS`, `totalSize`, and `completedSize` (the volume that is downloaded so far).
* `onProgress` is called with status `UNZIP_IN_PROGRESS`.
* `onFinish` is called with one of the following final status codes:

| Status code | Description |
|:------------|:------------|
| `SUCCESS` | Direct update finished with no errors. |
| `CANCELED` | Direct update was canceled (for example, because the `stop()` method was called). |
| `FAILURE_NETWORK_PROBLEM` | There was a problem with a network connection during the update. |
| `FAILURE_DOWNLOADING` | The file was not downloaded completely. |
| `FAILURE_NOT_ENOUGH_SPACE` | There is not enough space on the device to download and unpack the update file. |
| `FAILURE_UNZIPPING` | There was a problem unpacking the update file. |
| `FAILURE_ALREADY_IN_PROGRESS` | The start method was called while direct update was already running. |
| `FAILURE_INTEGRITY` | Authenticity of update file cannot be verified. |
| `FAILURE_UNKNOWN` | Unexpected internal error. |

If you implement a custom direct update listener, you must ensure that the app is reloaded when the direct update process is complete and the `onFinish()` method has been called. You must also call `wl_directUpdateChalengeHandler.submitFailure()` if the direct update process fails to complete successfully.

The following example shows an implementation of a custom direct update listener:
```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
    //show custom progress dialog
  },
  onProgress: function(status,totalSize,completedSize){
    //update custom progress dialog
  },
  onFinish: function(status){

    if (status == 'SUCCESS'){
      //show success message
      WL.Client.reloadApp();
    }
    else {
      //show custom error message

      //submitFailure must be called is case of error
      wl_directUpdateChallengeHandler.submitFailure();
    }
  }
};

wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  WL.SimpleDialog.show('Update Avalible', 'Press update button to download version 2.0', [{
    text : 'update',
    handler : function() {
      directUpdateContext.start(directUpdateCustomListener);
    }
  }]);
};
```
{: codeblock}

### Scenario: Running UI-less direct updates
{: #scenario-running-ui-less-direct-updates }
{{site.data.keyword.mobilefoundation_short}} supports UI-less direct update when the application is in the foreground.

To run UI-less direct updates, implement `directUpdateCustomListener`. Provide empty function implementations to the `onStart` and `onProgress` methods. Empty implementations cause the direct update process to run in the background.

To complete the direct update process, the application must be reloaded. The following options are available:
* The `onFinish` method can be empty as well. In this case, direct update will apply after the application has restarted.
* You can implement a custom dialog that informs or requires the user to restart the application. (See the following example.)
* The `onFinish` method can enforce a reload of the application by calling `WL.Client.reloadApp()`.

Here is an example implementation of `directUpdateCustomListener`:

```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
  },
  onProgress: function(status,totalSize,completeSize){
  },
  onFinish: function(status){
    WL.SimpleDialog.show('New Update Available', 'Press reload button to update to new version', [ {
      text : WL.ClientMessages.reload,
      handler : WL.Client.reloadApp
    }]);
  }
};
```
{: codeblock}

Implement the `wl_directUpdateChallengeHandler.handleDirectUpdate` function. Pass the `directUpdateCustomListener` implementation that you have created as a parameter to the function. Make sure `directUpdateContext.start(directUpdateCustomListener`) is called. Here is an example `wl_directUpdateChallengeHandler.handleDirectUpdate` implementation:

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  directUpdateContext.start(directUpdateCustomListener);
};
```
{: codeblock}

**Note:** When the application is sent to the background, the direct-update process is suspended.

### Scenario: Handling a direct update failure
{: #scenario-handling-a-direct-update-failure }
This scenario shows how to handle a direct update failure that might be caused, for example, by loss of connectivity. In this scenario, the user is prevented from using the app even in offline mode. A dialog is displayed offering the user the option to try again.

Create a global variable to store the direct update context so that you can use it subsequently when the direct update process fails. For example:

```JavaScript
var savedDirectUpdateContext;
```
{: codeblock}

Implement a direct update challenge handler. Save the direct update context here. For example:

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  savedDirectUpdateContext = directUpdateContext; // save direct update context

  var downloadSizeInMB = (directUpdateData.downloadSize / 1048576).toFixed(1).replace(".", WL.App.getDecimalSeparator());
  var directUpdateMsg = WL.Utils.formatString(WL.ClientMessages.directUpdateNotificationMessage, downloadSizeInMB);

  WL.SimpleDialog.show(WL.ClientMessages.directUpdateNotificationTitle, directUpdateMsg, [{
    text : WL.ClientMessages.update,
    handler : function() {
      directUpdateContext.start(directUpdateCustomListener);
    }
  }]);
};
```
{: codeblock}

Create a function that starts the direct update process by using the direct update context. For example:

```JavaScript
restartDirectUpdate = function () {
  savedDirectUpdateContext.start(directUpdateCustomListener); // use saved direct update context to restart direct update
};
```
{: codeblock}

Implement `directUpdateCustomListener`. Add status checking in the `onFinish` method. If the status starts with `FAILURE`, open a modal only dialog with the option **Try Again**. For example:

```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
    alert('onStart: totalSize = ' + totalSize + 'Byte');
  },
  onProgress: function(status,totalSize,completeSize){
    alert('onProgress: status = ' + status + ' completeSize = ' + completeSize + 'Byte');
  },
  onFinish: function(status){
    alert('onFinish: status = ' + status);
    var pos = status.indexOf("FAILURE");
    if (pos > -1) {
      WL.SimpleDialog.show('Update Failed', 'Press try again button', [ {
        text : "Try Again",
        handler : restartDirectUpdate // restart direct update
      }]);
    }
  }
};
```
{: codeblock}

When the user clicks the **Try Again** button, the application restarts the direct update process.

## Delta and Full Direct Update
{: #delta-and-full-direct-update }
Delta Direct Updates enables an application to download only the files that were changed since the last update instead of the entire web resources of the application. This reduces download time, conserves bandwidth, and improves overall user experience.

> <span class="glyphicon glyphicon-exclamation-sign" aria-hidden="true"></span> **Important:** A **delta update** is possible only if the client application's web resources are one version behind the application that is currently deployed on the server. Client applications that are more than one version behind the currently deployed application (meaning the application was deployed to the server at least twice since the client application was updated), receive a **full update** (meaning that the entire web resources are downloaded and updated).


## Sample application
{: #sample-application }
[Click to download ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/MobileFirst-Platform-Developer-Center/CustomDirectUpdate/tree/release80) the Cordova project.  

### Sample usage
{: #sample-usage }
Follow the sample's README.md file for instructions.

## CDN support
{: #cdn_support}

You can configure Direct Update requests to be served from a CDN (content delivery network) instead of from the MobileFirst Server.

### Advantages of using a CDN
{: #advantages-of-using-a-cdn }
Using a CDN instead of the MobileFirst Server to serve Direct Update requests has the following advantages:

* Removes network overheads from the MobileFirst Server.
* Increases transfer rates higher than the 250 MB/second limit when serving requests from a MobileFirst Server.
* Ensures a more uniform Direct Update experience for all users regardless of their geographical location.

### General requirements
{: #general-requirements }
To serve Direct Update requests from a CDN, ensure that your configuration conforms to the following conditions:

* The CDN must be a reverse proxy in front of the MobileFirst Server (or in front of another reverse proxy if needed).
* When building the application from your development environment, set up your target server to the CDN host and port instead of the host and port of the MobileFirst Server. For example, when running the MobileFirst CLI command `mfpdev server add`, provide the CDN host and port.
* In the CDN administration panel, you need to mark the following Direct Update URLs for caching to ensure that the CDN passes all requests to the MobileFirst Server except for the Direct Update requests. For Direct Update requests, the CDN determines whether it obtained the content. If it has, it returns it without going to the MobileFirst Server; if not, it goes to the MobileFirst Server, gets the Direct Update archive (.zip file), and stores it for the next requests for that specific URL. For applications that are built with v8.0 of {{site.data.keyword.mobilefoundation_short}}, the Direct Update URL is: `PROTOCOL://DOMAIN:PORT/CONTEXT_PATH/api/directupdate/VERSION/CHECKSUM/TYPE`.
The `PROTOCOL://DOMAIN:PORT/CONTEXT_PATH` prefix is constant for all runtime requests. For example: `http://my.cdn.com:9080/mfp/api/directupdate/0.0.1/742914155/full?appId=com.ibm.DirectUpdateTestApp&clientPlatform=android`

In the example, there are additional request parameters that are also part of the request.

* The CDN must allow caching of the request parameters. Two different Direct Update archives might differ only by the request parameters.
* The CDN must support TTL on the Direct Update response. The support is needed to support multiple direct updates for the same version.
* The CDN must not change or remove the HTTP headers that are used in the server-client protocol.

### Example CDN configuration
{: #example-cdn-configuration }
This example is based on using an Akamai CDN configuration that caches the Direct Update archive. The following tasks are completed by the network administrator, the MobileFirst administrator, and the Akamai administrator:

#### Network administrator
{: #network-administrator }
Create another domain in the DNS for your MobileFirst Server. For example, if your server domain is `yourcompany.com` you need to create an additional domain such as `cdn.yourcompany.com`.
In the DNS for the new `cdn.yourcompany.com` domain, set a `CNAME` to the domain name that is provided by Akamai. For example, `yourcompany.com.akamai.net`.

#### MobileFirst administrator
{: #mobilefirst-administrator }
Set the new `cdn.yourcompany.com` domain as the MobileFirst Sever URL for the MobileFirst applications. For example, for the Ant builder task, the property is:
```xml
<property name="wl.server" value="http://cdn.yourcompany.com/${contextPath}/"/>
```
{: codeblock}

#### Akamai administrator
{: #akamai-administrator }
1. Open the Akamai property manager and set the property **host name** to the value of the new domain.

    ![Set the property host name to the value of the new domain](images/direct_update_cdn_3.jpg)

2. On the Default Rule tab, configure the original MobileFirst Server host and port, and set the **Custom Forward Host Header** value to the newly created domain.

    ![Set the Custom Forward Host Header value to the newly created domain](images/direct_update_cdn_4.jpg)

3. From the **Caching Option** list, select **No Store**.

    ![From the Caching Option list, select No Store](images/direct_update_cdn_5.jpg)

4. From the **Static Content configuration** tab, configure the matching criteria according to the Direct Update URL of the application. For example, create a condition that states `If Path matches one of direct_update_URL`.

    ![Configure the matching criteria according to the Direct Update URL of the application](images/direct_update_cdn_6.jpg)

5. Configure the cache key behavior to use all request parameters in the cache key (you must do so to cache different Direct Update archives for different applications or versions). For example, from the **Behavior** list, select `Include all parameters (preserve order from request)`.

    ![Configure the cache key behavior to use all request parameters in the cache key](images/direct_update_cdn_8.jpg)

6. Set values similar to the following values to configure the caching behavior to make cache the Direct Update URL and to set TTL.

      ![Set values to configure the caching behavior](images/direct_update_cdn_7.jpg)

| Field | Value |
|:------|:------|
| Caching Option | Cache |
| Force Revaluation of Stale Objects | Serve stale if unable to validate |
| Max-Age | 3 minutes |


## Secure Direct Update
{: #secure-dc }

Disabled by default, Secure Direct Update prevents a 3rd-party attacker from altering the web resources that are transmitted from the MobileFirst Server (or from a Content Delivery Network (CDN)) to the client application.

**To enable Direct Update authenticity:**  
Using a preferred tool, extract the public key from the MobileFirst Server keystore and convert it to base64.  
The produced value should then be used as instructed below:

1. Open a **Command-line** window and navigate to the root of the Cordova project.
2. Run the command: `mfpdev app config` and select the **Direct Update Authenticity public key** option.
3. Provide the public key and confirm.

Any future Direct Update deliveries to client applications will be protected by Direct Update authenticity.

For secure Direct Update to work, a user-defined keystore file must be deployed in MobileFirst Server and a copy of the matching public key must be included in the deployed client application.

This topic describes how to bind a public key to new client applications and existing client applications that were upgraded. For more information on configuring the keystore in MobileFirst Server, see [Configuring the MobileFirst Server keystore ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/configuring-the-mobilefirst-server-keystore/){: new_window}

The server provides a built-in keystore that can be used for testing secure Direct Update for development phases.

>**Note:** After you bind the public key to the client application and rebuild it, you do not need to upload it again to the {{ site.data.keys.mf_server }}. However, if you previously published the application to the market, without the public key, you must republish it.

For development purposes, the following default, dummy public key is provided with MobileFirst Server:

```xml
-----BEGIN PUBLIC KEY-----
MIIDPjCCAiagAwIBAgIEUD3/bjANBgkqhkiG9w0BAQsFADBgMQswCQYDVQQGEwJJTDELMAkGA1UECBMCSUwxETA
PBgNVBAcTCFNoZWZheWltMQwwCgYDVQQKEwNJQk0xEjAQBgNVBAsTCVdvcmtsaWdodDEPMA0GA1UEAxMGV0wgRG
V2MCAXDTEyMDgyOTExMzkyNloYDzQ3NTAwNzI3MTEzOTI2WjBgMQswCQYDVQQGEwJJTDELMAkGA1UECBMCSUwxE
TAPBgNVBAcTCFNoZWZheWltMQwwCgYDVQQKEwNJQk0xEjAQBgNVBAsTCVdvcmtsaWdodDEPMA0GA1UEAxMGV0wg
RGV2MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzQN3vEB2/of7KAvuvyoIt0T7cjaSTjnOBm0N3+q
zx++dh92KpNJXj/a3o4YbwJXkJ7jU8ykjCYvjXRf0hme+HGhiIVwxJo54iqh76skDS5m7DaseFdndZUJ4p7NFVw
I5ixA36ZArSZ/Pn/ej56/RRjBeRI7AEGXUSGojBUPA6J6DYkwaXQRew9l+Q1kj4dTigyKL5Os0vNFaQyYu+bT2E
vnOixQ0DXm94IqmHZamZKbZLrWcOEfuAsSjKYOdMSM9jkCiHaKcj7fpEZhUxRRs7joKs1Ri4ihs6JeUvMEiG4gK
l9V3FP/Huy0pfkL0F8xMHgaQ4c/lxS/s3PV0OEg+7wIDAQABMA0GCSqGSIb3DQEBCwUAA4IBAQAgEhhqRl2Rgkt
MJeqOCRcT3uyr4XDK3hmuhEaE0nOvLHi61PoLKnDUNryWUicK/W+tUP9jkN5xRckdzG6TJ/HPySmZ7Adr6QRFu+
xcIMY+/S8j4PHLXBjoqgtUMhkt7S2/thN/VA6mwZpw4Ol0Pa2hyT2TkhQoYYkRwYCk9pxmuBCoH/eCWpSxquNny
RwrY25x0YzccXUaMI8L3/3hzq3mW40YIMiEdpiD5HqjUDpzN1funHNQdsxEIMYsWmGAwOdV5slFzyrH+ErUYUFA
pdGIdLtkrhzbqHFwXE0v3dt+lnLf21wRPIqYHaEu+EB/A4dLO6hm+IjBeu/No7H7TBFm
-----END PUBLIC KEY-----
```
{: codeblock}

** Important:** Do not use the public key for production purposes.

### Generating and deploying the keystore
{: #generating-and-deploying-the-keystore }
There are many tools available for generating certificates and extracting public keys from a keystore. The following example demonstrates the procedures with the JDK keytool utility and openSSL.

1. Extract the public key from the keystore file that is deployed in the {{ site.data.keys.mf_server }}.  
   >**Note:** The public key must be Base64 encoded.

   For example, assume that the alias name is `mfp-server` and the keystore file is **keystore.jks**.  
   To generate a certificate, issue the following command:

   ```bash
   keytool -export -alias mfp-server -file certfile.cert
   -keystore keystore.jks -storepass keypassword
   ```
   {: codeblock}

   A certificate file is generated.  
   Issue the following command to extract the public key:

   ```bash
   openssl x509 -inform der -in certfile.cert -pubkey -noout
   ```
   {: codeblock}

   > **Note:** Keytool alone cannot extract public keys in Base64 format.

2. Perform one of the following procedures:
    * Copy the resulting text, without the `BEGIN PUBLIC KEY` and `END PUBLIC KEY` markers into the mfpclient property file of the application, immediately after `wlSecureDirectUpdatePublicKey`.
    * From the command prompt, issue the following command: `mfpdev app config direct_update_authenticity_public_key <public_key>`

    For `<public_key>`, paste the text that results from Step 1, without the `BEGIN PUBLIC KEY` and `END PUBLIC KEY` markers.

3. Run the cordova build command to save the public key in the application.
