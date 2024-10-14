## USING VOLUMES IN KUBERNETES POD

In Kubernetes a volume is a directory, possibly with some data in it, which is accessible to the containers in a pod.

When a pod is deleted or a container restarts, all the data in the container file is also removed - in other to have access to an underlying data in the container, the kubernetes volume is used to solve this problem.

In kubernetes, to use a volume, you need to specify the volumes to provide for the Pod in *`.spec.volumes`* and declare where to mount those volumes in the containers.

Also, the following are to be noted about kubernetes volume
* Kubernetes volume are not a standalone Kubernetes object which can be either created of deleted on their own.
* Kubernetes volume are defined at the pod specification level.
* A volume is available to all container inside a pod, however, it must be mounted to the container that needs to access it.

##### Various types of volume kubernetes supports

**_Reference:_**

- kubernetes documentation
- Kubernetes Up & Running (O'REILLY)
