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
In kubernetes, deployment helps to deploy and automate the lifecycle of a kubernetes 
pods.
Just like the kubernetes replicaSet manages pods, the deployment object manages the 
replicaSet by defining a relationship to the label and label selector.

Deployment also helps to provide an update to the replicaSet and pods 
by describing a desired state and the deployment controller changes the 
state to the actual state.

Some of the use case for the Kubernetes deployment object are below.

* Declaring a pod new state.
* Create a deployment to rollout a new replicaSet.
* Rollback to an earlier version of the deployment.
* Scaling up the workload.

Blow is an example of using a deployment to manage 3 replicaSet
of nginx pods.

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
``` 
By submitting the above manifest into the kubectl commands, will
inturn create a deployment object with 3 replicas of pods.

Running the _`kubectl get deployment`_ and _`kubectl get rs`_ will display the following information about
the deployment and the replicaSet.

```
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           18s
```
```
NAME                     DESIRED   CURRENT   READY   AGE
nginx-deployment-756123   3         3         3      18s
```
The several advantage of using a kubernetes deployment include

* `Rolling updates:` This is the rolling out of new versions applications
by scaling new pods. Using deployment helps to minimize downtime and some potential
risks.
* `Rollback:` Deployment makes it easy to rollback to the previous state version incase
an error is encountered during the update.
* `Health check:` With deployment, you can also easily check if the pods are healthy
before scaling.


**_Reference:_**

- kubernetes documentations
- Kubernetes Up & Running (O'REILLY)
