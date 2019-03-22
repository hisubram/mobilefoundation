---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-29"

keywords: app versions, disabling apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Manage app versions
{: #manage_app_versions}

The Mobile Foundation application management capabilities provide Mobile Foundation Server users and administrators with granular control over user and device access to their applications.

Mobile Foundation server tracks all attempts to access your mobile infrastructure and stores information about the application, the user, and the device on which the application is installed. The mapping between the application, the user, and the device forms the basis for the server’s mobile-application management capabilities.

Using the Mobile Foundation Operations console, you can monitor and manage access to your resources. You can also manage your specific application version.

1.  Go to Mobile Foundation Operations console, click **Applications**, choose the application that you want to manage, select the specific version of the application you are interested in, from the **Versions** list displayed.
    ![Manage application version](images/app_version_management.png)

2. Under the **Management** tab, you see the options to set the application status for the selected application version. The supported states for an application version are,
   * Active
   * Active and Notifying
   * Access Disabled
3. An application version can be disabled by selecting the *Access Disabled* option, from under **Application Access > Status**.
4. You can also configure to upload the updated web resources for your Cordova application in the **Direct Update** section. Users connecting to Mobile Foundation server using this specific application version is then presented the option to update their application by using Direct Update.
5. You can also perform the following actions on the selected application version by using the following options in the **Actions** menu:
   *  Delete version
   *  Clone version
   *  Export version


For more information on managing devices, see [Managing devices](/docs/services/mobilefoundation?topic=mobilefoundation-manage_devices#manage_devices).
For more information on remotely disabling an app version, see [Remotely disable an app version](/docs/services/mobilefoundation?topic=mobilefoundation-remotely_disable_an_app_version#remotely_disable_an_app_version).
{: note}
