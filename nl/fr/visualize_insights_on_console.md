---

copyright:
  years: 2018
lastupdated: "2018-11-22"

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

Dans MobileFirst Analytics Console, vous pouvez afficher et configurer les rapports d'analyse, gérer les alertes et afficher les journaux du client. Lancez Analytics Console à partir de Mobile Foundation Operations Console en cliquant sur **Analytics Console** dans la barre de navigation de gauche.

Analytics Console est lancé et le tableau de bord par défaut apparaît. Si une application client a déjà envoyé des journaux et des données d'analyse au serveur, les rapports pertinents sont créés et affichés. Dans le tableau de bord, vous pouvez examiner les données d'analyse collectées. Ces données d'analyse peuvent être liées aux pannes d'application, aux sessions d'application et au temps de traitement du serveur. De plus, vous pouvez créer des graphiques personnalisés et gérer les alertes. 

Outre une vue récapitulative de vos analyses mobiles, la fonctionnalité d'analyse offre la possibilité d'effectuer une recherche brute dans les journaux client, les données capturées sur les incidents client et toutes les données supplémentaires que vous fournissez explicitement via des appels de fonction d'API client qui alimentent Mobile Analytics.

## Surveillance des données d'application 
{: #monitoring_app_data}

Mobile Analytics assure la surveillance et l'analyse de vos applications mobiles. Vous pouvez enregistrer des journaux d'application et surveiller des données avec le kit de développement de logiciels (SDK) de Mobile Analytics Client. Les développeurs peuvent contrôler à quel moment envoyer ces données au service Mobile Analytics. Lorsque les données sont livrées à Mobile Analytics, vous pouvez utiliser la console Mobile Analytics pour obtenir des informations d'analyse sur vos applications mobiles, vos appareils et les journaux des applications. 

Certaines des informations qui peuvent être dérivées sont les suivantes : 

**Mesures prédéfinies**

Avec des mesures prédéfinies, vous pouvez répondre à des questions telles que : 
* Quel est le nombre de nouveaux utilisateurs de mon application ? 
* Combien de personnes utilisent-elles activement mon application ?
* A quelle fréquence les personnes utilisent-elles mon application ?
* A quelle heure de la journée les personnes utilisent-elles mon application ?
* Quels sont les modèles d'appareil préférés des utilisateurs de mon application ? 
* Quand cesser la prise en charge de systèmes d'exploitation surannés ?
* Quelles sont les applications qui présentent des problèmes de performance ?

**Evénements personnalisés**

En ajoutant vos propres événements personnalisés, vous pouvez répondre à des questions telles que : 
* Quelles sont les fonctions les plus utilisées, et celles les moins utilisées ?
* A quel endroit les utilisateurs entrent-ils et quittent-ils mon application ?
* Quelles sont les activités que visualisent le plus les utilisateurs ?
* les utilisateurs suivent-ils jusqu'au bout les flux de travaux (par exemple, les entonnoirs de conversion) ?

## Rapports pouvant être visualisés : Utilisateurs 
{: #reports_visualized_users}

Ce rapport affiche un graphique indiquant le nombre d'utilisateurs actifs ayant utilisé l'application dans une plage de dates spécifiée. Le rapport indique également le nombre de nouveaux utilisateurs qui sont des utilisateurs uniques qui utilisent l'application pour la première fois, pour la plage de dates spécifiée. Les graphiques peuvent être filtrés selon le nom de l'application, le système d'exploitation ou les versions du système d'exploitation. 

## Rapports pouvant être visualisés : Sessions 
{: #reports_visualized_sessions}

Ce rapport affiche un graphique montrant les sessions d'application pour la plage de dates spécifiée. Une session est enregistrée lorsqu'une application est amenée au premier plan d'un appareil. Les graphiques peuvent être filtrés selon le nom de l'application, le système d'exploitation ou les versions du système d'exploitation. 

## Rapports pouvant être visualisés : Demandes de réseau 
{: #reports_visualized_network_requests}

Visualisez les données de demande de réseau de vos applications dans la console Mobile Analytics. 

Les données sont disponibles pour les mesures suivantes :

**Durée de la boucle** - définit la durée, en millisecondes, nécessaire à votre application pour effectuer des demandes de réseau.
**Nombre de demandes** - affiche la fréquence à laquelle une application effectue des demandes de réseau. Les données sont également affichées en tant que moyenne. Les graphiques peuvent être filtrés selon le nom de l'application, le système d'exploitation ou les versions du système d'exploitation. 

## Rapports pouvant être visualisés : Pannes 
{: #reports_visualized_crashes}

Vous pouvez afficher des informations sur les pannes de votre application dans la console Mobile Analytics afin de mieux surveiller et dépanner vos applications.

Sur la page Pannes, le tableau **Présentation de la panne** affiche les colonnes de données suivantes :

**Application** : nom d'application<br/>
**Pannes** : nombre total de pannes pour cette application<br/>
**Nombre total d'utilisations** : nombre total de fois qu'un utilisateur ouvre et ferme cette application<br/>
**Taux de pannes** : pourcentage de pannes par utilisation<br/>
Vous pouvez rapidement afficher des informations sur les pannes de votre application dans le tableau des Pannes. Le diagramme à barres
Pannes affiche un histogramme des pannes au fil du temps.<br/>

Vous pouvez afficher les données de panne de deux façons :

1.  Afficher le taux de panne : taux de panne dans le temps
2.  Afficher le nombre total de pannes : nombre total de pannes dans le temps

## Rapports pouvant être visualisés : Traitement des incidents 
{: #reports_visualized_troubleshooting}

La page **Traitement des incidents** dans la console Mobile Analytics offre une vue granulaire des pannes de votre application, à l'aide du tableau **Récapitulatif des pannes**.

Le tableau **Récapitulatif des pannes** inclut les colonnes de données suivantes et peut être trié :

* Pannes 
* Appareils
* Dernière panne
* Application
* Système d'exploitation
* Message

Cliquez sur l'icône + située en regard de n'importe quelle entrée pour afficher le tableau **Détails des pannes**, qui inclut les colonnes suivantes :

* Heure de la panne 
* Version de l'application 
* Version du système d'exploitation 
* Modèle d'appareil 
* ID d'appareil 
* Téléchargement : lien de téléchargement des journaux qui ont conduit à la panne 

Développez n'importe quelle entrée du tableau **Détails des pannes** pour obtenir plus de détails, y compris une trace de pile.

Les données du tableau **Récapitulatif des pannes** sont renseignées en interrogeant les journaux d'application de niveau irrécupérable. Si votre application ne collecte pas de journaux d'application de niveau irrécupérable, aucune donnée n'est disponible.
{: note}


## Rapports pouvant être visualisés : Commentaires en retour des utilisateurs 
{: #reports_visualized_userfeedback}

Les commentaires en retour des utilisateurs permettent une analyse des commentaires intégrés à l'application à l'aide de Mobile Analytics. Avec cette fonctionnalité de Mobile Analytics : 
* Les **utilisateurs et testeurs** peuvent enregistrer et envoyer des commentaires en retour et des rapports de bogues à partir de l'application pendant qu'ils exécutent et utilisent l'application.
* Les **propriétaires d'applications** ont une idée plus précise de l'expérience utilisateur de l'application grâce à ce retour d'informations riche en contexte.
* Les **développeurs** reçoivent cependant des contextes d'application précis permettant de diagnostiquer et de corriger les bogues ou les lacunes de fonctionnalités.

### Activation des commentaires en retour des utilisateurs 
{: #enable_user_feedback}

Effectuez les étapes suivantes pour permettre à votre application mobile de capturer les commentaires des utilisateurs. 

#### Instrumentation de votre application 
{: #instrument_app}

* Instrumentez votre application mobile pour passer en mode commentaires en retour. Appelez l'API `Analytics.triggerFeedbackMode();` pour appeler le mode commentaires en retour. <!--For more information, refer to the documentation [here](instrument_an_app.html)-->.
* L'API peut être appelée sur tout événement d'application tel que des boutons, des actions de menu ou des gestes. 

#### Réception de commentaires en retour des utilisateurs 
{: #receive_feedback}

* Les utilisateurs et les testeurs de votre application peuvent basculer en mode commentaires en retour en déclenchant l'action d'application instrumentée pour les commentaires en retour. 
* A partir du mode commentaires en retour, des commentaire en retour contextuels détaillés ainsi qu'une capture d'écran peuvent être rassemblés et envoyés à Mobile Analytics. 

#### Analyse des commentaires en retour des utilisateurs 
{: #analyze_feedback}

* Mobile Analytics reçoit et consolide les riches commentaires en retour contextuels envoyés par les applications mobiles. 
* Connectez-vous à la console Mobile Analytics et sélectionnez l'option **Commentaires en retour des utilisateurs** pour afficher les commentaires.
* Un propriétaire d'application peut consulter les commentaires en retour, ajouter des commentaires et les étiqueter avec un statut de révision. Les commentaires peuvent généralement être des actions planifiées, telles que des liens vers des problèmes Git, créés pour travailler sur les commentaires en retour, ou les commentaires peuvent indiquer pourquoi aucune action n'est requise sur les commentaires en retour. 
* Le statut de révision peut être utilisé pour gérer efficacement les commentaires en retour en les classant dans l'une des différentes options de statut de révision. 

