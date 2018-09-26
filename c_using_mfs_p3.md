---

copyright:
  years: 2016, 2018
lastupdated:  "2018-09-26"

---

#	Using the Developer Pro plan
{: #using_mobilefoundation_p3}

{{site.data.keyword.mobilefoundation_short}}: Developer Pro is suitable for team-based development and testing, this plan is not suitable for production.

After you create the {{site.data.keyword.mobilefoundation_short}} service instance using the Developer Pro plan, access the `Overview` page on {{site.data.keyword.Bluemix_notm}}, which provides you tutorials and videos to help you get started with the service.

## Pre-requisites
{: #prerequisites_p3}

Consider the following before you configure  {{site.data.keyword.mobilefoundation_short}}: Developer Pro service instance.
* {{site.data.keyword.mobilefoundation_short}}: Developer Pro is supported only with {{site.data.keyword.Db2_on_Cloud_short}} {{site.data.keyword.Bluemix_notm}} plans.

* You must have access to the {{site.data.keyword.Db2_on_Cloud_short}} (any plan other than the **Lite** plan) service instance credentials before you can configure the settings of your {{site.data.keyword.mobilefoundation_short}} service instance.

> **Note**: The {{site.data.keyword.Db2_on_Cloud_short}} (any plan other than the **Lite** plan) service instance can exist in any `Space` within your {{site.data.keyword.Bluemix_notm}} `Organization` or any other `Organization` that you have access to. Ensure that you have the permissions to access the `Space` where the {{site.data.keyword.Db2_on_Cloud_short}} service instance exists.


## Adding the database connection
{: #configure_dashdb_p3}

###  First steps
{: #firststeps_p3}

After you create the {{site.data.keyword.mobilefoundation_short}}: Developer Pro service instance, follow the procedure to get started.

### Setting up connection to Db2 on Cloud service instance
{: #connect_dashdb_p3}

After you create the {{site.data.keyword.mobilefoundation_short}} service instance using the Developer Pro plan, you see the *Overview* page where you need to specify the connection information for the {{site.data.keyword.Db2_on_Cloud_short}} (any plan other than the **Lite** plan) service instance. The {{site.data.keyword.mobilefoundation_short}} service instance must connect to the specified {{site.data.keyword.Db2_on_Cloud_short}} service instance.

If you do not have an existing {{site.data.keyword.Db2_on_Cloud_short}} service instance, you can create it.

Follow these steps to create a Db2 on Cloud service instance:

1. In the *Overview* page, select **Create New Service** section.

+ If you want a high available {{site.data.keyword.Db2_on_Cloud_short}} service instance, select `Yes` on the **High availability configuration** option.

+ Review the plan details and click **Create**.

A new {{site.data.keyword.Db2_on_Cloud_short}} service instance is created, which provides a dedicated {{site.data.keyword.Db2_on_Cloud_short}} instance with 8 GB RAM and 2 vCPUs, and 500 GB of storage.

Carry out the following steps to connect to an existing or new {{site.data.keyword.Db2_on_Cloud_short}} service instance:

1. Select the {{site.data.keyword.Bluemix_notm}} `Organization` where the {{site.data.keyword.Db2_on_Cloud_short}} service instance exists.

+ Select the {{site.data.keyword.Bluemix_notm}} `Space` where the {{site.data.keyword.Db2_on_Cloud_short}} service instance exists, from the list of spaces available in the selected `Organization`.   
> **Note:** If you do not see listed the `Organization` and `Space` where your {{site.data.keyword.Db2_on_Cloud_short}} service instance exists, then check if you are a member of that `Organization` and `Space`. You are required to have a *Developer* role access to the organization and space, as the {{site.data.keyword.mobilefoundation_short}} service accesses the credentials from the {{site.data.keyword.Db2_on_Cloud_short}} service.

+ Select the {{site.data.keyword.Db2_on_Cloud_short}} `Service Name` and `Credentials` to connect to the existing  {{site.data.keyword.Db2_on_Cloud_short}} service instance.

+  Test the connection to the specified {{site.data.keyword.Db2_on_Cloud_short}} service instance.

+  Click **Add**. This action creates the required tables in the configured {{site.data.keyword.Db2_on_Cloud_short}} database service instance.

In a few seconds, you can access the `Overview` page that provides you with  tutorials and videos to help you get started with the  {{site.data.keyword.mobilefoundation_short}} service.

> **Note**: You cannot change the {{site.data.keyword.Db2_on_Cloud_short}} service instance that is configured to be used by your {{site.data.keyword.mobilefoundation_short}} service instance. However, you can use the same {{site.data.keyword.Db2_on_Cloud_short}} service instance across multiple {{site.data.keyword.mobilefoundation_short}} service instances, as each {{site.data.keyword.mobilefoundation_short}} service instance creates its own schema in the selected {{site.data.keyword.Db2_on_Cloud_short}} service instance.

## Starting the MobileFirst server
{: #start_mobilefoundation_p3}

* To start the {{site.data.keyword.mfserver_short_notm}}, with default settings, click **Start Basic Server**.

* This selection provisions an {{site.data.keyword.mfserver_long_notm}} with the following settings:
    - Single Node with 1 GB of memory. This size is enough for development, moderate testing activities, and small scale production workloads.

    -	The `username` and `password` are automatically generated for you. You have access to them when the server is up and running.

    The process of provisioning your server starts. This process takes about 10 minutes, and a message window indicates the progress of this operation. When complete a dashboard is displayed where you can see:

      -	The status of your server that is running (state, size).

      -	The server route is created for you. Use this route in your mobile application to connect to the {{site.data.keyword.mfserver_short_notm}}.

      -	Your personal `username` and `password` to access the {{site.data.keyword.mfp_oc_short_notm}}. The `password` is hidden. Click **Show Password** icon to visualize it.

*	Click **Launch Console** to open the {{site.data.keyword.mfp_oc_short_notm}}.

With the console, you can manage your mobile apps, adapters, and mobile devices, use your server as a mobile backend, send push notifications, and do more.

##  Adding Mobile Analytics service
{: #adding_analytics_server_p3}

 You can now monitor your mobile application on {{site.data.keyword.mobilefirst}} server by connecting a Mobile Analytics service instance to the {{site.data.keyword.mobilefoundation_short}} service instance.

 Developer Pro plan creates the Mobile Analytics service instance.

 <!--Users can also attach volumes to the containers to persist data. The volume once selected cannot be changed. 20 GB is the default file share space available to the user. If the user needs additional storage space to persist analytics data, he is required to buy additional file share and create a volume using this file share. He can then select this new volume while deploying the analytics server.

 For more information on adding volumes to {{site.data.keyword.containerlong}}, refer to [Storing persistent data in a volume by using the {{site.data.keyword.Bluemix_notm}} Dashboard ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.ng.bluemix.net/docs/containers/container_volumes_ov.html#container_volumes_ui){: new_window}. -->

* Click **Add Analytics** to add the Mobile Analytics service instance to the {{site.data.keyword.mobilefoundation_short}} service instance.

<!--* You can choose the Mobile Analytics service configuration, a minimum of 1 GB and a maximum of 2 GB memory is supported for the Analytics server configuration.-->
  The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation.  

* Launch the Mobile Analytics service Console from the {{site.data.keyword.mfp_oc_short_notm}}.

* Single sign-on is enabled between the {{site.data.keyword.mfserver_short_notm}} and the Mobile Analytics service. Mobile Analytics service is configured with the same LTPA keys and user credentials as the {{site.data.keyword.mfserver_short_notm}}. You can use the same `username` and `password` to log in to the Mobile Analytics console as used to log in to the {{site.data.keyword.mfp_oc_short_notm}}.

For more information on Mobile Analytics service, you can refer to [MobileFirst Foundation Operational Analytics ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}.

> **Note:** Deleting the {{site.data.keyword.mobilefoundation_short}} service instance removes the Mobile Analytics service instance.

##  Deleting Mobile Analytics service
{: #deleting_analytics_server_p3}

You can now delete the Mobile Analytics service that was added to the {{site.data.keyword.mobilefoundation_short}} service instance, from the {{site.data.keyword.mobilefoundation_short}} service dashboard.

* Click **Delete Analytics** to delete the Mobile Analytics service that has been added to the {{site.data.keyword.mobilefoundation_short}} service instance.

  Clicking **Delete Analytics** deletes the analytics server instance. The process of deleting analytics instance takes about 10 minutes. You can refresh the screen to view the updated status. Deletion of analytics instance reenables the **Add Analytics** button. If you choose to add the Mobile Analytics service again, you can click this button.

## Re-creating the MobileFirst server
{: #recreate_mobilefoundation_p3}

*	Click **Recreate** to re-create the server.

* This action stops your existing server and deletes the data. A new server instance is created with an updated version, if available. This action takes a few minutes to complete.

> **Note**: Data from your previous server instance, which includes information on the apps and adapters is persisted in the configured {{site.data.keyword.Db2_on_Cloud_short}} service instance. This data is used to recreate your server.

##	Setting up advanced configuration
{: #using_mfs_advanced_p3}

Use the **Start Server with Advanced Configuration** from the `Overview` page to create the server with advanced or custom settings. You can also update the server settings to customize your server configuration by clicking the **Settings** tab. {{site.data.keyword.mobilefoundation_short}} gives you access to some advanced settings.

*	From the **Topology** tab, you can select the server size and memory based on your need. The default server is created with 1 GB of memory.
  - You can change the memory for your server based on your need to a maximum of 2 GB.

  - **Instances** displays the number of nodes that are created. This field cannot be  edited in {{site.data.keyword.mobilefoundation_short}}: Developer Pro. The number of nodes defaults to **1** in the Developer Pro plan.

See [{{site.data.keyword.mobilefoundation_long}} documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}, for more details.
