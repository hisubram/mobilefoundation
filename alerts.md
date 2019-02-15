---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-23"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Set up alerts
{: #setting_up_alerts}

Mobile Analytics provides the alerts feature, which allows you to define alerts to monitor your applications. You can configure thresholds, which if exceeded, trigger alerts to notify the Mobile Analytics console monitor. The triggered alerts can be visualized on the console, or the alerts can be handled by a custom webhook. This feature provides the means for detecting application log errors and application crashes from server log errors. Reactive thresholds and alerts keep you from having to sift through your logs data.

## Creating an alert definition for application logs
{: #creating_alert_def}

You can create an alert definition that is based on application logs.

In this example, you use application's log data to create an alert definition. Alert condition is checked in all application logs, which were received in the last 5 minutes, and continues to check every 5 minutes, until the alert definition is disabled or deleted. An alert is triggered for each device that sends three or more application error logs with the same application name and version.

1.  In the Mobile Analytics console, click **Definitions** to go to the alert definitions page.
2.  Click **Create Alert**.
3.  Provide the following values:
    * **Alert Name**: *Alert for app logs*
    * **Message**: *Error Message Alert*
    * **Query Frequency**: *5 minutes*
    * **Event Type**: *App Logs*
        * **Property**: *Log Level*
            * **Value**: *Error*
            * **Threshold**: *Total for Application Instance*<br/>
              If you choose the *Average for Application* option, the app logs are averaged by the number of devices. For example, if you have two devices and one device sends six app logs while the other device sends three app logs, the average is 4.5 app logs.
              {: note}
            * **Operator**: *is greater than or equals* 
            * Threshold value: *3*
4.  Click **Next** and provide the following value:
    * **Method**: *Analytics Console Only*<br/>
      Choose the *Analytics Console and Network Post* option, if you want to additionally send a POST message with a JSON payload to your customized URL. The following fields are available if you choose this option.
      * **Network Post URL**
      * **Headers**
      * **Authentication Type**
      * **POST Request Body**
5. Click **Save**.  

You've now created an alert definition to trigger an alert at the end of every 5 minute interval when the number of app logs reached the threshold of 3 or more error logs.

## Creating an alert definition for application crashes
{: #creating_alert_crashes}

You can create an alert definition to monitor application crashes.
In this example, you use application crash data to create an alert definition. This alert monitors all application crashes in the last 2 minutes, and continues to check every 2 minutes, until the alert definition is disabled or deleted. An alert is triggered for every application that crashed 5 or more times. 

1.  In the Mobile Analytics console, click **Definitions** to go to the alert definitions page.
2.  Click **Create Alert**.
3.  Provide the following values:
    * **Alert Name**: *Alert for Application Crashes*
    * **Message**: *App Crash Alert*
    * **Query Frequency**: *2 minutes*
    * **Event Type**: *Application Crashes*
        * **Application Name**: *Any application*
        * **Any application**: *Any version*
    * **Threshold**: *Crash Count*
    * **Operator**: *is greater than or equals* 
    * Threshold value: *5*
4.  Click **Distribution Method** or **Next** and provide the following value:
    * **Method**: *Analytics Console Only*<br/>
      Choose the *Analytics Console and Network Post* option, if you want to additionally send a POST message with a JSON payload to your customized URL. The following fields are available if you choose this option.
      * **Network Post URL**
      * **Headers**
      * **Authentication Type**
      * **POST Request Body**
5. Click **Save**.  

## Managing alert definitions
{: #managing_alert_def}

You manage your alert definitions from the alert **Definitions** page.

For each alert definition you can do the following operations:
* Toggle the check-box under the **Enabled** column to enable or disable a specific alert definition.
* If you want to create a copy of an alert definition and change some values, click the duplicate icon .
* Click the pencil icon, if you want to edit an alert definition.
* Click the trash icon, if you want to delete an alert definition.

## Viewing alerts
{: #viewing_alerts}

You can view the alerts, which were triggered from the alert **Logs** page.

1.  In the Mobile Analytics console, click **Logs**.
2.  Click the **+** icon for any of the alerts. This action displays the alert definition and the **Alert Instances** section.
    If the corresponding alert definition wasn’t deleted or modified, you can edit the alert definition by clicking **Edit Alert**. Otherwise, the **Edit Alert** button is unavailable and the following message displays:
    `This alert definition has since been modified or deleted.`
3.  You can optionally select an alert and click the trash icon to delete the alert.

