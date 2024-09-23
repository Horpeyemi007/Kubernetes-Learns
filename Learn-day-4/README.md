## MONITORING, LOGGING AND TROUBLESHOOTING IN KUBERNETES

Getting the logs about your kubernetes cluster can make one to understand what is happening. Kubernetes logs are very useful for debugging and monitoring the activity happening inside the cluster.

For example, a particular node might fail of your pod crashes - it is important to understand what might have led to such activity.

With the kubectl command, you can interact with the cluster and get a detailed information as to why a resource is not operating normally as expected.

The kubectl command can be used to troubleshoot the cluster node or some other kubernetes resources like (pods, service, deployment etc.)

Below are some of the kubectl command used for getting information about the activity of a cluster.

kubectl get nodes <'name of the node'>: This will provide information as to the nodes running and it will also display it's status if running or not.

kubectl describe node <'name of the node'>: This will retrieve a detail information about your node. For example when a status of a node is displayed as NotReady - the kubectl describe command can be used to get information about the activity happening in the node.

kubectl logs <'name of the pod'>: This will also output information from the container running inside a pod.

kubectl describe pod <'name of the pod'>: This is also display detailed information about your pod


**_Reference:_**

kubernetes documentation
