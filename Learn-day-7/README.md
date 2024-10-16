## USING VOLUMES IN KUBERNETES POD

In Kubernetes a volume is a directory, possibly with some data in it, which is accessible to the containers in a pod.

When a pod is deleted or a container restarts, all the data in the container file is also removed - in other to have access to an underlying data in the container, the kubernetes volume is used to solve this problem.

In kubernetes, to use a volume, you need to specify the volumes to provide for the Pod in *`.spec.volumes`* and declare where to mount those volumes in the containers.

Also, the following are to be noted about kubernetes volume
* Kubernetes volume are not a standalone Kubernetes object which can be either created of deleted on their own.
* Kubernetes volume are defined at the pod specification level.
* A volume is available to all container inside a pod, however, it must be mounted to the container that needs to access it.

##### Some types of volume kubernetes supports.
* **emptyDir** Volumes are stored on whatever medium that backs the node such as disk, SSD etc
* **hostPath** This mounts a file or directory from the host node's filesystem into your Pod
* **image** Mount the volume on a container image that is available on the host machine
* **local** Volume is mounted on a local storage device such as a disk, partition or directory.
* **nfs** Volume is mounted on an existing nfs file system.  

Using the **hostPath** approach - I will mount a kubernetes pod running mysql server onto the worker nodes as shown 
below.

```yml
apiVersion: v1
kind: Pod
metadata:
  name: dbpod
spec:
  containers:
  - image: mysql:5.7
    name: mysql
    resources:
      requests:
        cpu: "500m"
        memory: "128Mi"
      limits:
        cpu: "1000m"
        memory: "256Mi"
    env:
    - name: MYSQL_ROOT_PASSWORD
      value: "passwd"
    volumeMounts:
    # the path where the data is mounted from
    - mountPath: /var/lib/mysql
      name: dbvolume
  volumes:
  - name: dbvolume
    hostPath:
      # the location on the host where the data is mounted to
      path: /tmp/data
      # this will create the specified path if not exist
      type: DirectoryOrCreate
```

In the above example, wheen the pod is created - then all the data that are been stored inside 
the *`/var/lib/mysql`* will be mounted to *`/tmp/data`* in the worker node where the pod is running.

So, when the pod is eventually been deleted, restarted or it crashes, then we can still get the underlying 
its data.

Using this approach as a volume is not always recommended for a production use case, but should 
however go for a persistent storage type where the volume is been mounted on a remote cloud storage. 
eg (aws ebs,azure disk, google cloud storage etc.).


**_Reference:_**

- kubernetes documentation
- Kubernetes Up & Running (O'REILLY)
