---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: app versions, disabling apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Gestion des versions d'application
{: #manage_app_versions}

Les fonctionnalités de gestion d'applications de Mobile Foundation offrent aux utilisateurs et aux administrateurs de Mobile Foundation Server un contrôle granulaire sur l'accès des utilisateurs et des appareils à leurs applications.

Le serveur Mobile Foundation suit toutes les tentatives d'accès à votre infrastructure mobile et stocke des informations sur l'application, l'utilisateur et l'appareil sur lequel l'application est installée. Le mappage entre l'application, l'utilisateur et l'appareil constitue la base des capacités de gestion des applications mobiles du serveur.

A l'aide de la console Mobile Foundation Operations, vous pouvez surveiller et gérer l'accès à vos ressources. Vous pouvez également gérer la version de votre application spécifique.

1.  Accédez à la console Mobile Foundation Operations, cliquez sur **Applications**, choisissez l'application que vous souhaitez gérer, sélectionnez la version spécifique de l'application qui vous intéresse, dans la liste **Versions** affichée.
     ![Gestion des versions d'application](images/app_version_management.png)

2. Sous l'onglet **Gestion**, vous voyez les options permettant de définir le statut de l'application pour la version de l'application sélectionnée. Les états pris en charge pour une version d'application sont les suivants :
   * Actif
   * Actif et notification
   * Accès désactivé
3. Une version d'application peut être désactivée en sélectionnant l'option *Accès désactivé* sous **Accès à l'application > Statut**.
4. Vous pouvez également configurer le téléchargement des ressources Web mises à jour pour votre application Cordova dans la section **Mise à jour directe**. Les utilisateurs qui se connectent au serveur Mobile Foundation à l'aide de cette version d'application spécifique se voient ensuite proposer l'option de mettre à jour leur application à l'aide de la mise à jour directe.
5. Vous pouvez également effectuer les actions suivantes sur la version d'application sélectionnée, à l'aide des options suivantes du menu **Actions** :
   *  Supprimer la version
   *  Cloner la version
   *  Exporter une version


Pour plus d'informations sur la gestion des appareils, voir [Gestion des appareils](/docs/services/mobilefoundation?topic=mobilefoundation-manage_devices#manage_devices).
Pour plus d'informations sur la désactivation à distance d'une version de l'application,
voir [Désactivation à distance d'une version de l'application](/docs/services/mobilefoundation?topic=mobilefoundation-remotely_disable_an_app_version#remotely_disable_an_app_version).
{: note}
