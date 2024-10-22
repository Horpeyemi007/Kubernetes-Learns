## KUBERNETES REPLICAS, SERVICES AND DEPLOYMENT

### ReplicaSet

According to the kubernetes documentation - A ReplicaSet's purpose is to maintain a
stable set of replica Pods running at any given time.
This is another way of saying that a replicaSet is a way of ensuring or guaranteeing that
that a specific number of Pods is running.

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

Let's say i save the above manifest into a replica.yml file and submit it to the kubernetes cluster using the kubectl command, then it will create a replicaSet with the name `frontend` including also 3 set of running Pods that ot manages.

If i should delete one of the Pods, then the replicaSet will automatically bring up another Pods, as this is to ensure that the specified number if Pods is always running.

### Services

**_Reference:_**

- kubernetes documentations
- Kubernetes Up & Running (O'REILLY)
