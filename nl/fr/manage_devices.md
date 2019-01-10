---

copyright:
  years: 2018
lastupdated: "2018-11-29"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Gestion des appareils
{: #manage_devices}

L'accès aux appareils et l'état des appareils peuvent être gérés à partir de la console Mobile Foundation Operations. Un appareil est identifié de manière unique avec un identifiant appelé ID d'appareil, attribué par le SDK client de Mobile Foundation. Vous pouvez également définir un nom d'affichage pour votre appareil. Les zones d'ID d'appareil et de nom d'affichage d'appareil sont indexées pour la recherche. 

Les développeurs d'applications peuvent utiliser la méthode `setDeviceDisplayName` de la classe `WLClient` pour définir le nom d'affichage d'appareil. Voir [ici](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/) pour consulter la documentation `WLClient`. Les développeurs d'adaptateurs Java peuvent également définir le nom d'affichage de l'appareil à l'aide de la méthode `setDeviceDisplayName` de la classe `com.ibm.mfp.server.registration.external.model MobileDeviceData`.
{: tip}

Le serveur Mobile Foundation conserve les informations d'état pour chaque appareil qui accède au serveur. Les valeurs possibles de statut sont : 
* Actif 
* Perdu 
* Volé
* Arrivé à expiration  
* Désactivé 
  
Le statut par défaut d'un appareil est **Actif**, ce qui indique que l'accès à partir de cet appareil n'est pas bloqué. Vous pouvez remplacer le statut par **Perdu**, **Volé** ou **Désactivé** pour bloquer l'accès à vos ressources d'application à partir de l'appareil. Vous pouvez toujours restaurer le statut **Actif** pour autoriser de nouveau l'accès.  

Le statut **Arrivé à expiration** est un statut spécial défini par le serveur Mobile Foundation après qu'un appareil a atteint une durée d'inactivité préconfigurée. Ce statut est utilisé pour le suivi des licences et n'affecte par les droits d'accès d'un appareil. Lorsqu'un appareil dont le statut est **Arrivé à expiration** se reconnecte au serveur, son statut est restauré sur **Actif** et l'appareil reçoit l'accès au serveur.

## Appareils par bloc
{: #block_devices}

1. Accédez à la page **Appareils** à partir de la console Mobile Foundation Operations.
2. Utilisez la zone **Recherche** pour rechercher un appareil en fournissant l'ID utilisateur associé à l'appareil ou en fournissant le nom complet de l'appareil (s'il est défini sur un appareil). 
3. Pour chaque appareil renvoyé dans le résultat de la recherche, vous pouvez voir l'ID de l'appareil, le nom complet, le modèle de l'appareil, le système d'exploitation et la liste des ID d'utilisateurs associés à l'appareil. 
4. La colonne de statut de l'appareil indique le statut de l'appareil. Vous pouvez remplacer le statut de l'appareil par **Perdu**, **Volé** ou **Désactivé** pour bloquer l'accès de l'appareil aux ressources de l'application protégée. 
   
   Le fait de rétablir le statut **Actif** restaure les droits d'accès d'origine. {: note}


## Annuler l'enregistrement des appareils 
{: #unregister_devices}

1. Accédez à la page **Appareils** à partir de la console Mobile Foundation Operations.
2. Utilisez la zone **Recherche** pour rechercher un appareil en fournissant l'ID utilisateur associé à l'appareil ou en fournissant le nom complet de l'appareil (s'il est défini sur un appareil). 
3. Pour chaque appareil renvoyé dans le résultat de la recherche, vous pouvez voir l'ID de l'appareil, le nom complet, le modèle de l'appareil, le système d'exploitation et la liste des ID d'utilisateurs associés à l'appareil. 
4. Sélectionnez *Annuler l'enregistrement* dans la colonne **Actions**.

   L'annulation de l'enregistrement d'un appareil supprime les données d'enregistrement de toutes les applications Mobile Foundation installées sur l'appareil. De plus, le nom d'affichage de l'appareil, la liste des utilisateurs associés à l'appareil et les attributs publics que l'application enregistrée pour cet appareil sont supprimés. {: note}


N'**annulez pas l'enregistrement** d'un appareil, si vous avez l'intention de **bloquer** l'appareil. L'annulation de l'enregistrement d'un appareil supprime toutes les données relatives à cet appareil. Si un utilisateur tente d'accéder au serveur Mobile Foundation à l'aide du même appareil, il est invité à enregistrer l'appareil ce qui crée un nouvel ID de l'appareil sur le serveur. Cela signifie que l'appareil retrouve l'accès au serveur Mobile Foundation.
{: important}
