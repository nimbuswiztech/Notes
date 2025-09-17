# Part 5

**Kubernetes Real-Time Scenario-Based Interview Questions and Answers**

***

#### **1. Introduction and Kubernetes Architecture**

**Q1:** Your company is planning to migrate from a monolithic application to microservices using Kubernetes. How would you design the architecture for scalability and high availability?

**A1:** I would design the architecture with multiple nodes to ensure high availability. I’d use:

* **Master Node:** Runs the control plane components (API Server, Controller Manager, Scheduler, and etcd).
* **Worker Nodes:** Run application workloads managed by Kubelet and Kube-proxy.
* **Ingress Controller:** For external traffic management.
* **Horizontal Pod Autoscaler (HPA):** To handle dynamic scaling.
* **Pod Disruption Budgets (PDBs):** To ensure a minimum number of pods remain active.

***

#### **2. Installation of Kubernetes**

**Q2:** You need to set up a Kubernetes cluster on AWS EC2 manually. What steps would you follow?

**A2:**

* Provision EC2 instances with necessary configurations.
* Install Docker, kubeadm, kubelet, and kubectl.
* Initialize the master node with `kubeadm init`.
* Configure `kubectl` to use the new cluster.
* Join worker nodes using `kubeadm join`.
* Deploy a networking solution like Calico or Flannel.
* Verify cluster setup using `kubectl get nodes`.

***

#### **3. Steps to Create EKS Cluster**

**Q3:** Your organization is migrating to AWS EKS. How would you set up an EKS cluster?

**A3:**

* Install AWS CLI and `eksctl`.
* Create an IAM role with required permissions.
* Run `eksctl create cluster --name my-cluster --region us-east-1`.
* Configure `kubectl` to use the cluster.
* Deploy worker nodes and attach them to the cluster.
* Set up security groups and IAM roles for worker nodes.

***

#### **4. Basic Objects in Kubernetes**

**Q4:** How do you ensure a pod always runs in Kubernetes?

**A4:** Use a **Deployment** with a **ReplicaSet** ensuring at least one replica is always running.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx
```

***

#### **5. Pod Lifecycle & Troubleshooting**

**Q5:** A pod is stuck in `CrashLoopBackOff`. How do you troubleshoot it?

**A5:**

* Check logs: `kubectl logs <pod-name>`.
* Describe the pod: `kubectl describe pod <pod-name>`.
* Check if the container is failing due to incorrect commands or missing dependencies.
* Adjust `livenessProbe` and `readinessProbe` settings.
* Restart the pod or update the deployment.

***

#### **6. Services in Kubernetes**

**Q6:** How does a **ClusterIP** service differ from a **NodePort** service?

**A6:**

* **ClusterIP:** Default service type; exposes pods only within the cluster.
* **NodePort:** Exposes service on a static port (30000-32767) accessible from outside.

***

#### **7. Deployment Strategies**

**Q7:** Your application must support zero-downtime deployments. Which deployment strategy would you use?

**A7:** **Rolling Update** ensures a gradual rollout of new pods, replacing old ones.

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1
    maxUnavailable: 0
```

For a faster rollback option, **Blue/Green Deployment** is recommended.

***

#### **8. Liveness and Readiness Probes**

**Q8:** How would you ensure a pod restarts if it becomes unresponsive?

**A8:** Use **Liveness Probe**:

```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 10
```

***

#### **9. ConfigMaps & Secrets**

**Q9:** How would you inject environment variables securely into a pod?

**A9:** Use **Secrets**:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  username: dXNlcm5hbWU=
  password: cGFzc3dvcmQ=
```

Mount it in a pod:

```yaml
env:
  - name: DB_USER
    valueFrom:
      secretKeyRef:
        name: my-secret
        key: username
```

***

#### **10. Container Patterns**

**Q10:** What is a **Sidecar** container, and when would you use it?

**A10:** A Sidecar container runs alongside the main application container to provide additional functionality, like logging or proxying requests.

```yaml
containers:
  - name: main-app
    image: my-app
  - name: logging-sidecar
    image: fluentd
```

***

#### **Q11: How do you implement a Blue/Green deployment in Kubernetes?**

**Answer:**

A **Blue/Green deployment** in Kubernetes ensures zero-downtime releases by running two identical environments:

* **Blue**: Represents the current live version.
* **Green**: Represents the new version that is being tested.

**Steps to Implement:**

1. **Create Two Deployments:**
   * Deploy the **Blue** version of the application first.
   * Deploy the **Green** version separately with the updated code.
2. **Use a Kubernetes Service for Traffic Routing:**
   * A **Kubernetes Service** (e.g., LoadBalancer or ClusterIP) points to the Blue deployment initially.
   * After testing the Green deployment, update the service to route traffic to the Green deployment.
3. **Testing the Green Deployment:**
   * Use internal testing or separate traffic routing to validate the Green deployment.
4. **Switch Traffic:**
   * Once confirmed, update the Service selector to route traffic to the Green deployment.
5. **Rollback (if needed):**
   * If issues arise, revert the service back to the Blue deployment.

**Example YAML for Switching Traffic:**

```yaml
yamlCopyEditapiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  selector:
    app: my-app-green # Change this from "my-app-blue" to switch traffic
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```

***

#### **Q12: How does Horizontal Pod Autoscaler (HPA) work in Kubernetes?**

**Answer:**

**Horizontal Pod Autoscaler (HPA)** automatically scales the number of pods based on CPU/memory utilization or custom metrics.

**How HPA Works:**

1. **Defines Scaling Rules:**
   * You set a target metric (e.g., CPU at 50%).
2. **Metrics Collection:**
   * Kubernetes collects resource usage via Metrics Server or external sources.
3. **Scaling Decision:**
   * If resource usage exceeds the threshold, more pods are created.
   * If usage drops, unnecessary pods are removed.

**Example HPA YAML:**

```yaml
yamlCopyEditapiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: my-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

