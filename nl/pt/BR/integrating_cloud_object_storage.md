---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-24"

keywords: mobile foundation, integration, cloud object storage, COS, ibm cloud

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

# Usando {{ site.data.keyword.cos_full_notm }} com {{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}}
{: #using_ibm_cloud_object_storage_with_ibm_mobile_foundation}

O {{ site.data.keyword.IBM_notm}} {{ site.data.keyword.mobilefoundation_short}} (MF) entrega recursos de classificação corporativa que são projetados exclusivamente para suportar a construção e a implementação da próxima geração de apps móveis cognitivos, contextuais e personalizados. O {{ site.data.keyword.cos_full_notm }} (COS) é um armazenamento em nuvem flexível, com custo reduzido e escalável para dados não estruturados. Este guia de instruções explica como um aplicativo móvel que usa o {{ site.data.keyword.mobilefoundation_short}} pode se conectar e buscar ou fazer upload de dados para o {{ site.data.keyword.cos_full_notm }} por meio de um aplicativo Ionic. O aplicativo Ionic, o adaptador e os arquivos relacionados para este tutorial de instruções estão disponíveis [aqui](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App).
{: shortdesc}


## Pré-requisitos
{: #cos-prerequisites}

1. Instale o [mfpdev-cli](https://www.ibm.com/support/knowledgecenter/en/SSHS8R_8.0.0/com.ibm.worklight.dev.doc/dev/c_wl_cli_description.html) executando `npm install -g mfpdev-cli`. Essa CLI é usada para registrar o app Ionic e implementar o adaptador no servidor MF. Como alternativa, essas atividades podem ser executadas no painel do servidor MF.

2. Instale o [{{ site.data.keyword.cloud_notm}} CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-ibmcloud-cli) em sua máquina.

3. Instale a cli do Ionic executando `npm install -g ionic`

4. Instale o Cordova executando `npm install -g cordova`


## {{ site.data.keyword.mobilefoundation_short}} Configuração do servidor
{: #mobile-foundation-server-setup}

O servidor {{ site.data.keyword.mobilefoundation_short}} é configurado no {{ site.data.keyword.cloud_notm}}. Configure uma instância do {{ site.data.keyword.cloud_notm}} do servidor {{ site.data.keyword.mobilefoundation_short}} conforme a seguir,

* No Catálogo do {{ site.data.keyword.cloud_notm}}, procure por *{{ site.data.keyword.mobilefoundation_short}}*. Clique no quadro **{{ site.data.keyword.mobilefoundation_short}}**.

    ![MFPCatalog](images/mfp_catalog.png)

* Forneça um nome adequado para a instância do servidor {{ site.data.keyword.mobilefoundation_short}} e clique em **Criar**.

    ![BMMFPNew](images/bmmfpnew.png)

* Uma nova instância do servidor MF é criada e solicita credenciais de login.

    ![MFPLogin](images/mfp_login.png)

* As credenciais para efetuar login no servidor MF podem ser localizadas na guia **Credenciais** no menu.

    ![MFPcredentials](images/mfp_credentials.png)

* Forneça essas credenciais e efetue login para inserir o painel do MF.

    ![MFPDashboard](images/mfp_dashboard.png)


## Configuração do armazenamento de objeto de nuvem
{: #cloud-object-storage-setup}

* No Catálogo do {{ site.data.keyword.cloud_notm}}, procure por *Cloud Object Storage*. Clique no quadro **Armazenamento de Objeto**.

    ![Catalog](images/catalog.png)

* Forneça um nome para a instância do COS e clique em **Criar**.

    ![Create COS](images/cos_create.png)

* Em seguida, clique em **Depósitos** nas opções de menu. Forneça um nome adequado (nesta amostra, escolhemos nomear o depósito `sharedgallery`) para seu depósito e clique em **Criar**.

    ![Create Bucket](images/bucketcreate.png)

* Após a criação do depósito, a página inicial do depósito é semelhante à imagem a seguir, fornecendo opções para fazer upload de dados diretamente.

    ![Bucket Landing Page](images/bucketlanding.png)

* Aqui, inclua os três arquivos que são fornecidos na pasta **Histórias curtas** na [amostra](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App).


## MFP-App Ionic App e Adaptador Java
{: #mfp-cos-ionic-app-and-java-adapter}

Faça download desse [repositório git](https://github.com/MobileFirst-Platform-Developer-Center/COS_MF_Short_Stories_Ionic_App) ou clone-o. Esse repositório consiste em dois componentes principais:

1. Um Adaptador Java MF
2. Um aplicativo móvel iônico

### Configurando mfpdev-cli
{: #configuring-mfpdev-cli}

Inclua os detalhes do servidor na cli. No prompt de comandos, execute `mfpdev server add`.

```
? Enter the name of the new server profile:
```

Forneça um nome para o servidor e pressione Enter. Nesta amostra, o nome fornecido é `mfpserver`.

```
? Enter the fully qualified URL of this server:
```

Insira a URL do servidor. Para o servidor MF no {{ site.data.keyword.cloud_notm}}, a guia de credenciais de serviço contém a url. A porta para o servidor MF https no {{ site.data.keyword.cloud_notm}} é 443 e para a instância do servidor MF http é 80.

```
? Enter the MobileFirst Server administrator login ID: (admin)
```

Insira `admin` e pressione **Enter**.

```
? Insira a senha do administrador do MobileFirst Server:
```

Insira a senha disponível nas credenciais de serviço.

```
? Save the administrator password for this server?: (Y/n)
```

Com base em sua preferência, insira *Y/N* e insira os detalhes conforme solicitado pelos prompts.

```
? Enter the MobileFirst Server connection timeout in seconds: 30
```

Configure 30 segundos como o tempo limite padrão

```
? Tornar este servidor o padrão?: (Y/n)
```

Insira *Y* e pressione **Enter**.

***Saída Esperada***:

```
Verifying server configuration...
The following runtimes are currently installed on this server: mfp
Server profile 'mfpserver' added successfully.
```

### Configurando o adaptador MF Java
{: #configuring-the-mf-java-adapter}

Para se conectar a sua instância do COS, alguns detalhes de sua instância do COS precisam ser fornecidos no arquivo `adapter.xml`. Fornece valores para os campos a seguir:

1. **endpointURL**: esse campo é a URL do terminal público para seu objeto COS. Essa URL pode ser localizada no seu painel do COS, em **Depósitos (nas opções de menu) -> <nome-do-depósito> (`sharedgallery` nesta amostra) -> Configuração -> Terminais -> Público**
2. **AuthToken**: neste tutorial, estamos usando a autenticação do IAM.

Para que o adaptador java se conecte à sua instância do COS, a autenticação que usa o IAM ou HMAC é necessária. A seguir estão as etapas para obter o token do IAM. Para obter detalhes adicionais sobre os processos de autenticação do IAM e HMAC, clique [aqui](https://cloud.ibm.com/docs/services/cloud-object-storage/api-reference/api-reference-buckets.html#bucket-operations#AuthenticationOptions).

#### Obtendo o token Oauth do IAM usando a CLI do {{ site.data.keyword.cloud_notm}}
{: #obtaining-iam-oath-token-using-ibm-cloud-cli}

1. Primeiro, certifique-se de que tenha uma chave de API. Obtenha a chave de API do [{{ site.data.keyword.cloud_notm}} Identity and Access Management](https://cloud.ibm.com/iam/#/users).
2. Efetue login na {{ site.data.keyword.cloud_notm}} Plataforma usando a CLI.

  ```bash
	ibmcloud login --apikey <value>
  ```
  {: codeblock}
  Sua saída é semelhante ao fragmento a seguir,

	```text
	Authenticating...
	OK

	Targeted account <account-name> (<account-id>)

	Padrão do grupo de recursos direcionados

	Terminal da API:     https://api.us-south.cf.cloud.ibm.com (versão da API: 	2.128.0)
	Region:           us-south
	User:             <email-address>
	Account:          <account-name> (<account-id>)
	Resource group:   default
	```

3. Para obter todas as instâncias de serviço em sua conta do {{ site.data.keyword.cloud_notm}}, execute o comando a seguir na CLI.

	```bash
  ibmcloud resource service-instances
  ```
  {: codeblock}

	Expected output:

	```text
 	Retrieving service instances in resource group Default and all 	locations under account <account-name> as <email-address>...
	OK
	Name                                               Location     	State    Type               Tags
	<resource-instance-name>                           global       	active   service_instance
	```

4. Execute o comando a seguir para obter os detalhes de sua instância do COS. <instance-name> é o nome de seu serviço COS (`newObject` para este tutorial).

  ```bash
	ibmcloud resource service-instance <instance-name>
  ```
  {: codeblock}

	Expected output:

	```text
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

5. Para obter o token do IAM, execute o comando a seguir:
  ```bash
	ibmcloud iam oauth-tokens
  ```

	Saída esperada:

	```text
	IAM token:  Bearer <token>
	UAA token:  Bearer <refresh-token>
	```

Depois de incluir os valores *endpointURL* e *authToken*, construa o adaptador. Navegue até a pasta raiz do adaptador no prompt de comandos e execute

```bash
mfpdev adapter build
```
{: codeblock}

O arquivo `*.adapter` é criado na pasta `target`. Execute o comando a seguir,

```bash
mfpdev adapter deploy
```
{: codeblock}

O adaptador é implementado na instância do MF.

Como alternativa, o adaptador pode ser implementado no painel do servidor MF. Abra o painel do servidor MF, no menu, clique em **Adaptadores -> Novo** para abrir a página conforme mostrado na imagem a seguir.
![MFPNewAdapterRegister](images/mfp_new_adapter_register.png)

Em seguida, clique em **Implementar o adaptador** e faça upload do arquivo `.adapter` na pasta **target**.


### Configurando o aplicativo Ionic
{: #configuring-the-ionic-app}

No app, execute as etapas a seguir,

1. Navegue para a pasta que contém o aplicativo Ionic.

2. Adicione o plug-in MF cordova

	```bash
  ionic cordova plugin add cordova-plugin-mfp
  ```

3. Inclua a plataforma android ou iOS

  ```bash
	ionic cordova platform add android
  ```
	Ou

	```bash
	ionic cordova platform add ios
	```

4. Registre seu app no servidor MF, executando

  ```bash
	Registro do aplicativo mfpdev
	```

	Como alternativa, o app pode ser registrado no painel do servidor MF. Abra o painel do servidor MF e, no menu, clique em **Aplicativos -> Novo**.

	A página é carregada, conforme o que é mostrado na imagem a seguir.

	![MFPNewAppRegister](images/mfp_new_app_register.png)

	Insira os detalhes solicitados. Dê um nome para seu aplicativo na caixa de texto 'Nome do aplicativo'. Escolha a plataforma necessária.

	Para Android, a caixa de texto **Pacote** aceita o *Identificador do aplicativo*. Esse parâmetro pode ser localizado no 'AndroidManifest.xml' como pacote de seu aplicativo Android. O campo da caixa de texto **Versão** precisa ser preenchido com o valor *versionName* do `AndroidManifest.xml`.

	Para iOS, a caixa de texto **ID de pacote configurável** aceita *Identificador do aplicativo* (distinção entre maiúsculas e minúsculas). Esse parâmetro pode ser localizado no `mfpclient.plist` de seu aplicativo iOS. O campo da caixa de texto **Versão** precisa ser preenchido com o valor *version* do arquivo `mfpclient.plist` em seu aplicativo iOS.

5. Execute `ionic cordova prepare` para que as mudanças sejam filtradas para os ambientes incluídos.
6. Executar
  ```bash
	ionic cordova build android
  ```
	Ou
  ```bash
	ionic cordova build ios
  ```
	para assegurar que as mudanças de typescript sejam incluídas nos ambientes.

7. Conecte o dispositivo ou execute um emulador ou simulador e execute o comando a seguir.

	```bash
  ionic cordova run android
	```
	Ou
	```bash
	ionic cordova run ios
  ```

### Navegando no aplicativo Ionic
{: #navigating-the-ionic-application}

O aplicativo Ionic exibe uma lista de histórias curtas que foram transferidas por upload anteriormente (como a última etapa na seção [Configuração do Cloud Object Storage](#cloud-object-storage-setup)) de sua instância do COS criada. Ao selecionar uma opção de história específica, a história selecionada é carregada e exibida. Uma opção para incluir uma história também é fornecida, que é, então, transferida por upload para a instância do COS.

A lista de objetos COS iniciais é semelhante à imagem a seguir.

![COS before](images/cos_before.png)

A página inicial do aplicativo fornece uma opção para *Obter todas as histórias* ou *Incluir história*

![App home screen](images/app-home-screen.png)

Ao clicar em **Obter todas as histórias**, as histórias disponíveis no COS são exibidas.

![App select story](images/app-select-story.png)

Selecione uma história a ser carregada no menu suspenso e clique em **Carregar**

![App story loaded](images/app-story-loaded.png)

Em seguida, clique no botão **Incluir história** para incluir sua própria história. Insira um título para a história e o conteúdo da história na área de texto e clique em **Incluir**.

![App add input](images/app-add-input.png)

Depois que a história for incluída, uma mensagem de adição bem-sucedida da história será exibida.

![App story added](images/app-story-added.png)

O painel do COS tem agora a história incluída por meio do aplicativo também.

![COS added](images/cos_added.png)


O token oauth do IAM que é obtido usando `ibmcloud iam oauth-tokens` tem um prazo de expiração e o adaptador falha com uma exceção de `403 - Forbidden`. Deve-se, portanto, assegurar que o token seja válido no adaptador implementado para que o app funcione conforme esperado.
{: note}
