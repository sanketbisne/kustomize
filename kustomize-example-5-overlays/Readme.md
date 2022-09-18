Here we will discuss the main use case for kustomize

ie Bases and overlays.

Suppose we have multiple environments like
- Dev
- Stage
- Prod
- QA

In this case we want our configurations seperated on per environment basis, like in Dev we want our labels as env:test, for prod as env:prod , and for QA as env:qa.

or we want sepearate replicas for our environments, For Dev=1, Stage=2 and Prod=5. 

So we can create seperate folders for our environements, and maintain Base folder for our common configurations and overlays for different / changed values.

```
BASE FOLDER           # keep your common yaml files that will remain unchanged
-| Deployment.yaml
-| Service.yaml
-| kustomization.yaml # this include all the resources in this current folder.
OVERLAYS              # keep your different yaml files that will be changed per env.
-| Dev
   - kustomization.yaml # this includes the patches that will be applied on per env basis.
   - configmap.yaml
-| Stage
   - kustomization.yaml # this includes the patches that will be applied on per env basis.
   - configmap.yaml
-| Prod
   - kustomization.yaml # this includes the patches that will be applied on per env basis.
   - configmap.yaml
-| QA
   - kustomization.yaml # this includes the patches that will be applied on per env basis.
   - configmap.yaml
   - apache-deployment.yaml


Here we can have new configs , eg : apache-deployment.yaml which is not available in any other directory or Base configs.
we can add as many configs we need in base as well as overlays

```