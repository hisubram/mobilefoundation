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


# Delivering {{ site.data.keyword.mobilefoundation_short }} apps using {{ site.data.keyword.cloud_notm }} {{ site.data.keyword.jazzhub_short }}
{: #delivering_mobile_foundation_apps_using_ibm_cloud_devops_services}

This tutorial helps you to automate the delivery of apps and adapters to IBM  {{ site.data.keyword.mobilefoundation_short }} using the {{ site.data.keyword.jazzhub_title }}
 on {{ site.data.keyword.cloud_notm }}.

Here is an overview of the pipeline.

![overview_of_pipeline](images/p00_overview_of_pipeline.png)


## Prerequisites
{: #mpfapps-devops-prerequisites }

* [{{ site.data.keyword.cloud_notm }}](https://cloud.ibm.com/registration) Account
* [mfpdev-cli](https://www.npmjs.com/package/mfpdev-cli)
* A sample App and [MFP Adapter](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/)
* [GitHub](http://github.com/) Account
* **Optional:** [Bitbar](https://bitbar.com/testing/) instance and Bitbar API Key (You may make use of any service as per your requirements).


## Creating Continuous Delivery Service and Toolchain
{: #creating-continuous-delivery-service-and-toolchain }

* Search for "Continuous Delivery" the {{ site.data.keyword.cloud_notm }} Catalog (or [click here](https://cloud.ibm.com/catalog/services/continuous-delivery)).
* Create the service by providing service name, region, etc.

    In the following example we have used the service name as "MFP App/Adapter delivery Test", region/location as "London" and resource group as "Default".

    ![configuring_continuous_delivery_service](images/p01_configuring_continuous_delivery_service.png)

* From the {{ site.data.keyword.jazzhub_title }}
 section in the hamburger menu  on the left corner; create a toolchain and search for “Build your own toolchain” to create a toolchain from scratch.

    ![search_build_your_own_toolchain](images/p02_search_build_your_own_toolchain.png)

* Provide the toolchain name, region, etc. to configure.


## Integrating GitHub with the toolchain for version control and pipeline trigger
{: #integrating-github-with-the-toolchain}

* In the toolchain overview from the left menu. Click **Add a Tool** and search for GitHub.
* Configure GitHub tool for **GitHub server address**, **repository type** and **repository URL**.
* You can create a new repository, fork, clone or use an existing repository.

    In the following example we have used GitHub server as "[https://github.com](http://github.com/)", repository type as "Existing" and Repository URL as "https://github.com/sagar20896/mfp-devops-20181210030116092".

    ![configuring_toolchain](images/p03_configuring_toolchain.png)

### Adding the delivery pipeline to the toolchain
{: #adding-the-delivery-pipeline-to-the-toolchain}

Use **Add Tool** and search for *Delivery Pipeline*. Configure pipeline and click **Create integration**.

### Adding stages to the pipeline
{: #adding-stages-to-the-pipeline}

#### Stage 1 - Setting up {{ site.data.keyword.mobilefoundation_short }}

{: #stage1-setting-up-mobile-foundation}

In this stage we would be spinning up an instance of {{ site.data.keyword.mobilefoundation_short }}. Adding GitHub to the pipeline would be in this stage and it will trigger the pipeline whenever a change is pushed to the git repository. See GitHub configured as the input below. You may set the stage trigger based on your requirement. It could be manual or automatic.

In the following example we set Input Type as *Git repository*, Git repository as *mfp-devops-20181210030116092* and Git URL as *https://github.com/sagar20896/mfp-devops-20181210030116092* and branch as *master*.

![first_stage_git_input](images/p4_first_stage_git_input.png)

- Click **Add stage** and set the above configuration details in **Input** tab to point to the GitHub repo.
- In the **Jobs** tab, click **ADD JOB** and select *Deploy* as job type. Select **Deployer type** as *Cloud Foundry*.
- If you do not have an API key, you can create one for your {{ site.data.keyword.cloud_notm }} account [here](https://cloud.ibm.com/iam/#/apikeys).

Select/fill the other fields as required. Add the following in **Deploy script**:

```
	#!/bin/bash
	token=$(cf oauth-token | awk '{print $2}')
	json={\"deploy\":\"true\",\"name\":\"$INSTANCE_NAME\",\"token\":\"$token\"}
	echo "Input: $json"
	cf cs "Mobile Foundation" Developer $INSTANCE_NAME -c $json
	echo "Created Service Instance"
	sleep 1m
	cf create-service-key $INSTANCE_NAME Credentials-1
	echo "Created Service Keys"
	credentials=$(cf service-key $INSTANCE_NAME Credentials-1)
	echo "Credentials: $credentials"
	password=$(echo $credentials | grep \"password\": | awk -F "\"password\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	echo "Password: $password"
	url=$(echo $credentials | grep \"url\": | awk -F "\"url\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	echo "URL: $url"
	user=$(echo $credentials | grep \"user\": | awk -F "\"user\": " '{print $2 }' | awk -F , '{print $1 }' | awk -F \  '{print $1 }'| sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	echo "User: $user"
	until $(curl --output /dev/null --silent --head --fail $url/mfpconsole); do
	printf '.'
	sleep 5
	done
	# Deploying the confidential client
	curl -u $user:$password -X PUT -H "Content-Type: application/json" -d '{"clients":[{"id":"test","displayName":"Test","secret":"test","allowedScope":"*"},{"id": "admin","displayName": "admin","secret": "darwIn$hfr","allowedScope": "push.* mfp.admin.plugins settings.read"},{"id": "push","displayName": "push","secret": "darwIn$hfr","allowedScope": "authorization.introspect"}]}' $url/mfpadmin/management-apis/2.0/runtimes/mfp/confidentialclients
	exit
```
{: codeblock}

In the above script we use Cloud Foundry CLI to create a {{ site.data.keyword.mobilefoundation_short }} service instance.

![stage1_jobs_tab_config](images/p05_stage1_jobs_tab_config.png)

In the **Environment properties** tab, Add property *INSTANCE\_NAME* (as text property) as what you want the MobileFoundation instance name to be. It would be  used as an identifier across many stages.

![stage1_environment_properties](images/p06_stage1_environment_properties.png)

#### Stage 2 - Building an Adapter
{: #stage2-building-an-adapter}

In this stage we pull the adapter source code and build it.

- Click **Add stage** and configure the stage based on your preferences for adding GitHub repository as input. The **Stage trigger** must be set to *Run jobs when the previous stage is completed*.

- Switch to **Jobs** tab and add a build job. Select builder type as *Maven* and add the following in the build scripts.

```
	#!/bin/bash
	# To use Node.js 6.7.0, uncomment the following line:
	export PATH=/opt/IBM/node-v6.7.0/bin:$PATH
	echo "#### npm install -g mfpdev-cli"
	npm install -g mfpdev-cli
	echo "#### mfpdev --version"
	mfpdev --version
	echo "#### cd adapters/JavaAdapter"
	cd adapters/JavaAdapter
	echo "#### Build the adapter : mfpdev adapter build"
	mfpdev adapter build
```
{: codeblock}

In the above script we install [mfpdev-cli](https://www.npmjs.com/package/mfpdev-cli) to build adapters using adapter command in our repository's adapters/JavaAdapter.

In the following example we use **Builder type** as *npm* and use the script provided above in the build script. We leave **Working directory** and **Build archive directory** parameters empty.

![build_adapter_stage_jobs_config](images/p07_build_adapter_stage_jobs_config.png)

#### Stage 3 - Deploying an adapter
{: #stage3-deploying-an-adapter}

In this stage we will deploy the adapter to the {{ site.data.keyword.mobilefoundation_short }} instance that we created in the first stage.

- Add a new stage.
- Configure the input.
- Set the input type to **Build Artifacts**, **Stage** as the stage name where adapter is built, and the job that build the adapter in the build adapter stage.
- The **Stage trigger** must be set to *Run jobs when the previous stage is completed*.
- Add a deploy job from the **Jobs** tab.
- Select **Deployer type** as *Cloud Foundry* and configure the rest based on your preferences.

In the example below we use **Deployer type** as *Cloud Foundry*, **{{ site.data.keyword.cloud_notm }} Region** as *Dallas*, API key as the same that we created in the first stage.

![deploy_adapter](images/p08_deploy_adapter.png)


Use the below **Deploy script**:

```
	#!/bin/bash
	set -x
	# To use Node.js 6.7.0, uncomment the following line:
	export PATH=/opt/IBM/node-v6.7.0/bin:$PATH

	echo "Getting server and credentials"
	# get mfpserver url, username and password

	      credentials=$(cf service-key $INSTANCE_NAME Credentials-1)
	 echo "Credentials: $credentials"
	 password=$(echo $credentials | grep \"password\": | awk -F "\"password\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	 echo "Password: $password"
	 url=$(echo $credentials | grep \"url\": | awk -F "\"url\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/' | sed -e 's/https/http/g')
	 echo "URL: $url"
	 user=$(echo $credentials | grep \"user\": | awk -F "\"user\": " '{print $2 }' | awk -F , '{print $1 }' | awk -F \  '{print $1 }'| sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	 echo "User: $user"

	url=$url:80
	echo "#### npm install -g mfpdev-cli"
	npm install -g mfpdev-cli
	echo "#### mfpdev --version"
	mfpdev --version
	echo "#### cd adapters/JavaAdapter"
	cd adapters/JavaAdapter
	echo "#### adding server definition"
	mfpdev server add server1 --url $url --login $user --password $password --setdefault
	echo "#### Deploy Adapter : mfpdev adapter deploy"
	mfpdev adapter deploy
```
{: codeblock}

The above script uses mfpdev-cli's adapter deploy command to deploy adapter to the MFP instance.

- Set **INSTANCE_NAME** in the Environment properties tab as the one set in first stage when {{ site.data.keyword.mobilefoundation_short }} instance was created. We use the same instance name so that the adapter gets deployed to the instance we created in the first stage.


#### Stage 4 - Test Adapter
{: #stage4-test-adapter}

In this stage we intend to test the adapter that were built and deployed in the previous stages. In our sample adapter repository, we have scripts at ‘adapters/JavaAdapter/tests’ to test the adapter end points.

- Set the **Input** as *GitHub repository* same as the configurations of build adapters.

- Add *deploy job* in the **Jobs** tab. Select the **Deployer type** as *Cloud Foundry*. We use deploy job instead of test job because deploy has options to integrate with cloud foundry easily.

Use the following scripts for testing adapters.

```
	#!/bin/bash
	set -x
	# To use Node.js 6.7.0, uncomment the following line:
	export PATH=/opt/IBM/node-v6.7.0/bin:$PATH

	echo "Getting server and credentials"
	# get mfpserver url, username and password

	 credentials=$(cf service-key $INSTANCE_NAME Credentials-1)
	 echo "Credentials: $credentials"
	 password=$(echo $credentials | grep \"password\": | awk -F "\"password\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	 echo "Password: $password"
	 url=$(echo $credentials | grep \"url\": | awk -F "\"url\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/' | sed -e 's/https/http/g')
	 echo "URL: $url"
	 user=$(echo $credentials | grep \"user\": | awk -F "\"user\": " '{print $2 }' | awk -F , '{print $1 }' | awk -F \  '{print $1 }'| sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	 echo "User: $user"

	url=$url:80
	echo "#### npm install -g mfpdev-cli"
	npm install -g mfpdev-cli
	echo "#### mfpdev --version"
	mfpdev --version
	echo "#### adding server definition"
	mfpdev server add server1 --url $url --login $user --password $password --setdefault

	# Make sure to have the test confidential client. Try another attempt to deploy test confidential client
	curl -u $user:$password -X PUT -H "Content-Type: application/json" -d '{"clients":[{"id":"test","displayName":"Test","secret":"test","allowedScope":"*"},{"id": "admin","displayName": "admin","secret": "darwIn$hfr","allowedScope": "push.* mfp.admin.plugins settings.read"},{"id": "push","displayName": "push","secret": "darwIn$hfr","allowedScope": "authorization.introspect"}]}' $url/mfpadmin/management-apis/2.0/runtimes/mfp/confidentialclients

	echo "#### cd adapters/JavaAdapter/tests"
	cd adapters/JavaAdapter/tests
	echo "#### Testing Adapter Endpoints"
	./resource.sh
	./resource_greet.sh
	./resource_protected.sh
	./resource_testpost.sh
```
{: codeblock}

- Add *INSTANCE_NAME* in environment properties, same values that we add in previous stages.

You may use API testing framework for testing the adapters. In the example above we have shell scripts that call the adapter and test for correctness.


#### Stage 5 - Building apps with Fastlane
{: #stage5-building-apps-with-fastlane}

Input for this should be the GitHub repo we have used in the previous stages. This stage must get triggered after the previous stage (Test Adapter) passes.

For building the app we use deploy job template in the **Jobs** tab. Use *Cloud Foundry* as **Deployer type**.

Use the below deploy script to build the app:

```
	# To use Node.js 6.7.0, uncomment the following line:
	export LANG=en_US.UTF-8

	export PATH=/opt/IBM/node-v6.7.0/bin:$PATH

	echo "Getting server and credentials"
	# get mfpserver url, username and password

	 credentials=$(cf service-key $INSTANCE_NAME Credentials-1)
	 echo "Credentials: $credentials"
	 password=$(echo $credentials | grep \"password\": | awk -F "\"password\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	 echo "Password: $password"
	 url=$(echo $credentials | grep \"url\": | awk -F "\"url\": " '{print $2 }' | awk -F , '{print $1 }' | sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/' | sed -e 's/https/http/g')
	 echo "URL: $url"
	 user=$(echo $credentials | grep \"user\": | awk -F "\"user\": " '{print $2 }' | awk -F , '{print $1 }' | awk -F \  '{print $1 }'| sed 's/.\(.*\)/\1/' | sed 's/\(.*\)./\1/')
	 echo "User: $user"

	url=$url:80
	echo "#### npm install -g mfpdev-cli"
	npm install -g mfpdev-cli
	echo "#### mfpdev --version"
	mfpdev --version
	echo "#### cd apps/ResourceRequestAndroid"
	cd apps/ResourceRequestAndroid
	#echo "#### adding server definition"
	mfpdev server add server1 --url $url --login $user --password $password --setdefault
	echo "#### Register App with MFP Server : mfpdev app register"
	mfpdev app register


	export JAVA_HOME=/opt/IBM/java8
	cd /home/pipeline

	# Android sdk
	wget https://dl.google.com/android/repository/sdk-tools-linux-3859397.zip
	sudo apt-get install unzip
	unzip /home/pipeline/sdk-tools-linux-3859397.zip
	echo 'y' | /home/pipeline/tools/bin/sdkmanager --licenses
	echo 'y' | /home/pipeline/tools/bin/sdkmanager "platform-tools" "platforms;android-26"

	# Prereq for installing Fastlane: Install RVM
	gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
	\curl -L https://get.rvm.io | bash -s stable --ruby
	source /home/pipeline/.rvm/scripts/rvm get stable --autolibs=enable
	gem -v

	# Install Fastlane
	gem install fastlane -NV

	# Build the apk file
	cd /home/pipeline/$BUILD_ID/apps/ResourceRequestAndroid
	fastlane beta

	mkdir /home/pipeline/temp
	cp ./app/build/outputs/apk/app-release.apk /home/pipeline/temp
	mkdir /home/pipeline/appupload

	# Push the generated apk for git hub for testing
	git config --global user.name $gitPushUser
	git config --global user.email $gitPushEmail
	git config --global push.default matching
	cd /home/pipeline/appupload

	git clone $appPublishUrl .
	ls
	rm -rf

	cp /home/pipeline/temp/app-release.apk .
	git add app-release.apk
	git commit -m "released a new version of apk - build : ($BUILD_ID)"
	echo $apkGitPushUrl
	git push $apkGitPushUrl
```
{: codeblock}

In the above script we use `mfpdev-cli` to register the app to {{ site.data.keyword.mobilefoundation_short }}
 [Fastlane](https://fastlane.tools/) to build and release the app.

The environment variables used in the script shall be defined in the next **Environment properties** tab.

In the environment properties, add the below properties:

- *INSTANCE_NAME* as the **mfp instance name**
- *appPublishUrl* as **Repository** where the app should be published so that testing stage can pull it.
- *gitPushUser* as the **GitHub username**
- *gitPushEmail* as the **GitHub user’s email**
- *gitPushToken* as the **git push token**
- *apkGitPushUrl* as **https://$gitPushToken:x-oauth-basic@github.com/<path><SPACE><branch>**
    `eg.: https://$gitPushToken:x-oauth-basic@github.com/ShinojEdakkara/mfp-apps master`


#### Stage 6 - Test the app using Bitbar
{: #stage6-test-the-app-using-bitbar}

This stage also is open ended, you may use any testing framework based on your requirements. In this example we use [Bitbar](https://bitbar.com/testing/).

- Add the GitHub repository details about the test project you have (this would consist of all the test scripts and artifacts used to support the testing).

- For Bitbar, add a build job under **Jobs** tab and select the **Builder type** as *Maven*.

Use the build script similar to the below:

```
	#!/bin/bash
	cd /home/pipeline/$BUILD_ID/appium/sample-scripts/java
	mvn clean install -X -Dtest=$test -DexecutionType=$executionType -DapiKey=$bitbarApiKey -DapplicationPath=$applicationPath -Dtestdroid_project=$testdroid_project
```
{: codeblock}

For the above some script you would require a few environment variables.

- *screenshot\_dir* as **/home/pipeline/home/pipeline/$BUILD\_ID/target**
- *applicationPath* as the **GitHub path** to the application you intend to test
- *executionType* as the **Execution type**
- *test* as the **Test** you wish to perform
- *bitbarApiKey* as the **Bitbar API Key**
- *testdroid_project* as the as the **Project**.


#### Stage 7 - Tearing down {{ site.data.keyword.mobilefoundation_short }}
{: #stage7-tearing-down-mobile-foundation}

In this stage we would be tearing down the {{ site.data.keyword.mobilefoundation_short }} instance that was created in the first stage for build, deploy and testing in the previous stages.

There wouldn't be any input to this stage.

- Create a *Deploy job* in the **Jobs** tab.
- Use below script:

```
	#!/bin/bash
	sleep 60
	cf dsk -f $INSTANCE_NAME-Analytics Credentials-1
	cf ds -f $INSTANCE_NAME-Analytics
	cf dsk -f $INSTANCE_NAME Credentials-1
	cf ds -f $INSTANCE_NAME
	cf d -f $INSTANCE_NAME-Server
	exit
```
{: codeblock}

- Set *INSTANCE_NAME* in the **Environment properties** tab as the one set in first stage when {{ site.data.keyword.mobilefoundation_short }} instance was created.

