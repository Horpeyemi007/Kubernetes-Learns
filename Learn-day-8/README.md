## KUBERNETES REPLICAS, SERVICES AND DEPLOYMENT

### ReplicaSet

According to the kubernetes documentation - A ReplicaSet's purpose is to maintain a
stable set of replica Pods running at any given time.
This is another way of saying that a replicaSet is a way of ensuring or guaranteeing that a specific number of Pods is running.

To define a replicaSet, a selector field on how to identify a particular Pods to acquire
is always specified. Also All Pods acquired by a ReplicaSet have their owning ReplicaSet’s identifying information within their ownerReferences field.

Below is an example of a replicaSet managing a Pod.

```yml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: frontend
  labels:
    app: guestbook
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: nginx
          image: nginx:1.14.2
          ports:
            - name: http
              containerPort: 80
```

Let's say i save the above manifest into a replica.yml file and submit it to the kubernetes cluster using the kubectl command, then it will create a replicaSet with the name _`frontend`_ including also 3 set of running Pods that ot manages.

If i should delete one of the Pods, then the replicaSet will automatically bring up another Pods, as this is to ensure that the specified number if Pods is always running.

### Services

* A Service is a method for exposing a network application that is running as one or more Pods in your cluster.
i.e with the kubernetes service, you can expose a group of pods over a network. 

* Those pods are determined by a selector field that is been defined in the service object.

* The kubernetes service will now make those pods available on the network so that the 
client will be able to interact with it.

* You should use a kubernetes service for exposing pods over network because 
kubernetes pods are ephemeral resources (an individual pod can be destroyed and recreated 
if it's not healthy - meaning that a pod is not durable and reliable).

* A set of pod running an application in one moment could be different from the set of 
pods running in another moment and each pods gets its own ip address which 
is subject to changes when a particular pod is recreated.

* The client will need to interact with the application and because the ip address is 
not static due to its dynamic changes, then a service which will acts 
as an endpoint is been used.

* The service ip address is static and doesn't changes even if the pod ip address changes.

#### Service type in kubernetes
The various kubernetes service types include

* `clusterIP`: This is the default service type that is been used when creating 
a service object, even when you don't specify a type. It's used for communicating 
internally. i.e It provides internal communication access only.

* `NodePort`: This type will expose the service on each node to an allocated 
static port. The node port enables you to connect outside the cluster through 
the allocated port using the appropriate cluster. 

* `LoadBalancer`: This type expose the service externally through an external 
LoadBalancer from the cloud provider that is been integrated with your kubernetes cluster. This 
approach also provides external communication.

* `ExternalName`: This type maps the service to an external name field like a DNS Name (api.example.com).

Below is an example of a service of type *`NodePort`* that is exposed on a pod 
with an nginx running container.

```yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: myapp
spec:
  containers:
    - name: nginx
      image: nginx:1.14.2
      ports:
        - name: http-port
          containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: app-svc
spec:
  selector:
    app: myapp
  ports:
  - port: 80 # Port exposed within the cluster
    protocol: TCP
    nodePort: 30001 # Port accessible externally on each node
    targetPort: http-port # Port on the pods
  type: NodePort
```

### Deployment



**_Reference:_**

- kubernetes documentations
- Kubernetes Up & Running (O'REILLY)
