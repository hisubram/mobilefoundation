---

copyright:
  years: 2016, 2018
lastupdated:  "2018-02-13"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:tip: .tip}

# Getting started tutorial
{: #gettingstartedtemplate}

{{site.data.keyword.mobilefoundation_long}} expedites setting up an {{site.data.keyword.mfp_full}} environment using which you can develop, test, and operate enterprise mobile apps. {{site.data.keyword.mobilefoundation_short}} is available under the following different service plans: Developer, Developer Pro, Professional Per Capacity, and Professional 1 Application.
{:shortdesc}

Using the Professional 1 Application plan a single application built on any or all of the supported operating platforms such as Android, iOS, Windows or mobile web, can be managed. The Developer plan is best suited for development and test. You can review all the available plans [here](https://console.bluemix.net/catalog/services/mobile-foundation).

## Before you begin
{: #prereqs}

You will need a {{site.data.keyword.Bluemix}} account and an instance of the {{site.data.keyword.mobilefoundation_short}} service.

## Step 1: Create an instance of {{site.data.keyword.mobilefoundation_short}} service
{: #step1create}

1. In the {{site.data.keyword.Bluemix_notm}} **Catalog**, select **{{site.data.keyword.mobilefoundation_short}}**. The service configuration screen opens.
2. Give your service instance a name, or use the preset name.
3. Choose the region, organization and space where you would want to create the service instance.
4. Select your **Pricing Plan** and click **Create**.

## Step 2: Build you mobile channel
{: #buildmobilechannel}

### For {{site.data.keyword.mobilefoundation_short}}: Developer plan
{: #buildchanneldevplan}

After you create an instance of the {{site.data.keyword.mobilefoundation_short}}: Developer, you can start building your mobile channel by completing the following steps.

  1.  Create a {{site.data.keyword.mobilefirst_notm}} server instance with the default configuration, click **Start Basic Server**.

    `The basic server instance includes a single node, 1 GB of memory.`

  +	The `username` and `password` is automatically generated for you. You have access to them when the server is up and running.

    The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation. When complete a dashboard is displayed where you can see:
    * The status of your server that is running (state, size).
    *	The server route created for you. Use this route in your mobile application to connect to the {{site.data.keyword.mfserver_short_notm}}.
    *	Your personal `username` and `password` to access the {{site.data.keyword.mfp_oc_short_notm}}. The `password` is hidden. Click **Show Password** icon to visualize it.

  + Click **Launch Console** to launch the {{site.data.keyword.mfp_oc_short_notm}}.

To create a {{site.data.keyword.mobilefirst_notm}} server instance with advanced configuration for topology, security, and other server configuration, click **Start Server with Advanced Configuration**. See [Setting up advanced configuration](c_using_mfs_p1.html#using_mfs_advanced_p1), for more information.
{: tip}

### For {{site.data.keyword.mobilefoundation_short}}: Developer Pro plan
{: #buildchanneldevproplan}

After you create an instance of the {{site.data.keyword.mobilefoundation_short}}: Developer Pro service, you can start building your mobile channel by completing the following steps.

  1.  Connect to an existing {{site.data.keyword.Db2_on_Cloud_short}} service on {{site.data.keyword.Bluemix_notm}}.

      1.  Select the {{site.data.keyword.Bluemix_notm}} `Organization` where the {{site.data.keyword.Db2_on_Cloud_short}} service instance exists.

      + Select the {{site.data.keyword.Bluemix_notm}} `Space` where the {{site.data.keyword.Db2_on_Cloud_short}} service instance exists, from the list of spaces available in the selected `Organization`.

      + Select the {{site.data.keyword.Db2_on_Cloud_short}} `Service Name` and `Credentials` to connect to the existing  {{site.data.keyword.Db2_on_Cloud_short}} service instance.

      + Test the connection to the selected {{site.data.keyword.Db2_on_Cloud_short}} service instance by clicking **Test Connection**.

      + Click **Add**, followed by **Continue** on the pop up window asking for confirmation on the selected {{site.data.keyword.Db2_on_Cloud_short}} service. This action creates the required tables in the configured {{site.data.keyword.Db2_on_Cloud_short}} database service instance.

      > **Note:** After you add a {{site.data.keyword.Db2_on_Cloud_short}} connection to the {{site.data.keyword.mobilefoundation_short}} instance you will not be able to change it.

  2.  Create and start the server.

      1. Create a {{site.data.keyword.mobilefirst_notm}} server instance with the default configuration, click **Start Basic Server**.

      + This selection provisions an {{site.data.keyword.mfserver_long_notm}} with the following settings:

          - Single Node with 1 GB of memory. This size is enough for development, moderate testing activities, and small scale production workloads.

          -	The `username` and `password` are automatically generated for you. You have access to them when the server is up and running.

          The process of provisioning your server starts. This process takes about 10 minutes, and a message window indicates the progress of this operation. When complete a dashboard is displayed where you can see:
            -	The status of your server that is running (state, size).
            -	The server route is created for you. Use this route in your mobile application to connect to the {{site.data.keyword.mfserver_short_notm}}.
            -	Your personal `username` and `password` to access the {{site.data.keyword.mfp_oc_short_notm}}. The `password` is hidden. Click **Show Password** icon to visualize it.

      +	Click **Launch Console** to open the {{site.data.keyword.mfp_oc_short_notm}}.  

      To create a {{site.data.keyword.mobilefirst_notm}} server instance with advanced configuration for topology, security, and other server configuration, click **Start Server with Advanced Configuration**. See [Setting up advanced configuration](c_using_mfs_p3.html#using_mfs_advanced_p3), for more information.
      {: tip}

### For {{site.data.keyword.mobilefoundation_short}}: Professional Per Capacity plan
{: #buildchannelprofcapacityplan}

After you create an instance of the {{site.data.keyword.mobilefoundation_short}}: Professional Per Capacity service, you can start building your mobile channel by completing the following steps.

  1.  Connect to an existing {{site.data.keyword.Db2_on_Cloud_short}} service on {{site.data.keyword.Bluemix_notm}}.

      1.  Select the {{site.data.keyword.Bluemix_notm}} `Organization` where the {{site.data.keyword.Db2_on_Cloud_short}} service instance exists.

      + Select the {{site.data.keyword.Bluemix_notm}} `Space` where the {{site.data.keyword.Db2_on_Cloud_short}} service instance exists, from the list of spaces available in the selected `Organization`.

      + Select the {{site.data.keyword.Db2_on_Cloud_short}} `Service Name` and `Credentials` to connect to the existing  {{site.data.keyword.Db2_on_Cloud_short}} service instance.

      + Test the connection to the selected {{site.data.keyword.Db2_on_Cloud_short}} service instance by clicking **Test Connection**.

      + Click **Add**, followed by **Continue** on the pop up window asking for confirmation on the selected {{site.data.keyword.Db2_on_Cloud_short}} service. This action creates the required tables in the configured {{site.data.keyword.Db2_on_Cloud_short}} database service instance.

      > **Note:** After you add a {{site.data.keyword.Db2_on_Cloud_short}} connection to the {{site.data.keyword.mobilefoundation_short}} instance you will not be able to change it.

  2.  Create and start the server.

      1. Create a {{site.data.keyword.mobilefirst_notm}} server instance with the default configuration, click **Start Basic Server**.

      + This selection provisions an {{site.data.keyword.mfserver_long_notm}} with the following settings:
          -  2 nodes with 1 GB memory each. This size is good for development, moderate testing activities and small scale production workloads.

          -	The `username` and `password` are automatically generated for you. You have access to them when the server is up and running.

          The process of provisioning your server starts. This process takes about 10 minutes, and a message window indicates the progress of this operation. When complete a dashboard is displayed where you can see:
            -	The status of your server that is running (state, size).
            -	The server route is created for you. Use this route in your mobile application to connect to the {{site.data.keyword.mfserver_short_notm}}.
            -	Your personal `username` and `password` to access the {{site.data.keyword.mfp_oc_short_notm}}. The `password` is hidden. Click **Show Password** icon to visualize it.

      +	Click **Launch Console** to open the {{site.data.keyword.mfp_oc_short_notm}}.      

      To create a {{site.data.keyword.mobilefirst_notm}} server instance with advanced configuration for topology, security, and other server configuration, click **Start Server with Advanced Configuration**. See [Setting up advanced configuration](c_using_mfs_p4.html#using_mfs_advanced_p4), for more information.
      {: tip}

### For {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application plan
{: #buildchannelprof1appplan}

After you create an instance of the {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application service, you can start building your mobile channel by completing the following steps.

  1.  Connect to an existing {{site.data.keyword.Db2_on_Cloud_long}} service on {{site.data.keyword.Bluemix_notm}}.

      1.  Select the {{site.data.keyword.Bluemix_notm}} `Organization` where the {{site.data.keyword.Db2_on_Cloud_short}} service instance exists.

      + Select the {{site.data.keyword.Bluemix_notm}} `Space` where the {{site.data.keyword.Db2_on_Cloud_short}} service instance exists, from the list of spaces available in the selected `Organization`.

      + Select the {{site.data.keyword.Db2_on_Cloud_short}} `Service Name` and `Credentials` to connect to the existing  {{site.data.keyword.Db2_on_Cloud_short}} service instance.

      + Test the connection to the selected {{site.data.keyword.Db2_on_Cloud_short}} service instance by clicking **Test Connection**.

      + Click **Add**, followed by **Continue** on the pop up window asking for confirmation on the selected {{site.data.keyword.Db2_on_Cloud_short}} service. This action creates the required tables in the configured {{site.data.keyword.Db2_on_Cloud_short}} database service instance.

      > **Note:** After you add a {{site.data.keyword.Db2_on_Cloud_short}} connection to the {{site.data.keyword.mobilefoundation_short}} instance you will not be able to change it.

  2.  Create and start the server.

      1. Create a {{site.data.keyword.mobilefirst_notm}} server instance with the default configuration, click **Start Basic Server**.

        `The basic server instance includes a single node, 1 GB of memory.`

      + The `username` and `password` is automatically generated for you. You have access to them when the server is up and running.  

        The process of provisioning your server starts. This process takes about 10 minutes, and a message window indicates the progress of this operation. When complete a dashboard is displayed where you can see:
          -	The status of your server that is running (state, size).
          -	The server route is created for you. Use this route in your mobile application to connect to the {{site.data.keyword.mfserver_short_notm}}.
          -	Your personal `username` and `password` to access the {{site.data.keyword.mfp_oc_short_notm}}. The `password` is hidden. Click **Show Password** icon to visualize it.

      +  Click **Launch Console** to open the {{site.data.keyword.mfp_oc_short_notm}}.  

      To create a {{site.data.keyword.mobilefirst_notm}} server instance with advanced configuration for topology, security, and other server configuration, click **Start Server with Advanced Configuration**. See [Setting up advanced configuration](c_using_mfs_p2.html#using_mfs_advanced_p2), for more information.
      {: tip}

Go to [Using the Mobile Foundation service to set up MobileFirst Server ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/bluemix/using-mobile-foundation/){: new_window} to learn more about getting started with {{site.data.keyword.mobilefoundation_short}}.
{: tip}

## Step 3: Register your application in {{site.data.keyword.mobilefoundation_short}}
{: #registerapp}

After creating and starting your Mobile Foundation server instance, you can follow the steps below to register an Android application.

  1.  Launch the {{site.data.keyword.mfp_oc_short_notm}} by loading the URL: http://your-server-host:server-port/mfpconsole. Use the `username` and `password` generated at the time of provisioning.

  + In the  {{site.data.keyword.mfp_oc_short_notm}} **Dashboard**, click **New** next to **Applications**.

  + Provide *MFPStarterAndroid* as the **Application Name**.

  + **Choose Platform** to be *Android*.

  + Provide *com.ibm.mfpstarterandroid* as the **Application Identifier**.

  + Enter *1.0* as the **Version**.

  + Click **Register application**.

## Step 4: Download the sample application
{: #downloadapp}

  1.  From the {{site.data.keyword.mfp_oc_short_notm}} **Dashboard**, select **MFPStarterAndroid** under **Applications**.

  + Click **Get Starter Code** and select to download the Android application sample.

## Step 5: Edit the sample application
{: #editapp}

  1. Import the downloaded sample Android app, from step above, to an Android Studio.

  + From the **Project** sidebar menu in Android Studio, select the **app → java → com.ibm.mfpstarterandroid → ServerConnectActivity.java** file.

    * Add the following imports
      ```java
      import java.net.URI;
      import java.net.URISyntaxException;
      import android.util.Log;
      ```
      {: codeblock}

    * Paste the following code snippet, replacing the call to `WLAuthorizationManager.getInstance().obtainAccessToken`

        ```java
          WLAuthorizationManager.getInstance().obtainAccessToken("", new WLAccessTokenListener() {
            @Override
            public void onSuccess(AccessToken token) {
                System.out.println("Received the following access token value: " + token);
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        titleLabel.setText("Yay!");
                        connectionStatusLabel.setText("Connected to MobileFirst Server");
                    }
                });

                URI adapterPath = null;
                try {
                    adapterPath = new URI("/adapters/javaAdapter/resource/greet");
                } catch (URISyntaxException e) {
                    e.printStackTrace();
                }

                WLResourceRequest request = new WLResourceRequest(adapterPath, WLResourceRequest.GET);

                request.setQueryParameter("name","world");
                request.send(new WLResponseListener() {
                    @Override
                    public void onSuccess(WLResponse wlResponse) {
                        // Will print "Hello world" in LogCat.
                        Log.i("MobileFirst Quick Start", "Success: " + wlResponse.getResponseText());
                    }

                    @Override
                    public void onFailure(WLFailResponse wlFailResponse) {
                        Log.i("MobileFirst Quick Start", "Failure: " + wlFailResponse.getErrorMsg());
                    }
                });
            }

            @Override
            public void onFailure(WLFailResponse wlFailResponse) {
                System.out.println("Did not receive an access token from server: " + wlFailResponse.getErrorMsg());
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        titleLabel.setText("Bummer...");
                        connectionStatusLabel.setText("Failed to connect to MobileFirst Server");
                    }
                });
            }
        });
        ```
        {: codeblock}  

## Step 6: Deploy an adapter
{: #deployadapter}

  1. Download this [adapter artifact](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/javaAdapter.adapter){: download} and deploy it from {{site.data.keyword.mfp_oc_short_notm}} using **Actions → Deploy adapter**.

## Step 7: Test the application
{: #testapp}

  1. In Android Studio, from the **Project** sidebar menu, select the **app → src → main →assets → mfpclient.properties** file and edit the `protocol`, `host` and `port` properties with the correct values for your MobileFirst Server.

   The values are typically https, *your-server-address* and 443.
   {: tip}

  2. Click **Run App** in Android Studio.
     * You will see the app launched on a device emulator.
     * Click the **Ping MobileFirst Server** button in your launched application, this will display `Connected to MobileFirst Server`.
     * If the application was able to connect to the MobileFirst Server, a resource request call using the deployed Java adapter will take place.
     * The adapter response is then printed in Android Studio’s LogCat view.


## Next steps
{: #nextsteps}

You can follow the [Quick Start tutorials ![External link icon](../../icons/launch-glyph.svg "Quick Start tutorials")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/){: new_window} to work with more sample applications and to explore the working of {{site.data.keyword.mobilefoundation_short}}. Quick Start has tutorials explaining the working of {{site.data.keyword.mobilefoundation_short}} for  iOS, Android, Web, Cordova, Windows, and Xamarin apps.

# Related Links
{: #rellinks  notoc}

## Related Links
{: #general notoc}

*	[IBM MobileFirst Platform Foundation V8.0.0 product documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}
*	[IBM MobileFirst Platform Developer Center ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://mobilefirstplatform.ibmcloud.com){: new_window}
