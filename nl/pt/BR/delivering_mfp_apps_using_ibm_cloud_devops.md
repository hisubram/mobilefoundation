---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-13"

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


# Entregando apps do {{ site.data.keyword.mobilefoundation_short }} usando o {{ site.data.keyword.cloud_notm }} {{ site.data.keyword.jazzhub_short }}
{: #delivering_mobile_foundation_apps_using_ibm_cloud_devops_services}

Este tutorial ajuda você a automatizar a entrega de apps e adaptadores para o IBM {{ site.data.keyword.mobilefoundation_short }} usando o {{ site.data.keyword.jazzhub_title }}
 no {{ site.data.keyword.cloud_notm }}.

A imagem a seguir fornece uma visão geral do pipeline.

![overview_of_pipeline](images/p00_overview_of_pipeline.png)


## Pré-requisitos
{: #mpfapps-devops-prerequisites }

* [ Conta do {{site.data.keyword.cloud_notm }} ](https://cloud.ibm.com/registration)
* [ mfpdev-cli ](https://www.npmjs.com/package/mfpdev-cli)
* Um Aplicativo de amostra e [Adaptador MFP](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/)
* Conta [ GitHub ](http://github.com/)
* **Opcional:** a instância do [Bitbar](https://bitbar.com/testing/) e a chave de API do Bitbar (é possível usar qualquer serviço conforme seus requisitos).


## Criando o Continuous Delivery Service e a cadeia de ferramentas
{: #creating-continuous-delivery-service-and-toolchain }

* Procure "Continuous Delivery" no Catálogo do {{ site.data.keyword.cloud_notm }} (ou [clique aqui](https://cloud.ibm.com/catalog/services/continuous-delivery)).
* Crie o serviço fornecendo o nome do serviço, a região e assim por diante.

    No exemplo a seguir, usamos o nome do serviço como "Teste de entrega do app/adaptador MFP", região/local como "Londres" e grupo de recursos como "Padrão".

    ![configuring_continuous_delivery_service](images/p01_configuring_continuous_delivery_service.png)

* Na seção {{ site.data.keyword.jazzhub_title }}
no menu hambúrguer à esquerda, crie uma cadeia de ferramentas e procure “Construa sua própria cadeia de ferramentas” para criar uma cadeia de ferramentas do zero.

    ![search_build_your_own_toolchain](images/p02_search_build_your_own_toolchain.png)

* Forneça o nome da cadeia de ferramentas, a região e assim por diante para configurar.


## Integrando o GitHub à cadeia de ferramentas para controle de versão e acionador de pipeline
{: #integrating-github-with-the-toolchain}

* Na visão geral da cadeia de ferramentas no menu esquerdo. Clique em **Incluir uma ferramenta** e procure GitHub.
* Configure a ferramenta GitHub para o **Endereço do servidor GitHub**, **tipo de repositório** e **URL do repositório**.
* É possível criar um novo repositório, bifurcar, clonar ou usar um repositório existente.

    No exemplo a seguir, usamos o servidor GitHub como "[https://github.com](http://github.com/)", o tipo de repositório como "Existente" e a URL do repositório como "https://github.com/sagar20896/mfp-devops-20181210030116092".

    ![configuring_toolchain](images/p03_configuring_toolchain.png)

### Incluindo o Pipeline de Entrega para a Cadeia
{: #adding-the-delivery-pipeline-to-the-toolchain}

Use **Incluir ferramenta** e procure *Delivery Pipeline*. Configure o pipeline e clique em  ** Criar integração **.

### Incluindo estágios no pipeline
{: #adding-stages-to-the-pipeline}

#### Estágio 1 - Configurando o {{ site.data.keyword.mobilefoundation_short }}

{: #stage1-setting-up-mobile-foundation}

Nesse estágio, nós estaríamos acelerando uma instância do {{ site.data.keyword.mobilefoundation_short }}. Incluir GitHub no pipeline seria neste estágio e ele acionará o pipeline sempre que uma mudança for enviada por push para o repositório do git. As etapas a seguir mostram a configuração do GitHub. É possível configurar o acionador de estágio com base em seu requisito. Pode ser manual ou automático.

No exemplo a seguir, nós configuramos o Tipo de entrada como *Repositório Git*, o repositório Git como *mfp-devops-20181210030116092* e a URL do Git como *https://github.com/sagar20896/mfp-devops-20181210030116092* e ramificação como *master*.

![first_stage_git_input](images/p4_first_stage_git_input.png)

- Clique em **Incluir estágio** e configure a guia **Entrada** para apontar para o repositório GitHub, conforme mostrado na imagem.
- Na guia **Tarefas**, clique em **INCLUIR TAREFA** e selecione *Implementar* como o tipo de tarefa. Selecione **Deployer type** como *Cloud Foundry*.
- Se você não tiver uma chave de API, será possível criar uma para a sua conta do {{ site.data.keyword.cloud_notm }} [aqui](https://cloud.ibm.com/iam/#/apikeys).

Selecione / preencha os outros campos, conforme necessário. Inclua as linhas a seguir no **Script de implementação**:

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

No script acima, utilizamos a CLI do Cloud Foundry para criar uma instância de serviço do {{ site.data.keyword.mobilefoundation_short }}.

![stage1_jobs_tab_config](images/p05_stage1_jobs_tab_config.png)

Na guia **Propriedades do ambiente**, inclua a propriedade *INSTANCE\_NAME* (como a propriedade de texto) tal como você deseja que o nome da instância do MobileFoundation seja. Ele seria usado como um identificador em vários estágios.

![stage1_environment_properties](images/p06_stage1_environment_properties.png)

#### Estágio 2-Construindo um Adaptador
{: #stage2-building-an-adapter}

Nesse estágio, nós fazemos pull do código-fonte do adaptador e o construímos.

- Clique em **Incluir estágio** e configure o estágio com base em suas preferências para incluir o repositório GitHub como entrada. O **Acionador de estágio** deve ser configurado para *Executar tarefas quando o estágio anterior for concluído*.

- Alterne para a guia **Tarefas** e inclua uma tarefa de construção. Selecione o tipo de construtor como *Maven* e inclua as linhas a seguir nos scripts de construção.

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

No script acima, nós instalamos o [mfpdev-cli](https://www.npmjs.com/package/mfpdev-cli) para construir adaptadores usando o comando do adaptador no `adapters/JavaAdapter` de nosso repositório.

No exemplo a seguir, usamos o **Tipo de construtor** como *npm* e usamos o script que é fornecido no script de construção. Deixamos os parâmetros **Diretório ativo** e **Diretório de archive de compilação** vazios.

![build_adapter_stage_jobs_config](images/p07_build_adapter_stage_jobs_config.png)

#### Estágio 3-Implementando um adaptador
{: #stage3-deploying-an-adapter}

Nesse estágio, implementamos o adaptador na instância do {{ site.data.keyword.mobilefoundation_short }} que criamos no primeiro estágio.

- Incluir um estágio.
- Configure a entrada.
- Configure o tipo de entrada como **Artefatos de construção**, **Estágio** como o nome do estágio no qual o adaptador é construído e a tarefa que constrói o adaptador no estágio do adaptador de construção.
- O **Acionador de estágio** deve ser configurado para *Executar tarefas quando o estágio anterior for concluído*.
- Inclua uma tarefa de implementação na guia **Tarefas**.
- Selecione **Tipo de implementador** como *Cloud Foundry* e configure o restante com base em suas preferências.

No exemplo a seguir, usamos o **Tipo de implementador** como *Cloud Foundry*, **Região do {{ site.data.keyword.cloud_notm }}** como *Dallas*, a chave de API como a mesma que criamos no primeiro estágio.

![deploy_adapter](images/p08_deploy_adapter.png)


Use abaixo  ** Implementar script **:

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

O script usa o comando de implementação do adaptador do mfpdev-cli para implementar o adaptador na instância do MFP.

- Configure *INSTANCE_NAME* na guia **Propriedades do ambiente** com o valor igual ao configurado no primeiro estágio quando a instância do {{ site.data.keyword.mobilefoundation_short }} foi criada. Usamos o mesmo nome de instância para que o adaptador seja implementado na instância que criamos no primeiro estágio.


#### Estágio 4-Adaptador de Teste
{: #stage4-test-adapter}

Nesse estágio, pretendemos testar o adaptador que foi construído e implementado nos estágios anteriores. Em nosso repositório de adaptador de amostra, nós temos scripts em ‘adapters/JavaAdapter/tests’ para testar os terminais do adaptador.

- Configure a **Entrada** como *Repositório GitHub* de forma idêntica às configurações de adaptadores de construção.

- Inclua *tarefa de implementação* na guia **Tarefas**. Selecione o **Tipo de implementador** como *Cloud Foundry*. Usamos tarefa de implementação em vez de tarefa de teste porque a implementação tem opções para integrar facilmente ao Cloud Foundry.

Use os scripts a seguir para testar os adaptadores.

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

- Inclua *INSTANCE_NAME* em propriedades do ambiente, os mesmos valores que incluímos em estágios anteriores.

É possível usar a estrutura de teste de API para testar os adaptadores. Nesse exemplo, temos shell scripts que chamam o adaptador e testam a exatidão.


#### Estágio 5-Construindo apps com Fastlane
{: #stage5-building-apps-with-fastlane}

A entrada para esse estágio deve ser o repositório GitHub que usamos nos estágios anteriores. Esse estágio deve ser acionado após a passagem do estágio anterior (Testar adaptador).

Para construir o app, usamos o modelo de tarefa de implementação na guia **Tarefas**. Use o  * Cloud Foundry *  como  ** Deployer type **.

Use o script a seguir para construir o app:

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

No script acima, usamos `mfpdev-cli` para registrar o app no {{ site.data.keyword.mobilefoundation_short }}
[Fastlane](https://fastlane.tools/) para construir e liberar o app.

As variáveis de ambiente que são usadas no script são definidas na próxima guia **Propriedades do ambiente**.

Nas propriedades do ambiente, inclua as propriedades a seguir,

- *INSTANCE_NAME* como o **nome da instância mfp**
- *appPublishUrl* como **Repositório** no qual o app precisa ser publicado para que o estágio de teste possa puxá-lo.
- *gitPushUser* como o **nome do usuário do GitHub**
- *gitPushEmail* como o **e-mail do usuário do GitHub**
- *gitPushToken* como o **git push token**
- *apkGitPushUrl* como **https://$gitPushToken:x-oauth-basic@github.com/<path><SPACE><branch>**
    `eg.: https://$gitPushToken:x-oauth-basic@github.com/ShinojEdakkara/mfp-apps master`


#### Estágio 6 - Testar o app usando Bitbar
{: #stage6-test-the-app-using-bitbar}

Esse estágio também está aberto encerrado. É possível usar qualquer estrutura de teste com base em seus requisitos. Neste exemplo, usamos [Bitbar](https://bitbar.com/testing/).

- Inclua os detalhes do repositório GitHub sobre o projeto de teste que você tem (esse projeto consiste em todos os scripts de teste e artefatos que são usados para suportar o teste).

- Para Bitbar, inclua uma tarefa de construção na guia **Tarefas** e selecione o **Tipo de construtor** como *Maven*.

Use um script de construção semelhante ao script a seguir,

```
	#!/bin/bash
	cd /home/pipeline/$BUILD_ID/appium/sample-scripts/java
	mvn clean install -X -Dtest=$test -DexecutionType=$executionType -DapiKey=$bitbarApiKey -DapplicationPath=$applicationPath -Dtestdroid_project=$testdroid_project
```
{: codeblock}

Para o script acima, você precisa de algumas variáveis de ambiente.

- * screenshot\_dir *  como  ** /home/pipeline/home/pipeline/ $BUILD\_ID/target **
- *applicationPath* como o **Caminho do GitHub** para o aplicativo que você pretende testar
- * executionType *  como o  ** Tipo de execução **
- *test* como o **Teste** que você deseja executar
- * bitbarApiKey *  como a  ** Chave de API Bitbar **
- * testdroid_project *  como o  ** Projeto **.


#### Estágio 7 - Tearing down {{ site.data.keyword.mobilefoundation_short }}
{: #stage7-tearing-down-mobile-foundation}

Nesse estágio, eliminaremos a instância do {{ site.data.keyword.mobilefoundation_short }} que foi criada no primeiro estágio para construção, implementação e teste nos estágios anteriores.

Não haverá nenhuma entrada para esse estágio.

- Crie uma *Tarefa Implementar* na guia **Tarefas**.
- Use o script a seguir,

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

- Configure *INSTANCE_NAME* na guia **Propriedades do ambiente** como aquele configurado no primeiro estágio quando a instância do {{ site.data.keyword.mobilefoundation_short }} foi criada.
