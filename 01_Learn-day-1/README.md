## Using _kubectl_ to interacts with kubernetes cluster.

kubectl is used to communicate with the kubernetes cluster control plane through the command line using the kubernetes api.
<br><br>
By default, kubectl looks for a file named config in the **$HOME/.kube** directory.

kubectl uses the config file to identify and authenticate with cluster, and also to get or create information in the cluster.

Below is a sample of what the _config_ looks like.

```yml
apiVersion: v1
clusters:
  - cluster:
      certificate-authority-data: DATA+OMITTED
      server: https://your-k8s-cluster.com
      tls-server-name: api.internal.your-k8s-cluster.com
    name: cluster-name
contexts:
  - context:
      cluster: cluster-name
      user: cluster-name-user
    name: cluster-name
current-context: cluster-name
kind: Config
preferences: {}
users:
  - name: cluster-name-user
    user:
      client-certificate-data: REDACTED
      client-key-data: REDACTED
```

The main content of the config file include..

- cluster: Contains information about how to communicate with a kubernetes cluster

- contexts: Is a references to a cluster (how do I communicate with a kubernetes cluster), a user (how do I identify myself), and a namespace (what subset of resources do I want to work with)

- current-context: Contains the name of the context that you would like to use by default

- users: Refers to a map of reference names to user configs to authenticate with clusters
