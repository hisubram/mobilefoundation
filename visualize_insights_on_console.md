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

# Visualize insights on the console
{: #visualize_insights_on_console}

From the MobileFirst Analytics Console you can view and configure the Analytics reports, manage alerts, and view client logs. Launch the Analytics Console from the Mobile Foundation Operations Console by clicking the **Analytics Console** from the left navigation.

The Analytics Console is launched and the default dashboard appears. If a client application has already sent logs and analytics data to the server, the relevant reports are created and displayed. From the dashboard, you can review analytics data that is collected. This analytics data could be related to application crashes, application sessions, and server processing time. Additionally, you can create custom charts and manage alerts.

In addition to an at-a-glance view of your mobile analytics, the analytics feature includes the capability to perform a raw search against client logs, captured client crash data, and any extra data that you explicitly provide through client API function calls that feed into Mobile Analytics.

## Monitoring app data
{: #monitoring_app_data}

Mobile Analytics provides monitoring and analytics for your mobile applications. You can record application logs and monitor data with the Mobile Analytics Client SDK. Developers can control when to send this data to the Mobile Analytics Service. When the data is delivered to Mobile Analytics, you can use the Mobile Analytics console to get analytics insights about your mobile applications, devices, and application logs.

Some of the insights that can be derived:

**Pre-defined metrics**

With pre-defined metrics you can answer questions such as:
* How many new users do I have?
* How many people are actively using my application?
* How frequently are people using my application?
* What time of day are people using my application?
* What device models do my users prefer?
* When should I deprecate support for legacy operating systems?
* Which applications are experiencing performance issues?

**Custom events**

By adding your own custom events, you can answer questions such as:
* What features are used most and least?
* Where are users entering and leaving my app?
* What activities are users viewing most?
* Are users completing workflows in the app (for example, conversion funnels)?

## Reports that can be visualized: Users
{: #reports_visualized_users}

This report displays a chart showing the number of active users who have used the app within a specified date range. Report also shows the number of new users who are unique users who are using the app for the first time, for the specified date range.
The charts can be filtered on app name, operating system or operating system versions.

## Reports that can be visualized: Sessions
{: #reports_visualized_sessions}

This report displays a chart showing App Sessions for the specified date range. A session is recorded when an app is brought to the foreground of a device. The charts can be filtered on app name, operating system or operating system versions.

## Reports that can be visualized: Network Requests
{: #reports_visualized_network_requests}

Visualize network request data for your applications in the Mobile Analytics console.

Data is available for the following measurements:

**Round Trip Time** - defines the length of time, measured in milliseconds, that it takes for your app to make network requests.
**Request Count** - displays how often an app makes network requests. Data also displays as an average.
The charts can be filtered on app name, operating system or operating system versions.

## Reports that can be visualized: Crashes
{: #reports_visualized_crashes}

You can view information about your application crashes in the Mobile Analytics console to better monitor and troubleshoot your applications.

On the Crashes page, the **Crash Overview** table shows the following data columns:

**Application**: application name<br/>
**Crashes**: total number of crashes for that app<br/>
**Total Uses**: total number of times a user opens and closes that app<br/>
**Crash Rate**: percentage of crashes per use<br/>
You can quickly see information about your application crashes the Crashes table. The Crashes bar graph shows a histogram of crashes over time.<br/>

You can display crash data in two ways:

1.  Display crash rate: Crash rate over time
2.  Display total crashes: Total crashes over time

## Reports that can be visualized: Troubleshooting
{: #reports_visualized_troubleshooting}

The **Troubleshooting** page in the Mobile Analytics console offers a granular view of your app crashes, using the **Crash Summary** table.

The **Crash Summary** table is sortable and includes the following data columns:

* Crashes
* Devices
* Last Crash
* Application
* OS
* Message

Click the + icon next to any entry to display the **Crash Details** table, which includes the following columns:

* Time Crashed
* Application Version
* OS Version
* Device Model
* Device ID
* Download: Link to download the logs that led up to the crash

Expand any entry in the **Crash Details** table to get more details, including a stacktrace.

The data for the **Crash Summary** table is populated by querying the fatal level app logs. If your application doesn’t collect fatal application logs, no data is available.
{: note}


## Reports that can be visualized: User Feedback
{: #reports_visualized_userfeedback}

User Feedback provides for in-app feedback analysis using Mobile Analytics.
With this feature of Mobile Analytics -
* **Users and Testers** can record and send feedback and bug reports from the app, as they run and use the application.
* **App owners** get a deeper sense of the application's user experience with this context rich user feedback.
* **Developers** however, receive accurate application contexts to diagnose and fix bugs or feature gaps.

### Enabling User Feedback
{: #enable_user_feedback}

Complete the following steps to enable your mobile application to capture user feedback.

#### Instrument your app
{: #instrument_app}

* Instrument your mobile app to enter the feedback mode. Call the API `Analytics.triggerFeedbackMode();` to invoke the feedback-mode. <!--For more information, refer to the documentation [here](instrument_an_app.html)-->.
* The API can be called on any application event such as buttons, menu actions or gestures.

#### Receive user feedback
{: #receive_feedback}

* Users and testers of your app can switch over to the feedback mode by triggering the application action that is instrumented for feedback.
* From within the feedback mode, rich contextual feedback along with a screen capture can be gathered and sent to Mobile Analytics.

#### Analyze the user feedback
{: #analyze_feedback}

* Mobile Analytics receives and consolidates the rich contextual feedback sent from mobile applications.
* Log on to the Mobile Analytics console and select **User Feedback** option to view the feedback.
* An app owner can review the feedback, add comments and tag the feedback with a review status. Comments could typically be actions planned such as links to Git issues, which are created to work on the feedback or comments could be statement justifying why no action is required on the feedback.
* The review status can be used to efficiently manage feedback by categorizing them under one of the different review status options.

