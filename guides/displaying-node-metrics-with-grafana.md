# Displaying Node Metrics with Grafana
## What you need before beginning Grafana setup

https://docs.docker.com/get-docker/
https://docs.docker.com/compose/ (included in Docker for Desktop for macOS and Windows)
https://git-scm.com/

## Setting up

This guide uses a sample application from the grafana fundamentals tutorial. In this step supporting services such as Prometheus and Loki will also be set up.
To complete the exercises in this guide, you need to download the files to your local machine

Clone the github.com/grafana/tutorial-environment repository.

```bash
git clone https://github.com/grafana/tutorial-environment.git
```

Now change to the directory where you cloned this repository

```bash
cd tutorial-environment
```

Make sure Docker is running

```bash
docker ps
```
No errors means it is running. If you get an error, then start Docker and then run the command again.

Start the sample application:

```bash
docker-compose up -d
```

The first time you run docker-compose up -d, Docker downloads all the necessary resources for the tutorial. This might take a few minutes, depending on your internet connection.

{% hint style="info" %}
If you already have Grafana, Loki, or Prometheus running on your system, then you might see errors because the Docker image is trying to use ports that your local installations are already using. Stop the services, then run the command again.
{% endhint %}


Ensure all services are running:

```bash
docker-compose ps
```

In the State column, it should say Up for all services.

Browse to the sample application on localhost:8081.

## Grafana News

The sample application, Grafana News, lets you post links and vote for the ones you like. To add a link: In Title, enter {Example}. In URL, enter https://example.com. Then Click Submit to add the link.

## Log in to Grafana
Grafana is an open-source platform for monitoring and observability that lets you visualize and explore the state of your systems. Open a new tab. Browse to localhost:3000. Enter the username and password as admin, then log in. The first time you log in, it asks you to change your password. Remember to click Save. The first thing you see is the Home dashboard, which helps you get started. To the far left you can see the sidebar, a set of quick access icons for navigating Grafana.

## Add a metrics data source

The sample application exposes metrics which are stored in Prometheus. To be able to visualize the metrics from Prometheus, you first need to add it as a data source in Grafana. In the side bar, hover your cursor over the Configuration (gear) icon, and then click Data Sources and then click add data source. In the list of data sources, select Prometheus. In the URL box, enter http://prometheus:9090, then save. Prometheus should now available as a data source in Grafana.

## Exploring your metrics

Grafana Explore is a workflow for troubleshooting and data exploration. In this step, you’ll be using Explore to create ad-hoc queries to understand the metrics exposed by the sample application. Ad-hoc queries are queries that are made interactively, with the purpose of exploring data. An ad-hoc query is commonly followed by another, more specific query. In the side bar, click the Explore (compass rose) icon. 
In the Query editor, where it says Enter a PromQL query, enter

```bash
tns_request_duration_seconds_count
```

and then press Shift + Enter. A graph appears. In the top right corner, click the dropdown arrow on the Run Query button, and then select 5s. Grafana runs your query and updates the graph every 5 seconds. You just made your first PromQL query! PromQL is a powerful query language that lets you select and aggregate time series data stored in Prometheus.

tns_request_duration_seconds_count is a counter, a type of metric whose value only ever increases. Rather than visualizing the actual value, you can use counters to calculate the rate of change, i.e. how fast the value increases.

Add the rate function to your query to visualize the rate of requests per second. Enter the following in the Query editor and then press Shift + Enter.

```bash
rate(tns_request_duration_seconds_count[5m])
```

Immediately below the graph there’s an area where each time series is listed with a colored icon next to it. This area is called the legend. PromQL lets you group the time series by their labels, using the sum function.

Add the sum function to your query to group time series by route:

```bash
sum(rate(tns_request_duration_seconds_count[5m])) by(route)
```

Go back to the sample application and generate some traffic by adding new links, voting, or just refresh the browser. In the upper right corner, click the time picker, and select Last 5 minutes. By zooming in on the last few minutes, it’s easier to see when you receive new data, Depending on your use case, you might want to group on other labels. Try grouping by other labels, such as status_code, by changing the by(route) part of the query.

## Add a logging data source

Grafana supports log data sources, like Loki. Just like for metrics, you first need to add your data source to Grafana.

Just like before,
In the side bar, click the configuration icon, Data Sources, add, but this time click Loki. In the URL box, enter http://loki:3100.
Click Save & Test to save your changes.
And now Loki is now available as a data source in Grafana.

## Explore your logs

Grafana Explore not only lets you make ad-hoc queries for metrics, but lets you explore your logs as well. In the side bar, click the Explore (compass) icon. In the data source list at the top, select the Loki data source.

In the Query editor, enter:

```bash
{filename="/var/log/tns-app.log"}
```

Grafana displays all logs within the log file of the sample application. The height of each bar encodes the number of logs that were generated at that time. Click and drag across the bars in the graph to filter logs based on time. Not only does Loki let you filter logs based on labels, but on specific occurrences. Let’s generate an error, and analyze it with Explore. In the sample application, post a new link without a URL to generate an error in your browser that says empty url.

Go back to Grafana and enter the following query to filter log lines based on a substring:

```bash
{filename="/var/log/tns-app.log"} |= "error"
```

Click on the log line that says level=error msg="empty url" to see more information about the error. Logs are helpful for understanding what went wrong. Later in this guide, you’ll see how you can correlate logs with metrics from Prometheus to better understand the context of the error.

