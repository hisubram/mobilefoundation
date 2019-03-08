---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-01"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Visualisation des informations sur la console
{: #visualize_insights_on_console}

Pour visualiser les données d'analyse capturées et envoyées à partir de votre application, vous devez lancer la console Mobile Analytics en cliquant sur l'option **Analytics Console** dans la barre de navigation de gauche de la console Mobile Foundation Operations. 

La console Mobile Analytics peut s'exécuter dans deux modes : 
  - **Mode démonstration ACTIVE**, qui est destiné uniquement à des fins de démonstration et affiche les différentes vues d'analyse (graphiques et tableaux) à l'aide de flux de données simulées. 
  - **Mode démonstration DESACTIVE**, qui affiche les différentes vues d'analyse en fonction des flux de données en temps réel provenant de vos applications [instrumentées pour Mobile Analytics](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app). 
  
Toutes les vues d'analyse peuvent être élaguées en appliquant des filtres aux options *Nom de l'application*, *Version*, *Système d'exploitation du terminal* et *Période*, ce qui vous permet d'obtenir des informations émanant de différents points de vue. 

Pour visualiser des informations pour votre application, vérifiez que : 
  - Votre application est correctement instrumentée pour capturer et envoyer les données d'analyse pertinentes au service Mobile Analytics. 
  - Vous avez désactivé le mode démonstration dans la console d'analyse. 
  - Vous appliquez les filtres appropriés. Par exemple, vérifiez que vous avez sélectionné une période lorsque votre application a été déployée dans la zone et est active chez des utilisateurs. 

La console Mobile Analytics fournit différents types d'analyse de l'utilisation et des performances de votre application mobile, selon les catégories indiquées dans le volet de navigation de gauche de la console d'analyse. Les sections suivantes détaillent les différentes vues d'analyse :  


## Utilisateurs
{: #reports_visualized_users}
Cette vue vous aide à approfondir vos connaissances sur les modèles d'intégration utilisateur, comme le nombre d'utilisateurs actifs ayant utilisé l'application dans une plage de dates donnée et une comparaison du nombre de nouveaux utilisateurs par rapport au nombre d'utilisateurs existants recommençant à utiliser votre application. Les graphiques de cette vue peuvent être filtrés selon le *nom de l'application*, le *système d'exploitation* ou la *version du système d'exploitation*. 

## Sessions
{: #reports_visualized_sessions}
Cette vue vous aide à approfondir vos connaissances des modèles d'utilisation de votre application en termes de *sessions d'application* pour la plage de dates spécifiée. Une session est enregistrée lorsqu'une application est amenée au premier plan d'un appareil. Vous obtiendrez plus de détails sur les heures de la journée où votre application est la plus et la moins utilisée, ce qui peut être utile pour votre entreprise. Les graphiques de cette vue peuvent être filtrés selon le *nom de l'application*, le *système d'exploitation* ou la *version du système d'exploitation*. 

## Demandes de réseau
{: #reports_visualized_network_requests}
Cette vue vous aide à approfondir vos connaissances sur votre expérience de l'application, car elle effectue des appels d'API vers les systèmes de back end. Elle comporte des tableaux et des graphiques qui permettent de savoir quelles sont les fonctions de vos systèmes de back end les plus utilisées et quels sont leurs temps de réponse et leur stabilité, et si vous devez envisager de rééquilibrer vos systèmes de support de back end. 

Cette vue contient des graphiques qui tracent sur une plage de données spécifique la durée moyenne d'aller-retour des appels d'API sortants de votre application, le nombre de demandes effectuées par appel d'API, le nombre de demandes ayant abouti par rapport à celles qui ont échoué, regroupées par codes de réponse. Les graphiques de cette vue peuvent être filtrés selon le nom de l'application, le système d'exploitation ou les versions du système d'exploitation.

## Pannes
{: #reports_visualized_crashes}
Cette vue vous aide à savoir dans quelle mesure votre application a été stable sur une période donnée et à décider si la conception/l'implémentation de votre application doit être corrigée. Elle fournit des graphiques qui opposent le nombre de pannes au nombre total d'utilisations et au taux de panne global. Les graphiques de cette vue peuvent être filtrés selon le *nom de l'application*, le *système d'exploitation* ou la *version du système d'exploitation*. 


## Traitement des incidents
{: #reports_visualized_troubleshooting}
Cette vue fournit toutes les informations nécessaires à un développeur d'applications pour traiter les incidents d'une application. Elle offre une analyse plus détaillée des pannes de votre application en ce qui concerne les périphériques affectés, le système d'exploitation hôte, l'heure spécifique de la panne, la trace de pile au moment de la panne, ainsi que les journaux de panne qui peuvent être téléchargés à des fins d'analyse plus détaillée.   

Pour regrouper les journaux de panne, recherchez les journaux d'application qui ont été consignés au niveau FATAL. Le SDK du client d'analyse pour Android et iOS natif traite les exceptions non interceptées et consigne des détails à leur sujet sous la forme de messages de journal de niveau FATAL. Toutefois, dans le cas de Cordova, les pannes sur la couche Javascript doivent être traitées par le développeur et les journaux de panne doivent être envoyés au service Mobile Analytics pour être visualisés et analysés dans la console Mobile Analytics.
{: note}


## Commentaires en retour des utilisateurs
{: #reports_visualized_userfeedback}
Cette vue vous permet d'approfondir vos connaissances sur l'expérience interactive réelle de vos utilisateurs lorsqu'ils se servent de l'application, et de connaître plus précisément leurs impressions. 

* Les **propriétaires d'application** peuvent accéder à une vue enrichie détaillée et contextuelle des bogues et à d'autres commentaires en retour envoyés par les
**utilisateurs et les testeurs** et consignés lors de l'exécution de l'application. 
* Les **développeurs** peuvent recevoir des contextes d'application précis pour diagnostiquer et corriger des bogues ou des écarts de fonctionnalité. 
* Les **propriétaires d'application** et les **développeurs** peuvent utiliser cette vue pour gérer également des actions sur les commentaires en retour reçus, par exemple l'enregistrement de commentaires ou de liens vers des problèmes créés dans les systèmes de suivi des bogues. Un statut global des vérifications peut également être défini pour chaque commentaire afin d'aider à récapituler les actions effectuées suite aux commentaires. 

## Graphiques personnalisés
Cette vue étend Mobile Analytics à des cas personnalisés où les **propriétaires d'application** et les **développeurs** souhaiteraient créer leur propre analyse spécifique à l'application. Cette fonction vous permet de générer vos propres vues d'analyse (graphiques, tableaux, etc.) à partir de données d'analyse standard capturées par le SDK client, mais également de personnaliser les données ou les données spécifiques à l'application qui sont consignées. Reportez-vous [ici](/docs/services/mobilefoundation?topic=mobilefoundation-build_custom_charts#build_custom_charts) pour plus d'informations sur cette fonction d'analyse étendue. 

