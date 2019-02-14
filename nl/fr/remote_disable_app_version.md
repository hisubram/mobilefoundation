---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-29"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Désactivation à distance d'une version de l'application
{: #remote_disable_app_version}

Dans cette section, nous verrons comment désactiver l'accès des utilisateurs à une version spécifique d'une application sur un système d'exploitation mobile spécifique et comment adresser un message personnalisé à l'utilisateur.

Vous pouvez utiliser la console Mobile Foundation Operations pour gérer l'accès aux applications.

1. Sélectionnez la version de votre application dans la section **Applications** de la barre de navigation de la console, puis sélectionnez l'application **Gestion**.
2. Remplacez le statut par **Accès désactivé**.
3. Indiquez une URL pour la nouvelle version de l'application (généralement, dans le magasin d'applications public ou privé approprié), dans la zone **Adresse URL de la dernière version**. 
   Dans certains environnements, Application Center fournit une URL permettant d'accéder directement à la vue détaillée de la version d'une application. Voir [Propriétés d'application](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/appcenter/appcenter-console/#application-properties).
   {: tip}

4. Ajoutez dans la zone **Message de notification par défaut** le message de notification personnalisé à afficher lorsque l'utilisateur tente d'accéder à l'application. L'exemple de message suivant demande aux utilisateurs d'effectuer la mise à niveau vers la dernière version :
   `Cette version n'est plus prise en charge. Effectuez une mise à niveau vers la dernière version.`
5. Dans la section **Environnements locaux pris en charge**, vous pouvez éventuellement fournir le message de notification dans d'autres langues.
6. Sélectionnez **Sauvegarder** pour appliquer vos modifications.

Lorsqu'un utilisateur exécute une application qui a été désactivée à distance, une boîte de dialogue contenant le message personnalisé configuré s'affiche. Le message apparaît pour toute interaction d'application nécessitant d'accéder à une ressource protégée ou lorsque l'application tente d'obtenir un jeton d'accès. Si vous avez fourni une URL de mise à niveau de version, la boîte de dialogue affiche un bouton **Obtenir la nouvelle version** pour la mise à niveau vers une version plus récente, en plus du bouton par défaut **Fermer**. <br/>
Si l'utilisateur ferme la boîte de dialogue sans mettre à niveau la version, il peut continuer à travailler avec les ressources de l'application non protégées. Cependant, toute interaction d'application nécessitant l'accès à une ressource protégée entraîne l'affichage de la fenêtre de dialogue et l'accès à l'application ou à l'utilisateur n'est pas autorisé.


