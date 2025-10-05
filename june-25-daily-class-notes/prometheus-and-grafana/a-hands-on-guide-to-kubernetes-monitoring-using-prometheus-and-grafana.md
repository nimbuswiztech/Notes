# ⎈ A Hands-On Guide to Kubernetes Monitoring Using Prometheus & Grafana🛠️

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*qBwb4cI9dTYInvlo2oD3LA.png" alt="" height="394" width="700"><figcaption><p>Promethes &#x26; Grafana Architecture by Anvesh Muppeda</p></figcaption></figure>

### Introduction <a href="#efa3" id="efa3"></a>

Inthe dynamic world of containerized applications and microservices, monitoring is indispensable for maintaining the health, performance, and reliability of your infrastructure. Kubernetes, with its ability to orchestrate containers at scale, introduces new challenges and complexities in monitoring. This is where tools like `Prometheus` and `Grafana` come into play.

**Prometheus** is an open-source systems monitoring and alerting toolkit originally built at SoundCloud. It excels at monitoring metrics and providing powerful query capabilities against time-series data. Meanwhile, **Grafana** complements Prometheus by offering visualization capabilities through customizable dashboards and graphs.

In this blog post, we will guide you through the process of setting up `Prometheus` and `Grafana` on a Kubernetes cluster using `Helm`. By the end of this tutorial, you will have a robust monitoring solution that allows you to:

* Collect and store metrics from your Kubernetes cluster and applications.
* Visualize these metrics through intuitive dashboards.
* Set up alerts based on predefined thresholds or anomalies.
* Gain insights into the performance and resource utilization of your cluster.

Whether you are deploying your first Kubernetes cluster or looking to enhance your existing monitoring setup, understanding how to leverage Prometheus and Grafana effectively is essential. Let’s dive into the step-by-step process of deploying and configuring these powerful tools on Kubernetes.

### Prerequisites <a href="#id-61b7" id="id-61b7"></a>

Before we get started, ensure you have the following:

* A running `Kubernetes` cluster.
* `kubectl` command-line tool configured to communicate with your cluster.
* `Helm` (the package manager for Kubernetes) installed.

### Setting up Prometheus and Grafana <a href="#id-8fb6" id="id-8fb6"></a>

### Step 1: Adding the Helm Repository <a href="#id-87ec" id="id-87ec"></a>

First, add the Prometheus community Helm repository and update it:

```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

### Step 2: Installing Prometheus and Grafana <a href="#id-3ede" id="id-3ede"></a>

Create a `custom-values.yaml` file to customize the Helm chart installation. This file will configure Prometheus and Grafana to be exposed via NodePorts.

```
# custom-values.yaml
prometheus:
  service:
    type: NodePort
grafana:
  service:
    type: NodePort
```

Then, install the `kube-prometheus-stack` using Helm:

```
helm upgrade --install kube-prometheus-stack prometheus-community/kube-prometheus-stack -f custom-values.yaml
```

Output:

```
$ helm upgrade --install kube-prometheus-stack prometheus-community/kube-prometheus-stack -f custom-values.yaml
Release "kube-prometheus-stack" does not exist. Installing it now.
NAME: kube-prometheus-stack
LAST DEPLOYED: Sun Jun 16 17:04:53 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
kube-prometheus-stack has been installed. Check its status by running:
  kubectl --namespace default get pods -l "release=kube-prometheus-stack"