## Build a dashboard
A dashboard gives you an at-a-glance view of your data and lets you track metrics through different visualizations. Dashboards consist of panels, each representing a part of the story you want your dashboard to tell. Every panel consists of a query and a visualization. The query defines what data you want to display, whereas the visualization defines how the data is displayed. In the side bar, hover your cursor over the Create (plus sign) icon and then click Dashboard. Click Add new panel.

In the Query editor below the graph, enter the query from earlier and then press Shift + Enter:

```bash
sum(rate(tns_request_duration_seconds_count[5m])) by(route)
```

In the Legend field, enter {{route}} to rename the time series in the legend. The graph legend updates when you click outside the field. In the Panel editor on the right, under Settings, change the panel title to “Traffic”. Click Apply in the top-right corner to save the panel and go back to the dashboard view. Click the Save dashboard (disk) icon at the top of the dashboard to save your dashboard. Enter a name in the New name field and then click Save.

## Annotate events
When things go bad, it often helps if you understand the context in which the failure occurred. Time of last deploy, system changes, or database migration can offer insight into what might have caused an outage. Annotations allow you to represent such events directly on your graphs. In the next part of the tutorial, we will simulate some common use cases that someone would add annotations for. To manually add an annotation, click anywhere in your graph, then click Add annotation. In Description, enter Migrated user database. Click Save. Grafana adds your annotation to the graph. Hover your mouse over the base of the annotation to read the text. Grafana also lets you annotate a time interval, with region annotations.

Add a region annotation

Press Ctrl (or Cmd on macOS), then click and drag across the graph to select an area. In Description, enter Performed load tests. In Tags, enter testing.
Manually annotating your dashboard is fine for those single events. For regularly occurring events, such as deploying a new release, Grafana supports querying annotations from one of your data sources. Let’s create an annotation using the Loki data source we added earlier. At the top of the dashboard, click the Dashboard settings (gear) icon. Go to Annotations and click Add Annotation Query. In Name, enter Errors. In Data source, select Loki.

In Query, enter the following query:

```bash
{filename="/var/log/tns-app.log"} |= "error"
```

Click Add. Grafana displays the Annotations list, with your new annotation. Click the Go back arrow to return to your dashboard. The log lines returned by your query are now displayed as annotations in the graph. Being able to combine data from multiple data sources in one graph allows you to correlate information from both Prometheus and Loki.

## Set up an alert
Alerts allow you to identify problems in your system moments after they occur. By quickly identifying unintended changes in your system, you can minimize disruptions to your services. Alerts consists of two parts:

Notification channel - How the alert is delivered. When the conditions of an alert rule are met, the Grafana notifies the channels configured for that alert.
Alert rules - When the alert is triggered. Alert rules are defined by one or more conditions that are regularly evaluated by Grafana.

## Configure a notification channel
In this step, you’ll send alerts using web hooks. To test your alerts, you first need to have a place to send them.

Browse to requestbin.com. Under the Create Request Bin button, click the public bin link. You request bin is now waiting for the first request. Copy the endpoint URL. Next, configure a notification channel for web hooks to send notifications to your Request Bin.

In the side bar, hover your cursor over the Alerting (bell) icon and then click Notification channels.
Click Add channel.
In Name, enter RequestBin.
In Type, select webhook.
In Url, paste the endpoint to your request bin.
Click Send Test to send a test alert to your request bin.
Navigate back to the request bin you created earlier. On the left side, there’s now a POST / entry. Click it to see what information is sent by Grafana.
Click Save.

## Configure an alert rule
Now that Grafana knows how to notify you, it’s time to set up an alert rule:
In the sidebar, click Dashboards -> Manage.

Click the dashboard you created earlier. Click the Traffic panel title and then click Edit. Click the Alert tab under the graph to access the settings for alerting. Click Create Alert. In Name, enter My alert. In Evaluate every, enter 5s. For the purpose of this tutorial, the evaluation interval is intentionally short to make it easier to test. In For, enter 0m. This setting makes Grafana wait until an alert has fired for a given time, before Grafana sends the notification. In the Conditions section, you can change the white text by clicking it and then choosing a new option or typing text in the blank field.

Change the alert condition to:
WHEN last() OF query(A, 1m, now) IS ABOVE 0.2

This condition evaluates whether the last value of any of the time series from one minute back is more than 0.2 requests per second. In the Notifications section, click the plus sign (+) next to Send to and then click RequestBin. Click the Go back arrow and then click the Save dashboard icon. Enter a note to describe your changes. Something like, Added My alert. Notes like this are optional, but can be useful when trying to understand changes made to dashboards over time.

You now have an alert set up to send a notification using a web hook. See if you can trigger it by generating some traffic on the sample application. Browse to localhost:8081. Refresh the page repeatedly to generate traffic. When Grafana triggers the alert, it sends a request to the web hook you set up earlier. Browse to the Request Bin you created earlier, to inspect the alert notification.

## Pause an alert
Once you’ve acknowledged an alert, consider pausing it. This can be useful to avoid sending subsequent alerts, while you work on a fix. In the Grafana side bar, hover your cursor over the Alerting icon and then click Alert Rules. All the alert rules configured so far are listed, along with their current state.
Find your alert in the list, and click the Pause icon on the right. The Pause icon turns into a Play icon.
Click the Play icon to resume evaluation of your alert.
Clean up the local environment
The guide has running Docker containers. When you want to clean up this local tutorial environment, run

```bash
docker-compose down -v
```

## Summary
In this guide you learned about fundamental features of Grafana.


## 
help from: https://grafana.com/tutorials/grafana-fundamentals/
