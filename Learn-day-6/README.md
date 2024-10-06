## LABEL

In kubernetes, **Label** are key/value pairs that can be attached to a kubernetes objects like pods or replicaSet. They are used to specify identity attribute to an object and also organize of select a subset of an object.

A kubernetes object like a single pod, can contains sets of labels in the form if key/value pairs - but each key must be unique. i.e Each key in a given object must be unique.

**_Below is an example of a label in the metadata section of a kubernetes object._**

```yml
metadata:
  name: myapp
  labels:
    app: javaapp
    env: dev
```

So, I will create three pods all running a nginx app, one with different labels and use the **_kubectl_** command to view the pod that has a label attached to it.

```yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx1
  labels:
    app: javaapp
    env: prod
spec:
  containers:
    - name: nginx
      image: nginx:1.14.2
      ports:
        - containerPort: 80

---
apiVersion: v1
kind: Pod
metadata:
  name: nginx2
  labels:
    app: javaapp
    env: dev
spec:
  containers:
    - name: nginx
      image: nginx:1.14.2
      ports:
        - containerPort: 80

---
apiVersion: v1
kind: Pod
metadata:
  name: nginx3
  labels:
    app: nodeapp
    env: dev
spec:
  containers:
    - name: nginx
      image: nginx:1.14.2
      ports:
        - containerPort: 80
```

by running the command **_kubectl get pod --show-labels --selector='app=javaapp'_** will display all the running pods with their corresponding labels.

```yml
NAME     READY   STATUS    RESTARTS   AGE   LABELS
nginx1   1/1     Running   0          9s    app=javaapp,env=prod
nginx2   1/1     Running   0          9s    app=javaapp,env=dev
nginx3   1/1     Running   0          9s    app=nodeapp,env=dev
```

In other to display a pod with some specific label - I will run the command **_kubectl get pod --show-labels --selector='app=javaapp'_** and **_kubectl get pod --show-labels --selector='env=dev'_** will display ony the pod with the specified label.

```yml
kubectl get pod --show-labels --selector='app=javaapp'

nginx1   1/1     Running   0          15m   app=javaapp,env=prod
nginx2   1/1     Running   0          15m   app=javaapp,env=dev
```

```yml
kubectl get pod --show-labels --selector='env=dev'

NAME     READY   STATUS    RESTARTS   AGE   LABELS
nginx2   1/1     Running   0          19m   app=javaapp,env=dev
nginx3   1/1     Running   0          19m   app=nodeapp,env=dev
```

**_Reference:_**

- kubernetes documentation
- Kubernetes Up & Running (O'REILLY)
