kustomize patches

Kustomize patches helps us to target one or more specific sections in k8s resource.

example : if we want to update the number of replicas in deployments , we use customize patch.

To create a patch 3 parameters must be provided.

- operation type: add/remove/replace
- target: what resource should our patch be applied on.
  - kind
  - version/Group
  - Name
  - Namespace
  - labelSelector
  - AnnotationSelector
- Value: what is the value that will either be replaced or added with.( only needed for add and replce options)

------------------------------------------------------------------------------------------------------------------

<h3> There are 2 types of Patches in Kustomize </h3>

- Json 6902 Patch

- Strategic merge patch


# Json 6902 Patch (INLINE) - changing name of deployment

We have a Deployment called "nginx-app-deployment", and we need to change the name to "nginx-web-deployment"

Here we will target two resources.

- kind
- name

our kustomization.yaml will look like this

```

patches:
  - target:
      kind: Deployment
      name: nginx-app-deployment
    patches: |-
       - op: add/remove/replace  #we will select replace
         path: /metadata/name
         value: nginx-web-deployment

```
# Json 6902 Patch (SEPERATE FILE) - changing name of deployment

In the kustomization.yaml file, we provide the path of the yaml file

```
patches:
  - path: replica-path.yaml   
    target:
       kind: Deployment
       name: nginx-web-deployment

```
In  replica-path.yaml   , add  the patch we want to apply

```
- op: replace
  path: /spec/replicase
  value: 5

```
# Json 6902 Patch  - updating replicas.( INLINE )



It is just like normal kubernetes yaml file. It take the older configurations and merge it with the new yaml files. Patch will be updates to newer one.

We will update replicas from 1 -> 2

```

patches:
  - target:
      kind: Deployment
      name: nginx-web-deployment
    patches: |-
       - op: add/remove/replace  #we will select replace
         path: /spec/replicas
         value: 2


```
Patch to remove label from deployment

```

patches:
  - target:
      kind: Deployment
      name: mongo-deployment
    patch: |-
      - op: remove
        path: /spec/template/metadata/labels/org

```


# Strategic Merge Patch - updating replicas.(SEPERATE FILE )

### kustomiztion.yaml
```
patches:
 - replica-patch.yaml
```
### replica-patch.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata: 
  name: nginx-web-deployment
spec:
  replicas: 3
```


# Strategic Merge Patch - updating replicas.(INLINE )

### kustomiztion.yaml

We just pass the  content of yaml file inside kustomization.yaml file.

```
patches:
 - patch: |-
    apiVersion: apps/v1
    kind: Deployment
    metadata: 
        name: nginx-web-deployment
    spec:
        replicas: 3
```

