I will be showcasing a simple project to put together all i have been learning on kubernetes..

This project is about deploying a web application on kubernetes and will make use of the
following kubernetes objects.

* Deployment
* ReplicaSet
* ConfigMap
* Services
* Pods
* Secrets
* Ingress
* Namespaces
* Labels

Let's get started..

In the project, i will be deploying a java spring-boot application that uses mysql as a database and nginx as a web proxy server.

Also , i already have a online domain which i will connect to a kubernetes ingress controller object that will take care of the routing request.