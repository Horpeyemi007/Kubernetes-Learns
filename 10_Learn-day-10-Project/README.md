I will be showcasing a simple project to put together all i have been learning on kubernetes..

This project is about deploying a web application on kubernetes and will make use of the
following kubernetes objects.

- Deployment
- ReplicaSet
- ConfigMap
- Services
- Pods
- Secrets
- Ingress
- Namespaces
- Labels

Let's get started..

In the project, i will be deploying a java spring-boot application that uses mysql as a database and nginx as a web proxy server.

Also, i already have a online domain which i will connect to a kubernetes ingress controller object that will take care of the routing request.

Firstly, i will be creating the following yaml kubernetes manifest

- `pjt-configmap.yml`: This will be used to store some configurations for mysql database.
- `pjt-secret.yml`: Used to store database password connection details.
- `pjt-dbdeploy.yml`: This will create a deployment and service of type _ClusterIP_ object for the database.
- `pjt-appdeploy.yml`: This will create a deployment and service of type _ClusterIP_ object for the application.
- `pjt-appingress.yml`: This will allow external access to the services object and also provide routing rules.

### Steps to set up the project

1. Make sure you already have a kubernetes environment set up and preferable this should be on the cloud.
2. A domain name such as `myaddress.com, devopssite.online` is also required as this will be used for the ingress routing rules
3. Clone the github repository and navigate to the `Learn-day-10-Project` folder
4. Create an nginx ingress controller using the _kubectl_ command - i have use the aws provider in this project by running the command `kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.12.0-beta.0/deploy/static/provider/aws/deploy.yaml` to create the ingress controller.
5. After running the command to create the ingress controller - this will create a namespace with the name `nginx-ingress` and also setup a network load balancer on the cloud provider.
6. Once the network load balancer hsa been created, copy the DNS address and create a `CNAM` record on your domain provider with a any name, **for example** _kubesite_ and paste the value as the DNS address of the load balance.
7. Inside the `Learn-day-10-Project` folder, run the command `kubectl create -f .`
8. Verify that all the kubernetes object has been created and is running and once this is complete, then navigate to your browser using the name in _step 6_ with your domain name - _eg_ kubesite.myaddress.com. This should display the application homepage.
