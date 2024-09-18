## Kubernetes control-plane and nodes

Kubernetes runs your workload by placing containers into Pods to run on Nodes. A node may be a virtual or physical machine, depending on the cluster. Each node is managed by the control plane and contains the services necessary to run Pods.

A node is also referred to as a worker machine.

The components on a node include the kubelet, a container runtime, and the kube-proxy.

A Node can have multiple pods, and the Kubernetes control plane automatically handles scheduling the pods across the Nodes in the cluster. The control plane's automatic scheduling takes into account the available resources on each Node.

Every Kubernetes Node runs at least:

Definition of terms.

- kubelet: An agent that runs on each node in the cluster. It makes sure that containers are running in a Pod - and its also responsible for communication between the Kubernetes control plane and the Node; it manages the Pods and the containers running on a machine.
- control-plane: Also, referred to as the master, is the container orchestration layer that exposes the API and interfaces to define, deploy, and manage the lifecycle of containers. It acts as the brain of the cluster, making worldwide decisions about the cluster.
- container runtime: A component that empowers Kubernetes to run containers effectively. It is responsible for pulling the container image from a registry, unpacking the container, and running the application. examples: docker, containerd, cri-o

<br><br>
@ reference: kubernetes documentation
