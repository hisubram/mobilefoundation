---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-22"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}

# Génération de graphiques personnalisés
{: #build_custom_charts}

La vue Graphiques personnalisés de la console Mobile Analytics vous permet de générer vos propres visualisations à partir des données d'analyse capturées et stockées. Cela vous aide à mieux identifier les informations prêtes à l'emploi ou même à les étendre en toute transparence à l'analyse métier formulée à partir de données personnalisées. 

Cette vue vous permet de choisir l'un des ensembles de données d'analyse pris en charge, puis de sélectionner l'un des types de graphiques pris en charge pour tracer l'ensemble de données. Vous pouvez même élaguer la visualisation en définissant des filtres à appliquer sur les données tracées.   

Ensembles de données pris en charge :
 * Journaux d'application
 * Sessions d'application
 * Données personnalisées
 * Transactions de réseau
 
Types de graphiques pris en charge :
 * Diagramme à barres
 * Organigramme
 * Diagramme linéaire
 * Groupe d'indicateurs 
 * Graphique circulaire
 * Tableau
 
La sélection de l'ensemble de données, le type de graphique à tracer, la définition des caractéristiques du graphique et les filtres de données à appliquer peuvent être rassemblés et sauvegardés dans une définition de graphique personnalisé. Vous pouvez créer et sauvegarder autant de définitions de graphique personnalisé que vous le souhaitez. Les graphiques personnalisés sauvegardés s'affichent dans la vue Graphiques personnalisés avec les données d'analyse pertinentes tracées.  

## Création d'un graphique personnalisé
{: #creating_custom_chart}

Pour créer un graphique personnalisé, procédez comme suit : 

1.  Cliquez sur le bouton **Créer un graphique** de l'onglet **Graphiques personnalisés** dans le tableau de bord Mobile Analytics. 
2.  Sur l'onglet **Paramètres généraux**, sélectionnez **Titre du graphique**, **Type d'événement** et le **Type de graphique**. 
3.  Lorsque vous sélectionnez le *Type d'événement* et le *Type de graphique*, l'onglet **Définition du graphique** s'affiche. Utilisez l'onglet *Définition du graphique* pour définir le graphique pour le type de graphique que vous avez sélectionné auparavant. Après avoir défini le graphique, vous pouvez définir les filtres et les propriétés du graphique. 
4.  Les **Filtres du graphique** sont utilisés pour affiner le graphique personnalisé. Vous pouvez définir plusieurs filtres pour un graphique. Par exemple, après avoir défini un graphique pour visualiser la durée moyenne d'une session d'application si vous souhaitez visualiser ce graphique uniquement pour une application spécifique, vous
pouvez créer un filtre comme suit : 
    * Sélectionnez **Nom de l'application** pour **Propriété**. 
    * Sélectionnez **Est égal à** pour **Opérateur**. 
    * Sélectionnez le nom de votre application pour **Valeur**. 
    * Cliquez sur **Ajouter un filtre**.
    Le filtre de noms d'application est ajouté au tableau des filtres de votre graphique. 
5.  Les propriétés du graphique sont disponibles pour les types de graphique **Tableau**, **Diagramme à barres** et **Diagramme linéaire**. Les propriétés du graphique visent à améliorer le mode de présentation des données afin que la visualisation soit plus efficace.
    Si vous avez créé un graphique de type **Tableau**, les propriétés du graphique permettent de définir la taille de page du tableau, la zone à trier, ainsi que l'ordre de tri de la zone.
    Si vous avez créé un graphique de type **Diagramme à barres** ou **Diagramme linéaire**, les propriétés du graphique permettent de définir des lignes de seuil pour ajouter un cadre de référence pour les personnes qui surveillent le graphique. 

## Obtention d'informations personnalisées à partir de journaux de données personnalisées
{: #creating_custom_chart_for_client_logs}    

Si vous souhaitez obtenir plus d'informations personnalisées comme la trace des utilisateurs sur l'application, vous devez d'abord capturer les informations de trace des utilisateurs pertinentes telles que la page choisie, l'option sélectionnée ou le bouton activé comme données personnalisées, puis les consigner. Consultez la rubrique sur l'[instrumentation de votre application](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app) pour savoir comment consigner les données personnalisées. 

Créez ensuite une définition de graphique personnalisé avec des données personnalisées comme type d'événement et choisissez un type de graphique. Lorsque vous poursuivez la définition des **Propriétés du graphique** ou des **Filtres du graphique**, vous remarquez que les types et les valeurs des données personnalisées s'affichent dans les boîtes de liste déroulante. Effectuez les sélections appropriées pour le type d'informations que vous recherchez.   

Le niveau de détail et l'utilité des informations personnalisées dépendent complètement de l'efficacité ou de la pertinence des données personnalisées définies et capturées dans vos applications.
{: note}

## Types de graphiques
{: #types_of_charts}

### Diagramme à barres
{:  #bar_graph}

Le diagramme à barres permet de visualiser des données numériques sur un axe des X. Lorsque vous définissez un diagramme à barres, vous devez d'abord choisir la valeur de l'axe des X. Vous pouvez choisir parmi les valeurs suivantes. 

* **Chronologie** - si vous voulez que vos données s'affichent sous forme de tendance (par exemple, durée moyenne de la session d'application au fil du temps), choisissez *Chronologie* pour l'axe des X. 
* **Propriété** - choisissez Propriété si vous voulez voir une répartition du nombre pour la propriété spécifique. Si vous choisissez Propriété pour l'axe des X, l'option Total est implicitement choisie pour l'axe des Y. Par exemple, choisissez *Propriété* pour l'axe des X et *Nom de l'application* pour *Propriété* afin de voir un nombre pour un type d'événement donné, qui est réparti par nom d'application. 

Après avoir défini une valeur pour l'axe des X, vous pouvez définir une valeur pour l'axe des Y. Si vous choisissez *Chronologie* pour l'axe des X, vous pouvez choisir les valeurs suivantes pour l'axe des Y. 

* **Moyenne** - calcule la moyenne d'une propriété numérique dans le type d'événement fourni. 
* **Total** - nombre total d'une propriété dans le type d'événement fourni. 
* **Unique** - nombre unique d'une propriété dans le type d'événement fourni.
Après avoir défini les axes du graphique, vous devez choisir une valeur pour Propriété. 

### Diagramme linéaire
{:  #line_graph}

Le diagramme linéaire permet de visualiser certains indicateurs au fil du temps. Ce type de graphique est particulièrement utile lorsque vous souhaitez visualiser la tendance des données au fil du temps. La première valeur à définir lorsque vous créez un diagramme linéaire est **Mesure**, qui peut avoir les valeurs suivantes. 

* **Moyenne** - calcule la moyenne d'une propriété numérique dans le type d'événement fourni. 
* **Total** - nombre total d'une propriété dans le type d'événement fourni. 
* **Unique** - nombre unique d'une propriété dans le type d'événement fourni.
Après avoir défini la mesure, vous devez choisir une valeur pour **Propriété**. 

### Organigramme
{:  #flow_chart}

L'organigramme permet de visualiser la répartition du flux d'une propriété sur une autre. Vous devez définir les propriétés suivantes pour un organigramme. 

* **Source** - valeur d'un noeud source dans le diagramme. 
* **Destination** - valeur du noeud de destination dans le digramme. 
* **Propriété** - valeur de propriété provenant du noeud source ou du noeud de destination.
L'organigramme vous permet de voir la répartition de densité de diverses sources qui sont acheminées vers une destination, ou inversement. Par exemple, si vous souhaitez voir la répartition des gravités de journal pour une application, vous pouvez définir les valeurs suivantes. 

* Sélectionnez *Nom de l'application* pour **Source**. 
* Sélectionnez *Niveau de journalisation* pour **Destination**. 
* Sélectionnez le nom de votre application pour **Propriété**. 

### Groupe d'indicateurs
{:  #metric_group}

Le groupe d'indicateurs peut être utilisé pour visualiser un seul indicateur qui est mesuré en tant que valeur moyenne, nombre total ou nombre unique. Pour définir un groupe d'indicateurs, vous devez définir l'une des valeurs possibles suivantes pour **Mesure**. 

* **Moyenne** - calcule la moyenne d'une propriété numérique dans le type d'événement fourni. 
* **Total** - nombre total d'une propriété dans le type d'événement fourni. 
* **Unique** - nombre unique d'une propriété dans le type d'événement fourni.
Après avoir défini la mesure, vous devez choisir une valeur pour *Propriété*. Cet indicateur est affiché dans le groupe d'indicateurs. 

### Graphique circulaire
{:  #pie_chart}

Le graphique circulaire peut être utilisé pour visualiser la répartition du nombre de valeurs pour une propriété particulière. Par exemple, si vous souhaitez voir la répartition des pannes, définissez les valeurs suivantes. 

* Sélectionnez *Session d'application* pour **Type d'événement**. 
* Sélectionnez *Graphique circulaire* pour **Type de graphique**. 
* Sélectionnez *Fermé par* pour **Propriété**.
Le graphique circulaire généré montre la répartition des sessions d'application qui ont été fermées par l'utilisateur à la place des sessions d'application qui ont été fermées par une panne. 

### Tableau
{:  #table}

Le tableau est utile si vous souhaitez voir les données brutes. La génération d'un tableau est aussi simple que l'ajout de colonnes pour les données brutes que vous souhaitez voir. Etant donné que toutes les propriétés ne sont pas requises pour des types d'événement spécifiques, des valeurs nulles peuvent apparaître dans votre tableau. Si vous souhaitez éviter que ces lignes apparaissent dans votre tableau, ajoutez un filtre *Existe* pour une propriété spécifique dans l'onglet **Filtres du graphique**. 

