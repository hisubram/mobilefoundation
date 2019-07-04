---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile foundation, integration, devops, ibmcloud, pipeline

subcollection:  mobilefoundation
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


# Distribution d'applications {{ site.data.keyword.mobilefoundation_short }} à l'aide d'{{ site.data.keyword.cloud_notm }} {{ site.data.keyword.jazzhub_short }}
{: #delivering_mobile_foundation_apps_using_ibm_cloud_devops_services}

Ce tutoriel vous aide à automatiser la distribution des applications et des adaptateurs à IBM {{ site.data.keyword.mobilefoundation_short }} en utilisant {{ site.data.keyword.jazzhub_title }} sur {{ site.data.keyword.cloud_notm }}.

La figure suivante offre une vue d'ensemble du pipeline.

![overview_of_pipeline](images/p00_overview_of_pipeline.png "Les six étapes du pipeline DevOps")


## Prérequis
{: #mpfapps-devops-prerequisites }

* [Compte {{ site.data.keyword.cloud_notm }}](https://cloud.ibm.com/registration)
* [mfpdev-cli](https://www.npmjs.com/package/mfpdev-cli)
* Exemple d'application et [adaptateur MFP](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/)
* Compte [GitHub](http://github.com/)
* *Facultatif :* Instance [Bitbar](https://bitbar.com/testing/) et clé d'API Bitbar (vous pouvez utiliser n'importe quel service en fonction de vos besoins).


## Création d'un service de distribution continue et d'une chaîne d'outils
{: #creating-continuous-delivery-service-and-toolchain }

* Recherchez "distribution continue" dans le catalogue {{ site.data.keyword.cloud_notm }} (ou [cliquez ici](https://cloud.ibm.com/catalog/services/continuous-delivery)).
* Créez le service en indiquant le nom du service, la région, etc.

    Dans l'exemple suivant, nous utilisons le nom de service *MFP App/Adapter delivery Test*, la région/l'emplacement *London* et le groupe de ressources *Default*.

    ![configuring_continuous_delivery_service](images/p01_configuring_continuous_delivery_service.png "Page de création du catalogue pour l'instance de service Mobile Foundation")

* Dans le menu de navigation, sélectionnez **DevOps**, puis cliquez sur **Créer une chaîne d'outils** et recherchez “Construisez votre propre chaîne d'outils” pour créer une toute nouvelle chaîne d'outils.

    ![search_build_your_own_toolchain](images/p02_search_build_your_own_toolchain.png "Page de création de votre propre chaîne d'outils avec résultat de la recherche pour construire votre propre chaîne d'outils")

* Indiquez le nom de la chaîne d'outils, la région, etc. pour effectuer la configuration.


## Intégration de GitHub à la chaîne d'outils pour le contrôle de version et le déclenchement de pipeline
{: #integrating-github-with-the-toolchain}

* Dans la page **Présentation** de la navigation, cliquez sur **Ajouter un outil** et recherchez GitHub.
* Configurez l'**adresse de serveur GitHub**, le **type de référentiel** et l'**URL de référentiel** dans l'outil GitHub.
* Vous pouvez créer un nouveau référentiel, une nouvelle bifurcation ou un nouveau clone, ou utiliser un référentiel existant.

    L'exemple suivant utilise le serveur GitHub "[https://github.com](http://github.com/)", le type de référentiel *Existing* et l'URL de référentiel *https://github.com/sagar20896/mfp-devops-20181210030116092*.

    ![configuring_toolchain](images/p03_configuring_toolchain.png "Ecran de configuration de l'intégration qui affiche les zones Serveur GitHub, Type de référentiel et URL de référentiel")

### Ajout du pipeline de distribution à la chaîne d'outils
{: #adding-the-delivery-pipeline-to-the-toolchain}

Utilisez **Ajouter un outil** et recherchez *Pipeline de distribution*. Configurez le pipeline et cliquez sur **Créer une intégration**.

### Ajout d'étapes au pipeline
{: #adding-stages-to-the-pipeline}

#### Etape 1 - Configuration de {{ site.data.keyword.mobilefoundation_short }}

{: #stage1-setting-up-mobile-foundation}

Dans cette étape, nous allons utiliser une instance de {{ site.data.keyword.mobilefoundation_short }}. L'ajout de GitHub au pipeline s'effectue au cours de cette étape et déclenche le pipeline lorsqu'une modification est introduite dans le référentiel Git. Les étapes suivantes illustrent la configuration GitHub. Vous pouvez définir le déclencheur d'étape en fonction de vos besoins. Il peut être manuel ou automatique.

Dans l'exemple suivant, nous avons défini *Git repository* comme type d'entrée, *mfp-devops-20181210030116092* comme référentiel Git,  *https://github.com/sagar20896/mfp-devops-20181210030116092* comme URL Git et *master* comme branche.

![first_stage_git_input](images/p4_first_stage_git_input.png "Ecran de configuration de Mobile Foundation avec l'onglet Entrée sélectionné")

- Cliquez sur **Ajouter une étape** et configurez l'onglet **Entrée** pour qu'il pointe vers le référentiel GitHub, comme indiqué dans l'image.
- Dans l'onglet **Travaux**, cliquez sur **AJOUTER UN TRAVAIL** et sélectionnez *Déployer* comme type de travail. Sélectionnez *Cloud Foundry* comme **Type de déployeur**.
- Si vous n'avez pas de clé d'API, vous pouvez en créer une pour votre compte {{ site.data.keyword.cloud_notm }} [ici](https://cloud.ibm.com/iam/#/apikeys).

Sélectionnez/remplissez les autres zones si nécessaire. Ajoutez les lignes suivantes dans le **Script de déploiement** :

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

Dans le script précédent, nous avons utilisé l'interface de ligne de commande Cloud Foundry pour créer une instance de service {{ site.data.keyword.mobilefoundation_short }}. 

![stage1_jobs_tab_config](images/p05_stage1_jobs_tab_config.png "Ecran de configuration de Mobile Foundation avec l'onglet Travaux sélectionné")

Dans l'onglet **Propriétés d'environnement**, ajoutez la propriété *INSTANCE\_NAME* (en tant que propriété de texte) selon le nom que vous voulez affecter à l'instance MobileFoundation. Elle sera utilisée comme identificateur dans de nombreuses étapes.

![stage1_environment_properties](images/p06_stage1_environment_properties.png "Ecran de configuration de Mobile Foundation avec l'onglet Propriétés d'environnement sélectionné")

#### Etape 2 - Génération d'un adaptateur
{: #stage2-building-an-adapter}

Dans cette étape, nous extrayons le code source de l'adaptateur et le générons.

- Cliquez sur **Ajouter une étape** et configurez l'étape en fonction de vos préférences pour ajouter le référentiel GitHub comme entrée. L'option **Déclencheur d'étape** doit être définie sur *Exécuter les travaux lorsque l'étape précédente est terminée*.

- Accédez à l'onglet **Travaux** et ajoutez un travail de génération. Sélectionnez le type de générateur *Maven* et ajoutez les lignes suivantes dans les scripts de génération.

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

Dans le script précédent, nous avons installé [mfpdev-cli](https://www.npmjs.com/package/mfpdev-cli) pour générer des adaptateurs à l'aide d'une commande d'adaptateur dans le chemin `adapters/JavaAdapter` de notre référentiel. 

Dans l'exemple suivant, nous utilisons *npm* comme **Type de générateur** et le script fourni dans le script de génération. Nous laissons les paramètres **Répertoire de travail** et **Répertoire d'archivage de génération** vides.

![build_adapter_stage_jobs_config](images/p07_build_adapter_stage_jobs_config.png "Ecran BuildAdapter avec l'onglet Travaux sélectionné")

#### Etape 3 - Déploiement d'un adaptateur
{: #stage3-deploying-an-adapter}

Dans cette étape, nous déployons l'adaptateur sur l'instance {{ site.data.keyword.mobilefoundation_short }} que nous avons créée lors de la première étape.

- Ajoutez une étape.
- Configurez l'entrée.
- Définissez le type d'entrée sur **Artefacts de génération**, **Etape** comme nom d'étape au cours de laquelle l'adaptateur est généré, ainsi que le travail qui génère l'adaptateur lors de l'étape correspondante.
- L'option **Déclencheur d'étape** doit être définie sur *Exécuter les travaux lorsque l'étape précédente est terminée*.
- Ajoutez un travail de déploiement à partir de l'onglet **Travaux**.
- Sélectionnez *Cloud Foundry* comme **Type de déployeur** et configurez le reste en fonction de vos préférences.

Dans l'exemple suivant, nous utilisons *Cloud Foundry* comme **Type de déployeur**, *Dallas* comme **Région {{ site.data.keyword.cloud_notm }}**, ainsi que la clé d'API
que nous avons créée lors de la première étape.

![deploy_adapter](images/p08_deploy_adapter.png "Ecran Déployer avec l'onglet Travaux sélectionné")


Utilisez le **Script de déploiement** ci-dessous.

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

Le script utilise la commande mfpdev-cli pour déployer l'adaptateur sur l'instance MFP.

- Définissez le paramètre *INSTANCE_NAME* dans l'onglet **Propriétés d'environnement** en utilisant la valeur définie au cours de la première étape lors de la création de l'instance {{ site.data.keyword.mobilefoundation_short }}. Nous utilisons le même nom d'instance que lors du déploiement de l'adaptateur sur l'instance que nous avons créée au cours de la première étape.


#### Etape 4 - Test de l'adaptateur
{: #stage4-test-adapter}

Dans cette étape, nous allons tester l'adaptateur qui a été généré et déployé lors des étapes précédentes. Dans notre exemple de référentiel d'adaptateur, des scripts se trouvent dans le chemin ‘adapters/JavaAdapter/tests’ pour tester les noeuds finaux de l'adaptateur.

- Définissez le *référentiel GitHub* indiqué dans les configurations d'adaptateurs de génération comme **Entrée**.

- Ajoutez *travail de déploiement* dans l'onglet **Travaux**. Sélectionnez *Cloud Foundry* comme **Type de déployeur**. Nous utilisons un travail de déploiement plutôt qu'un travail de test car le déploiement comporte des options qui s'intègrent facilement à Cloud Foundry.

Utilisez les scripts suivants pour tester les adaptateurs.

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

- Ajoutez *INSTANCE_NAME* dans les propriétés d'environnement, en utilisant les valeurs ajoutées lors des étapes précédentes.

Vous pouvez utiliser l'infrastructure de test d'API pour tester les adaptateurs. Dans cet exemple, des scripts shell appellent l'adaptateur et testent s'il est correct.


#### Etape 5 - Génération d'applications avec Fastlane
{: #stage5-building-apps-with-fastlane}

L'entrée pour cette étape doit être le référentiel GitHub que nous avons utilisé lors des étapes précédentes. Cette étape doit être déclenchée une fois que l'étape précédente (Test de l'adaptateur) a réussi.

Pour générer l'application, nous utilisons un modèle de travail de déploiement dans l'onglet **Travaux**. Utilisez *Cloud Foundry* comme **Type de déployeur**.

Utilisez le script suivant pour générer l'application :

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

Dans le script précédent, nous avons utilisé `mfpdev-cli` pour enregistrer l'application dans {{ site.data.keyword.mobilefoundation_short }}
 [Fastlane](https://fastlane.tools/) en vue de sa génération et de sa publication. 

Les variables d'environnement qui sont utilisées dans le script sont définies dans l'onglet **Propriétés d'environnement** suivant.

Dans les propriétés d'environnement, ajoutez les propriétés suivantes :

- *INSTANCE_NAME* comme **nom d'instance mfp**
- *appPublishUrl* comme **Référentiel** où l'application doit être publiée afin que l'étape de test puisse l'extraire.
- *gitPushUser* comme **Nom d'utilisateur GitHub**
- *gitPushEmail* comme **Adresse électronique de l'utilisateur GitHub**
- *gitPushToken* comme **Jeton push git**
- *apkGitPushUrl* comme **https://$gitPushToken:x-oauth-basic@github.com/<chemin><ESPACE><branche>**
    `eg.: https://$gitPushToken:x-oauth-basic@github.com/ShinojEdakkara/mfp-apps master`


#### Etape 6 - Test de l'application à l'aide de Bitbar
{: #stage6-test-the-app-using-bitbar}

Cette étape est également ouverte. Vous pouvez utiliser n'importe quelle infrastructure de test en fonction de vos besoins. Dans cet exemple, nous utilisons [Bitbar](https://bitbar.com/testing/).

- Ajoutez les informations du référentiel GitHub dont vous disposez sur le projet de test (ce projet est composé de tous les artefacts et scripts de test qui sont utilisés pour la prise en charge du test).

- Pour Bitbar, ajoutez un travail de génération sous l'onglet **Travaux** et sélectionnez *Maven* comme **Type de générateur**.

Utilisez un script de génération similaire au script suivant :

```
	#!/bin/bash
	cd /home/pipeline/$BUILD_ID/appium/sample-scripts/java
	mvn clean install -X -Dtest=$test -DexecutionType=$executionType -DapiKey=$bitbarApiKey -DapplicationPath=$applicationPath -Dtestdroid_project=$testdroid_project
```
{: codeblock}

Le script précédent requiert quelques variables d'environnement. 

- *screenshot\_dir* comme **/home/pipeline/home/pipeline/$BUILD\_ID/target**
- *applicationPath* comme **chemin d'accès GitHub** à l'application que vous souhaitez tester
- *executionType* comme **Type d'exécution**
- *test* comme **Test** à exécuter
- *bitbarApiKey* comme **Clé d'API Bitbar**
- *testdroid_project* comme **Projet**.


#### Etape 7 - Désassemblage de {{ site.data.keyword.mobilefoundation_short }}
{: #stage7-tearing-down-mobile-foundation}

Lors de cette étape, nous allons désassembler l'instance {{ site.data.keyword.mobilefoundation_short }} qui a été créée au cours de la première étape pour la
génération, le déploiement et le test dans les étapes précédentes.

Cette étape ne requiert aucune entrée.

- Créez un *Travail de déploiement* dans l'onglet **Travaux**.
- Utilisez le script suivant :

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

- Définissez le paramètre *INSTANCE_NAME* dans l'onglet **Propriétés d'environnement** en utilisant la valeur définie au cours de la première étape lors de la création de l'instance {{ site.data.keyword.mobilefoundation_short }}.
