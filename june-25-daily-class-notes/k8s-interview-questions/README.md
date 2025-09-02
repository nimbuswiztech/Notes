# K8s interview questions

scenario-based Kubernetes interview questions.&#x20;

***

### Cluster Operations & Troubleshooting

1. How would you replace a failed node in a Kubernetes cluster without disrupting running applications?
2. A Pod is stuck in "Pending" state. Walk me through the troubleshooting steps you’d take.
3. Several services suddenly become unavailable. How do you identify and fix the root cause?
4. What diagnostics would you run if a critical production service is not reachable but all Pods show healthy?
5. How do you debug unexpected resource exhaustion in a Kubernetes cluster?
6. After a deployment, Pods keep crashing in a CrashLoopBackOff state. How do you investigate and resolve this?
7. Your deployment works in dev but crashes in prod after an image update. What steps do you take?
8. If Pod logs aren’t providing clues about failures, what are your next actions?
9. How do you handle a memory leak in one of your microservices running as a Kubernetes Pod?
10. What would you do if Pod scheduling fails despite having sufficient reported resources?

***

### Deployments, Updates & Rollbacks

11. Explain how you’d implement a rolling update with zero downtime.
12. If a new deployment fails, how do you rollback safely via Kubernetes commands?
13. Describe a blue/green deployment strategy with Kubernetes.
14. How would you perform a canary deployment using Kubernetes native resources?
15. What parameters would you tune when increasing the replica count during a surge of users?

***

### Networking and Services

16. A Service is configured, but traffic isn’t reaching Pods. What could be wrong?
17. How do you debug network latency between two Pods in different namespaces?
18. Explain troubleshooting steps for failed DNS resolution in your cluster.
19. How do you expose a Kubernetes service externally with security and scalability as priorities?
20. When would you use ClusterIP, NodePort, and LoadBalancer services?

***

### Scaling & Performance

21. Your cluster needs to scale rapidly during peak hours. How do you set up autoscaling?
22. How do you optimize resource requests and limits for different workloads?
23. What metrics would you monitor to ensure high performance in production?
24. Describe a scenario where horizontal pod autoscaler would be preferable to vertical pod autoscaler.
25. How would you proactively prevent resource starvation in multi-tenant environments?

***

### Configuration & Secrets Management

26. How do you update a running Pod’s configuration without downtime?
27. Describe steps to securely manage secrets and configuration files in Kubernetes.
28. How do you rotate secrets for a production application running in Kubernetes?
29. Explain how ConfigMaps and Secrets differ—when would you use each?

***

### Storage & Volumes

30. A Pod loses access to its persistent data after restart. How do you debug?
31. How would you safely migrate data for a stateful workload within Kubernetes?
32. Describe how you’d set up backups for persistent volumes in the cluster.

***

### Security & Policies

33. How do you enforce network isolation between different applications or tenants?
34. What is RBAC and how would you use it in a production cluster?
35. How would you audit access and API requests in a Kubernetes cluster?
36. Steps to detect and recover from a compromised node in your cluster.

***

### CI/CD & Automation

37. How would you automate the deployment pipeline for an app on Kubernetes?
38. Which CI/CD tools integrate best with Kubernetes from your experience?
39. How can you roll back an application automatically if health checks fail post-deployment?
40. Walk through the process of building, testing, and deploying a container image to Kubernetes.

***

### Cloud Integrations (GCP, AWS, Azure)

41. How do you set up cluster auto-scaling in AWS EKS?
42. Explain the process for enabling workload identity in Google Kubernetes Engine.
43. How do you leverage Azure Container Registry within AKS deployments?
44. What steps enable cross-cloud deployments for hybrid workloads?

***

### Advanced Real-Time Scenarios

45. Describe handling a root certificate change that impacts all cluster workloads.
46. If etcd starts reporting high latency, how do you investigate and fix it?
47. Explain how you’d create a custom Helm chart for a microservices deployment.
48. How do you migrate from self-hosted Kubernetes to a managed cloud provider with minimal downtime?
49. Steps for disaster recovery in a Kubernetes environment.
50. How do you ensure compliance and audit requirements are met in regulated industries like finance or healthcare?
