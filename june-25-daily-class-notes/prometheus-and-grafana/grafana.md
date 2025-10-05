# Grafana

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*GihnWRf7Fdx6wVX5" alt="" height="234" width="700"><figcaption></figcaption></figure>

Grafana is an open-source platform for monitoring and observability. It allows you to query, visualize, alert, and understand your metrics no matter where they are stored. Create, explore, and share dashboards with your team and foster a data-driven culture. This article will outline how to install Grafana on your own. If you’re looking to try it out without much setup, or if you’re looking for industrial-level service, start our [free trial](https://www.hostedgraphite.com/accounts/signup-metricfire/) and get your dashboards up and running in minutes.

### But what do I use it for? <a href="#id-039d" id="id-039d"></a>

Grafana helps you visualize metrics, send alerts and understand the metrics from various data sources with its plugin architecture. Let’s look at a few use cases. You can …

1. Visualize the machine’s CPU, memory, network, and other resource utilization with Prometheus and Grafana.
2. Visualize and alert for a high number of incoming web requests in your web app.
3. Monitor the health of the components of your platform like APIs, frontend apps, databases, etc.

And many more!

### Get Grafana up and running <a href="#id-7bb8" id="id-7bb8"></a>

Grafana can be [installed on various platforms](https://www.metricfire.com/hosted-grafana) using native packages. Here we will install it using [Docker](https://docker.com/). Put GF\_SECURITY\_ADMIN\_PASSWORD as the admin password you want to set at the time of installation (Don’t worry you can change it after the installation). Set GF\_SERVER\_ROOT\_URL appropriately depending upon your reverse proxy or ingress configuration. A full configuration example can be found [here](https://grafana.com/docs/installation/configuration/).

```
docker run \
-d \
-p 3000:3000 \
--name=grafana \
-e "GF_SERVER_ROOT_URL=http://localhost:3000" \
-e "GF_SECURITY_ADMIN_PASSWORD=secret" \
grafana/grafana
```

For simplicity, we are setting it to localhost. After running the command above you can open http://localhost:3000 in your browser and log in with the username admin and password set above.

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*RjCQIEqkOeyl2FiD" alt="" height="356" width="700"><figcaption></figcaption></figure>

### Understanding Grafana <a href="#bfda" id="bfda"></a>

Now that we have things up and running, let’s understand the basic concepts of Grafana.

#### Data Sources <a href="#fafd" id="fafd"></a>

This is the most fundamental and important component responsible for pulling the data out of third parties like Elasticsearch, Cloudwatch, Graphite, InfluxDB, etc.

#### Dashboards and Widgets <a href="#id-9aff" id="id-9aff"></a>

A widget is a simple visualization panel for a single set of multiple metrics. There are many widgets like graphs, logs lists, tables, heatmaps, singlestat, and much more available through plugins.

A dashboard is a group of widgets, but it provides a lot more features like folders, variables (for changing visualizations throughout widgets), time ranges, and auto refresh of widgets.

#### Alerts <a href="#id-107a" id="id-107a"></a>

Grafana supports alerting on individual widgets based on rules defined by the user. Alerts can be sent to different channels, including Microsoft Teams, Slack, email, Webhooks, and PagerDuty.

#### Plugins <a href="#id-9c2a" id="id-9c2a"></a>

Plugins can be used to extend Grafana’s existing functionality or add new data sources and widget types, which makes Grafana really extensible. Check out the [official plugin repository](https://grafana.com/grafana/plugins) and instructions on [how to install](https://grafana.com/docs/plugins/installation/).

#### Users <a href="#id-6a57" id="id-6a57"></a>

Grafana provides user management with user permissions like editor, viewer, or admin.

### Creating your first dashboard on Grafana <a href="#fabd" id="fabd"></a>

Now that we know a bit about Grafana let’s create a dashboard on Grafana. For simplicity of not setting up more infrastructure, we will use [TestData](https://grafana.com/docs/features/datasources/testdata/) data source from Grafana.

Go to the plus icon on the left side of the homepage and create a dashboard with the name Test Data Dashboard. Add a new panel and add a query.

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*yGpraZXP2-8munbo" alt="" height="352" width="700"><figcaption></figcaption></figure>

Click on the query icon on the left and add a default source (which is Grafana), it will add a random walk graph.

To customize the visualization click on the visualization icon and select graph type. There are several other options like line width, color, stacking, etc. There are other types of visualizations such as Tables, Single Stats, Gauge, etc.

Save the graph. It should look something like below.

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*xZITPqaRIhBvr-v2" alt="" height="357" width="700"><figcaption></figcaption></figure>

### Setting up alerts <a href="#id-1b71" id="id-1b71"></a>

Visualizing metrics is really useful but nobody will be able to sit at a computer watching a dashboard 24/7!!! Alerts help to inform about critical metrics such as high memory usage.

Let’s set up a Slack alert on the dashboard we just built. Click on Alerting on the homepage and go to notification channels and add a new notification channel for Slack, it requires credentials, a slack channel name, and a username.

PS: Test Data Source of Grafana does not support alerting.

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*CXXl5L06-Wk25OwW" alt="" height="351" width="700"><figcaption></figcaption></figure>

After setting up an alert destination we will go to the widget’s alert icon to setup the alerts.

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*6_Jb7yIAAzkwjHkw" alt="" height="264" width="700"><figcaption></figcaption></figure>

We will set up the condition, i.e. if the average value in a 1 min time range is greater than 10 it will fire the alert. We also select where these alerts should be sent to, in this case, it’s being sent to the Metricfire slack that we just added. There is a text field available to add any text that is related to these alerts, which can be added as a template.

### Where to go from here <a href="#id-5e61" id="id-5e61"></a>

Congratulations!!! You have your first monitoring dashboard with Alerting set up. Now, there are lots of things we can explore further about Grafana!

#### Production setup <a href="#id-3e4c" id="id-3e4c"></a>

General production setup is done with either k8s or docker swarm for high availability since this is critical to monitoring.

A k8s deployment can be done via the [prometheus operator](https://github.com/coreos/prometheus-operator)’s [helm chart](https://github.com/helm/charts/tree/master/stable/prometheus-operator). This deploys a complete prometheus + Grafana stack with pre-built k8s monitoring dashboards and alerts.

#### Common Patterns <a href="#id-8a49" id="id-8a49"></a>

Grafana + Grahite = ❤️. This is the most common combination for metrics reporting and alerting. Grafana is a great visualization tool but it doesn’t do data persistence, so Graphite is generally used to collect metrics and Grafana operates on Graphite.

#### Plugins <a href="#id-83f1" id="id-83f1"></a>

Plugins are one of the most powerful features of Grafana. It has a huge repository of community-built data sources and widgets which extends Grafana in a great way.

#### Administration <a href="#id-03f9" id="id-03f9"></a>

Grafana provides an [HTTP API](https://grafana.com/docs/http_api/) to get the dashboards in JSON format. These dashboards can be created again by supplying the payload stored as backup. These dashboards can also be imported through Grafana web UI. Many third-party pre-built dashboards for tools like Elasticsearch, Airflow, and k8s are available which can be imported this way as well.

Get to know our [Grafana as a Service](https://www.metricfire.com/hosted-grafana) better, and check out how MetricFire can fit into your monitoring environment. Get a [free trial](https://www.hostedgraphite.com/accounts/signup-metricfire/) and start making Grafana dashboards now. Feel free to [book a demo](https://metricfire.com/demo) if you have questions about what Grafana can do for you.

### Try MetricFire now! <a href="#id-7a1d" id="id-7a1d"></a>

Get MetricFire free for 14 days. No credit card is required.
