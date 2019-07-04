---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation plans, migration of plans

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

# Migration du plan de service Mobile Foundation

Les instances Mobile Foundation créées à l'aide des plans obsolètes doivent être mises à jour avec les nouveaux plans. La mise à jour des plans peut également être nécessaire en fonction de l'utilisation de l'instance.
{: shortdesc}

## Exemple de scénario : migration du plan Professionnel par appareil vers le plan Professionnel, 1 application

1. Dans le tableau de bord IBM Cloud, sélectionnez l'instance IBM Mobile Foundation que vous souhaitez migrer.
2. Sélectionnez **Plan** dans le panneau de navigation.
   ![Plan Mobile Foundation existant](images/existing-plan.png)
3. Dans les plans de tarification répertoriés, sélectionnez Professionnel, 1 application.
   ![Nouveau plan Mobile Foundation](images/new-plan.png)
4. Cliquez sur le bouton **Sauvegarder**.
     La migration vers le plan Professionnel, 1 application est maintenant terminée et toutes les données existantes sont conservées. La facturation est modifiée et il n'y a pas de temps d'indisponibilité.
5. Après la migration du plan, l'instance de Mobile Foundation doit être recréée à partir du tableau de bord de service pour que la configuration appropriée soit prise en compte. Cette mise à jour nécessite un temps d'indisponibilité court. Vous devrez planifier le temps d'indisponibilité. Sélectionnez **Gérer** dans le panneau de navigation, puis cliquez sur **Recréer**.

Si vous utilisez l'un des plans obsolètes, vous devez migrer vers un nouveau plan.
{: note}

## Migrations de plan prises en charge

* Le plan *Développeur* (obsolète) peut être mis à jour uniquement vers le nouveau plan *Développeur*.
* Le plan *Développeur Pro* (obsolète) peut être mis à jour uniquement vers le plan *Professionnel par appareil* ou *Professionnel, 1 application*.
* Le plan *Professionnel par capacité* (obsolète) peut être mis à jour uniquement vers le plan *Professionnel par appareil* ou *Professionnel, 1 application*.
* Le plan *Professionnel par capacité* peut être mis à jour uniquement vers le plan *Professionnel, 1 application*.
* Le plan *Professionnel, 1 application* peut être mis à jour uniquement vers le plan *Professionnel par appareil*.
* La mise à jour du plan n'est pas prise en charge pour le nouveau plan *Développeur*.
