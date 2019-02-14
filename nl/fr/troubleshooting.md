---

copyright:
  years: 2018, 2019
lastupdated: "2018-02-12"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Identification et résolution des problèmes
{: #troubleshooting}

Pour isoler et résoudre les problèmes que vous rencontrez avec vos produits
{{site.data.keyword.IBM_notm}}, vous pouvez utiliser les informations de dépannage. Celles-ci contiennent les instructions à suivre pour identifier les problèmes liés aux ressources fournies avec vos produits {{site.data.keyword.IBM_notm}},
parmi lesquels {{site.data.keyword.mobilefoundation_short}}.
{: shortdesc}

## Techniques pour le traitement des problèmes
{: #ts_overview}

La *recherche de panne* est une approche systématique de la résolution d'un problème. Il s'agit de déterminer la raison pour laquelle un problème s'est produit et d'identifier les moyens d'y remédier. Certaines techniques courantes peuvent vous aider dans votre recherche de panne.

La première étape de la recherche de panne
est de décrire complètement le problème. Les descriptions de
problème aident le représentant du support technique {{site.data.keyword.IBM_notm}} et vous-même à savoir où commencer pour trouver la cause du problème. Cette étape vous amènera à vous poser quelques questions élémentaires :

- Quels sont les symptômes du problème ?
- Où le problème se produit-il ?
- Quand le problème se produit-il ?
- Dans quelles circonstances le problème se produit-il ?
- Le problème peut-il être reproduit ?

Les réponses à ces questions donnent généralement une bonne description du problème qui peut conduire à sa résolution.

### Quels sont les symptômes du problème ?

Lorsque vous commencez à décrire un problème, la question la plus évidente est
"Quel est le problème ?" Cette question, qui semble simple, peut cependant être décomposée en plusieurs questions plus ciblées qui permettront de mieux décrire le problème. Ces questions peuvent être :

- Par qui ou par quoi le problème est-il signalé ?
- Quels sont les codes d'erreur et les messages ?
- Comment la défaillance du système se produit-elle ? Par exemple, est-ce une boucle, un blocage, un arrêt brutal, une dégradation des performances ou un résultat incorrect ?

### Où le problème se produit-il ?

Il n'est pas toujours aisé de déterminer à quel endroit survient le problème, mais
c'est l'une des étapes les plus importantes pour sa résolution. De nombreuses couches de
technologie peuvent exister entre les composants qui signalent le problème et les composants défaillants. Réseaux, disques et pilotes figurent parmi les composants à examiner dans votre investigation du problème.

Les questions suivantes vous aideront à cerner l'endroit où se produit le problème pour vous permettre d'isoler la couche où il se situe :

- Le problème est-il spécifique à une plateforme ou un système d'exploitation,
ou est-il commun à plusieurs plateformes ou systèmes d'exploitation ?
- L'environnement et la configuration actuels sont-ils pris en charge ?
- Tous les utilisateurs rencontrent-ils ce problème ?
- (Pour les installations multisites) Tous les sites rencontrent-ils ce problème ?

Si le problème est signalé par une couche en particulier, cela ne signifie pas pour autant qu'il
est originaire de cette couche. L'identification de l'origine d'un problème revient
en partie à comprendre l'environnement dans lequel il existe. Prenez quelques instants
pour décrire complètement l'environnement du problème, notamment le système
d'exploitation et sa version, tous les logiciels correspondants et leur version, ainsi
que les informations sur le matériel. Vérifiez que l'environnement d'exécution est bien une configuration
prise en charge : de nombreux problèmes sont imputables à des niveaux
de logiciels incompatibles qui n'ont pas été prévus pour fonctionner
ensemble ou dont le fonctionnement concomitant n'a pas encore fait
l'objet de tests exhaustifs.

### Quand le problème se produit-il ?

Etablissez une chronologie détaillée des événements qui ont conduit à une
défaillance, surtout lorsqu'il s'agit d'un problème qui ne s'est produit qu'une fois. Il est plus facile de décrire le déroulement en commençant par la fin :
commencez par le moment où l'erreur a été signalée (aussi précisément que
possible, même à la milliseconde près) et remontez le cours des événements à l'aide des journaux
et informations disponibles. Généralement, il n'est pas nécessaire de remonter plus loin que le premier événement suspect enregistré dans un
journal de diagnostic.

Pour développer une chronologie détaillée des événements, répondez à ces questions :

- Le problème se produit-il seulement à des moments particuliers du jour ou de la nuit ?
- A quelle fréquence le problème se produit-il ?
- Quelle suite d'événements a précédé le moment où le problème a été signalé ?
- Le problème se produit-il après un changement d'environnement, comme la mise à
niveau ou l'installation d'un logiciel ou de matériel ?

Les réponses à ces questions fournissent un cadre de référence pour l'analyse du problème.

### Dans quelles circonstances le problème se produit-il ?

Savoir quels systèmes et applications étaient en cours d'exécution au moment où le problème s'est produit est essentiel
à la recherche de panne. Ces questions sur votre environnement peuvent vous aider à identifier la cause première du problème :

- Le problème se produit-il toujours lorsque la même tâche est exécutée ?
- Faut-il qu'une certaine succession d'événements se produise pour que le problème
survienne ?
- D'autres applications échouent-elles au même moment ?

La réponse à ces types de questions peut vous aider à décrire l'environnement dans
lequel le problème se produit et à faire le lien avec d'éventuelles dépendances. N'oubliez pas que si plusieurs problèmes se produisent à peu près au même moment, cela ne signifie pas pour autant qu'ils sont liés entre eux.

### Le problème peut-il être reproduit ?

Du point de vue de la recherche de panne, le problème idéal est celui qui peut être reproduit. Généralement, dès lors qu'un problème peut être reproduit, vous disposez de plus d'outils
ou de procédures pour en rechercher la cause. Par conséquent, les problèmes que vous pouvez
reproduire sont souvent plus faciles à déboguer et à résoudre.

Ces problèmes présentent toutefois un inconvénient. S'ils ont un impact important sur l'activité, vous ne souhaitez pas qu'ils se reproduisent. Si possible, recréez le problème dans un environnement de test ou de développement qui vous offrira davantage de souplesse et de contrôle pour mener votre enquête.

- Le problème peut-il être recréé sur un système test ?
- Plusieurs utilisateurs ou applications rencontrent-ils le même type de problème ?
- Est-il possible de recréer le problème en exécutant une unique commande, un ensemble de commandes ou une application particulière ?


##  Limitations connues
{: #knownlimitations_mfp}

* L'interface utilisateur du service {{site.data.keyword.mobilefoundation_short}} n'utilise pas le modèle spécifique à l'environnement local sélectionné par l'utilisateur pour l'affichage des nombres.

## Aide Mobile Foundation
{: #getting_help_mobilefoundation}

Si vous avez des problèmes ou des questions quand vous utilisez {{site.data.keyword.mobilefoundation_short}}, vous pouvez obtenir de l'aide en recherchant des informations précises ou en posant des questions via un forum. Vous pouvez aussi ouvrir un ticket de demande de service.

Quand vous utilisez les forums pour poser une question, prenez soin d'étiqueter cette dernière de façon à ce qu'elle soit vue par les équipes de développement {{site.data.keyword.Bluemix_notm}}.

En cas de questions d'ordre technique sur le développement et le déploiement d'une application avec {{site.data.keyword.mobilefoundation_short}}, postez votre question sur le forum [Stack Overflow ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://stackoverflow.com/search?q=ibm-mobilefirst+bluemix){:new_window} en lui adjoignant les balises `bluemix` et `ibm-mobilefirst`.

Pour des questions relatives au service et aux instructions de mise en route, utilisez le forum [IBM developerWorks dW Answers ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/answers/topics/mobilefirst/?smartspace=bluemix){:new_window}. Incluez les étiquettes `bluemix` et `mobilefirst`.

Pour plus d'informations sur l'ouverture d'un ticket de demande de service IBM, sur les niveaux de support disponibles ou les niveaux de gravité des tickets, voir la rubrique décrivant
[Comment contacter le support ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/docs/get-support/getstarttssup.html#typesofsupport  ){: new_window}.
