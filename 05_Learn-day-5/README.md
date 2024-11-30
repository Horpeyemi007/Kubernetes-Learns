## KUBERNETES IMPERATIVE AND DECLARATIVE

In other to manage or deploy resource in kubernetes can be achieved using 2 approaches.

These are the

1. Imperative approach
2. Declarative approach

- **_Imperative:_** This is the simplest way to create/manage a kubernetes workload. In this approach, you simply write a command in a form of sentence that instruct kubernetes the kind of specific action to perform.
  <br><br>
  Below is an example of an imperative command that will run a Pod with the name 'testPod' with an nginx image.

```yml
kubectl run testPod --image=nginx
```

- **_Declarative:_** In this approach of creating/managing a kubernetes workload, you simply defined some attribute of section key fields items.
  These items are the configured desired state that kubernetes will use to provision your workload/resources.

A declarative approach is always recommended because with it, kubernetes can always monitor the actual state of the resources created to ensure that they are within the desired state.

Also, with the declarative approach, there is room for proper documentation and review of your code especially in a production environment where various teams are involved.
<br><br>
Below is an example of an declarative command that will run a Pod with the name 'testPod' with an nginx image.

```yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
    - name: testPod
      image: nginx:1.14.2
      ports:
        - containerPort: 80
          name: http
          protocol: TCP
```

<br>

**_Reference:_**

- kubernetes documentation
- Kubernetes Up & Running (O'REILLY)
