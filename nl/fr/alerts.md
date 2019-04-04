---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-23"

keywords: mobile analytics, set up alerts, alert definitions

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Configuration d'alertes
{: #set_up_alerts}

Mobile Analytics propose la fonction Alertes, qui permet la définition d'alertes pour surveiller vos applications. Vous pouvez configurer des seuils qui, en cas de dépassement, déclenchent des alertes pour avertir le moniteur de la console Mobile Analytics. Les alertes déclenchées peuvent être visualisées sur la console ou peuvent être configurées pour être relayées sur un webhook personnalisé afin d'être traitées correctement.

Cette fonction permet de détecter et de signaler les violations de seuil observées dans les journaux d'erreur de l'application, lors de pannes d'application et de transactions réseau. La réactivité aux alertes et aux dépassements de seuil permet d'adapter la réponse aux conditions de performance de l'application sans avoir à surveiller constamment les vues/graphiques correspondants.

Cette fonction est disponible en version BÊTA.
{: note}

Les sections suivantes détaillent la création, la gestion des alertes et leur affichage sur la console Mobile Analytics.

## Création d'une définition d'alerte pour les journaux d'application
{: #creating_alert_def}

Vous pouvez créer une définition d'alerte basée sur les journaux d'application.  Par exemple, si vous souhaitez surveiller vos journaux d'application toutes les 5 minutes pour vérifier si une version spécifique de votre application a consigné des erreurs plus de trois fois, voici comment vous pouvez configurer cette opération. 

1.  Dans la console Mobile Analytics, cliquez sur **Définitions** pour accéder à la page des définitions d'alerte.
2.  Cliquez sur **Créer une alerte**.
3.  Indiquez les valeurs suivantes :
    * **Nom de l'alerte** : *Alerte pour les journaux d'application*
    * **Message** : *Alerte de message d'erreur*
    * **Fréquence de requête**: *5 minutes*
    * **Type d'événement** : *Journaux d'application*
        * **Propriété** : *Niveau de journalisation*
            * **Valeur** : *Erreur*
            * **Seuil** : *Total pour l'instance d'application*<br/>
              Si vous choisissez l'option *Moyenne pour l'application*, la moyenne des journaux d'application est calculée en fonction du nombre de périphériques. Par exemple, si vous disposez de deux périphériques et que l'un d'eux envoie six journaux d'application tandis que l'autre en envoie trois, la moyenne est égale à 4,5 journaux d'application.
              {: note}
            * **Opérateur** : *est supérieur ou égal à*
            * Valeur de seuil : *3*
4.  Cliquez sur **Suivant** et indiquez la valeur suivante :
    * **Méthode** : *Console d'analyse seulement*<br/>
      En outre, choisissez l'option *Console d'analyse et publication sur le réseau* si vous voulez envoyer un message POST avec un contenu JSON à votre adresse URL personnalisée. Les zones suivantes sont disponibles si vous choisissez cette option.
      * **Adresse URL de publication sur le réseau**
      * **En-têtes**
      * **Type d'authentification**
      * **Corps de la demande de publication**
5. Cliquez sur **Sauvegarder**.  

Vous avez maintenant créé une définition d'alerte pour déclencher une alerte à la fin de chaque intervalle de 5 minutes lorsque le nombre de journaux d'application a atteint le seuil de 3 journaux d'erreur au minimum. 

Cette alerte reste opérationnelle et active, en surveillant la fréquence de configuration jusqu'à ce que la définition d'alerte soit désactivée ou supprimée.
{: note}

## Création d'une définition d'alerte pour les pannes d'application
{: #creating_alert_crashes}

Voici un exemple de configuration d'alertes liées aux pannes d'application. Cette alerte surveille l'ensemble des pannes d'application toutes les 2 minutes et déclenche une alerte si le nombre de pannes observées dépasse 5. 

1.  Dans la console Mobile Analytics, cliquez sur **Définitions** pour accéder à la page des définitions d'alerte.
2.  Cliquez sur **Créer une alerte**.
3.  Indiquez les valeurs suivantes :
    * **Nom de l'alerte** : *Alerte pour les pannes d'application*
    * **Message** : *Alerte de panne d'application*
    * **Fréquence de requête**: *2 minutes*
    * **Type d'événement** : *Pannes d'application*
        * **Nom de l'application** : *Toute application*
        * **Toute application** : *Toute version*
    * **Seuil** : *Nombre de pannes*
    * **Opérateur** : *est supérieur ou égal à*
    * Valeur de seuil : *5*
4.  Cliquez sur **Méthode de distribution** ou **Suivant** et indiquez la valeur suivante :
    * **Méthode** : *Console d'analyse seulement*<br/>
      En outre, choisissez l'option *Console d'analyse et publication sur le réseau* si vous voulez envoyer un message POST avec un contenu JSON à votre adresse URL personnalisée. Les zones suivantes sont disponibles si vous choisissez cette option.
      * **Adresse URL de publication sur le réseau**
      * **En-têtes**
      * **Type d'authentification**
      * **Corps de la demande de publication**
5. Cliquez sur **Sauvegarder**.  

Cette alerte reste opérationnelle et active, en surveillant la fréquence de configuration jusqu'à ce que la définition d'alerte soit désactivée ou supprimée.
{: note}

## Gestion des définitions d'alerte
{: #managing_alert_def}

La page **Définitions** d'une alerte vous permet de gérer vos définitions d'alerte.

Pour chaque définition d'alerte, vous pouvez effectuer les opérations suivantes : 
* Cochez ou non la case située sous la colonne **Activé** pour activer ou désactiver une définition d'alerte spécifique.
* Si vous souhaitez créer une copie d'une définition d'alerte et modifier certaines valeurs, cliquez sur l'icône de duplication.
* Cliquez sur l'icône du crayon, si vous souhaitez modifier une définition d'alerte.
* Cliquez sur l'icône de la corbeille, si vous voulez supprimer une définition d'alerte.

## Affichage d'alertes
{: #viewing_alerts}

Vous pouvez afficher les alertes qui ont été déclenchées à partir de la page **Journaux**.

1.  Dans la console Mobile Analytics, cliquez sur **Journaux**.
2.  Cliquez sur l'icône **+** de n'importe quelle alerte. Cette action affiche la définition d'alerte et la section **Instances d'alerte**.
    Si la définition d'alerte correspondante n'a pas été supprimée ou modifiée, vous pouvez l'éditer en cliquant sur **Editer l'alerte**. Sinon, le bouton **Editer l'alerte** n'est pas disponible et le message suivant s'affiche :
    `Depuis, cette définition d'alerte a été modifiée ou supprimée.`
3.  Vous pouvez éventuellement sélectionner une alerte et cliquer sur l'icône de la corbeille pour supprimer l'alerte.
