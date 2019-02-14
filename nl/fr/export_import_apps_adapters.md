---

copyright:
  years: 2018
lastupdated:  "2018-08-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Exportation et importation d'applications et d'adaptateurs
{: #export_import_apps_adapters}

A partir de {{site.data.keyword.mfp_oc_short_notm}}, sous certaines conditions, vous pouvez exporter une application ou l'une de ses versions, puis l'importer sur un autre serveur. Vous pouvez également exporter et réimporter des adaptateurs. Utilisez cette fonctionnalité à des fins de réutilisation ou de sauvegarde ou pour migrer à travers les instances {{site.data.keyword.mfserver_short_notm}}.

Si vous disposez du rôle administrateur **mfpadmin** et du rôle déployeur **mfpdeployer**, vous pouvez exporter une version ou toutes les versions d'une application. L'application ou la version est exportée sous la forme d'un fichier compressé `.zip` dans lequel sont sauvegardés l'*ID application*, les *descripteurs*, les *données d'authenticité * et les *ressources Web*. Vous pouvez importer ultérieurement l'archive pour redéployer l'application ou la version sur le même serveur ou sur un autre.

Examinez attentivement votre cas d'utilisation :
* Le fichier d'exportation inclut les données d'authenticité d'application. Ces données sont propres à la génération d'une application mobile. L'application mobile inclut l'URL du serveur. Par conséquent, si vous souhaitez utiliser un autre serveur, vous devez régénérer l'application. Transférer uniquement les fichiers d'application exportés ne fonctionnerait pas.
* Certains artefacts peuvent varier d'un serveur à un autre. Les données d'identification push varient selon que vous travaillez dans un environnement de développement ou de production.
* La configuration d'exécution de l'application (qui contient l'état actif ou désactivé et les profils de journal) peut être transférée dans certains cas, mais pas tous.
* Le transfert de ressources Web peut ne pas avoir de sens dans certains cas, par exemple lorsque vous recréez l'application pour qu'elle utilise un nouveau serveur.
{: tip}

##  Prérequis
{: #prereq}

Lancez {{site.data.keyword.mfp_oc_short_notm}}.

##  Procédure
{: #procedure}

1.  Dans la barre latérale de navigation, sélectionnez une application, une version d'application ou un adaptateur.

2.  Sélectionnez **Actions > Exporter une application**, **Exporter une version** ou **Exporter l'adaptateur**.
     Vous êtes invité à enregistrer le fichier archive `.zip` qui encapsule les ressources exportées. L'aspect de la boîte de dialogue dépend de votre navigateur et le dossier cible dépend des paramètres de votre navigateur.

3.   Sauvegardez le fichier archive.
      Le nom de fichier archive inclut le nom et la version de l'application ou de l'adaptateur, pa exemple, `export_applications_com.sample.zip`.

4.   Pour réutiliser une archive d'exportation existante, sélectionnez **Actions > Importer une application**, **Importer la version** ou **Importer un adaptateur**, parcourez l'archive et cliquez sur **Déployer**.
      Le panneau principal de la console affiche les détails de l'application ou de l'adaptateur importés.

##    Résultats
{: #results}

Si vous importez sur le même serveur, l'application ou la version n'est pas nécessairement restaurée telle qu'elle a été exportée. En d'autres termes, le redéploiement au moment de l'importation ne supprime pas les modifications ultérieures. En revanche, si certaines ressources d'application sont modifiées entre le moment de l'exportation et celui du redéploiement lors de l'importation, seules les ressources incluses dans l'archive exportée sont redéployées dans leur état d'origine.
<br/>
Par exemple, si vous exportez une application sans données d'authenticité, vous chargez des données d'authenticité, puis vous importez l'archive initiale, les données d'authenticité ne sont pas effacées.
