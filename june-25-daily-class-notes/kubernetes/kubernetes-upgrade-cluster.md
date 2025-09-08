# Kubernetes upgrade cluster

**Steps to perform Kubernetes upgrade cluster**

The brief steps involved (in the provided order) to perform Kubernetes upgrade are:

**Controller node**

1. Update kubeadm
2. Drain the controller node (Evict any Kubernetes resources and disable scheduling)
3. Generate upgrade plan
4. Perform Kubernetes upgrade
5. Update kubectl and kubelet
6. Un-cordon the controller node (Enable scheduling)
7. Verify

**Worker node(s)** - Complete these steps on one worker node at a time, don't execute these steps in-parallel on multiple worker nodes)

1. Update kubeadm
2. Drain the respective node (Evict any Kubernetes resources and disable scheduling)
3. Perform Kubernetes upgrade
4. Update kubectl and kubelet
5. Un-cordon the respective worker node (Enable scheduling)
6. Verify
