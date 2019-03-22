---

copyright:
  years: 2018, 2019
lastupdated:  "2018-12-20"

keywords: mobile foundation plans, migration of plans

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

# Migrate the Mobile Foundation service plan

Mobile Foundation instances created using the deprecated plans need to be updated to the new plans. Plan update may also be needed based on the instance usage.
{: shortdesc}

## Sample scenario: Migrate from the Professional Per Device plan to the Professional 1 Application plan

1. From the IBM Cloud dashboard, select the IBM Mobile Foundation instance you want to migrate.
2. Select **Plan** from the left navigation.
   ![Existing Mobile Foundation plan](images/existing-plan.png)
3. From the listed pricing plans, select Professional 1 Application.
   ![New Mobile Foundation plan](images/new-plan.png)
4. Click the **Save** button and confirm the plan migration.
     Migration to Professional 1 Application is now completed and all the existing data is retained. The billing is changed and there’s no downtime.
5. After the plan migration, the Mobile Foundation instance needs to be re-created from the service dashboard for the right configuration to take effect. This update requires a short downtime. You'll need to plan for the downtime. Select **Manage** from the left navigation and click **Recreate**.

If you’re on one of the deprecated plans, you must migrate to a new plan.
{: note}

## Supported plan migrations

* *Developer* (deprecated) plan can be updated only to the new *Developer* plan.
* *Developer Pro* (deprecated) plan can be updated only to *Professional Per Device* or *Professional 1 Application* plan.
* *Professional Per Capacity* (deprecated) plan can be updated only to *Professional Per Device* or *Professional 1 Application* plan.
* *Professional Per Device* plan can be updated only to *Professional 1 Application* plan.
* *Professional 1 Application* plan can be updated only to *Professional Per Device* plan.
* Plan update isn’t supported for the new *Developer* plan.
