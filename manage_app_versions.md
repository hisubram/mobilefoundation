---

copyright:
  years: 2018
lastupdated: "2018-11-29"

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

Using the Mobile Foundation Operations console you can monitor and manage access to your resources, you can also manage your specific application version.

1.  Go to Mobile Foundation Operations console, click **Applications**, choose the application you want to manage, select the specific version of the application you are interested in, from the **Versions** list displayed.
    ![Manage application version](images/app_version_management.png)

2. Under the **Management** tab, you will see the options to set the application status for the selected application version. The supported states for an application version are:
   * Active
   * Active and Notifying
   * Access Disabled
3. An application version can be disabled by selecting the *Access Disabled* option, from under **Application Access > Status**.
4. You can also configure to upload the updated web resources for your Cordova application in the **Direct Update** section. Users connecting to Mobile Foundation server using this specific application version is then presented the option to update their application using Direct Update.
5. You can also perform the following actions on the selected application version, using the following options in the **Actions** menu:
   *  Delete version
   *  Clone version
   *  Export version


See [Managing devices](manage_devices.html), to learn about managing devices. See [Remotely disable an app version](remote_disable_app_version.html), to learn about remotely disabling a specific version of an app.
{: note}

