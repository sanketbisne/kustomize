If we change the values of configmaps , then new configmaps are generated and automatically gets applied on kubernetes deployment. and the older configmap remains in the cluster.

To remove the older config map , we will delete them or prune it

to keep a watch over the config details we need to add option

```
    options:
      labels:
          app-config: my-config

```

after applying, we need to add the option to command 

```

kubectl apply -k . --prune -l app-config=my-config

```

In this way we get rid of older configmaps and keep the latest one in the cluster