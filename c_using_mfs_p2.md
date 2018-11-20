---

copyright:
  years: 2016, 2018
lastupdated:  "2018-11-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

#	Using the Professional 1 Application plan
{: #using_mobilefoundation_p2}

With the Professional 1 Application plan, users can create 1 mobile application with various mobile operating systems.
After you create the {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application service instance, read the following procedure to get started with the service.

## Pre-requisites
{: #prerequisites_p2}

Consider the following before you configure  {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application service instance.
* {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application is supported only with {{site.data.keyword.Db2_on_Cloud_short}} and  {{site.data.keyword.composeForPostgreSQL}} {{site.data.keyword.Bluemix_notm}} plans.

* You should have access to the {{site.data.keyword.Db2_on_Cloud_short}} or {{site.data.keyword.composeForPostgreSQL}} service instance credentials before you can configure the settings of your {{site.data.keyword.mobilefoundation_short}} service instance.

> **Note**: The {{site.data.keyword.Db2_on_Cloud_short}} (any plan other than the **Lite** plan) or {{site.data.keyword.composeForPostgreSQL}} service instance can exist in any `Space` within your {{site.data.keyword.Bluemix_notm}} `Organization` or any other `Organization` that you have access to. Ensure that you have the permissions to access the `Space` where the {{site.data.keyword.Db2_on_Cloud_short}} or {{site.data.keyword.composeForPostgreSQL}} service instance exists.


## Adding the database connection
{: #configure_dashdb_p2}

###  First steps
{: #firststeps_p2}

After you create the {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application service instance, follow the procedure to get started.

### Setting up connection to Db2 on Cloud service instance
{: #connect_dashdb_p2}

After the {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application service instance is created you'll see the *Overview* page. Here you'll need to specify the connection information for the {{site.data.keyword.Db2_on_Cloud_short}} (any plan other than the **Lite** plan) or {{site.data.keyword.composeForPostgreSQL}} service instance that the {{site.data.keyword.mobilefoundation_short}} service instance should connect to.

If you don't have an existing Db2 on Cloud instance, you can create a new {{site.data.keyword.Db2_on_Cloud_short}} (any plan other than the **Lite** plan) or {{site.data.keyword.composeForPostgreSQL}} service instance.

Follow these steps to create a new Db2 service instance:

1. In the *Overview* page, select **Create New Service** section.

+ Select `Yes` on the **High availability configuration** option, if you want high available {{site.data.keyword.Db2_on_Cloud_short}} service instance.

+ Review the plan details and click **Create**.

A new {{site.data.keyword.Db2_on_Cloud_short}} service instance is created, which provides a dedicated {{site.data.keyword.Db2_on_Cloud_short}} instance with 8 GB RAM and 2 vCPUs, and 500 GB of storage.

Follow these steps to connect to an existing {{site.data.keyword.Db2_on_Cloud_short}} service instance or to the {{site.data.keyword.Db2_on_Cloud_short}} service instance that you just created:

1. Select the {{site.data.keyword.Bluemix_notm}} `Organization` where the {{site.data.keyword.Db2_on_Cloud_short}} service instance exists.

+ Select the {{site.data.keyword.Bluemix_notm}} `Space` where the {{site.data.keyword.Db2_on_Cloud_short}} service instance exists, from the list of spaces available in the selected `Organization`.   
> **Note:** If you don't see listed the `Organization` and `Space` where your {{site.data.keyword.Db2_on_Cloud_short}} service instance exists, then check if you're a member of that `Organization` and `Space`. You're required to have a *Developer* role access to the organization and space. The {{site.data.keyword.mobilefoundation_short}} service accesses the credentials from the {{site.data.keyword.Db2_on_Cloud_short}} service.

+ Select the {{site.data.keyword.Db2_on_Cloud_short}} `Service Name` and `Credentials` to connect to the existing  {{site.data.keyword.Db2_on_Cloud_short}} service instance.

+  Test the connection to the specified {{site.data.keyword.Db2_on_Cloud_short}} service instance.

+  Click **Add**. This action creates the required tables in the configured {{site.data.keyword.Db2_on_Cloud_short}} database service instance.

In a few seconds, you can access the `Overview` page that provides you with  tutorials and videos to help you get started with the  {{site.data.keyword.mobilefoundation_short}} service.

> **Note**: You can't change the {{site.data.keyword.Db2_on_Cloud_short}} service instance that is configured to be used by your {{site.data.keyword.mobilefoundation_short}} service instance. However, you can use the same {{site.data.keyword.Db2_on_Cloud_short}} service instance across multiple {{site.data.keyword.mobilefoundation_short}} service instances, as each {{site.data.keyword.mobilefoundation_short}} service instance creates its own schema in the selected {{site.data.keyword.Db2_on_Cloud_short}} service instance.


## Starting the MobileFirst server
{: #start_mobilefoundation_p2}

* To start the {{site.data.keyword.mfserver_short_notm}}, with default settings, click **Start Basic Server**.

* This selection creates an {{site.data.keyword.mfserver_long_notm}} with the following settings:
    -  1 GB of memory. This size is enough for development, moderate testing activities and small scale production workloads.

    -	The `username` and `password` is automatically generated for you. You have access to them when the server is up and running.

    The process of creating your server starts. This process takes about 10 minutes, and a message window indicates the progress of this operation. When complete a dashboard is displayed where you can see:

      -	The status of your server that is running (state, size).

      -	The server route is created for you. Use this route in your mobile application to connect to the {{site.data.keyword.mfserver_short_notm}}.

      -	Your personal `username` and `password` to access the {{site.data.keyword.mfp_oc_short_notm}}. The `password` is hidden. Click **Show Password** icon to visualize it.

*	Click **Launch Console** to open the {{site.data.keyword.mfp_oc_short_notm}}.

With the console you can manage your mobile apps, adapters and mobile devices, use your server as a mobile backend, send push notifications, and do more.

## Re-creating the MobileFirst server
{: #recreate_mobilefoundation_p2}

*	Click **Recreate** to re-create the server.

* This action stops your existing server and deletes the data. A new server instance is created with an updated version, if available. This action takes a few minutes to complete.

> **Note**: Data from your previous server instance including information on the apps and adapters is persisted in the configured {{site.data.keyword.Db2_on_Cloud_short}} service instance. This data is used to recreate your server.

##	Setting up advanced configuration
{: #using_mfs_advanced_p2}

Use the **Start Server with Advanced Configuration** from the `Overview` page to create the server with advanced or custom settings. You can also update the server settings to customize your server configuration by clicking the **Configuration** tab. {{site.data.keyword.mobilefoundation_short}} gives you access to some advanced settings.

*	From the **Topology** tab, you can select the server size and number of server instances based on your need. The default 1 GB server is enough for development and light testing.
  - Select the correct size for your server based on your need.

  - **Instances** displays the number of nodes that are created.

## Mobile Analytics
{: #mobile_analytics}

Mobile Analytics server is included and preconfigured with the Mobile Foundation: Developer plan service instance.

* Launch the Mobile Analytics Console from the {{site.data.keyword.mfp_oc_short_notm}}.

For more information on Mobile Analytics, you can refer to [MobileFirst Foundation Operational Analytics](https://cloud.ibm.com/docs/services/mobileanalytics/mobileanalytics_overview.html#about-mobile-analytics){: new_window}.

See [{{site.data.keyword.mobilefoundation_long}} documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/bluemix/){: new_window}, for more details.
{: note}