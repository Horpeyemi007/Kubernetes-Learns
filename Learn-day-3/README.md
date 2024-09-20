## KUBERNETES NAMESPACE AND PODS

### NAMESPACE
Namespaces provide a mechanism for isolating groups of resources within a single cluster.

Names of resources need to be unique within a namespace, but not across namespaces.

Namespaces are intended for use in environments with many users spread across multiple teams, or projects.

Namespaces cannot be nested inside one another and each Kubernetes resource can only be in one namespace.

**_There are four initial namespace in a kubernetes cluster by default when created._**

**_default_**: Kubernetes includes this namespace so that you can start using your new cluster without first creating a namespace.

**_kube-node-lease_**: This namespace holds Lease objects associated with each node. Node leases allow the kubelet to send heartbeats so that the control plane can detect node failure.

**_kube-public_**: This namespace is readable by all clients (including those not authenticated). This namespace is mostly reserved for cluster usage, in case that some resources should be visible and readable publicly throughout the whole cluster.

**_kube-system_**: This namespace is for objects created by the Kubernetes system.

<br>

**_creating and viewing manespace_**

Creating a nnamespace in kubernetes can be easily done through the kubectl command as below
```
kubectl create namespace mynamespace
```

The namespace can also be viewed with the command below
```
kubectl get namespace mynamespace

This will return the following
// @TODO:
```
<br>

### PODS
Pods are the smallest deployable units of computing that you can create and manage in Kubernetes.

A Pod represents a set of running containers on your cluster with shared namespaces and shared filesystem volumes.

Before, you can run a pod - a container runtime needs to be installed into each node in the cluster.

When a Pod gets created either directly by you, or indirectly by a controller, the new Pod is scheduled to run on a Node in your cluster. Therefor the Pod remains on that node until the Pod finishes execution ,and the Pod object is deleted. A Pod is also evicted for any lack of resources, or the node fails.

A Pod is not a process, but an environment for running containers and a Pod persists until it is deleted.

<br>

**_A pod can be created by writing a YAML manifest through the declarative approach._**

The example below illustrate a basic configuration that will create a Pod which consists of a container running the image **nginx:1.14.2**

```yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

By saving the above yaml file with the name **_pod.yaml_** and run the command **_"kubectl create pod -f pod.yaml"_**.

This will create a the pod running a nginx container image.

The pod can also be viewed by running the _"kubectl get pod command"_ with the output as shown below.

```
```

### POD LIFECYCLE

Pods creation follow a defined lifecycle, starting in the Pending phase, moving through Running if at least one of its primary containers starts OK, and then through either the Succeeded or Failed phases depending on whether any container in the Pod terminated in failure.

### POD PHASE

The phase of a Pod is a simple, high-level summary of where the Pod is in its lifecycle.

Here are the possible values for phase:

**_Pending_**: The Pod has been accepted by the Kubernetes cluster, but one or more of the containers has not been set up and made ready to run. This includes time a Pod spends waiting to be scheduled as well as the time spent downloading container images over the network.

**_Running_**: The Pod has been bound to a node, and all of the containers have been created. At least one container is still running, or is in the process of starting or restarting.

**_Succeeded_**: All containers in the Pod have terminated in success, and will not be restarted.

**_Failed_**:	All containers in the Pod have terminated, and at least one container has terminated in failure. That is, the container either exited with non-zero status or was terminated by the system, and is not set for automatic restarting.

**_Unknown_**: For some reason the state of the Pod could not be obtained. This phase typically occurs due to an error in communicating with the node where the Pod should be running.


**When a pod is failing to start repeatedly, CrashLoopBackOff may appear in the Status field. Similarly.**
<br>

**_Reference_**

kubernetes documentation