Visit https://github.com/prometheus-operator/kube-prometheus for instructions on how to create & configure Alertmanager and Prometheus instances using the Operator.
```

### Step 3: Verifying the Installation <a href="#id-70eb" id="id-70eb"></a>

After the installation, you can verify that the Prometheus and Grafana services are created and exposed on NodePorts:

```
kubectl get services
```

You should see output similar to this, showing the services with their respective NodePorts:

```
$ kubectl get services                                   
NAME                                             TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                         AGE
alertmanager-operated                            ClusterIP   None             <none>        9093/TCP,9094/TCP,9094/UDP      5m19s
kube-prometheus-stack-alertmanager               ClusterIP   10.245.239.151   <none>        9093/TCP,8080/TCP               5m22s
kube-prometheus-stack-grafana                    NodePort    10.245.30.17     <none>        80:31519/TCP                    5m22s
kube-prometheus-stack-kube-state-metrics         ClusterIP   10.245.26.205    <none>        8080/TCP                        5m22s
kube-prometheus-stack-operator                   ClusterIP   10.245.19.171    <none>        443/TCP                         5m22s
kube-prometheus-stack-prometheus                 NodePort    10.245.151.164   <none>        9090:30090/TCP,8080:32295/TCP   5m22s
kube-prometheus-stack-prometheus-node-exporter   ClusterIP   10.245.22.30     <none>        9100/TCP                        5m22s
kubernetes                                       ClusterIP   10.245.0.1       <none>        443/TCP                         57d
prometheus-operated                              ClusterIP   None             <none>        9090/TCP                        5m19s
```

### Step 4: Accessing Prometheus and Grafana <a href="#af5b" id="af5b"></a>

To access Prometheus and Grafana dashboards outside the cluster, you need the external IP of any node in the cluster and the NodePorts on which the services are exposed.

Get the external IP addresses of your nodes:

```
kubectl get nodes -o wide
```

You should see output similar to this:

```
$ kubectl get nodes -o wide
NAME                   STATUS   ROLES    AGE   VERSION   INTERNAL-IP   EXTERNAL-IP      OS-IMAGE                         KERNEL-VERSION   CONTAINER-RUNTIME
pool-t5ss0fagn-jeb47   Ready    <none>   57d   v1.29.1   10.124.0.2    146.190.55.222   Debian GNU/Linux 12 (bookworm)   6.1.0-17-amd64   containerd://1.6.28
```

Use the external IP of any node and the NodePorts to access the dashboards:

* Prometheus: [`http://`](http://203.0.113.10:30001/)`146.190.55.222`[`:`](http://203.0.113.10:30001/)`30090`
* Grafana: [`http://`](http://203.0.113.10:30000/)`146.190.55.222`[`:`](http://203.0.113.10:30000/)`31519`

### Access Prometheus <a href="#db8b" id="db8b"></a>

Use the below link as above to access the Prometheus UI

http://\<PUBLIC-IP>:\<PROMETHEUS-PORT>

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*R3ca_81vYwKw7TwPZ8r8hQ.png" alt="" height="233" width="700"><figcaption><p>Prometheus UI</p></figcaption></figure>

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*2XXerU14IYKQRKHrhX4YTw.png" alt="" height="376" width="700"><figcaption><p>Prometheus Alerts</p></figcaption></figure>

### Access Grafana Default Dashboards <a href="#c4f4" id="c4f4"></a>

Use the below link as above to access the Grafana UI

`http://<PUBLIC-IP>:<GRAFANA-PORT>`

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*18PvG1qqeAm7jWJfFpQriQ.png" alt="" height="375" width="700"><figcaption><p>Grafana UI</p></figcaption></figure>

Use the below command to get the Grafana Admin login:

**Username**:

```
$ kubectl get secret --namespace default kube-prometheus-stack-grafana -o jsonpath="{.data.admin-user}" | base64 --decode ; echo
admin
```

**Password**:

```
$ kubectl get secret --namespace default kube-prometheus-stack-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
prom-operator
```

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*LIjIY90mSjAmJMEEp1V-hQ.png" alt="" height="370" width="700"><figcaption><p>Grafana Dashboard</p></figcaption></figure>

#### Default Dashboards <a href="#id-5b4e" id="id-5b4e"></a>

By default our previous setup will add few dashboards:

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*cKCai1ZcxUtmlfY4Lu1lTg.png" alt="" height="370" width="700"><figcaption></figcaption></figure>

Using these dashboards we can easily monitor our kubernetes cluster

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*SYT9FAU-gS60vruIsR2LRw.png" alt="" height="372" width="700"><figcaption><p>Pod Dashboard example</p></figcaption></figure>

### Add/Create new Dashboards <a href="#id-56f1" id="id-56f1"></a>

We also have the flexibility to create our own dashboards from scratch or import multiple Grafana dashboards from the Grafana library.

To import a Grafana dashboard, follow these steps:

**Step 1:** Access the Grafana [library](https://grafana.com/grafana/dashboards/).

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*c2Vr2RczDmIgi_mmk-FBXQ.png" alt="" height="334" width="700"><figcaption><p>Grafana Library</p></figcaption></figure>

**Step 2.** Select the desired dashboard ID to add.

Considering `K8s/Storage/Volumes/Namespace` Dashboard

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*k30lk4D1zxHahz8jPFREfA.png" alt="" height="335" width="700"><figcaption><p><code>K8s/Storage/Volumes/Namespace</code> Dashboard</p></figcaption></figure>

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*3c0uiZbGO0YJBz1xlXInkQ.png" alt="" height="278" width="700"><figcaption><p><code>K8s/Storage/Volumes/Namespace</code> Dashboard ID</p></figcaption></figure>

Copy the Id of `K8s/Storage/Volumes/Namespace` Dashboard i.e., `11455`

**Step 3: Import selected Dashboard in Grafana**

Access Dashboard section & click on Import section.

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*1weJlzDJ0tLHGXc1gCQrBg.png" alt="" height="377" width="700"><figcaption></figcaption></figure>

Now enter the ID of the target new Dashboard i.e., 11455.

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*T4v2tvP-JDPyHsbyiTGU6g.png" alt="" height="376" width="700"><figcaption></figcaption></figure>

Click on Load to load the new dashboard into Grafana.

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*XQkB3ltfYk1o5KQPsBgikQ.png" alt="" height="375" width="700"><figcaption></figcaption></figure>

Click on Import to import the new Dashboard & Access it.

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*te-oE82i8ZvnBfoKTXY5YA.png" alt="" height="344" width="700"><figcaption></figcaption></figure>

These steps allow us to easily integrate any dashboard from the Grafana library.

### Prometheus Architecture <a href="#e73c" id="e73c"></a>

Prometheus is a powerful monitoring and alerting toolkit designed for reliability and scalability. Understanding its architecture helps in leveraging its full potential. The Prometheus architecture comprises several key components:

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*0vXfW4gPKFcwC2alH2qMyA.gif" alt="" height="394" width="700"><figcaption><p>Animated Promethes &#x26; Grafana Architecture by Anvesh Muppeda</p></figcaption></figure>

### Prometheus Server <a href="#id-0571" id="id-0571"></a>

The Prometheus server is the core component responsible for:

1. **Data Scraping**: Prometheus periodically scrapes metrics from configured targets, which are typically HTTP endpoints exposing metrics in a specified format.
2. **Data Storage**: It stores all scraped samples locally using a time series database. Prometheus is designed to be efficient with both storage and retrieval of time series data.
3. **Querying**: Prometheus allows you to query the time series data via the Prometheus Query Language (PromQL), which enables complex aggregations and calculations.

### Prometheus Components <a href="#id-5a98" id="id-5a98"></a>

1. **Prometheus Server**: The main component that does the bulk of the work, including scraping metrics from targets, storing the data, and providing a powerful query interface.
2. **Pushgateway**: An intermediary service used for pushing metrics from short-lived jobs that cannot be scraped directly by Prometheus. This is particularly useful for batch jobs and other processes with a finite lifespan.
3. **Exporters**: Exporters are used to expose metrics from third-party systems as Prometheus metrics. For example, Node Exporter collects hardware and OS metrics from a node, while other exporters exist for databases, web servers, and more.
4. **Alertmanager**: This component handles alerts generated by the Prometheus server. It can deduplicate, group, and route alerts to various receivers such as email, Slack, PagerDuty, or other notification systems.
5. **Service Discovery**: Prometheus supports various service discovery mechanisms to automatically find targets to scrape. This includes static configuration, DNS-based service discovery, and integrations with cloud providers and orchestration systems like Kubernetes.
6. **PromQL**: The powerful query language used by Prometheus to retrieve and manipulate time series data. PromQL supports a wide range of operations such as arithmetic, aggregation, and filtering.

### Data Flow in Prometheus <a href="#id-2d54" id="id-2d54"></a>

1. **Scraping Metrics**: Prometheus scrapes metrics from HTTP endpoints (targets) at regular intervals. These targets can be predefined or discovered dynamically through service discovery.
2. **Storing Metrics**: Scraped metrics are stored as time series data, identified by a metric name and a set of key-value pairs (labels).
3. **Querying Metrics**: Users can query the stored metrics using PromQL. Queries can be executed via the Prometheus web UI, HTTP API, or integrated with Grafana for visualization.
4. **Alerting**: Based on predefined rules, Prometheus can evaluate metrics data and trigger alerts. These alerts are sent to Alertmanager, which then processes and routes them to the appropriate notification channels.

### Example of Prometheus Workflow <a href="#id-82e3" id="id-82e3"></a>

1. **Service Discovery**: Prometheus discovers targets to scrape metrics from using service discovery mechanisms. For example, in a Kubernetes environment, it discovers pods, services, and nodes.
2. **Scraping**: Prometheus scrapes metrics from discovered targets at defined intervals. Each target is an endpoint exposing metrics in a format Prometheus understands (typically plain text).
3. **Storing**: Scraped metrics are stored in Prometheus’s time series database, indexed by the metric name and labels.
4. **Querying**: Users can query the data using PromQL for analysis, visualization, or alerting purposes.
5. **Alerting**: When certain conditions are met (defined by alerting rules), Prometheus generates alerts and sends them to Alertmanager.
6. **Alertmanager**: Alertmanager processes the alerts, deduplicates them, groups them if necessary, and sends notifications to configured receivers.

### Understanding the Kubernetes Objects <a href="#eb45" id="eb45"></a>

The Helm chart deploys various Kubernetes objects to set up Prometheus and Grafana.

```
$ kubectl get all                                         
NAME                                                            READY   STATUS    RESTARTS   AGE
pod/alertmanager-kube-prometheus-stack-alertmanager-0           2/2     Running   0          38m
pod/kube-prometheus-stack-grafana-76858ff8dd-76bn4              3/3     Running   0          38m
pod/kube-prometheus-stack-kube-state-metrics-84958579f9-g44sk   1/1     Running   0          38m
pod/kube-prometheus-stack-operator-554b777575-hgm8b             1/1     Running   0          38m
pod/kube-prometheus-stack-prometheus-node-exporter-cl98x        1/1     Running   0          38m
pod/prometheus-kube-prometheus-stack-prometheus-0               2/2     Running   0          38m

NAME                                                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                         AGE
service/alertmanager-operated                            ClusterIP   None             <none>        9093/TCP,9094/TCP,9094/UDP      38m
service/kube-prometheus-stack-alertmanager               ClusterIP   10.245.239.151   <none>        9093/TCP,8080/TCP               38m
service/kube-prometheus-stack-grafana                    NodePort    10.245.30.17     <none>        80:31519/TCP                    38m
service/kube-prometheus-stack-kube-state-metrics         ClusterIP   10.245.26.205    <none>        8080/TCP                        38m
service/kube-prometheus-stack-operator                   ClusterIP   10.245.19.171    <none>        443/TCP                         38m
service/kube-prometheus-stack-prometheus                 NodePort    10.245.151.164   <none>        9090:30090/TCP,8080:32295/TCP   38m
service/kube-prometheus-stack-prometheus-node-exporter   ClusterIP   10.245.22.30     <none>        9100/TCP                        38m
service/kubernetes                                       ClusterIP   10.245.0.1       <none>        443/TCP                         57d
service/prometheus-operated                              ClusterIP   None             <none>        9090/TCP                        38m

NAME                                                            DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
daemonset.apps/kube-prometheus-stack-prometheus-node-exporter   1         1         1       1            1           kubernetes.io/os=linux   38m

NAME                                                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/kube-prometheus-stack-grafana              1/1     1            1           38m
deployment.apps/kube-prometheus-stack-kube-state-metrics   1/1     1            1           38m
deployment.apps/kube-prometheus-stack-operator             1/1     1            1           38m

NAME                                                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/kube-prometheus-stack-grafana-76858ff8dd              1         1         1       38m
replicaset.apps/kube-prometheus-stack-kube-state-metrics-84958579f9   1         1         1       38m
replicaset.apps/kube-prometheus-stack-operator-554b777575             1         1         1       38m

NAME                                                               READY   AGE
statefulset.apps/alertmanager-kube-prometheus-stack-alertmanager   1/1     38m
statefulset.apps/prometheus-kube-prometheus-stack-prometheus       1/1     38m
```

Here’s a brief explanation of each type of object used:

#### **Deployments:** <a href="#id-349a" id="id-349a"></a>

Deployments ensure that a specified number of pod replicas are running at any given time. They manage the creation, update, and deletion of pods. In this setup, Deployments are used for:

* **Grafana:** Manages the Grafana instance, ensuring it is always available.
* **Kube-State-Metrics:** Exposes Kubernetes cluster-level metrics.

Example:

```
deployment.apps/kube-prometheus-stack-grafana              1/1     1            1           38m
deployment.apps/kube-prometheus-stack-kube-state-metrics   1/1     1            1           38m
```

#### **StatefulSets:** <a href="#id-7027" id="id-7027"></a>

StatefulSets are used for managing stateful applications that require persistent storage and stable network identities. They ensure that the pods are deployed in a specific order and have unique, stable identifiers. In this setup, StatefulSets are used for:

* **Prometheus:** Ensures Prometheus instances have persistent storage for metric data.
* **Alertmanager:** Manages the Alertmanager instances.

Example:

```
statefulset.apps/alertmanager-kube-prometheus-stack-alertmanager   1/1     38m
statefulset.apps/prometheus-kube-prometheus-stack-prometheus       1/1     38m
```

#### **DaemonSets:** <a href="#id-0393" id="id-0393"></a>

DaemonSets ensure that a copy of a pod is running on all (or some) nodes in the cluster. They are commonly used for logging and monitoring agents. In this setup, DaemonSets are used for:

* **Node Exporter:** Collects hardware and OS metrics from the nodes.

Example:

```
daemonset.apps/kube-prometheus-stack-prometheus-node-exporter   1/1     38m
```

### Cleanup Section <a href="#ca59" id="ca59"></a>

Use the below command to uninstall the prometheus stack

```
$ helm uninstall kube-prometheus-stack
```

### Advantages of Using Prometheus and Grafana <a href="#id-3439" id="id-3439"></a>

Using Prometheus and Grafana together provides a powerful and flexible monitoring solution for Kubernetes clusters. Here are some of the key advantages:

### Prometheus <a href="#id-3d60" id="id-3d60"></a>

1. **Open Source and Community-Driven**: Prometheus is a widely adopted open-source monitoring solution with a large community, ensuring continuous improvements, support, and a plethora of plugins and integrations.
2. **Dimensional Data Model**: Prometheus uses a multi-dimensional data model with time series data identified by metric name and key/value pairs. This makes it highly flexible and powerful for querying.
3. **Powerful Query Language (PromQL)**: Prometheus Query Language (PromQL) allows for complex queries and aggregations, making it easy to extract meaningful insights from the collected metrics.
4. **Efficient Storage**: Prometheus has an efficient storage engine designed for high performance and scalability. It uses a local time series database, making it fast and reliable.
5. **Alerting**: Prometheus has a built-in alerting system that allows you to define alerting rules based on metrics. Alerts can be sent to various receivers like email, Slack, or custom webhooks using the Alertmanager component.
6. **Service Discovery**: Prometheus supports multiple service discovery mechanisms, including Kubernetes, which makes it easy to dynamically discover and monitor new services as they are deployed.

### Grafana <a href="#id-2c4a" id="id-2c4a"></a>

1. **Rich Visualization**: Grafana provides a wide range of visualization options, including graphs, charts, histograms, and heatmaps, allowing you to create comprehensive dashboards.
2. **Customizable Dashboards**: Grafana dashboards are highly customizable, enabling you to create tailored views that meet the specific needs of your team or organization.
3. **Integration with Multiple Data Sources**: While Grafana works seamlessly with Prometheus, it also supports many other data sources such as Elasticsearch, InfluxDB, and Graphite, making it a versatile tool for centralized monitoring.
4. **Alerting**: Grafana offers its own alerting system, allowing you to set up alert rules on dashboard panels and receive notifications via multiple channels, such as email, Slack, and PagerDuty.
5. **Templating**: Grafana allows the use of template variables in dashboards, making them reusable and more interactive. This feature helps in creating dynamic and flexible dashboards.
6. **User Management and Sharing**: Grafana supports user authentication and role-based access control, making it easier to manage access to dashboards. Dashboards can also be easily shared with team members or embedded in other applications.
7. **Plugins and Extensions**: Grafana has a rich ecosystem of plugins for different data sources, panels, and apps, allowing you to extend its functionality to meet your specific monitoring needs.

### Combined Benefits <a href="#id-3951" id="id-3951"></a>

1. **Comprehensive Monitoring Solution**: Together, Prometheus and Grafana provide a complete monitoring solution, from metrics collection and storage (Prometheus) to powerful visualization and analysis (Grafana).
2. **Scalability**: Both Prometheus and Grafana are designed to scale with your infrastructure. Prometheus can handle millions of time series, while Grafana can manage numerous dashboards and data sources.
3. **Real-Time Monitoring and Alerting**: With Prometheus’s real-time metrics collection and Grafana’s real-time visualization, you can monitor your infrastructure’s health continuously and get alerted to issues promptly.
4. **Ease of Use**: Setting up Prometheus and Grafana is straightforward, especially with tools like Helm for Kubernetes, making it easy to deploy and manage the monitoring stack.
5. **Extensibility**: Both tools are highly extensible, allowing you to integrate them with other systems and customize them to fit your specific requirements.

By leveraging the strengths of Prometheus and Grafana, you can ensure that your Kubernetes environment is well-monitored, making it easier to maintain performance, reliability, and efficiency.

### Conclusion <a href="#b8c8" id="b8c8"></a>

Setting up Prometheus and Grafana on Kubernetes using Helm is straightforward and provides a powerful monitoring solution for your cluster. By exposing the services via NodePorts, you can easily access the dashboards from outside the cluster. This setup allows you to monitor your cluster’s performance, visualize metrics, and set up alerts to ensure your applications run smoothly.

### Source Code <a href="#d44a" id="d44a"></a>

You’re invited to explore our [GitHub repository](https://github.com/anveshmuppeda/kubernetes), which houses a comprehensive collection of source code for Kubernetes.

[GitHub - anveshmuppeda/kubernetes: Kuberntes Complete NotesKuberntes Complete Notes. Contribute to anveshmuppeda/kubernetes development by creating an account on GitHub.github.com](https://github.com/anveshmuppeda/kubernetes?source=post_page-----b0e00b1ae039---------------------------------------)

Also, if we welcome your feedback and suggestions! If you encounter any issues or have ideas for improvements, please open an issue on our [GitHub repository](https://github.com/anveshmuppeda/kubernetes/issues). 🚀
