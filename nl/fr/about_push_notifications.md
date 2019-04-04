---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-26"

keywords: Push Notifications, notifications, unicast notifications, tag notifications, interactive notifications, silent notifications, configure DataPower

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

# Notifications push
{: #push_notifications}

Les notifications désignent la capacité d'un appareil mobile à recevoir des messages "envoyés par commande push" à partir d'un serveur. Les notifications sont reçues indépendamment du fait que l'application s'exécute en avant-plan ou en arrière-plan.   
{: shortdesc}

{{ site.data.keyword.IBM_notm }} {{ site.data.keyword.mobilefoundation_short }} fournit un ensemble unifié de méthodes d'API pour envoyer des notifications push à des applications iOS, Android, Windows 8.1 Universal, Windows 10 UWP et Cordova (iOS, Android). Les notifications sont envoyées de {{ site.data.keyword.mfserver_short_notm }} vers l'infrastructure du fournisseur (Apple, Google, Microsoft, passerelles SMS Gateway), puis vers les appareils appropriés. Le mécanisme de notification unifiée rend tout le processus de communication avec les utilisateurs et les appareils transparent pour le développeur. 

## Prise en charge des appareils
{: #device-support }
Les notifications push sont prises en charge pour les plateformes suivantes dans {{ site.data.keyword.mobilefoundation_short }} : 

* iOS 8.x ou version ultérieure
* Android 4.x ou version ultérieure
* Windows 8.1, Windows 10

## Notifications push
{: #push-notifications-forms }
Les notifications peuvent prendre plusieurs formes : 

* **Alerte** (iOS, Android, Windows) - message de texte contextuel
* **Son** (iOS, Android, Windows) - fichier son lu lorsqu'une notification est reçue
* **Badge** (iOS), Vignette (Windows) - représentation graphique affichant un texte court ou une image
* **Bannière** (iOS), Toast (Windows) - message de texte contextuel qui disparaît en haut de l'affichage de l'appareil
* **Interactive** (iOS 8 et versions ultérieures) - boutons d'action à l'intérieur de la bannière d'une notification reçue
* **Silencieuse** (iOS 8 et versions ultérieures) - envoi de notifications sans déranger l'utilisateur

### Types de notification push
{: #push-notification-types }

* **Notifications d'étiquette**
{: #tag-notifications }

    Les notifications d'étiquette sont des messages de notification qui ciblent tous les appareils abonnés à une étiquette spécifique.   

    Elles permettent la segmentation des notifications en fonction des domaines ou des sujets. Les destinataires des notifications peuvent
choisir de ne recevoir les notifications que si elles concernent un sujet ou une rubrique qui les intéresse. Par conséquent, les notifications basées
sur des étiquettes constituent un moyen de segmenter les destinataires. Cette fonction vous permet de définir des étiquettes et d'envoyer ou de recevoir
des messages par étiquettes. Un message cible uniquement les appareils qui sont abonnés à une étiquette spécifique. 

* **Notifications de diffusion**
{: #broadcast-notifications }

    Les notifications de diffusion constituent une forme de notifications push d'étiquette qui ciblent tous les appareils abonnés et sont activées par défaut pour toutes les applications {{ site.data.keyword.mobilefirst_notm }} activées pour la notification push par le biais d'un abonnement à une étiquette `Push.all` réservée (créée automatiquement pour chaque appareil). Les notifications de diffusion peuvent être désactivées en se désabonnant de l'étiquette `Push.all` réservée. 

* **Notifications unicast**
{:# unicast-notifications }

    Les notifications unicast, ou notifications authentifiées par l'utilisateur, sont sécurisées à l'aide d'OAuth. Ces messages de notification ciblent un appareil spécifique ou un ou plusieurs ID utilisateur. L'ID utilisateur indiqué dans l'abonnement utilisateur peut provenir du contexte de sécurité sous-jacent. 

* **Notifications interactives**
{: #interactive-notifications-overview }

    Avec une notification interactive, lorsqu'une notification arrive, les utilisateurs peuvent agir sans ouvrir l'application. Lorsqu'une notification interactive arrive, l'appareil affiche des boutons d'action avec le message de notification. Actuellement, les notifications interactives sont prises en charge sur les appareils dotés d'iOS version 8 au minimum. Si une notification interactive est envoyée sur un appareil iOS doté d'une version antérieure à la version 8, les actions de notification ne s'affichent pas. 

    > Apprenez à gérer les [notifications interactives](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications). 

* **Notifications silencieuses**
{: #silent-notifications-overview }

    Les notifications silencieuses sont des notifications qui n'affichent pas d'alertes ou ne dérangent pas l'utilisateur. Lorsqu'une notification silencieuse arrive, le code de gestion d'application s'exécute en arrière-plan sans mettre l'application à l'avant-plan. Actuellement, les notifications silencieuses sont prises en charge sur les appareils iOS dotés de la version 7 au minimum. Si la notification silencieuse est envoyée à des appareils iOS dotés d'une version antérieure à la version 7, la notification est ignorée si l'application s'exécute en arrière-plan. Si l'application s'exécute en avant-plan, la méthode de rappel de notification est appelée. 

    > Apprenez à gérer les [notifications silencieuses](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications). 

    Les notifications unicast ne contiennent pas d'étiquette. Le message de notification peut cibler plusieurs appareils ou utilisateurs en spécifiant plusieurs ID appareil ou ID utilisateur respectivement, dans le bloc cible de l'API du message POST.
    {: note}

## Paramètres de proxy
{: #proxy-settings }

Utilisez les paramètres de proxy pour définir le proxy facultatif via lequel les notifications sont envoyées à APNS et FCM. Vous pouvez définir le proxy à l'aide des propriétés de configuration **push.apns.proxy.*** et **push.gcm.proxy.***. Pour plus d'informations, voir [Liste des propriétés JNDI pour le service push de {{ site.data.keyword.mfserver_short_notm }}](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/installation-configuration/production/server-configuration/#list-of-jndi-properties-for-mobilefirst-server-push-service).

WNS ne prend pas en charge les proxys.
{: note}

### Utilisation de WebSphere DataPower comme noeud final de notification push
{: #proxy-settings-datapower-endpoint }

Vous pouvez configurer DataPower pour qu'il accepte les demandes de notification provenant de {{ site.data.keyword.mfserver_short_notm }} et les réachemine vers FCM et WNS. 

APNS n'est pas pris en charge.
{: note}

#### Configuration de {{ site.data.keyword.mfserver_short_notm }}
{: #proxy-settings-datapower-1 }

Configurez la propriété JNDI suivante dans `server.xml`. 
```xml
<jndiEntry jndiName="imfpush/mfp.push.dp.endpoint" value = '"https://host"' />
<jndiEntry jndiName="imfpush/mfp.push.dp.gcm.port" value = '"port"' />
<jndiEntry jndiName="imfpush/mfp.push.dp.wns.port" value = '"port"' />
```
{: codeblock}

où `host` est le nom d'hôte de DataPower et `port` le numéro de port sur lequel le gestionnaire frontal HTTPS Front Side Handler est configuré pour FCM et WNS. 

#### Configuration de DataPower
{: #proxy-settings-datapower-2 }

1. Connectez-vous au dispositif DataPower. 
2. Accédez à **Services** > **Multi-Protocol Gateway** > **New Multi-Protocol Gateway**. 
3. Indiquez un nom qui vous permet d'identifier la configuration. 
4. Sélectionnez XML Manager, Multi-Protocol Gateway Policy comme valeur par défaut et définissez le paramètre URL Rewrite Policy sur none. 
5. Sélectionnez le bouton d'option **static-backend**, puis l'une des options suivantes pour **set Default Backend URL** : 
    - Pour FCM,	`https://gcm-http.googleapis.com`
    - Pour WNS,	`https://hk2.notify.windows.com`
6. Sélectionnez le passe-système comme type de réponse et type de demande. 

#### Génération d'un certificat
{: #proxy-settings-datapower-3 }

Pour générer un certificat, choisissez l'une des options suivantes : 

- Pour FCM,
	1. A partir de la ligne de commande, exécutez `Openssl` pour obtenir les certificats FCM. 
	2. Exécutez la commande suivante :
		```
		openssl s_client -connect gcm-http.googleapis.com:443
		```
    {: codeblock}

	3. Copiez le contenu à partir de *-----BEGIN CERTIFICATE-----  to -----END CERTIFICATE-----* et sauvegardez-le dans un fichier portant l'extension `.pem`. 

- Pour WNS,
	1. A partir de la ligne de commande, utilisez `Openssl` pour obtenir les certificats WNS. 
	2. Exécutez la commande suivante :
		```
		openssl s_client -connect https://hk2.notify.windows.com:443
		```
    {: codeblock}
	3. Copiez le contenu à partir de *-----BEGIN CERTIFICATE-----  to -----END CERTIFICATE-----* et sauvegardez-le dans un fichier portant l'extension `.pem`. 

#### Paramètres d'arrière-plan
{: #proxy-settings-datapower-4 }

- Pour FCM et WNS,
    1. Créez un certificat de chiffrement. 

        a. Accédez à **Objects** > **Crypto Configuration** et cliquez sur **Crypto certificate**. 

        b. Indiquez un nom qui vous permet d'identifier le certificat de chiffrement. 

        c. Cliquez sur **Upload** pour télécharger le certificat FCM généré. 

        d. Définissez l'option **Password Alias** sur none. 

        e. Cliquez sur **Generate key**. 

        ![Configuration du certificat de chiffrement](images/bck_1.gif)

    2. Créez des données d'identification de validation de chiffrement. 

        a. Accédez à **Objects** > **Crypto Configuration** et cliquez sur **Crypto Validation Credential**. 

        b. Indiquez un nom unique. 

        c. Pour Certificates, sélectionnez le certificat de chiffrement que vous avez créé à l'étape précédente - Etape 1. 

        d. Pour **Certificate Validation Mode**, sélectionnez Match exact certificate or immediate issuer. 

        e. Cliquez sur **Apply**. 

        ![Configuration des données d'identification de validation de chiffrement](images/bck_2.gif)

    3. Créez des données d'identification de validation de chiffrement : 

        a. Accédez à **Objects** > **Crypto Configuration** et cliquez sur **Crypto Profile**. 

        b. Cliquez sur **Add**.

        c. Indiquez un nom unique. 

        d. Pour **Validation Credentials**, sélectionnez les données d'identification de validation créées à l'étape précédente - Etape 2 dans le menu déroulant, puis définissez l'option Identification Credentials sur **none**. 

        e. Cliquez sur **Apply**. 

        ![Configuration du profil de chiffrement](images/bck_3.gif)

    4. Créez un profil de proxy SSL : 

        a. Accédez à **Objects** > **Crypto Configuration** > **SSL Proxy Profile**. 

        b. Choisissez l'une des options suivantes : 

            - Pour SMS, définissez l'option **SSL Proxy Profile** sur none. 

            - Pour FCM et WNS avec une URL de back end sécurisée (HTTPS), procédez comme suit : 

                i. Cliquez sur **Add**. 

                ii. Indiquez un nom qui vous permet d'identifier le profil de proxy SSL ultérieurement. 

                iii. Définissez l'option **SSL Direction** sur **Forward** dans le menu déroulant. 

                iv. Pour Forward (Client) Crypto Profile, sélectionnez le profil de chiffrement créé à l'étape 3. 

                v. Cliquez sur **Apply**. 

        ![Configuration du profil de proxy SSL](images/bck_4.gif)

    5. Dans la fenêtre Multi-Protocol Gateway, sous **Back side settings**, sélectionnez **Proxy Profile** pour l'option **SSL client type** et sélectionnez le profil de proxy SSL créé à l'étape 4. 

        ![Configuration du profil de proxy SSL](images/bck_5.gif)

#### Paramètres frontaux
{: #proxy-settings-datapower-5 }

- Pour FCM et WNS : 

    1. Créez une paire clé-certificat en utilisant le nom d'hôte de DataPower comme valeur de nom usuel : 

        a. Accédez à **Administration** > **Miscellaneous** et cliquez sur **Crypto Tools**. 

        b. Entrez le nom d'hôte de DataPower comme valeur pour le nom usuel. 

        c. Sélectionnez **Export private key** si vous prévoyez d'exporter la clé privée ultérieurement, puis cliquez sur **Generate Key**. 

        ![Création d'une paire clé-certificat](images/frnt_1.gif)

    2. Créez des données d'identification de chiffrement : 

        a. Accédez à **Objects** > **Crypto Configuration** et cliquez sur **Crypto Identification Credentials**. 

        b. Cliquez sur **Add**.

        c. Indiquez un nom unique. 

        d. Pour la clé de chiffrement et le certificat, sélectionnez la clé et le certificat générés à l'étape précédente - Etape 1 dans la zone de liste. 

        e. Cliquez sur **Apply**. 

        ![Création de données d'identification de chiffrement](images/frnt_2.gif)

    3. Créez un profil de chiffrement : 

        a. Accédez à **Objects** > **Crypto Configuration** et cliquez sur **Crypto Profile**. 

        b. Cliquez sur **Add**.

        c. Indiquez un nom unique. 

        d. Pour les données d'identification, sélectionnez celles qui ont été créées à l'étape précédente - Etape 2 dans la zone de liste. Définissez l'option Validation credentials sur none. 

        e. Cliquez sur **Apply**. 

        ![Configuration du profil de chiffrement](images/frnt_3.gif)

    4. Créez un profil de proxy SSL : 

        a. Accédez à **Objects** > **Crypto Configuration** > **SSL Proxy Profile**. 

        b. Cliquez sur **Add**.

        c. Indiquez un nom unique. 

        d. Sélectionnez **Reverse** pour l'option SSL Direction dans la zone de liste. 

        e. Pour Reverse (Server) Crypto Profile, sélectionnez le profil de chiffrement créé à l'étape précédente - Etape 3.   

        f. Cliquez sur **Apply**. 

        ![Configuration du profil de proxy SSL](images/frnt_4.gif)

    5. Créez un gestionnaire frontal HTTPS : 

        a. Accédez à **Objects** > **Protocol Handlers** > **HTTPS Front Side Handler**. 

        b. Cliquez sur **Add**. 

        c. Indiquez un nom unique. 

        d. Pour **Local IP address**, sélectionnez l'alias correct ou laissez la valeur par défaut (0.0.0.0). 

        e. Indiquez un port disponible. 

        f. Pour **Allowed methods and versions**, sélectionnez HTTP 1.0, HTTP 1.1, POST method, GET method, URL with ?, URL with #, URL with...

        g. Sélectionnez **Proxy Profile** comme type de serveur SSL. 

        h. Pour SSL proxy profile (obsolète), sélectionnez le profil de proxy SSL créé à l'étape précédente - Etape 4. 

        i. Cliquez sur **Apply**. 

        ![Configuration du gestionnaire frontal HTTPS](images/frnt_5.gif)

    6. Sur la page Configure Multi-Protocol Gateway, sous **Front side settings**, sélectionnez le gestionnaire frontal HTTPS créé à l'étape 5 pour la
zone **Front Side Protocol**, puis cliquez sur **Apply**. 

        ![Configuration générale](images/frnt_6.gif)

    Le certificat utilisé par DataPower dans les paramètres frontaux est auto-signé. La connexion à DataPower échoue si le certificat n'est pas ajouté au magasin de clés JRE utilisé par {{ site.data.keyword.mobilefirst_notm }}. 

## Etapes suivantes
{: #next-steps }

Exécutez la configuration requise côté serveur et côté client afin d'envoyer et de recevoir des notifications push : 

* [Configuration de notifications push](/docs/services/mobilefoundation?topic=mobilefoundation-configure_push_notifications#configure_push_notifications)
* [Envoi de notifications push](/docs/services/mobilefoundation?topic=mobilefoundation-send_push_notifications#send_push_notifications)
* [Traitement des notifications push dans les applications client](/docs/services/mobilefoundation?topic=mobilefoundation-handling_push_notifications_in_client_applications#handling_push_notifications_in_client_applications)
* [Notifications silencieuses](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [Notifications interactives](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
