---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-13"

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

# Using {{ site.data.keyword.cos_full_notm }} with {{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}}
{: #using_ibm_cloud_object_storage_with_ibm_mobile_foundation}

{{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}} (MF) delivers enterprise-grade capabilities uniquely designed to support building and deploying of the next generation of cognitive, contextual and personalized mobile apps. {{ site.data.keyword.cos_full_notm }} (COS) is a flexible, cost-effective and scalable cloud storage for unstructured data. This how-to guide will explain how a mobile application using {{ site.data.keyword.mobilefoundation_short}} can connect and fetch/upload data to {{ site.data.keyword.cos_full_notm }} through an ionic application. The ionic application, adapter and related files for this how-to tutorial are available [here](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App).
{: shortdesc}


## Prerequisites
{: #cos-prerequisites}

1. Install the [mfpdev-cli](https://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_wl_cli_description.html) by running `npm install -g mfpdev-cli`. This cli is used to register the ionic app and deploy the adapter to the MF server. Alternatively these activities can be performed from the MF server dashboard.

2. Install [{{ site.data.keyword.cloud_notm}} CLI](https://console.bluemix.net/docs/cli/index.html#overview) on your machine.

3. Install ionic cli by executing `npm install -g ionic`

4. Install cordova by executing `npm install -g cordova`


## {{ site.data.keyword.mobilefoundation_short}} Server setup
{: #mobile-foundation-server-setup}

The {{ site.data.keyword.mobilefoundation_short}} server is set on {{ site.data.keyword.cloud_notm}}. Set up an {{ site.data.keyword.cloud_notm}} instance of an {{ site.data.keyword.mobilefoundation_short}} server as follows:

* In the {{ site.data.keyword.cloud_notm}} Catalog, search for "{{ site.data.keyword.mobilefoundation_short}}". Click on the **{{ site.data.keyword.mobilefoundation_short}}** tile.

    ![MFPCatalog](images/mfp_catalog.png)

* Provide a suitable name for the {{ site.data.keyword.mobilefoundation_short}} server instance and click on **Create**.

    ![BMMFPNew](images/bmmfpnew.png)

* A new MF server instance gets created and asks for login credentials.

    ![MFPLogin](images/mfp_login.png)

* The credentials to login to the MF server can be found in the **Credentials** tab in the left side menu.

    ![MFPcredentials](images/mfp_credentials.png)

* Provide these credentials and login to enter the MF dashboard.

    ![MFPDashboard](images/mfp_dashboard.png)


## Cloud Object Storage Setup
{: #cloud-object-storage-setup}

* In the {{ site.data.keyword.cloud_notm}} Catalog, search for "Cloud Object Storage". Click on the **Object Storage** tile.

    ![Catalog](images/catalog.png)

* Provide a name to your COS instance and click on **Create**.

    ![Create COS](images/cos_create.png)

* Next, click on **Buckets** in the left pane menu options. Provide a suitable name (in this sample we have chosen to name the bucket 'sharedgallery') for your bucket and click on **Create**.

    ![Bucket create](images/bucketcreate.png)

* Once the bucket is created, the landing page of the bucket will look as below, providing you options to upload data directly.

    ![Bucket Landing Page](images/bucketlanding.png)

* Here, add the 3 files provided in the **Short stories** folder in the [sample](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App).


## MFP-COS Ionic App and Java Adapter
{: #mfp-cos-ionic-app-and-java-adapter}

Download the above [git repo](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App) or clone it. This repo consists of two main components:

1. An MF Java Adapter
2. An ionic mobile application

### Configuring mfpdev-cli
{: #configuring-mfpdev-cli}

Add the server details to the cli. On the command prompt execute 	`mfpdev server add`.

```
? Enter the name of the new server profile:
```

Provide a name for the server and press enter. In this sample, the name provided is `mfpserver`.

```
? Enter the fully qualified URL of this server:
```

Enter the url of the server. For MF server on {{ site.data.keyword.cloud_notm}}, the service credentials tab contains the url. The port for the https MF server on {{ site.data.keyword.cloud_notm}} is 443 and for the http MF server instance is 80.

```
? Enter the MobileFirst Server administrator login ID: (admin)
```

Enter `admin` and press **Enter**.

```
? Enter the MobileFirst Server administrator password:
```

Enter the password available in the service credentials.

```
? Save the administrator password for this server?: (Y/n)
```

Based on your preference, enter *Y/N* and enter details as asked by the prompts.

```
? Enter the MobileFirst Server connection timeout in seconds: 30
```

Set 30 sec as the default timeout

```
? Make this server the default?: (Y/n)
```

Enter *Y* and press **Enter**.

***Expected output***:

```
Verifying server configuration...
The following runtimes are currently installed on this server: mfp
Server profile 'mfpserver' added successfully.
```

### Configuring the MF Java adapter
{: #configuring-the-mf-java-adapter}

To connect to your COS instance, some details of your COS instance need to be provided in the `adapter.xml` file. Furnish values for the following:

1. **endpointURL**: This is the public endpoint url for your COS object. This can be found on your COS's dashboard -> Buckets (on the left menu options) -> <your-bucket-name> (sharedgallery in this sample) -> Configuration -> Endpoints -> Public
2. **AuthToken**: In this tutorial we will be using the IAM authentication.

For the java adapter to connect to your instance of COS, authentication using IAM or HMAC is needed. Below are the steps to get the IAM token. For further details on IAM and HMAC authentication processes, click [here](https://console.bluemix.net/docs/services/cloud-object-storage/api-reference/api-reference-buckets.html#bucket-operations#AuthenticationOptions).

#### Obtaining IAM Oauth token using {{ site.data.keyword.cloud_notm}} CLI
{: #obtaining-iam-oath-token-using-ibm-cloud-cli}

1. First, make sure you have an API key. Get this from [{{ site.data.keyword.cloud_notm}} Identity and Access Management](https://console.bluemix.net/iam/#/users).
2. Login to the {{ site.data.keyword.cloud_notm}} Platform using the CLI.

    ```
	`bx login --apikey <value>`
    ```
    Your output will be similar to:

	```
	Authenticating...
	OK

	Targeted account <account-name> (<account-id>)

	Targeted resource group default

	API endpoint:     https://api.ng.bluemix.net (API version: 	2.75.0)
	Region:           us-south
	User:             <email-address>
	Account:          <account-name> (<account-id>)
	Resource group:   default
	```

3. To get all the service instances on your {{ site.data.keyword.cloud_notm}} account, run the below command on the CLI.

	```
    `bx resource service-instances`
    ```
	Expected output:

	```
 	Retrieving service instances in resource group Default and all 	locations under account <account-name> as <email-address>...
	OK
	Name                                               Location     	State    Type               Tags
	<resource-instance-name>                           global       	active   service_instance
	```

4. Execute the below to get details of your COS instance. Here <instance-name> is the name of your COS service (newObject in case of this tutorial).

    ```
	`bx resource service-instance <instance-name>`
    ```
	Expected output:

	```
	Retrieving service instance <sinstance-name> in resource group 	Default under account <account-name> as<email-address>...
	OK

	Name:                  <resource-instance-name>
	ID:                    <resource-instance-id>
	Location:                 global
	Service Name:          cloud-object-storage
	Resource Group Name:   Default
	State:                 active
	Type:                  service_instance
	Tags:
	```

5. To get IAM token:
    ```
	`bx iam oauth-tokens`
    ```

	Expected output:

	```
	IAM token:  Bearer <token>
	UAA token:  Bearer <refresh-token>
	```

After adding the endpointURL and the authToken values, build the adapter. Navigate to the adapter's root folder in the command prompt and execute

```
`mfpdev adapter build`
```
{: codeblock}

This creates the '*.adapter' file in the "target" folder. Execute 	

```
`mfpdev adapter deploy`
```
{: codeblock}

This deploys the adapter to the MF instance.

Alternatively, the adapter can be deployed on the MF server dashboard. Open the MF server dashboard, on the left side menu , click on **Adapters -> New** to open the below page.

![MFPNewAdapterRegister](images/mfp_new_adapter_register.png)

Then click on **Deploy Adapter** and upload the `.adapter` file from the **target** folder.


### Configuring the Ionic App
{: #configuring-the-ionic-app}

In the app:

1. Navigate to the folder containing the Ionic application.

2. Add the cordova MF plugin

	```
    `ionic cordova plugin add cordova-plugin-mfp`
    ```

3. Add the android or ios platform

    ```
	`ionic cordova platform add android`
    ```
	or

	```
	`ionic cordova platform add ios`
	```

4. Register your app to the MF server by executing

    ```
	`mfpdev app register`
	```

	Alternatively, the app can be registered on the MF server dashboard. Open the MF server dashboard and on the left side menu click on **Applications -> New**.

	The below page loads:

	![MFPNewAppRegister](images/mfp_new_app_register.png)

	Enter the details requested. Give a name for your application in the textbox 'Application Name'. Choose the required platform.

	For Android, the 'Package' textbox accepts the 'Application identifier'. This can be found in the 'AndroidManifest.xml' as "package" of your Android application. The 'Version' textbox field has to be filled with the 'versionName' value from the 'AndroidManifest.xml'

	For IOS, the 'Bundle ID' textbox accepts 'Application identifier (case sensitive)'. This can be found in the 'mfpclient.plist' of your IOS application. The 'Version' textbox field has to be filled with the 'version' value from the 'mfpclient.plist' file in your IOS application.

5. Execute `ionic cordova prepare` for changes to percolate to the environments added.
6. Execute
    ```
	`ionic cordova build android`
    ```
	or
    ```
	`ionic cordova build ios`
    ```
	to ensure that the typescript changes are added to the environments.

7. Attach device or run emulator/simulator and execute the command

	```
    `ionic cordova run android`
	```
	or
	```
	`ionic cordova run ios`
    ```

### Navigating the Ionic application
{: #navigating-the-ionic-application}

The Ionic application displays a list of short stories that have been uploaded previously (as the last step in the section [Cloud Object Storage Setup](#cloud-object-storage-setup)). from your created COS instance. On selecting a particular story option, the selected story is loaded and displayed. An options to add a new story is also provided which is then uploaded to the COS instance.

The initial COS objects list looks like this.

![COS before](images/cos_before.png)

The home page of the application provides an option to either "Get all stories" or "Add story"

![App home screen](images/app-home-screen.png)

On clicking **Get all stories**, the stories available on COS are displayed.

![App select story](images/app-select-story.png)

Select a story to be loaded from the drop down menu and click on **Load**

![App story loaded](images/app-story-loaded.png)

Next, click on the **Add story** button to add a story of your own. Enter a title for the story and the story's content in the text area and click **Add**.

![App add input](images/app-add-input.png)

Once the story is added, a message of successful addition of the story is displayed.

![App story added](images/app-story-added.png)

The COS dashboard now will have the story added from the application as well.

![COS added](images/cos_added.png)


The IAM oauth token that is obtained using `bx iam oauth-tokens` has an expiration time and the adapter will fail with an exception of "403 - Forbidden". It has to be hence ensured that the token is valid on the deployed adapter for the app to function as expected.
{: note}
