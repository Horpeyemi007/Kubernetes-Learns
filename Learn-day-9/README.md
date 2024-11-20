## KUBERNETES CONFIG MAP & SECRETS

### ConfigMaps
In kubernetes, a configMap is used to that stores data (non-confidential data) in key-value pairs, as this is to enable the separation of configuration from the application. ConfigMaps can hold various types of data, allowing for flexibility in managing the application's configuration.

Example of a configMap

```yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: game-demo
data:
  # property-like keys; each key maps to a simple value
  player_initial_lives: "3"
  ui_properties_file_name: "user-interface.properties"

  # file-like keys
  game.properties: |
    enemy.types=aliens,monsters
    player.maximum-lives=5    
  user-interface.properties: |
    color.good=purple
    color.bad=yellow
    allow.textmode=true  
```

#### Using a configMap
A configMap can be used in some of the following ways:

* As a FileSystem: A configMap can be mounted inside a pod by creating a file each based 
on the key name assigned.
* As an environmental variables: a configMap can also be used to dynamically set the value of an environmental variable 

Example of using a configMap inside a pod...

```yml
apiVersion: v1
kind: Pod
metadata:
  name: configmap-demo-pod
spec:
  containers:
    - name: demo
      image: alpine
      command: ["sleep", "3600"]
      env:
        # Define the environment variable
        - name: PLAYER_INITIAL_LIVES
          valueFrom:
            configMapKeyRef:
              name: game-demo           # The ConfigMap this value comes from.
              key: player_initial_lives # The key to fetch.
        - name: UI_PROPERTIES_FILE_NAME
          valueFrom:
            configMapKeyRef:
              name: game-demo
              key: ui_properties_file_name
      volumeMounts:
      - name: config
        mountPath: "/config"
        readOnly: true
  volumes:
  # You set volumes at the Pod level, then mount them into containers inside that Pod
  - name: config
    configMap:
      # Provide the name of the ConfigMap you want to mount.
      name: game-demo
      # An array of keys from the ConfigMap to create as files
      items:
      - key: "game.properties"
        path: "game.properties"
      - key: "user-interface.properties"
        path: "user-interface.properties"
```

The above will create a new volume of type configMap inside the pod with the name `config`. The volume will
be mounter at the `/config` path. Environmental variables are also specified of which their values will be 
referenced using their keys as also referenced in the `game-demo` configMap data keys.


### Secrets
Kubernetes secrets are objects that are used to hold some sort of sensitive data like password, tokens or some other kinds of private information. They are stored as Base64-encoded strings which are not a form of strong encryption.

By using the kubernetes secret, this implies that you do not need to include confidential information inside the application code, instead they can be written as a kubernetes secret and to be used by a pod.

The way Kubernetes secrets works are similar to a configMap, but they are specifically used to hold confidential information.

_**`Note`**_: Secrets in kubernetes are by default stored ad plain text in the etcd storage for the cluster.

**Example of creating a kubernetes secret**:

Lets say i have a text file named `dbpasswd` used to store application password configuration. Inside the file contains the 
following database password information

```
admin@1234#
```
To create a kubernetes secret from the text, i will 
run the command `kubectl create secret generic mysql-password --from-file=./dbpasswd`

This command will create the db password from the file as an encoded kubernetes secrets. The secret
kubernetes object can be viewed by running the command `kubectl get secret mysql-password -o yaml` which will 
display the output as below

```
apiVersion: v1
data:
  dbpasswd: YWRtaW5AMTIzNCMK
kind: Secret
metadata:
  creationTimestamp: "2024-11-19T12:27:15Z"
  name: mysql-password
  namespace: default
  resourceVersion: "5852"
  uid: 31b70a8a-9ea4-4cdd-9126-bbb0e8ce91b9
type: Opaque
```

just like configMap, kubernetes secrets can also be exposed to pod and are always created at the pod creation time.
The following shows how to expose a secret to pod through the volume

```yml
apiVersion: v1
kind: Pod
metadata:
 name: my-pod
spec:
 containers:
  - name: my-app
    image: nginx
    imagePullPolicy: Always
    volumeMounts:
    - name: passwd-vol
      mountPath: "/tls-data"
      readOnly: true
 volumes:
  - name: passwd-vol
    secret:
      secretName: mysql-password
```

**_Reference:_**

- kubernetes documentations
- Kubernetes Up & Running (O'REILLY)
