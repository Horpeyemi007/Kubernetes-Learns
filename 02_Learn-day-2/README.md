## Kubernetes control-plane and nodes

Kubernetes runs your workload by placing containers into Pods to run on Nodes. A node may be a virtual or physical machine, depending on the cluster. Each node is managed by the control plane and contains the services necessary to run Pods.

A node is also referred to as a worker machine.

The components on a node include the kubelet, a container runtime, and the kube-proxy.

A Node can have multiple pods, and the Kubernetes control plane automatically handles scheduling the pods across the Nodes in the cluster. The control plane's automatic scheduling takes into account the available resources on each Node.

**Definition of terms**.

- kubelet: An agent that runs on each node in the cluster. It makes sure that containers are running in a Pod - and its also responsible for communication between the Kubernetes control plane and the Node; it manages the Pods and the containers running on a machine.
- control-plane: Also, referred to as the master, is the container orchestration layer that exposes the API and interfaces to define, deploy, and manage the lifecycle of containers. It acts as the brain of the cluster, making worldwide decisions about the cluster.
- container runtime: A component that empowers Kubernetes to run containers effectively. It is responsible for pulling the container image from a registry, unpacking the container, and running the application. examples: docker, containerd, cri-o
- kube-proxy: Is a network proxy that runs on each node in your cluster, implementing part of the Kubernetes Service concept.
- cluster: A set of worker machines, called nodes, that run containerized applications. Every cluster has at least one worker node. The worker node(s) host the Pods that are the components of the application workload. The control plane manages the worker nodes and the Pods in the cluster.

_Below is an example of a kubernetes cluster with one control-plane and a worker node_

Using the kubectl command, we can interact with the kubernetes cluster and check the node by calling the command below.

**_kubectl get nodes_** - This will display the control plane and all worker nodes.

```
NAME                  STATUS   ROLES           AGE     VERSION
i-01514af2516230a14   Ready    node            3m47s   v1.30.2
i-0d08e5c9f05896685   Ready    control-plane   6m41s   v1.30.2
```

<br>

**_Reference_**

kubernetes documentation
