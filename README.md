# 10 - Container Orchestration with Kubernetes 

## Exercise 1 - Create a Kubernetes cluster

On ubuntu run the following command:

``` bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```
after minikube has been installed run it via (this requires docker runtime to be available on the system)
``` bash
minikube start driver=docker
```

## Exercise 2 - Deploy Mysql with 2 replicas
Create the files:
* [mysql_service](mysql_headless_service.yaml)
* [mysql_pv](mysql_pv.yaml)
* [mysql_statefulset](mysql_statefulset.yaml)
* [db_secrets](java-app_secret.yaml)

and run:
``` bash
kubectl apply -f mysql_headless_service.yaml
kubectl apply -f mysql_pv.yaml
kubectl apply -f java-app_secret.yaml
kubectl apply -f mysql_statefulset.yaml
```

## Exercise 3 - Deploy your Java Application with 2 replicas
Create the following file:
* [java-app_service](java-app_service.yaml)
* [java-app_configmap](java-app_configmap.yaml)
* [java-app_deployment](java-app_deployment.yaml)

and run
``` bash
kubectl apply -f java-app_service.yaml
kubectl apply -f java-app_configmap.yaml
kubectl apply -f java-app_deployment.yaml
```

## Exercise 4 - Deploy phpmyadmin
Create the file:
* [phpmyadmin_service](phpmyadmin_service.yaml)
* [phpmyadmin_deployment](phpmyadmin_deployment.yaml)

and run
``` bash
kubectl apply -f phpmyadmin_service.yaml
kubectl apply -f phpmyadmin_deployment.yaml
```

## Exercise 5 - Deploy Ingress Controller

Run the command:
``` bash
minikube addons enable ingress
```

## Exercise 6 - Create ingress rule
Create the file:
[java-app_ingress](java-app_ingress.yaml)

and run the command:
``` bash
kubectl apply -f java-app_ingress.yaml
```

Now add the ip of the minikube cluster mapped to the hostname of your choice (here it is my-java-app.com) to the /etc/hosts file
You can get the ip via:
``` bash
minikube ip
```

## Exercise 7 - Port-forwarding for phpmyadmin
Run the command:

``` bash
kubectl port-forward services/phpmyadmin 8081:8081
```

## Exercise 8 - Create helm Chart for java app

create an empty helm chart shell:
``` bash
helm create java-app
```
create the respective template files:
* [deployment](java-app/templates/deployment.yaml)
* [service](java-app/templates/service.yaml)
* [configmap](java-app/templates/configmap.yaml)
* [ingress](java-app/templates/ingress.yaml)
* [secret](java-app/templates/secret.yaml)

also adjust the [values file](java-app/values.yaml) respectively.
Then run

``` bash
helm install my-java-app java-app -f values.yaml
```
