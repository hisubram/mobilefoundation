---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-01"

keywords: mobile analytics, charts, visualize data, analytics console

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Visualize insights on the console
{: #visualize_insights_on_console}

To visualize insights from the analytics data that is captured and sent from your application, you must launch the Mobile Analytics console by clicking the **Analytics Console** option from the left navigation of the Mobile Foundation Operations console.

The Mobile Analytics Console can be run in two modes:
  - **Demo Mode ON**, which is purely for demonstration purposes and shows the different analytics views (charts and tables) by using simulated data feeds.
  - **Demo Mode OFF**, which shows the various analytics views based on realtime data feeds coming from your applications that are [instrumented for Mobile Analytics](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app).

All analytics views can be pruned by applying filters around *application name*, *version*, *device OS*, and *time period*, thus allowing you to obtain insights from different perspectives.

To visualize insights for your application, ensure the following.
  - Your application is instrumented to capture and send the relevant analytics data to Mobile Analytics service.
  - You turned OFF the Demo mode in the Analytics console
  - You apply the right filters. For example, ensure that you select a time period when your application is deployed in the field and is active with users.

The Mobile Analytics console provides different types of analysis of your mobile application usage and performance as categorized in the left navigation pane of the Analytics console.  The following sections detail the different analytics views:


## Users
{: #reports_visualized_users}
This view helps you get insights into 'User Onboarding Patterns' such as the number of active users who used the app within a specified date range and a comparison of the number of new users versus existing users who return to use your app.
The charts in this view can be filtered on *app name*, *operating system* or *operating system version*.

## Sessions
{: #reports_visualized_sessions}
This view helps you get insights into your application's 'Usage Patterns' in terms of *App Sessions* for the specified date range. A session is recorded when an app is brought to the foreground of a device.  You get insights to what times of the day your application is most and least used and this information can lead to useful business insights. The charts in this view can be filtered on *app name*, *operating system* or *operating system version*.

## Network Requests
{: #reports_visualized_network_requests}
This view helps you get insights about your application's experience as it makes API calls to the backend systems.  This view has tables and charts that provides a peek into what are the most used functions of your backend systems and what is their response time and stability and if you must consider rebalancing your backend support systems.

This view contains charts that plot against a data range that is selected, the Average RoundTrip Time of your application's outbound API calls, the number of requests made per API call, the number of succeeded requests versus failed ones grouped by the response codes.  The charts in this view can be filtered on app name, operating system or operating system versions.

## Crashes
{: #reports_visualized_crashes}
This view helps you with insights as to how stable your application was between a selected time period and helps you decide whether your application design or implementation must be fixed.  It provides charts that contrast the number of crashes against the total number of uses and the overall crash rate.  The charts in this view can be filtered on *app name*, *operating system* or *operating system version*.


## Troubleshooting
{: #reports_visualized_troubleshooting}
This view provides all the necessary information an application developer might need to troubleshoot an application.  This view provides a more detailed analysis of your application's crashes in terms of the devices that are affected, the host OS, specific time of the crash, the stacktrace at the time of the crash and also crash logs that can be downloaded for a more detailed analysis.  

Crash logs are gathered by looking up for app logs that are logged at the FATAL level.  The Analytics Client SDK for Android and iOS native handles uncaught exceptions and logs details about them as FATAL level log messages.  However, if Cordova, any crashes at the JavaScript layer needs to be handled by the developer and crash logs that are sent to the Mobile Analytics service to be viewed and analyzed in the Mobile Analytics console.
{: note}


## User Feedback
{: #reports_visualized_userfeedback}
This view provides insights into the actual interactive experience your users are undergoing while they use the app and how do they feel about it.

* **App owners** can get a detailed, context rich view of bugs, and other feedback sent by **Users and Testers**  as recorded while you run the application
* **Developers** can receive accurate application contexts to diagnose and fix bugs or feature gaps.
* **App owners** and **Developers** can use this view to also manage actions on feedback received such as recording comments or links to issues created in bug-tracking systems.  An overall review status can also be set to each feedback to help in summarizing actions taken on user feedback.

## Custom Charts
This view extends Mobile Analytics to custom cases where **App owners** and **Developers** would like to build their own, application-specific analytics.   Using this facility you can build your own analytics views (charts, tables and so on) around standard analytics data that is captured by the Client SDK and also custom data or application specific data that is logged.  For more information on this extended analytics facility, see [here](/docs/services/mobilefoundation?topic=mobilefoundation-build_custom_charts#build_custom_charts).
