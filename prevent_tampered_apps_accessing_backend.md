---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-19"

keywords: mobile foundation security, restrict backend access, tampered apps

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

# Prevent tampered apps accessing backend
{: #prevent_tampered_apps_accessing_backend}

Application authenticity helps to check the application validity before enabling any services, thus preventing tampered application from accessing the backend services.
{: shortdesc}

To properly secure your application, enable the predefined MobileFirst application-authenticity security check (``appAuthenticity``). When enabled, this check validates the authenticity of the application before providing it with any services. Applications in production environment should have this feature enabled.

To enable application authenticity, you can either follow the on-screen instructions in the **MobileFirst Operations Console → [your-application] → Authenticity**, or review the information below.

* **Availability**

    Application authenticity is available in all supported platforms (iOS, Android, Windows 8.1 Universal, Windows 10 UWP) in both Cordova and native applications.

* **Limitations**

    Application authenticity does not support **Bitcode** in iOS. Therefore, before using application authenticity, disable Bitcode in the Xcode project properties.

## Application Authenticity Flow
{: #appauthenticityflow}

The application-authenticity security check is run during the application’s registration to MobileFirst Server, which occurs the first time an instance of the application attempts to connect to the server. By default the authenticity check does not run again.

Once app authenticity is enabled and if customer needs to introduce any changes in their application, then the application version needs to be upgraded.

See [Configuring application authenticity](#configappauthenticity) to learn how to customize this behavior.

### Enabling Application Authenticity
{: #enableappauthenticity}

For application authenticity to be enabled in your application:

1. Open the MobileFirst Operations Console in your favorite browser.
2. Select your application from the navigation sidebar and click on the **Authenticity** menu item.
3. Toggle the **On/Off** button in the **Status** box.

![Enabling Application Authenticity](/images/enable_application_authenticity.png)

MobileFirst Server validates the application’s authenticity on the first attempt to connect to the server. To apply this validation also to protected resources, add the appAuthenticity security check to the protecting scope.

### Disabling Application Authenticity
{: #disableappauthenticity}

Some changes to the application during development might cause it to fail the authenticity validation. Accordingly, it is recommended to disable application authenticity during the development process. Applications in production environment should have this feature enabled.

To disable application authenticity, toggle back the **On/Off** button in the **Status** box.

## Configuring Application Authenticity
{: #configappauthenticity}

By default, Application Authenticity is checked only during client registration. However, just like any other security check, you can decide to protect your application or resources with the ``appAuthenticity`` security check from the console, following the instructions under Protecting resources.

You can configure the predefined application-authenticity security check with the following property:

* ``expirationSec``: Defaults to 3600 seconds / 1 hour. Defines the duration until the authenticity token expires.

After an authenticity check has completed, it does not occur again until the token has expired based on the set value.

To configure the ``expirationSec`` property:

1. Load the MobileFirst Operations Console, navigate to **[your application] → Security → Security-Check Configurations**, and click on **New**.
2. Search for the ``appAuthenticity`` scope element.
3. Set a new value in seconds.

    ![Configuring expiration in number of seconds](/images/configuring_expirationSec.png)

## Build Time Secret (BTS)
{: #buildtimesecret}

The Build Time Secret (BTS) is an **optional tool to enhance authenticity validation**, for iOS applications only. The tool injects the application with a secret determined at build time, which is later used in the authenticity validation process.

The BTS tool can be downloaded from the **MobileFirst Operations Console → Download Center**.

To use the BTS tool in Xcode:

1. Under the **Build Phases** tab click the **+** button and create new **Run Script Phase**.
2. Copy the path of BTS Tool and paste in the new “Run Script Phase” you have created.
3. Drag the run script phase above the **Compile sources phase**.

The tool should be used when building a production version of the application.

## Troubleshooting application authenticity
{: #troubleshooting-app-authenticity}

### Reset
{: #trblreset}

The application authenticity algorithm uses application data and metadata in its validation. The first device to connect to the server after enabling application authenticity provides a “fingerprint” of the application, containing some of this data.

It is possible to reset this fingerprint, providing the algorithm with new data. This could be useful during development (for example after changing the application in Xcode). To reset the fingerprint, use the **reset** command from the mfpadm CLI.

After resetting the fingerprint, the appAuthenticity security check continues to work as before (this will be transparent to the user).

### Validation Types
{: #trblvalidationtypes}

Mobile First Platform Foundation provides static and dynamic app authenticity for applications. These validation types differ on the algorithm and attributes used to generate app authenticity seeds. By default, when application authenticity is enabled, it uses the dynamic validation algorithm. Both the validation types ensure the security of the application. Dynamic app authenticity uses strict validations and checks for app authenticity. For static app authenticity, we use slightly relaxed algorithm, which will not use all the validation checks as used in dynamic app authenticity.

The dynamic app authenticity is configurable from MobileFirst Console. The internal algorithm takes care of generating app authenticity data based on the options chosen in console. For static app authenticity, one need to use the mfpadm CLI.

For enabling static app authenticity and to switch between validation types, use the mfpadm CLI:

```bash
mfpadm --url=  --user=  --passwordfile= --secure=false app version [RUNTIME] [APPNAME] [ENVIRONMENT] [VERSION] set authenticity-validation TYPE
```
{: codeblock}

TYPE can either be dynamic or static.
