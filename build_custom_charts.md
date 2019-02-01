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

# Build Custom Charts
{: #build_custom_charts}

Custom charts allow you to visualize the collected analytics data in your analytics data store as charts that aren’t available by default in the MobileFirst Analytics Console. This visualization feature is a powerful way to analyze business-critical data.

Available custom chart types are **App Session**, **Network Transactions**, **Push Notifications**, **Client Logs**, **Server Logs** and **Custom Data**.

## Creating a custom chart
{: #creating_custom_chart}

Create a custom chart using the following steps:

1.  Click the **Create Chart** button in the **Custom Charts** tab from the Mobile Analytics dashboard.
2.  In the **General Settings** tab, select **Chart Title**, **Event Type** and the **Chart Type**.
3.  On selecting the *Event Type* and *Chart Type*, the **Chart Definition** tab appears. Use the *Chart Definition* tab to define the chart for the specified chart type that you previously selected. After you define the chart, you can set the chart filters and chart properties.
4.  **Chart Filters** are used to fine-tune the custom chart. Many filters can be defined for a chart.
    For example, if you’re interested in seeing the average app session duration for a particular app, you can specify the following options:
    * Select **Application Name** for **Property**.
    * Select **Equals** for **Operator**.
    * Select the name of your app for **Value**.
    * Click **Add Filter**.
    The app name filter is added to the table of filters for your chart.
5.  Chart properties are available for the **Table**, **Bar Graph**, and **Line Graph** chart types. The goal of chart properties is to enhance how the data is presented so that the visualization is more effective.
    If you created a **Table** chart, the chart properties are set to define the table page size, the field on which to sort, and the sort order of the field.
    If you created a **Bar Graph** or **Line Graph** chart, the chart properties are set to label threshold lines to add a frame of reference for anyone who is monitoring the chart.

## Creating a custom chart for client logs
{: #creating_custom_chart_for_client_logs}    

You can create a custom chart for client logs that consist of log information, which is sent with the platform’s Logger API.
The log information also includes contextual information about the device, including environment, app name, and app version.

1.  You must log custom events to populate custom charts. Use the following API methods to create custom events depending on the application OS.
    * **JavaScript (Cordova)**
      ```Javascript
      WL.Analytics.log({"key" : 'value'});
      ```
    * **JavaScript (Web)**
      ```Javascript
      ibmmfpfanalytics.addEvent({'Purchases':'radio'});
      ibmmfpfanalytics.addEvent({'src':'App landing page','target':'About page'});
      ```
    * **Android**
      ```Java
      JSONObject json = new JSONObject();
      try {
          json.put("key", "value");
      } catch (JSONException e) {
          // TODO Auto-generated catch block
          e.printStackTrace();
      }

      WLAnalytics.log("Message", json);
      ```
    * **iOS (Objective-C)**
      ```objectivec
      NSDictionary *inventory = @{@"property" : @"value",};

      [[WLAnalytics sharedInstance] log:@"Custom event" withMetadata:inventory];
      [[WLAnalytics sharedInstance] send];
      ```
    * **iOS (Swift)**
      ```swift
      let metadata: [NSObject: AnyObject] = ["foo": "bar"];  
      WLAnalytics.sharedInstance().log("hello", withMetadata: metadata);
      ```
2. From the client app, populate the data by sending captured logs to the server. See Sending captured logs.

3. In the MobileFirst Analytics Console, click the **Custom Charts** tab and continue to create a chart:
   **Chart Title**: Application and Log Levels
   **Event Type**: Client Logs
   **Chart Type**: Flow Chart

4. Click the **Chart Definition** tab and provide the following values:
   **Source**: Application Name
   **Destination**: Log Level
   **Property**: Your app name

5. Click the **Save** button.

## Export, import or download a custom chart
{: #exp_imp_download_custom_chart}   

You can export and import custom chart definitions in the Analytics Console. If you’re moving from a test environment to a production deployment, you can save time by exporting your custom chart definitions instead of re-creating your custom charts.

* Click the **Download Chart** icon to download a file in JSON format from the Analytics Console.
* Click the **Custom Charts** tab in the Analytics Console dashboard.
* Click **Export Charts** to download a JSON file with your chart definition.
* Choose a location to save the JSON file.
* Click **Import Charts** to import your JSON file. If you import a custom chart definition that exists, you create duplicate definitions, which also means that the Analytics Console shows duplicate custom charts.

## Types of charts
{: #types_of_charts}

### Bar graph
{:  #bar_graph}

The bar graph allows for visualization of numeric data over an X-axis. When you define a bar graph, you must choose the value for X-Axis first. You can choose from the following possible values.

* **Timeline** - if you want to see your data as a trend (for example, average app session duration over time), choose *Timeline* for X-Axis .
* **Property** - choose Property if you want to see a count breakdown for the specific property. If you choose Property for X-Axis, then Total is implicitly chosen for Y-Axis. For example, choose Property for X-Axis and Application Name for Property to see a count for a specified event type, which is broken down by app name.

After you define a value for X-Axis, you can define a value for Y-Axis. If you choose *Timeline* for X-Axis, you can choose the following possible values for Y-Axis.

* **Average** - averages a numeric property in the supplied event type.
* **Total** - a total count of a property in the supplied event type.
* **Unique** - a unique count of a property in the supplied event type.
After you define the chart axes, you must choose a value for Property.

### Line graph
{:  #line_graph}

The line graph allows for the visualization of some metric over time. This type of chart is valuable when you want to visualize data trending over time. The first value to define when you create a line graph is Measure, which has the following possible values.

* **Average** - averages a numeric property in the supplied event type.
* **Total** - a total count of a property in the supplied event type.
* **Unique** - a unique count of a property in the supplied event type.
After you define the measurement, you must choose a value for Property.

### Flow chart
{:  #flow_chart}

The flow chart allows for the visualization of flow breakdown of one property to another. For a flow chart, the following properties must be set.

* **Source** - the value of a source node in the diagram.
* **Destination** - the value of the destination node in the diagram.
* **Property** - a property value from either the source node or the destination node.
With the flow chart, you can see the density breakdown of various sources that flow to a destination, or the other way around. For example, if you want to see the breakdown of log severities for an app, you can define the following values.

* Select Application Name for Source.
* Select Log Level for Destination.
* Select the name of your app for Property.

### Metric group
{:  #metric_group}

The metric group can be used to visualize a single metric that is measured as either an average value, a total count, or a unique count. To define a metric group, you must define one of the following possible values for Measure.

* **Average** - averages a numeric property in the supplied event type.
* **Total** - a total count of a property in the supplied event type.
* **Unique** - a unique count of a property in the supplied event type.
After you define the measurement, you must choose a value for Property. This metric is displayed in the metric group.

### Pie chart
{:  #pie_chart}

The pie chart can be used to visualize the count breakdown of values for a particular property. For example, if you want to see a crash breakdown, define the following values.

* Select App Session for Event Type.
* Select Pie Chart for Chart Type.
* Select Closed By for Property.
The resulting pie chart shows the breakdown of app sessions that were closed by the user instead of app sessions, which were closed by a crash.

### Table
{:  #table}

The table is useful when you want to see the raw data. Building a table is as simple as adding columns for the raw data that you want to see.
Since not all properties are required for specific event types, null values can appear in your table. If you want to prevent these rows from appearing in your table, add an Exists filter for a specific property in the **Chart Filters** tab

