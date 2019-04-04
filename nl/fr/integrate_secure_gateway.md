---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-13"

keywords: integration, mobile foundation, secure gateway

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Etablissement d'une connexion sécurisée à un système sur site à l'aide du service Secure Gateway
{: #integrate_secure_gateway}

Lorsque vous générez des applications mobiles d'entreprise, vous intégrez généralement vos applications à des systèmes d'enregistrement existants. Pour accéder aux données stockées dans votre centre de données sur site à partir d'une application mobile et les optimiser, vous pouvez utiliser le [service Mobile Foundation](https://cloud.ibm.com/catalog/services/mobile-foundation) avec le [service Secure Gateway](https://cloud.ibm.com/catalog/services/secure-gateway) dans [IBM Cloud](https://cloud.ibm.com/). Le service Secure Gateway fournit une connectivité rapide, facile et sécurisée et établit un tunnel entre IBM Cloud et le système distant auquel vous voulez vous connecter.

Ce tutoriel explique comment accéder aux noeuds finaux HTTP dans votre centre de données sur site à partir d'adaptateurs Mobile Foundation qui s'exécutent dans IBM Cloud, à l'aide du service Secure Gateway.

## Prérequis
{: #prereq_int_sec_gw}

Pour suivre ce tutoriel, vous avez besoin d'un noeud final HTTP au sein de votre pare-feu d'entreprise qui expose les systèmes de données d'enregistrement. Vous pouvez aussi créer un noeud final de test dans votre environnement local à l'aide de [cet exemple de projet ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/MobileFirst-Platform-Developer-Center/MFPSecureGatewayIonic/tree/master/NodeJSHTTPProject) `Node.js`.

Accédez au dossier de projet et exécutez votre serveur HTTP.

```bash
npm install
node app.js
```
{: codeblock}

## Scénario d'intégration avec le service Secure Gateway
{: #secure_gateway}

L'image ci-dessous représente l'architecture utilisée dans le scénario d'intégration expliqué dans ce tutoriel.

![Diagramme de l'architecture](images/SecureGatewayArchi.png)

## Implémentation de l'intégration Secure Gateway
{: #implementing_sg_integration}

### Création d'une instance de service Secure Gateway
Connectez-vous à IBM Cloud et créez une instance du [service Secure Gateway](https://cloud.ibm.com/catalog/services/secure-gateway/).

![IBM Cloud](images/SecureGatewayInst.gif)

Une fois l'instance du service Secure Gateway créée, suivez les étapes ci-après pour configurer le service Secure Gateway entre IBM Cloud et votre environnement sur site.

### Ajout d'une passerelle
{: #add_gateway}

Dans le tableau de bord du service Secure Gateway, cliquez sur **Ajouter une passerelle** pour créer une passerelle en indiquant le nom de passerelle de votre choix.

![Ajout d'une passerelle](images/AcmeAddGateway.gif)


### Ajout d'un client Secure Gateway
{: #add_sg_client}

![Ajout d'un client 2](images/AcmeAddClient.gif)

Depuis votre nouvelle passerelle, dans l'onglet **Clients**, cliquez sur **Connecter un client**.

Vous pouvez utiliser tout client de votre choix et exécuter le client Secure Gateway dans votre environnement sur site. Les étapes de configuration du client Secure Gateway sont disponibles dans la console Secure Gateway.

Dans ce tutoriel, nous allons utiliser l'option de conteneur Docker pour exécuter le client Secure Gateway.
Suivez les étapes ci-dessous :
*   Installez Docker sur votre machine sur site s'il n'est pas déjà installé.
*   Lancez un terminal et exécutez le client Secure Gateway dans un conteneur avec la commande affichée dans la console du service.
    ```bash
    docker run –it ibmcom/secure-gateway-client <gatewayId>
    ```
    {: codeblock}
    `gatewayId` est affiché dans la console conformément à l'image ci-dessous.

### Ajout d'une destination
{: #add_destination}

![Ajout d'une destination](images/AcmeAddDest.gif)

Depuis votre nouvelle passerelle, dans l'onglet **Destinations**, cliquez sur **Ajouter une destination**.

Le service Secure Gateway vous permet de vous connecter à un noeud final sur site depuis un environnement IBM Cloud ou de vous connecter depuis un environnement sur site à une ressource de cloud. Pour ce scénario, sélectionnez Sur site comme emplacement de ressource et indiquez le nom d'hôte/l'IP et le port de la ressource sur site. De plus, indiquez un nom de destination de votre choix.

Pour que la demande puisse être acheminée depuis le client Secure Gateway au noeud final de la ressource, vous devez ajouter la ressource à la liste d'accès.
Exécutez la commande suivante dans le terminal du client Secure Gateway (qui s'exécute en tant que conteneur dans le contexte d'exécution Docker dans ce scénario) pour ajouter la ressource à la liste d'accès.

```
acl allow <resourceHost>:<resourcePort>
```
{: codeblock}
`resourceHost` et `resourcePort` sont des détails du noeud final de la ressource sur site.

A présent, la destination est configurée. Le service Secure Gateway va remplir les détails d'hôte et de port du cloud, que vous pouvez utiliser pour accéder à la ressource sur site depuis l'environnement de cloud.

![Onglet Destination](images/AcmeCloudPopulate.gif)

### Configuration du service Secure Gateway avec Mobile Foundation et un adaptateur Mobile Foundation
{: #configuration_sg_mfp}

Dans ce tutoriel, nous allons utiliser une instance du service Mobile Foundation dans IBM Cloud pour configurer le serveur Mobile Foundation. Le service Mobile Foundation dans IBM cloud permet de mettre à disposition le serveur Mobile Foundation dans le contexte d'exécution Liberty sous forme d'application Cloud Foundry. Il vous permet d'exécuter dans IBM Cloud n'importe quel projet Mobile Foundation développé dans un environnement local.

### Configuration du serveur Mobile Foundation dans IBM cloud
{: #mf_server_setup}

Créez une instance du [service Mobile Foundation](https://cloud.ibm.com/catalog/services/mobile-foundation) depuis la console IBM Cloud.

Depuis la console du service Mobile Foundation, créez le [serveur Mobile Foundation ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/bluemix/using-mobile-foundation/).


### Génération et déploiement d'un adaptateur Mobile Foundation
{: #deploying_mf_adapter}

Dans ce tutoriel, nous allons connecter le noeud final de Secure Gateway à l'aide d'un adaptateur Mobile Foundation. [Téléchargez ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/MobileFirst-Platform-Developer-Center/Adapters/tree/release80/JavaHTTP) l'adaptateur Mobile Foundation JavaHTTP.

Générez et déployez l'adaptateur dans la console Mobile Foundation Operations avec les commandes [mfpdev-cli](/docs/services/mobilefoundation?topic=mobilefoundation-mobile_foundation_cli#mobile_foundation_cli).
```bash
mfpdev adapter build 
mfpdev adapter deploy
```
{: codeblock}

Vous trouverez plus d'informations sur la génération et le déploiement d'adaptateurs [ici ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/).
{: tip}

Indiquez les détails d'hôte et de port du cloud pour le noeud final de la ressource dans l'adaptateur JavaHTTP obtenu dans la section précédente.

![Configuration de l'adaptateur](images/AdapterConfiguration.png)

où `cap-sg-prd-5.securegateway.appdomain.cloud` et `18946` sont l'hôte et le port de Secure Gateway respectivement.

A présent, l'adaptateur Mobile Foundation est configuré et le service Mobile Foundation peut fonctionner avec un système sur site dans l'entreprise, à l'aide du service Secure Gateway.

### Création et enregistrement d'un modèle d'application Mobile Foundation
{: #registering_sample_app}

Téléchargez le modèle d'application Mobile Foundation [ici](https://github.com/MobileFirst-Platform-Developer-Center/MFPSecureGatewayIonic/), suivez les instructions du `Readme` et enregistrez l'application dans la console Mobile Foundation Operations.

Exécutez l'application, indiquez les données d'identification pour la connexion, puis cliquez sur le bouton *Login*. Cliquez sur le bouton *Fetch Acme Writers* pour appeler votre noeud final sur site via Secure Gateway à l'aide de l'adaptateur JavaHTTP déployé dans votre console Mobile Foundation Operations. Recevez les données de votre choix depuis l'environnement sur site.

![Application recevant des données sur site](images/AcmePublishersApp.gif)

Vous pouvez vous connecter à plusieurs noeuds finaux sur site en configurant plusieurs destinations dans le service Secure Gateway et en déployant des adaptateurs Mobile Foundation pour la connexion à l'hôte de cloud respectif du noeud final. Vous pouvez également configurer le service Secure Gateway avec une sécurité supplémentaire pour vous assurer que les communications au noeud final ont lieu sur HTTPS et avec une sécurité côté application. Vous trouverez les [détails ici](/docs/services/SecureGateway?topic=securegateway-getting-started-with-sg#getting-started-with-sg).


## Récapitulatif
{: #summary_int_sec_gw}

En suivant ce tutoriel, vous établissez une connexion sécurisée entre les adaptateurs Mobile Foundation qui s'exécutent dans IBM Cloud et un noeud final HTTP sur site à l'aide du service Secure Gateway.
