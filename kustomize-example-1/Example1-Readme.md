Kustomize

Kustomize is the tool which helps us to declare our yaml files in more structured manner.

lets say we have 4 environments 
- dev, stage, qa and prod
and we have deployment.yaml, service.yaml, ingress.yaml,namespace.yaml in each environment.

Instead of typing kubectl apply -f <dir/file.yaml> in each directory , we can make use of kustomize

create a kustomization.yaml file
and add these file names inside this file and type command


> Download the kustomize binary , if you are using mac, just type
 `brew install kustomize`

> kustomize build ~/someApp | kubectl apply -f -

OR

> kubectl apply -k ./


In this example 1, we have 2 directories. nginx and apache.
each having its own deployment and service.

type the below command

`kustomize build . | kubectl apply -f -`

we can see the below output

```
service/httpd-service created
service/nginx-service created
deployment.apps/apache-deployment created
deployment.apps/nginx-deployment created 

```
Similarly if we want to delete the deployments created we can just type the below commands

`kustomize build . | kubectl delete -f -`


we can see the below output

```

service "httpd-service" deleted
service "nginx-service" deleted
deployment.apps "apache-deployment" deleted
deployment.apps "nginx-deployment" deleted

```
