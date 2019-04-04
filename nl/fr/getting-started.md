---

copyright:
  years: 2016, 2019
lastupdated:  "2019-03-14"

keywords: getting started, mobile foundation, plans, configure mobile foundation server, sample app, setup

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

# Tutoriel d'initiation
{: #getting-started-tutorial}

{{site.data.keyword.mobilefoundation_long}} accélère la configuration d'un environnement {{site.data.keyword.mfp_full}} qui vous permet de développer, tester et exécuter des applications mobiles d'entreprise. {{site.data.keyword.mobilefoundation_short}} offre les différents plans de service suivants : 
* **Lite** : met à disposition une instance hébergée du serveur Foundation qui est limitée par la mémoire et l'UC. Autorise un nombre illimité d'applications, le nombre total d'appareils connectés sur toutes les applications étant limité à 10. Gratuit et à utiliser à des fins d'essai uniquement. 
* **Développeur** : met à disposition une instance du serveur Foundation dans le compte de l'utilisateur. Autorise un nombre illimité d'applications, le nombre total d'appareils connectés sur toutes les applications étant limité à 10. Gratuit et à utiliser à des fins de développement et de test uniquement. 
* **Professionnel par appareil** : met à disposition une instance du serveur Foundation dans le compte de l'utilisateur et facturé en fonction du nombre d'appareils activement connectés. 
* **Professionnel, 1 application** : met à disposition une instance du serveur Foundation dans le compte de l'utilisateur et autorise la connexion active d'un nombre illimité d'utilisateurs et d'appareils pour une seule application uniquement.     
{: shortdesc}

Vous pouvez voir tous les plans disponibles [ici](https://cloud.ibm.com/catalog/services/mobile-foundation).
{: note}

Créez une instance de service {{site.data.keyword.mobilefoundation_short}} qui utilise l'un des plans pris en charge en suivant ce tutoriel d'initiation. Vous pouvez ensuite enregistrer une application. Téléchargez et éditez l'application enregistrée, déployez un adaptateur et enfin testez l'application.

## Avant de commencer
{: #prereqs-gs}

Vous avez besoin d'un compte {{site.data.keyword.Bluemix}} et d'une instance du service {{site.data.keyword.mobilefoundation_short}}.

## Etape 1 : créez une instance du service {{site.data.keyword.mobilefoundation_short}}
{: #step1create}

1. Dans le **catalogue** {{site.data.keyword.Bluemix_notm}}, sélectionnez [**{{site.data.keyword.mobilefoundation_short}}**](https://cloud.ibm.com/catalog/services/mobile-foundation). L'écran de configuration du service s'ouvre.
2. Donnez un nom à votre instance de service ou utilisez le nom prédéfini.
3. Choisissez la région, l'organisation et l'espace où vous voulez créer l'instance de service.
4. Sélectionnez votre **Plan de tarification** et cliquez sur **Créer**.

## Etape 2 : construisez votre canal d'accès mobile
{: #buildmobilechannel}


### Pour le plan {{site.data.keyword.mobilefoundation_short}} : Lite
{: #buildchannelliteplan}
Après avoir créé une instance de {{site.data.keyword.mobilefoundation_short}} : Lite, vous pouvez commencer à construire votre canal d'accès mobile en effectuant les étapes suivantes.

* Vous pouvez instantanément accéder à l'instance hébergée du serveur Mobile Foundation et l'utiliser. 

  Cette sélection crée une instance hébergée d'{{site.data.keyword.mfserver_long_notm}} avec les paramètres suivants :
  *	1 Go de mémoire, ce qui est suffisant pour essayer les fonctionnalités d'{{site.data.keyword.mfserver_long_notm}}.   

  * Pour accéder au serveur Mobile Foundation à l'aide de l'interface de ligne de commande, vous avez besoin de vos données d'identification, qui sont disponibles lorsque vous cliquez sur **Données d'identification pour le service** dans le panneau de navigation gauche de la console IBM Cloud.


### Pour le plan {{site.data.keyword.mobilefoundation_short}} : Développeur
{: #buildchanneldevplan}

Après avoir créé une instance de {{site.data.keyword.mobilefoundation_short}} : Développeur, vous pouvez commencer à construire votre canal d'accès mobile en effectuant les étapes suivantes.

* Vous pouvez instantanément accéder et utiliser le serveur Mobile Foundation.

  Cette sélection crée une instance {{site.data.keyword.mfserver_long_notm}} avec les réglages suivants :
  *	1 Go de mémoire. Cette taille suffit aux activités de développement et aux activités de test peu intensives, ainsi qu'aux charges
de travail de production à petite échelle. 

  * Pour accéder au serveur Mobile Foundation à l'aide de l'interface de ligne de commande, vous avez besoin de vos données d'identification, qui sont disponibles lorsque vous cliquez sur **Données d'identification pour le service** dans le panneau de navigation gauche de la console IBM Cloud.

### Pour le plan {{site.data.keyword.mobilefoundation_short}} : Professionnel par appareil
{: #buildchannelprofdeviceplan}

Après avoir créé une instance du service {{site.data.keyword.mobilefoundation_short}} : Professionnel par appareil, vous pouvez commencer à construire votre canal d'accès mobile en procédant comme suit.

  1.  Connectez-vous à une instance de service {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} sur {{site.data.keyword.Bluemix_notm}}.

      1.  Sélectionnez l'`Organisation` {{site.data.keyword.Bluemix_notm}} dans laquelle se trouve l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}}.

      + Depuis la liste des espaces disponibles dans l'`Organisation` sélectionnée, sélectionnez l'`espace` {{site.data.keyword.Bluemix_notm}} dans lequel se trouve l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} .

      + Sélectionnez les `Nom de service` et `Données d'identification` {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} pour vous connecter à l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}}.

      + Testez la connexion à l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} sélectionnée en cliquant sur **Tester la connexion**.

      + Cliquez sur **Ajouter**, puis sur **Continuer** dans la fenêtre vous demandant confirmation concernant le service {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} sélectionné. Cette action permet de créer les tables requises dans l'instance de service de base de données {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} configurée.

      Une fois que vous avez ajouté une connexion {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} à l'instance {{site.data.keyword.mobilefoundation_short}}, vous ne pouvez plus la modifier.
      {: note}
  2.  Créez et démarrez le serveur.

      1. Créez une instance de serveur {{site.data.keyword.mobilefirst_notm}} avec la configuration par défaut en cliquant sur **Démarrer le serveur de base**.

      + Cette sélection met à disposition une instance {{site.data.keyword.mfserver_long_notm}} avec les réglages suivants :
          - Deux noeuds avec 1 Go de mémoire chacun. Cette taille convient aux activités de développement et aux activités de test modérées, ainsi qu'aux charges
de travail de production à petite échelle.

          -	Le `nom d'utilisateur` et le `mot de passe` sont générés automatiquement pour vous. Vous pouvez y accéder une fois que le serveur est en opération.

          Le processus de création de votre serveur commence. Ce processus prend environ 10 minutes et une fenêtre de message indique la progression de l'opération. A son terme, un tableau de bord s'affiche et présente les éléments suivants :
            -	L'état du serveur que vous exécutez (état, taille).
            -	La route de serveur créée pour vous. Utilisez cette route dans votre application mobile pour vous connecter à {{site.data.keyword.mfserver_short_notm}}.
            -	Vos `nom d'utilisateur` et `mot de passe` personnels pour accéder à la console {{site.data.keyword.mfp_oc_short_notm}}. Le `mot de passe` est masqué. Cliquez sur l'icône **Afficher le mot de passe** pour le visualiser.

      +	Cliquez sur **Lancer la console** pour ouvrir la console {{site.data.keyword.mfp_oc_short_notm}}.      

      Pour créer une instance de serveur {{site.data.keyword.mobilefirst_notm}} avec la configuration avancée de la topologie, de la sécurité et d'autres paramètres de configuration du serveur, cliquez sur **Démarrer le serveur avec la configuration avancée**. Pour plus d'informations, voir [Configuration avancée](/docs/services/mobilefoundation?topic=mobilefoundation-c_using_mfs_p5#using_mfs_advanced_p5).
      {: tip}

### Pour le plan {{site.data.keyword.mobilefoundation_short}} : Professionnel, 1 application
{: #buildchannelprof1appplan}

Après avoir créé une instance du service {{site.data.keyword.mobilefoundation_short}} : Professionnel, 1 application, vous pouvez commencer à générer votre canal d'accès mobile en procédant comme suit :

  1.  Connectez-vous à une instance de service {{site.data.keyword.Db2_on_Cloud_long}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL_full}} sur {{site.data.keyword.Bluemix_notm}}.

      1.  Sélectionnez l'`Organisation` {{site.data.keyword.Bluemix_notm}} dans laquelle se trouve l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}}.

      + Depuis la liste des espaces disponibles dans l'`Organisation` sélectionnée, sélectionnez l'`espace` {{site.data.keyword.Bluemix_notm}} dans lequel se trouve l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} .

      + Sélectionnez les `Nom de service` et `Données d'identification` {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} pour vous connecter à l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}}.

      + Testez la connexion à l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} sélectionnée en cliquant sur **Tester la connexion**.

      + Cliquez sur **Ajouter**, puis sur **Continuer** dans la fenêtre vous demandant confirmation concernant le service {{site.data.keyword.Db2_on_Cloud_short}} ou {{site.data.keyword.composeForPostgreSQL}} sélectionné. Cette action permet de créer les tables requises dans l'instance de service de base de données {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} configurée.

      Une fois que vous avez ajouté une connexion {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} à l'instance {{site.data.keyword.mobilefoundation_short}}, vous ne pouvez plus la modifier.
      {: note}

  2.  Créez et démarrez le serveur.

      1. Créez une instance de serveur {{site.data.keyword.mobilefirst_notm}} avec la configuration par défaut en cliquant sur **Démarrer le serveur de base**.

        `L'instance de serveur de base comprend un unique noeud et 1 Go de mémoire.`

      + Le `nom d'utilisateur` et le `mot de passe` sont générés automatiquement pour vous. Vous pouvez y accéder une fois que le serveur est en opération.  

        Le processus de création de votre serveur commence. Ce processus prend environ 10 minutes et une fenêtre de message indique la progression de l'opération. A son terme, un tableau de bord s'affiche et présente les éléments suivants :
          -	L'état du serveur que vous exécutez (état, taille).
          -	La route de serveur créée pour vous. Utilisez cette route dans votre application mobile pour vous connecter à {{site.data.keyword.mfserver_short_notm}}.
          -	Vos `nom d'utilisateur` et `mot de passe` personnels pour accéder à la console {{site.data.keyword.mfp_oc_short_notm}}. Le `mot de passe` est masqué. Cliquez sur l'icône **Afficher le mot de passe** pour le visualiser.

      +  Cliquez sur **Lancer la console** pour ouvrir la console {{site.data.keyword.mfp_oc_short_notm}}.  

      Pour créer une instance de serveur {{site.data.keyword.mobilefirst_notm}} avec la configuration avancée de la topologie, de la sécurité et d'autres paramètres de configuration du serveur, cliquez sur **Démarrer le serveur avec la configuration avancée**. Pour plus d'informations, voir [Configuration avancée](/docs/services/mobilefoundation?topic=mobilefoundation-c_using_mfs_p2#using_mfs_advanced_p2).
      {: tip}

Accédez à [Using the Mobile Foundation service to set up MobileFirst Server![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/bluemix/using-mobile-foundation/){: new_window} pour vous familiariser avec {{site.data.keyword.mobilefoundation_short}}.
{: note}

## Etape 3 : enregistrez votre application dans {{site.data.keyword.mobilefoundation_short}}
{: #registerapp}

Une fois votre instance de serveur Mobile Foundation créée et démarrée, vous pouvez effectuer les étapes ci-après pour enregistrer une application Android.

  1.  Appelez {{site.data.keyword.mfp_oc_short_notm}} en chargeant l'URL `http://<your-server-host>:<server-port>/mfpconsole`. Utilisez le `nom d'utilisateur` et le `mot de passe` qui ont été générés au moment de la mise à disposition.

  + Dans le **Tableau de bord** de {{site.data.keyword.mfp_oc_short_notm}}, cliquez sur **Nouveau** en regard de **Applications**.

  + Indiquez *MFPStarterAndroid* comme **Nom de l'application**.

  + Dans **Choisir une plateforme**, sélectionnez *Android*.

  + Indiquez *com.ibm.mfpstarterandroid* comme **Identificateur d'application**.

  + Entrez *1.0* comme **Version**.

  + Cliquez sur **Enregistrer l'application**.

## Etape 4 : téléchargez l'exemple d'application
{: #downloadapp}

  1.  Dans le **Tableau de bord** de {{site.data.keyword.mfp_oc_short_notm}}, sélectionnez **MFPStarterAndroid** sous **Applications**.

  + Cliquez sur **Obtenir le code de démarrage** et choisissez de télécharger l'exemple d'application Android.

## Etape 5 : éditez l'exemple d'application
{: #editapp}

  1. Importez l'exemple d'application Android téléchargé à l'étape précédente dans un projet Android Studio.

  + Dans la barre de menus latérale **Project** d'Android Studio, sélectionnez le fichier **app → java → com.ibm.mfpstarterandroid → ServerConnectActivity.java**.

    * Ajoutez les importations suivantes
      ```java
      import java.net.URI;
      import java.net.URISyntaxException;
      import android.util.Log;
      ```
      {: codeblock}

    * Copiez le fragment de code suivant et collez-le à la place de l'appel existant `WLAuthorizationManager.getInstance().obtainAccessToken`

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

## Etape 6 : déployez un adaptateur
{: #deployadapter}

  1. Téléchargez cet [artefact adaptateur](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/javaAdapter.adapter){: download} et déployez-le à partir de {{site.data.keyword.mfp_oc_short_notm}} en utilisant la sélection **Actions → Déployer un adaptateur**.

## Etape 7 : testez l'application
{: #testapp}

  1. Dans la barre de menus latérale **Project** d'Android Studio, sélectionnez le fichier **app → src → main →assets → mfpclient.properties** et éditez les propriétés `protocol`, `host` et `port` avec des valeurs correctes correspondant à votre instance de serveur MobileFirst.

   Ces valeurs sont normalement https (pour le protocole), *adresse-de-votre-serveur* et 443 (pour le numéro de port).
   {: tip}

  2. Dans Android Studio, cliquez sur **Run App** (ou option équivalente de la version française).
     * L'application doit démarrer dans un émulateur d'appareil mobile.
     * Cliquez sur **Ping MobileFirst Server** dans votre application. Vous devez voir s'afficher le message `Connected to MobileFirst Server`.
     * Si l'application est parvenue à se connecter au serveur MobileFirst, l'adapteur Java qui est déployé effectue un appel de demande de ressource. 
     * La réponse de l'adaptateur sera ensuite imprimée dans la vue LogCat d'Android Studio.


## Etapes suivantes
{: #nextsteps-gs}

Vous pouvez suivre les [tutoriels de démarrage rapide ![Icône de lien externe](../../icons/launch-glyph.svg "Tutoriels de démarrage rapide")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/){: new_window} pour découvrir et manipuler d'autres exemples d'application et explorer le fonctionnement de {{site.data.keyword.mobilefoundation_short}}.

La page Démarrage rapide donne accès à des tutoriels expliquant le principe de fonctionnement de {{site.data.keyword.mobilefoundation_short}} pour les applications iOS, Android, Web, Cordova, Windows, React Native, Ionic et Xamarin.
