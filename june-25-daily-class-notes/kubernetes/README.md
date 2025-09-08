# Kubernetes

[https://nimbuswiztech-jun-25.gitbook.io/kubernetes](https://nimbuswiztech-jun-25.gitbook.io/kubernetes)

Kubernetes, often abbreviated as K8s, has emerged as the industry standard for container orchestration. It simplifies the deployment, scaling, and management of containerized applications, making it an essential tool for modern software development. In this beginner-friendly tutorial, we’ll explore the basics of Kubernetes, its key concepts, and how to get started with your first cluster.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/0*6oaF1wTuwtaeQV9z" alt="" height="457" width="700"><figcaption></figcaption></figure>

## Understanding Kubernetes <a href="#id-1498" id="id-1498"></a>

Kubernetes is an open-source container orchestration platform developed by Google. It automates the deployment, scaling, and management of containerized applications, allowing developers to focus on building and running their applications without worrying about the underlying infrastructure.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*RVt_XXgBzQsRM4jkb7TUkg.png" alt="" height="286" width="700"><figcaption></figcaption></figure>

A Kubernetes cluster consists of two types of resources:

* The **Control Plane** **(Master Node)** coordinates the cluster.
* **Nodes** are the workers that run applications. They serve as hosts for pads that contain the containers.

## Key Concepts <a href="#b74c" id="b74c"></a>

Before diving into Kubernetes, it’s essential to understand some key concepts:

* Pods: The smallest deployable unit in Kubernetes, consisting of one or more containers.
* Nodes: The physical or virtual machines that serves as a worker machine in a Kubernetes cluster. Each node has a **Kubelet**, which is an agent for managing the node and communicating with the Kubernetes control plane. A Kubernetes cluster that handles production traffic should have a minimum of three nodes because if one node goes down, both an [etcd](https://kubernetes.io/docs/concepts/overview/components/#etcd) member and a control plane instance are lost, and redundancy is compromised. You can mitigate this risk by adding more control plane nodes.
* Deployment: A Kubernetes resource that manages the lifecycle of pods, ensuring that the desired number of replicas are running at all times.
* Service: An abstraction that defines a set of pods and a policy for accessing them.
* Namespace: A virtual cluster within a Kubernetes cluster, used to divide cluster resources among multiple users or teams.
* **The Control Plane is responsible for managing the cluster.** The Control Plane coordinates all activities in your cluster, such as scheduling applications, maintaining applications’ desired state, scaling applications, and rolling out new updates. When you deploy applications on Kubernetes, you tell the control plane to start the application containers. The control plane schedules the containers to run on the cluster’s nodes. **Node-level components, such as the kubelet, communicate with the control plane using the** [**Kubernetes API**](https://kubernetes.io/docs/concepts/overview/kubernetes-api/), which the control plane exposes. End users can also use the Kubernetes API directly to interact with the cluster.

> Control Planes manage the cluster and the nodes that are used to host the running applications.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*Uhu1Uhn-qC5j4fhkrlR20w.png" alt="" height="465" width="700"><figcaption></figcaption></figure>

## Master Node (**Control Plane)** <a href="#a827" id="a827"></a>

The control plane in Kubernetes consists of several key components that manage and control the cluster’s state and orchestrate verious operations. These components work together to maintain the desired state of the cluster and ensure that applications are running smoothly. The main components of the Kubernetes Control Plane are:

**kube-apiserver**:

* The Kubernetes API server acts as the front-end for the Kubernetes control plane.
* It exposes the Kubernetes API, which allows users, administrators, and other components to interact with the cluster.
* All administrative operations and cluster state changes are performed through the API server.

**kube-controller-manager**:

* The kube-controller-manager is responsible for running various controller processes that regulate the state of the cluster.
* Controllers continuously monitor the cluster’s state through the API server and take corrective actions to maintain the desired state.
* Examples of controllers include the Node Controller, Replication Controller, Endpoint Controller, and Service Account & Token Controller.

**kube-scheduler**:

* The kube-scheduler is responsible for determining where to deploy new pods within the cluster.
* It evaluates various factors such as resource requirements, node health, and affinity/anti-affinity rules to make optimal scheduling decisions.
* The scheduler assigns pods to nodes based on these factors and ensures that the workload is evenly distributed across the cluster.

**etcd:**

* etcd is a distributed key-value store that serves as the cluster’s persistent storage for configuration data.
* It stores all cluster state information, including configuration settings, API objects, and metadata.
* etcd ensures consistency and reliability by providing a highly available and distributed storage solution.

These components work together to maintain the desired state of the Kubernetes cluster, handle resource allocation and scheduling, and provide a reliable and scalable platform for deploying and managing containerized applications. Each component plays a crucial role in ensuring the overall health and functionality of the Kubernetes control plane.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*alZI96ZH_4Ozh-XS1vEYlw.png" alt="" height="504" width="700"><figcaption></figcaption></figure>

## Worker Node <a href="#id-5242" id="id-5242"></a>

In Kubernetes, the node, also known as a _**worker node**_ or _**minion**_, is a physical or virtual machine that runs containerized applications. Each node in the cluster hosts one or more pods, which are the smallest deployable units in Kubernetes. Nodes are managed by the control plane and perform the actual work of running and managing containers. The main components of a Kubernetes node are:

**kubelet**:

* The kubelet is an agent that runs on each node and is responsible for managing the lifecycle of pods.
* It ensures that containers are running and healthy by starting, stopping, and monitoring them according to the pod specifications defined in the Kubernetes API.
* The kubelet communicates with the Kubernetes API server to receive pod specifications, report node status, and perform other node-related tasks.

**kube-proxy**:

* The kube-proxy is a network proxy that runs on each node and is responsible for implementing Kubernetes service abstraction.
* It maintains network rules and performs packet forwarding for accessing services running in the cluster.
* kube-proxy uses iptables or other networking mechanisms to route traffic to the appropriate pods based on service definitions.

**Container Runtime**:

* The container runtime is the software responsible for running containers on the node.
* Kubernetes supports various container runtimes, including Docker, containerd, and CRI-O.
* The container runtime pulls container images, creates container instances, and manages container lifecycle operations such as starting, stopping, and restarting containers.

**kubelet Container Logs**:

* kubelet is also responsible for collecting and forwarding container logs to the cluster’s logging infrastructure.
* Logs generated by containers running on the node are stored locally and can be accessed through tools like kubectl or retrieved by centralized logging solutions for monitoring and troubleshooting purposes.

These components work together on each node to ensure the proper execution and management of containers within the Kubernetes cluster. By distributing workloads across multiple nodes and coordinating their activities, Kubernetes provides a scalable and resilient platform for deploying and running containerized applications.

## Setting Up Your Kubernetes Cluster <a href="#id-372d" id="id-372d"></a>

There are several ways to set up a Kubernetes cluster, including using managed Kubernetes services provided by cloud providers like Google Kubernetes Engine (GKE), Amazon Elastic Kubernetes Service (EKS), or Microsoft Azure Kubernetes Service (AKS). Alternatively, you can set up a local cluster using tools like Minikube or Kind (Kubernetes in Docker).

For the purpose of this tutorial, let’s use Minikube to set up a local Kubernetes cluster:

* Install Minikube by following the instructions provided in the official documentation.
* Start Minikube using the `minikube start` command.
* Verify that your cluster is up and running by running `kubectl cluster-info`.

**Deploying Your First Application**: Now that you have a Kubernetes cluster up and running, let’s deploy a simple application:

* Create a Kubernetes deployment manifest file (`deployment.yaml`) with the following content:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world
spec:
  replicas: 3
  selector:
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
      - name: hello-world
        image: nginx:latest
        ports:
        - containerPort: 80
```

* Apply the deployment manifest using the `kubectl apply -f deployment.yaml` command.
* Verify that the deployment is running by running `kubectl get pods`.

**Accessing Your Application**: To access your deployed application, you need to expose it via a Kubernetes service:

* Create a Kubernetes service manifest file (`service.yaml`) with the following content:

```
apiVersion: v1
kind: Service
metadata:
  name: hello-world
spec:
  selector:
    app: hello-world
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort
```

* Apply the service manifest using the `kubectl apply -f service.yaml` command.
* Retrieve the URL to access your application by running `minikube service hello-world --url`.

## Useful commands: <a href="#df19" id="df19"></a>

* If the k9s Command is not found, try:

```
alias k9s=’/home/your-user-name/.local/bin/k9s’
```

* To find out which cluster you are using:

```
kubectl config get-contexts
```

* To change the cluster (let _kubectl_ interact with this cluster)

```
kubectl config use-context <cluster-name>
```

* View cluster information

```
kubectl config view — minify — flatten — context=<cluster-name>
```

* To get a list of secrets

```
kubectl get secrets -n liberty-apps
```

* To find a secret

```
kubectl get secret <secret-name> -n <namespace> -o json | jq ‘{name: .metadata.name,data: .data|map_values(@base64d)}’
```

* To create a secret

```
kubectl create secret generic <secret-name> — from-literal=secret=<value> -n <namespace>
```

* To find the failing errors

```
kubectl describe pod <podname> -n <namespace>
```

**Conclusion**: Congratulations! You’ve successfully deployed your first application on Kubernetes. This tutorial has covered the basics of Kubernetes, including its key concepts, setting up a cluster, deploying applications, and accessing them. As you continue your Kubernetes journey, explore more advanced topics such as scaling, networking, and managing stateful applications. Happy container orchestrating!