***

#### **Q13: What happens if a node fails in Kubernetes?**

**Answer:**

If a Kubernetes node fails:

1. **Pods on the failed node become unavailable.**
2. **Kubelet stops sending heartbeats, and the node is marked "NotReady."**
3. **The Kubernetes Scheduler reschedules the affected pods to healthy nodes.**
4. **If using a persistent volume, Kubernetes tries to reattach the volume on a new node.**
5. **Cluster Autoscaler may add new nodes if needed.**

**Monitoring:** Use `kubectl get nodes` to check node status.

***

#### **Q14: How do you handle persistent storage in Kubernetes?**

**Answer:**

Persistent storage in Kubernetes is managed using **PersistentVolumes (PVs) and PersistentVolumeClaims (PVCs).**

1. **Define a PersistentVolume (PV):**
   * Specifies storage details (e.g., AWS EBS, NFS, Azure Disk).
2. **Create a PersistentVolumeClaim (PVC):**
   * Applications use PVCs to request storage dynamically.
3. **Mount the PVC to a Pod:**
   * The pod automatically gets access to the requested storage.

**Example PersistentVolume & PVC YAML:**

```yaml
yamlCopyEditapiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: standard
  hostPath:
    path: "/mnt/data"
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

***

#### **Q15: What is the purpose of a readiness probe?**

**Answer:**

A **readiness probe** ensures traffic is sent only to pods that are ready to serve requests.

1. **Kubernetes checks the readiness probe on startup.**
2. **If the probe fails, the pod is not added to the Service load balancer.**
3. **If the probe passes, the pod starts receiving traffic.**

**Example Readiness Probe in Deployment YAML:**

```yaml
yamlCopyEditreadinessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 10
```

***

#### **Q16: How do you perform Canary deployments in Kubernetes?**

**Answer:**

A **Canary deployment** gradually introduces a new version of an application while monitoring its impact.

1. **Deploy the stable version (v1).**
2. **Deploy a small number of new version pods (v2) alongside v1.**
3. **Use a service with weight-based routing (e.g., Istio, Nginx, Traefik).**
4. **Gradually increase traffic to v2.**
5. **If successful, roll out v2 completely. If not, rollback to v1.**

***

#### **Q17: How does Kubernetes RBAC work?**

**Answer:**

RBAC (**Role-Based Access Control**) controls user permissions in Kubernetes.

1. **Roles & RoleBindings:** Apply permissions within a namespace.
2. **ClusterRoles & ClusterRoleBindings:** Apply permissions across all namespaces.
3. **Permissions are granted based on verbs (`get`, `list`, `update`, `delete`).**

**Example RBAC YAML:**

```yaml
yamlCopyEditapiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list"]
```

***

#### **Q18: What is the difference between EBS and EFS in Kubernetes?**

**Answer:**

| Feature     | EBS (Elastic Block Store) | EFS (Elastic File System)  |
| ----------- | ------------------------- | -------------------------- |
| Type        | Block storage             | Shared file system         |
| Access      | Single node only          | Multiple nodes             |
| Use Case    | Databases, logs           | Shared storage across pods |
| Scalability | Fixed size                | Auto-scalable              |
| Performance | High IOPS                 | Moderate performance       |

***

#### **Q19: How do you monitor a Kubernetes cluster?**

**Answer:**

Monitoring a Kubernetes cluster can be done using:

1. **Prometheus:** Collects metrics from pods and nodes.
2. **Grafana:** Visualizes Prometheus data.
3. **Fluentd + Elasticsearch + Kibana (EFK):** Log aggregation and analysis.
4. **AWS CloudWatch:** Monitors EKS clusters.

**Example Prometheus Deployment:**

```yaml
yamlCopyEditapiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: my-app-monitor
spec:
  selector:
    matchLabels:
      app: my-app
  endpoints:
  - port: metrics
```

***

#### **Q20: How do you enforce network policies in Kubernetes?**

**Answer:**

**Network Policies** restrict pod-to-pod communication based on rules.

**Example Network Policy (Allow only frontend to talk to backend):**

```yaml
yamlCopyEditapiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend-to-backend
spec:
  podSelector:
    matchLabels:
      role: backend
  ingress:
  - from:
    - podSelector:
        matchLabels:
          role: frontend
    ports:
    - protocol: TCP
      port: 80
```

This prevents unauthorized pods from accessing backend services.
